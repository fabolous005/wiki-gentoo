<!-- source: https://wiki.gentoo.org/wiki/YubiKey/PAM | group: Gentoo Wiki (Main) | wiki-title: YubiKey/PAM -->
---
title: YubiKey/PAM
url: https://wiki.gentoo.org/wiki/YubiKey/PAM
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-11-27"
fingerprint: b801481808a6bdcd
license: CC BY-SA 4.0
---

# YubiKey/PAM

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Many YubiKeys can be configured to provide FIDO/U2F authentication.  This can be configured with the **pam\_u2f.so** module in PAM.

## Introduction

YubiKeys provide several interfaces which can be used for authentication or encryption.  The [U2F/FIDO](https://www.yubico.com/authentication-standards/fido-u2f/) module can be used to provide authentication for both SSH and PAM, and is commonly used with web services.

[PAM](https://wiki.gentoo.org/wiki/PAM) is used to provide centralized authentication on Linux systems, using pluggable modules.  It is typically used as the backend for TTY authentication as well as most local services which require authentication, such as the login manager, screensavers/lockscreens, and privilege escalation tools such as doas or sudo. Using PAM control directives, the required authentication factors can be adjusted depending on what type of service is attempting authentication.

When a username and password are used for authentication, PAM typically uses a combination of the /etc/passwd and /etc/shadow files to map users to their passwords. In order to use a YubiKey with PAM, a file which maps users to their YubiKeys is needed. This can be a central file such as /etc/u2f\_mappings or a per-user file such as \~/.config/Yubico/u2f\_keys.

## Installation

### Kernel

Support for raw USB HID devices is required in the kernel for the YubiKey to function.

**Enable support for raw HID devices**

```
 Device Drivers  --->
   HID support  --->
     -*- HID bus support
     [*]   /dev/hidraw raw HID device support 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_HIDRAW</code> to find this item.
     USB HID support  --->
       [*]   /dev/hiddev raw HID device support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_USB_HIDDEV</code> to find this item.
### USE flags


### Emerge

PAM, is modular by design, and adding modules is straightforward. [sys-auth/pam\_u2f](https://packages.gentoo.org/packages/sys-auth/pam_u2f)<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> is required to use a YubiKey with PAM.  This package provides the PAM module as well as tools to assist in the configuration of this module.

`root #``emerge --ask sys-auth/pam_u2f`
## Configuration

### plugdev group

When [udev](https://wiki.gentoo.org/wiki/Udev) is being used, /lib/udev/rules.d/70-libfido2-u2f.rules defines rules that change the ownership of the YubiKey or other fido2/U2F compliant device's associated /dev/hidraw{n} to be owned by the **plugdev** group with the **0660** mode.  In order for non-root users to access this, which is required to use pamu2fcfg, users must be added to the **plugdev** group.

To check the current user's groups, run:

`user $````
groups
```
tty wheel audio video kvm users larry

If plugdev is not listed, the user can be added to the group by running:

`root #``usermod -a -G plugdev larry`
The user needs to log out and log back in for the group membership to take effect.

### Mapping user-tokens

In order to authenticate with PAM using pam\_u2f, a key token must be mapped to a user - unless the **nouserok** `module argument` is specified. By default, these mappings are read from \~/.config/Yubico/u2f\_keys.

#### Creating user-token mapping (per-user file)

To create a per-user mapping, insert the YubiKey and run pamu2fcfg to create a u2f key mapping for the current user:

`user $````
mkdir -p ~/.config/Yubico
```
`user $````
pamu2fcfg > ~/.config/Yubico/u2f_keys
```
Enter the u2f pin and tap the presence detection pad once it starts blinking.

##### Mapping additional keys

To map an additional key to the current user, replace the YubiKey with the next one and run:

`user $````
pamu2fcfg -n >> ~/.config/Yubico/u2f_keys
```
Touch the YubiKey when it starts blinking.

#### Creating user-token mapping (central file)

To create a central mapping file, insert the YubiKey and run (replacing `user` with the appropriate username):

`root #````
pamu2fcfg -uuser >> /etc/u2f_mappings
```
Touch the YubiKey when it starts blinking.

##### Mapping additional keys

A little more care is needed when mapping an additional key to a user if a central file is used. It is possible to directly concatenate the output of pamu2fcfg if a second mapping is created right after the first one. Each user is represented by a single line with colon-delimited entries corresponding to a YubiKey:

**`/etc/u2f_mappings`**

**Format of the YubiKey mapping file**

Manually copy/pasting the output of the following command onto the end of the relevant user's line in the mapping file is recommended in order to maintain its integrity:

`root #````
pamu2fcfg -n
```
Touch the YubiKey when it starts blinking. Repeat for any remaining YubiKeys.

### PAM U2F

Global system authentication is configured through /etc/pam.d/system-auth. Taking a backup of the current PAM configuration will make it easy to revert changes if needed.

#### PAM options

While configuring [PAM](https://wiki.gentoo.org/wiki/PAM) service files in /etc/pam.d/ to work with a YubiKey or other U2F compliant device, several options can be used:

| Option | Type | Description | 
|---|---|---|
| `required` | control value | The given PAM module must succeed in order for the entire `management group type` (such as **auth**) to succeed. If the PAM module fails, other PAM modules are still executed (even though it is already certain that the service itself will be denied). | 
| `sufficient` | control value | If the given PAM module succeeds, authentication succeeds and the other PAM modules are not executed. If the PAM module fails, then the failure is ignored and PAM continues with the next module. | 
| `[value1=action1 value2=action2 ...]` | control values | Advanced combinations using one or many **value**s, validating the return code of the **value** against the respective **action**, can be used instead of standard control values. | 
| `nouserok` | module argument | Specific to the pam\_u2f.so module, allowing for successful authentication in the event that the user is missing an authorization mapping or it is invalid. Without this, any users missing a mapping will fail authentication. | 
| `cue` | module argument | Specific to the pam\_u2f.so module, prompts *Please touch the device.* when attempting authentication. If not set, the YubiKey will flash as a prompt, but there may be no on-screen indication. | 
| `authfile=/etc/u2f_mappings` | module argument | Specific to the pam\_u2f.so module, sets the path for a central mapping file. If this is not set, \~/.config/Yubico/u2f\_keys is used by default. | 

#### Testing PAM with a YubiKey

In order to test that everything works, temporarily configure PAM to use a YubiKey without locking the user out if pam\_u2f fails by adding the following line to the top of /etc/pam.d/system-auth:

**`/etc/pam.d/system-auth`**

Attempting to log in as a user with a YubiKey mapped should now prompt for it. Providing a correct YubiKey should result in a successful login.

#### Requiring a YubiKey

To require a YubiKey to authenticate with PAM, replace `sufficient` with `required`:

**`/etc/pam.d/system-auth`**

#### Requiring a password and a YubiKey

To require both a password and a YubiKey to authenticate with PAM, modify the file to include the following:

**`/etc/pam.d/system-auth`**

`success=1` means PAM will skip over one module if the current one succeeds. In this case it will jump to the pam\_u2f module if the correct password is given.

`nouserok` is included here so that users without a mapping configured are able to authenticate as well. Leave this out to require all users to provide both a password and a YubiKey.

#### Requiring a password or a YubiKey

To require either a password or a YubiKey to authenticate with PAM (but preferring the YubiKey), modify the file to include the following:

**`/etc/pam.d/system-auth`**

`nouserok` is not included here because it would result in successful authentication without prompting for a password from users without a mapping configured.

#### Requiring a YubiKey for Sudo authentication

If for some reason, it's desirable to only require YubiKey authentication for sudo, but not **system-auth**, the following configuration can be used:

**`/etc/pam.d/sudo`**

**Require YubiKey auth in addition to**system-auth****

## Troubleshooting

If no user is able to authenticate after completing the above, then a broken PAM configuration is the likely culprit. Even if no active root login is available, the system can still be fixed and authentication mechanisms restored by either live booting or booting into single-user mode.

### Fixing PAM through live boot

First, completely power off the machine. Insert the bootable medium and boot from it through the machine's firmware boot menu. There are no universal instructions since this process can vary greatly from machine to machine, so consult the relevant documentation if unfamiliar with how to do this.

Open up a root shell when booted, locate the block device corresponding to *your* root filesystem, and mount it (making sure to specify any required mount options):

`root #````
fdisk -l
```
`root #````
mount [-o options] device /mnt
```
Next, either restore a backup PAM configuration or manually edit /mnt/etc/pam.d/system-auth to undo any changes. To non-destructively undo changes, comment out the necessary entries by prepending a `#` and add any new entries if needed.

Once done, commit the changes to disk, unmount *your* root filesystem, and reboot:

`root #````
sync
```
`root #````
umount /mnt
```
`root #````
reboot
```
Authentication should be fully restored.

### Fixing PAM through single-user mode

To enter single-user mode first reboot the machine. When the GRUB menu appears, press `E` to bring up the menu entry editor. Any edits made in here are temporary and do not edit the on-disk GRUB configuration.

Locate the line which loads the kernel and append `init=/bin/sh` to it. The actual content and number of kernel command line arguments is likely to differ from system to system, but the end result should look similar to the following:

Press `F10` to boot using the present command list.

Once the sh prompt appears, the root filesystem will need to be re-mounted as read/write:

`sh#````
mount -o remount,rw /
```
Only specifying `/` will instruct mount to read the entries in /etc/fstab to find the correct block device and to apply the mount options specified therein.

Next, either restore a backup PAM configuration or manually edit /etc/pam.d/system-auth to undo any changes. To non-destructively undo changes, comment out the necessary entries by prepending a `#` and add any new entries if needed.

Once done, commit the changes to disk, re-mount the root filesystem as read-only, and exit:

`sh#````
sync
```
`sh#````
mount -o remount,ro /
```
`sh#````
exit
```
This will not be a clean exit and the kernel will panic with the message `Kernel panic - not syncing: Attempted to kill init!`. This is fine because all the filesystem changes were manually sync-ed.

Finally, reboot the system. Authentication should be fully restored.

## See also

- [PAM](https://wiki.gentoo.org/wiki/PAM) — allows (third party) services to provide an authentication module for their service which can then be used on PAM enabled systems.
- [PAM securetty](https://wiki.gentoo.org/wiki/PAM_securetty) — restricting root authentication with [PAM](https://wiki.gentoo.org/wiki/PAM).
- [Google Authenticator](https://wiki.gentoo.org/wiki/Google_Authenticator) — describes an easy way to setup two-factor authentication on Gentoo.

## External resources

- [pam.conf(5)](http://www.man7.org/linux/man-pages/man5/pam.conf.5.html), the man page describing PAM configuration files.
- [U2F Key Generation](https://developers.yubico.com/U2F/Protocol_details/Key_generation.html), a description of how keys are generated for U2F on YubiKeys.
