<!-- source: https://wiki.gentoo.org/wiki/Framework_Expansion_Cards | group: Gentoo Wiki (Main) | wiki-title: Framework Expansion Cards -->
---
title: Framework Expansion Cards
url: https://wiki.gentoo.org/wiki/Framework_Expansion_Cards
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-04-28"
fingerprint: "1aae5bb718c39788"
license: CC BY-SA 4.0
---

# Framework Expansion Cards

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The [Framework Laptop 13](https://wiki.gentoo.org/wiki/Framework_Laptop_13) and [Framework Laptop 16](https://wiki.gentoo.org/wiki/Framework_Laptop_16) use an expansion card system that allows the user to customize the laptop's ports and functionality. Each connects to an underlying USB-C interface. See the [Framework marketplace](https://frame.work/marketplace/expansion-cards).

## Hardware

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes |  | 
|---|---|---|---|---|---|---|---|
| DisplayPort | Framework | Works | 32ac:0003 | Requires CONFIG\_TYPEC\_DP\_ALTMODE=y (CONFIG\_TYPEC\_TBT\_ALTMODE=y if using thunderbolt dock) compiling as a module doesn't work on Kernel 6.18 or higher. |  |  |  | 
| 2.5GB Ethernet | Framework (RTL8156) | Works | 0bda:8156 | r8152 ( [net-misc/r8152](https://packages.gentoo.org/packages/net-misc/r8152)) |  | Will show up but never gain link with vanilla kernel. Need official driver kernel module from [net-misc/r8152](https://packages.gentoo.org/packages/net-misc/r8152). |  | 
| HDMI | Framework | Works | 32ac:0002 | uhci\_hcd |  |  |  | 
| SSD | Framework | Works | 13fe:6500 (1TB) | usb\_storage, uas |  |  |  | 
| MicroSD | Framework | Works | 090c:3350 | usb\_storage, uas |  |  |  | 
| SD (full size) | Framework | Works | 32ac:0009 | usb\_storage, uas |  |  |  | 
| USB-A | Framework | Works |  | xhci\_hcd |  |  |  | 
| USB-C | Framework | Works |  | xhci\_hcd | 5.14.15 | Required for charging |  | 
| Audio | Framework | Works | 32ac:0010 | snd\_usb\_audio |  |  |  |
