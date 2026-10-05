<!-- source: https://wiki.gentoo.org/wiki/PAM/U2F | group: Gentoo Wiki (Main) | wiki-title: PAM/U2F -->
---
title: PAM/U2F
author: Authorizations
url: https://wiki.gentoo.org/wiki/PAM/U2F
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-07-08"
fingerprint: be098810094793e9
license: CC BY-SA 4.0
---

# PAM/U2F

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The [sys-auth/pam\_u2f](https://packages.gentoo.org/packages/sys-auth/pam_u2f) package provides two-factor authentication through a FIDO U2F USB device, allowing users to authenticate at a press of a button against their system.

The FIDO alliance' [Universal 2nd Factor](https://en.wikipedia.org/wiki/Universal_2nd_Factor) approach provides a simple two-factor authentication method using specialized USB or NFC devices. The remote service (or, in the case of this article, the local system) sends a challenge as well as a handle, identifying the service itself. This data is passed on to the USB or NFC device, which signs the necessary data with a private key that only that device knows. The signed data is then returned to the service which validates if the signature is valid (as well as other controls, for instance to make sure there is no replay attack or man-in-the-middle).

On Linux, users can authenticate themselves with the USB device thanks to the [sys-auth/pam\_u2f](https://packages.gentoo.org/packages/sys-auth/pam_u2f) PAM module. A one-time registration is needed so that the PAM module knows which public keys are valid for a user. After this, users can authenticate themselves by pressing the key on the USB device.

## Installation

### Kernel

Most U2F USB keys identify themselves as raw HIDs (**H**uman **I**nterface **D**evices). Two kernel configuration modules are needed:

- `CONFIG_HID`
- `CONFIG_USB_HIDDEV`

**Enable support for USB Human Interface Devices**

In many situations, this kernel configuration is already active. If support for /proc/config.gz is enabled, then the following command can verify proper support is available in the running kernel:

`user $``zgrep -E "HID(DEV|RAW)" /proc/config.gz`
CONFIG\_HIDRAW
CONFIG\_USB\_HIDDEV

If the /proc/config.gz does not exist, users can either use the proper config file in /boot or /usr/src/linux/.config, although there is no guarantee that that configuration file is the same one as was used for the currently running Linux kernel.

### Emerge

`root #``emerge --ask sys-auth/pam_u2f`
### Authorizations

As U2F uses USB devices (although NFC is supported by the FIDO Alliance as well, for PAM authentication, USB keys are needed) end users who want to use U2F authentication should be part of the usb group:

`root #``gpasswd -a` `<username>` usb
Although this is not always mandatory (only when the end user is allowed to manage its own authorization mapping files, as will be discussed later), this is a best practice anyhow as users who have an U2F key want to use it to authenticate against remote services as well, such as Google. In that case, USB access is required anyway.

## Configuration

The installation of the package will install the pam\_u2f.so file in /lib64/security, where all other PAM modules are stored. This however does not automatically enable the use of the module.

### Testing

Before configuring the system to use the pam\_u2f.so module, first ensure that the USB device functions properly and that the interface works. For that, plug in the USB device and check if the Linux kernel detects it properly.

`user $``dmesg`
\[ 5504.582586\] usb 3-2: new full-speed USB device number 6 using xhci\_hcd
\[ 5504.751688\] usb 3-2: New USB device found, idVendor=1050, idProduct=0120
\[ 5504.751691\] usb 3-2: New USB device strings: Mfr=1, Product=2, SerialNumber=0
\[ 5504.751693\] usb 3-2: Product: Security Key by Yubico
\[ 5504.751694\] usb 3-2: Manufacturer: Yubico
\[ 5504.752712\] hid-generic 0003:1050:0120.0002: hiddev96,hidraw1: USB HID v1.10 Device \[Yubico Security Key by Yubico\] on usb-0000:00:14.0-2/input0

If the detection works properly, then the system device manager should have created a device file for it (as the package installs the proper udev rules). From the about output, the device file would be /dev/hidraw1.

### Registration

The U2F PAM module has two modus operandi to store the registered device (public) keys. Either a central file is used (called "central authorization mapping"), or users store the public key from the device in their home directories (called "individual authorization mapping". The latter might be the most flexible, as users can update the file themselves when needed (for instance when they acquire an additional U2F key), but requires the home directory to be available. As such, encrypted home directories cannot be supported as those are only decrypted after successful authentication.

- In case of individual authorization mappings, each user has to create the \~/.config/Yubico/u2f\_keys file and store the key handle and user keys of one or more U2F devices. Don't worry about what that is - there is a nice command that will help generate the file later on.
- In case of central authorization mappings, a central file has to be created by the administrator which will contain the same information. Unlike with individual authorization mappings, administrators can freely decide what the path of the file is. The main documentation suggests /etc/u2f\_mappings.

To generate the proper content, plug in the USB device if it hasn't been already, and use the pamu2fcfg command. In the following command, its output is sent to the individual authorization mapping file as mentioned earlier on. Change accordingly to /etc/u2f\_mappings if a central authorization mapping will be used.

`user $``mkdir ~/.config/Yubico``user $``pamu2fcfg -u``<username>` > ~/.config/Yubico/u2f_keys
The command will trigger the USB device, so press the button on the device to create the proper registration.

If successful, the target file will contain a single line, starting with the username, and then followed by two base64 strings. The first string contains the keyhandle, while the second string is the user key.

When multiple devices need to be registered, have all devices plugged in so that the command will fetch the handles and keys from all detected devices. If that isn't possible, repeat the steps but with a temporary target file. Then join the file, resulting in a single line but with multiple handles and keys, like so:

### Configuring PAM

With the registration finished, configure the proper PAM services to use the U2F USB device.

The next example shows how the pam\_u2f.so file is used during the authentication for local system logins:

**`/etc/pam.d/system-local-login`**

**Enabling U2F PAM in local system logins**

In the above example, successful authentication through U2F is mandatory, while other authentication approaches (such as password-based authentication) are still needed as well. In this case, multi-factor authentication is implemented. If authentication through U2F should suffice, then use sufficient rather than required.

When the system uses a central authorization mapping file, add the authfile argument like so:

**`/etc/pam.d/system-local-login`**

**Enabling U2F PAM in local system logins with central authentication mapping**

When not all users on the system are required to use U2F, add the `nouserok` option to have the PAM module continue if the user is not mentioned in the authorization mapping file:

**`/etc/pam.d/system-local-login`**

**Enabling U2F PAM for only the listed users**

A full list of supported arguments and options is available in the [main documentation](https://github.com/Yubico/pam-u2f).

### Emergency procedure

Forcing the use of the U2F USB device does require that the device is always available for use, and that the PAM module itself is never with any errors. There are a number of risks involved here, which need to be properly covered.

To prevent issues when the USB device itself is malfunctioning, it is a best practice to always register two USB devices, and keep one in a safe place for emergency reasons. Also don't forget to register the USB device both for the regular accounts as well as administrative accounts (like root).

To prevent issues with the PAM module itself, it is possible to have particular PAM services not use the U2F PAM module. For instance, the SSH service is probably not going to use local U2F because the PAM module (by default) will attempt to use a USB device plugged in to the target system itself (the system that has the SSH daemon running) and not the client (the system that connects to the SSH daemon). Of course, properly protect those services differently (for instance through mandatory use of SSH public key authentication).

In the worst case, the administrator can still reboot the system in single user mode and modify the PAM service definitions accordingly.

## Usage

With the above configuration, whenever a local logon is triggered (be it through a terminal or a graphical manager), the use of the U2F USB device is necessary. If the device is not plugged in, or the user does not confirm the logon in due time, authentication will fail.

## Troubleshooting

### pamu2fcfg mentions no U2F device is available

When attempting to register a U2F USB device, the output of the pamu2fcfg command mentions that no U2F device is available:

`user $``pamu2fcfg -u``<username>`
No U2F device available, please insert one now, you have 15 seconds.

This means that the pamu2fcfg command failed to detect the USB device. The trivial problem here is that the USB device itself is not plugged in yet, but the problem might be elsewhere as well.

The pamu2fcfg command uses the udev information, obtained from the udev tables, to find out where the USB device is found. Hence, the command requires access to /var/run/udev/data. Make sure that the user that runs the command (if it isn't root) can read files within that location.

Once obtained, the command will write to and read from the USB device, so the user that runs the command must have read and write privileges on the device file itself. By default, this is granted by having the user be part of the usb group.

### SELinux is preventing access

Any SELinux domain that wants to interact with the U2F USB device must have the following policy active:

Without these rules, the U2F libraries will fail to either know where the USB device is attached to (the first two policy lines) or interact with the device itself (the third policy line).

## Removal

Before removing the [sys-auth/pam\_u2f](https://packages.gentoo.org/packages/sys-auth/pam_u2f) package, make sure that all references to pam\_u2f.so are removed from the PAM service definitions. A PAM service configuration that refers to a missing module will result in failed authentication, and might lock users out of their own system.

### Unmerge

`root #``emerge --ask --depclean --verbose sys-auth/pam_u2f`
## See also

- [PAM](https://wiki.gentoo.org/wiki/PAM) — allows (third party) services to provide an authentication module for their service which can then be used on PAM enabled systems.

## External resources

- [The pam-u2f project on github](https://github.com/Yubico/pam-u2f) is the master of the pam\_u2f code, and contains information about common configurations and usages.
