<!-- source: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_X1_Carbon_3rd_Gen | group: Gentoo Wiki (Main) | wiki-title: Lenovo ThinkPad X1 Carbon 3rd Gen -->
---
title: Lenovo ThinkPad X1 Carbon 3rd Gen
url: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_X1_Carbon_3rd_Gen
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "58fac7cdda23261"
license: CC BY-SA 4.0
---

# Lenovo ThinkPad X1 Carbon 3rd Gen

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | Intel i7-5600 series |  | N/A | N/A | 4.12.12 |  | 
| WQHD Touchscreen | 2540x1440 |  | N/A | N/A | 4.12.12 | Does not need the Xorg mutouch driver for the touchscreen to work | 
| WiFi | Intel Wireless |  | N/A | iwlwifi | 4.12.12 |  | 
| Synaptics or Elan Touchpad | N/A |  | N/A | N/A | 4.12.12 |  | 
| Video card | Intel 5000-series \[Intel 5600\] |  | N/A | intel i965 | 4.12.12 |  | 

## Installation

### Firmware

External firmware is required for Wi-Fi to work:

`root #``emerge --ask sys-kernel/linux-firmware`
### Kernel

KERNEL **Enable support for hardware drivers, filesystems and features**

### Emerge

FILE **`/etc/portage/make.conf`**
