<!-- source: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_Twist_S230u | group: Gentoo Wiki (Main) | wiki-title: Lenovo ThinkPad Twist S230u -->
---
title: Lenovo ThinkPad Twist S230u
url: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_Twist_S230u
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "3712ad4fd1a739f3"
license: CC BY-SA 4.0
---

# Lenovo ThinkPad Twist S230u

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| Keyboard | N/A |  | N/A | N/A | 4.2 |  | 
| Wi-Fi | Intel Corporation Centrino Wireless-N 2230 |  | N/A | iwlwifi | 4.2 |  | 
| Sound | N/A |  | N/A | N/A | 4.2 |  | 
| Webcam | N/A |  | N/A | N/A | 4.2 |  | 
| Card Reader | RTS5229 PCI Express Card Reader |  | N/A | N/A | 4.2 |  | 

## Installation

Using a standard [stage3](https://gentoo.org/downloads/) installation everything should pretty much work out of the box.

### Firmware

The wireless card requires external firmware (**iwlwifi-2030-6.ucode**):

`root #``emerge --ask sys-kernel/linux-firmware`
### Kernel

**External firmware (optional, can be loaded as a module)**

**Wi-Fi**

**Sound**

**Webcam**

**Card Reader**

### Emerge

Driver for card reader:

`root #``emerge --ask sys-block/rts5229`
## Troubleshooting

### Keyboard/Trackpoint/Touchpad does not work

For some reason on kernels >= 4.2 the keyboard, trackpoint and touchpad do not work on the first boot. To work around this problem, add **i8042.nomux=1 i8042.reset** to your kernel parameters:

**`/etc/default.grub`**

**add to kernel command line**
