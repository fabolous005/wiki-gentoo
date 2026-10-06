<!-- source: https://wiki.gentoo.org/wiki/Corsair_Strafe_RGB | group: Gentoo Wiki (Main) | wiki-title: Corsair Strafe RGB -->
---
title: Corsair Strafe RGB
url: https://wiki.gentoo.org/wiki/Corsair_Strafe_RGB
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-08-24"
fingerprint: "56245556571319ce"
license: CC BY-SA 4.0
---

# Corsair Strafe RGB

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The Corsair Strafe RGB is a mechanical gaming keyboard that is possible of operating cross platforms as a simple USB device. There is ongoing, open source development effort on GitHub (see the link the infobox to the right) to support the advanced features of the keyboard with a system daemon.

## Hardware

The device shows up in lsusb with an ID of `1b1c:1b20 Corsair`.

## Installation

### Kernel

The ckb daemon (installed in the step below) requires user level driver support in order to operate properly. Enable this option in the kernel:

**Enabling`CONFIG_INPUT_UINPUT` support**

```
Device Drivers -->
   Input Device Support -->
      Miscellaneous devices -->
         <*> User level driver support
```
### Emerge

A daemon is required in order to send configuration instructions and firmware updates to the keyboard.

`root #``emerge --ask app-misc/ckb`
## Configuration

### Services

In order to configure the keyboard, and display the beautiful color effects, a daemon must be running.

#### OpenRC

Set the ckb-daemon to start on system boot:

`root #``rc-update add ckb-daemon default`
To start the service now:

`root #``service ckb-daemon start`
#### systemd

For systemd, ensure the ckb.service file will be loaded on system boot:

`root #``systemctl enable ckb.service`
Start the service now via:

`root #``systemctl start ckb.service`
## Usage

One the daemon is running and the kernel has been configured, start the ckb client program. The icon should now show up in most GUI toolbars. It is also possible to start program from the command-line with:

`user $``ckb`
Once the client has been started it will live in the system tray. Be sure to check the "Start ckb at login" checkbox which can be found in the Settings tab. This will start the client with each system boot.

## Troubleshooting

### System boots slowly, hangs on USB device

It is a known issue that the Strafe can cause the system to boot slowly. Generally this is the kernel hanging during USB initialization. Passing `usbhid.quirks=0x1B1C:0x1B20:0x20000408` to the kernel command line is work around for this issue.

For GRUB2, simply:

**`/etc/default/grub`**

```
GRUB_CMDLINE_LINUX="usbhid.quirks=0x1B1C:0x1B20:0x20000408"
```
Then be sure to regenerate GRUB2's configuration file:

`root #``grub2-mkconfig -o /boot/grub/grub.cfg`
Other bootloaders can be handled accordingly.

### Keyboard stops working after a key press

dmesg output looks like the following:

The solution is not known yet...

## See also

- [Razer BlackWidow Chroma](https://wiki.gentoo.org/index.php?title=Razer_BlackWidow_Chroma&action=edit&redlink=1) - Installation and configuration instructions on Gentoo.
