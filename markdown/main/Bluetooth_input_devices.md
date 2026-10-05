<!-- source: https://wiki.gentoo.org/wiki/Bluetooth_input_devices | group: Gentoo Wiki (Main) | wiki-title: Bluetooth input devices -->
---
title: Bluetooth input devices
url: https://wiki.gentoo.org/wiki/Bluetooth_input_devices
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-21"
fingerprint: "4641fa1ed09a3940"
license: CC BY-SA 4.0
---

# Bluetooth input devices

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes the setup of [Bluetooth](https://wiki.gentoo.org/wiki/Bluetooth) input devices, for example a bluetooth mouse, on a Linux system.

## Installation

### Kernel

Both [Bluetooth](https://wiki.gentoo.org/wiki/Bluetooth) and [evdev](https://wiki.gentoo.org/wiki/Evdev) support is necessary in the kernel. The following options are also required.

### BlueZ settings

Change the value of `UserspaceHID` to `true` in /etc/bluetooth/input.conf to enable user-space HID support:

**`/etc/bluetooth/input.conf`**

```
# Enable HID protocol handling in userspace input profile
# Defaults to false (HIDP handled in HIDP kernel module)
UserspaceHID=true
```
User-space HID support also requires the User-space I/O driver for HID input devices (`CONFIG_UHID`) to be enabled:

**Enabling user-space-hid support**

## Configuration

To configure the input devices use the specialized desktop management tools:

- [net-wireless/gnome-bluetooth](https://packages.gentoo.org/packages/net-wireless/gnome-bluetooth) for [GNOME](https://wiki.gentoo.org/wiki/GNOME)
- [kde-plasma/bluedevil](https://packages.gentoo.org/packages/kde-plasma/bluedevil) for [KDE](https://wiki.gentoo.org/wiki/KDE)
- [net-wireless/blueman](https://packages.gentoo.org/packages/net-wireless/blueman) is a generic GTK client (i.e. for use with Openbox/i3, etc)

Some Bluetooth input devices are initially not in HID mode, but in HCI mode. This is handled by udev in /lib/udev/rules.d/97-hid2hci.rules. Additional devices can be added in a custom rule file which needs to be placed in /etc/udev/rules.d. Refer to the [udev](https://wiki.gentoo.org/wiki/Udev#Rules) article for more details.

## See also

- [Bluetooth](https://wiki.gentoo.org/wiki/Bluetooth) — describes the configuration and usage of Bluetooth controllers and devices.
