<!-- source: https://wiki.gentoo.org/wiki/Apple_Keyboard | group: Gentoo Wiki (Main) | wiki-title: Apple Keyboard -->
---
title: Apple Keyboard
url: https://wiki.gentoo.org/wiki/Apple_Keyboard
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-03-23"
fingerprint: fec69929797db189
license: CC BY-SA 4.0
---

# Apple Keyboard

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Installation

### Kernel

**Enabling the Apple HID driver for Linux 4.1 or higher**

## Configuration

### Behavior

The behavior of the function keys (`F1`-`F12`) defaults to their special functions (screen brightness, volume control, etc.). To change the behavior of the function keys to their traditional functions (`F5` - page refresh, `F11` - full screen, etc.), set the `fn` key mode to function keys first:

`root #``echo 2 > /sys/module/hid_apple/parameters/fnmode`
When using a non-English layout, the `<` and `>` keys may be mapped to the `^` and `°` keys. To correct this, disable the hard-coded ISO layout of the keyboard:

`root #``echo 0 > /sys/module/hid_apple/parameters/iso_layout`
If the Apple HID driver is built into the kernel, create the following [local.d](https://wiki.gentoo.org/wiki/Local.d) script to have the module parameters set at boot:

**`/etc/local.d/hid_apple.start`**

```
#!/bin/sh
# set fn key mode to function keys first
echo 2 > /sys/module/hid_apple/parameters/fnmode
# disable iso layout
echo 0 > /sys/module/hid_apple/parameters/iso_layout
```
Set the local.d script as executable:

`root #``chmod +x /etc/local.d/hid_apple.start`
If the Apple HID driver is built as a module, add the following to /etc/modprobe.d/keyboard.conf to have the module parameters set at boot:

**`/etc/modprobe.d/keyboard.conf`**
