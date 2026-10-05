<!-- source: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_X1_Tablet_(Gen_2) | group: Gentoo Wiki (Main) | wiki-title: Lenovo ThinkPad X1 Tablet (Gen 2) -->
---
title: Lenovo ThinkPad X1 Tablet (Gen 2)
url: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_X1_Tablet_(Gen_2)
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "2708a5dfdf26b9f3"
license: CC BY-SA 4.0
---

# Lenovo ThinkPad X1 Tablet (Gen 2)

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | i7-7Y75 |  | N/A | N/A | 5.10 |  | 
| Wi-Fi | N/A |  | N/A | iwlwifi | 5.10 |  | 
| microSD card reader | N/A |  | N/A | N/A | 5.10 |  | 
| Touchscreen | WCOM5115:00 |  | 056A:5115 | N/A | 5.10 | Requires Kernel \< 5.10 to work. | 

### Accessories

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| Dock | ThinkPad OneLink+ Dock |  | N/A | N/A | 5.10 |  | 

## Installation

### Kernel

KERNEL **Wi-Fi (kernel 5.10)**

KERNEL **Ethernet (ThinkPad OneLink+ Dock)**

KERNEL **microSD Card Reader**

KERNEL **DisplayLink (USB-C)**

KERNEL **Touchscreen**

## Troubleshooting

### Keyboard, trackpoint and touchpad do not work

For some reason on kernels >= 4.2 the keyboard, trackpoint and touchpad do not work on first boot. To get around this problem add **i8042.nomux=1 i8042.reset** to the kernel parameters:

FILE **`/etc/default.grub`**
