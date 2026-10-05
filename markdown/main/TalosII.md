<!-- source: https://wiki.gentoo.org/wiki/TalosII | group: Gentoo Wiki (Main) | wiki-title: TalosII -->
---
title: TalosII
url: https://wiki.gentoo.org/wiki/TalosII
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-05-21"
fingerprint: e759299e11b29a0b
license: CC BY-SA 4.0
---

# TalosII

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The Talos II is a dual socket Power 9 motherboard from Raptor Computing Systems. It features opensource firmware and [openBMC](https://www.openbmc.org/).

## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | IBM Power 9 |  | N/A | N/A | 4.17.12 | Requires kernel version 4.16+, ideally 4.17+. Also benefits from very recent gcc releases. | 
| Video card | ASpeed Graphics |  | 1a03:2000 | ast | 4.17.12 | Kernel driver works without issue. xf86-video-ast has build issues, xf86-video-modesetting works fine with glamor disabled. | 

## Installation

The system uses [petitboot](https://github.com/open-power/petitboot) as main bootloader. The grub based ppc64le install media iso contents can be copied to a partition of an usb storage and used.

Make sure to pass **console=hvc0** as boot argument if you are using the OpenBMC console.

### OpenBMC

The [openbmc](https://www.openbmc.org/) will ask for an address if the first Ethernet port is connected.
It would listen to port 22 and port 2200. Petitboot will be directly reachable from port 2200.

### Kernel

Use powernv\_defconfig.

To enable hardware accelerated virtualization:

**Enable hardware virtualization, kvm\_hv**

## Configuration

### Bootloader

The Talos uses petitboot as part of its firmware and bootloader. This means there is no need to install grub/yaboot/etc, as all petitboot needs is the config file. kboot has a delightfully simple config format, which is easily written.
