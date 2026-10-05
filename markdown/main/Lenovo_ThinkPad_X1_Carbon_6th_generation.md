<!-- source: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_X1_Carbon_6th_generation | group: Gentoo Wiki (Main) | wiki-title: Lenovo ThinkPad X1 Carbon 6th generation -->
---
title: Lenovo ThinkPad X1 Carbon 6th generation
url: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_X1_Carbon_6th_generation
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "87b12c1ecfe2e4d5"
license: CC BY-SA 4.0
---

# Lenovo ThinkPad X1 Carbon 6th generation

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

## Installation

Overall, follow everything in the [installation guide](https://wiki.gentoo.org/wiki/Handbook:AMD64) with minor changes.

### Firmware

Wireless requires [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) to be merged. See [Linux firmware](https://wiki.gentoo.org/wiki/Linux_firmware) for more details.

`root #``emerge --ask sys-kernel/linux-firmware`
### Kernel

KERNEL **NVMe drive (kernel version 4.13)**
