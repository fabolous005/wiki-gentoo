<!-- source: https://wiki.gentoo.org/wiki/Apple_Thunderbolt_to_Gigabit_Ethernet_Adapter | group: Gentoo Wiki (Main) | wiki-title: Apple Thunderbolt to Gigabit Ethernet Adapter -->
---
title: Apple Thunderbolt to Gigabit Ethernet Adapter
url: https://wiki.gentoo.org/wiki/Apple_Thunderbolt_to_Gigabit_Ethernet_Adapter
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2019-03-01"
fingerprint: cdc8aa29b7a2771e
license: CC BY-SA 4.0
---

# Apple Thunderbolt to Gigabit Ethernet Adapter

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Hardware

The Apple Thunderbolt to Gigabit Ethernet Adapter carries the model number: `A1433 EMC 2590`

`root #``lspci -nnk`
Ethernet controller \[0200\]: Broadcom Corporation NetXtreme BCM57762 Gigabit Ethernet PCIe \[14e4:1682\]
Subsystem: Apple Inc. Device \[106b:00f6\]
Kernel driver in use: tg3

## Installation

### Kernel

The recommended minimum version of Linux to use is 4.3. This release fixes an issue that prevented the hot-plugging of Thunderbolt Ethernet devices on Apple hardware<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.

**Enabling Apple Thunderbolt to Gigabit Ethernet Adapter support**

**Enabling Thunderbolt hot-plugging support**
