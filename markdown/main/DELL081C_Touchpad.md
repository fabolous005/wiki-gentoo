<!-- source: https://wiki.gentoo.org/wiki/DELL081C_Touchpad | group: Gentoo Wiki (Main) | wiki-title: DELL081C Touchpad -->
---
title: DELL081C Touchpad
url: https://wiki.gentoo.org/wiki/DELL081C_Touchpad
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-07-22"
fingerprint: "3d99fa51079aa124"
license: CC BY-SA 4.0
---

# DELL081C Touchpad

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## dmesg

`user $``dmesg | grep 044E:121F`
\[    6.067024\] input: DELL081C:00 044E:121F Mouse as /devices/pci0000:00/0000:00:15.1/i2c\_designware.1/i2c-7/i2c-DELL081C:00/0018:044E:121F.0002/input/input11
\[    6.067109\] input: DELL081C:00 044E:121F Touchpad as /devices/pci0000:00/0000:00:15.1/i2c\_designware.1/i2c-7/i2c-DELL081C:00/0018:044E:121F.0002/input/input12
\[    6.067174\] input: DELL081C:00 044E:121F UNKNOWN as /devices/pci0000:00/0000:00:15.1/i2c\_designware.1/i2c-7/i2c-DELL081C:00/0018:044E:121F.0002/input/input13
\[    6.067222\] hid-multitouch 0018:044E:121F.0002: input,hidraw1: I2C HID v1.00 Mouse \[DELL081C:00 044E:121F\] on i2c-DELL081C:00

## libinput list-devices

`user $``libinput list-devices | grep DELL081C`
Device:           DELL081C:00 044E:121F UNKNOWN
Kernel:           /dev/input/event12
Group:            7
Seat:             seat0, default
Capabilities:     keyboard 
Tap-to-click:     n/a
Tap-and-drag:     n/a
Tap drag lock:    n/a
Left-handed:      n/a
Nat.scrolling:    n/a
Middle emulation: n/a
Calibration:      n/a
Scroll methods:   none
Click methods:    none
Disable-w-typing: n/a
Disable-w-trackpointing: n/a
Accel profiles:   n/a
Rotation:         0.0
