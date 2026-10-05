<!-- source: https://wiki.gentoo.org/wiki/Intel_DQ77MK | group: Gentoo Wiki (Main) | wiki-title: Intel DQ77MK -->
---
title: Intel DQ77MK
url: https://wiki.gentoo.org/wiki/Intel_DQ77MK
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-22"
fingerprint: "264d472dcdaa5863"
license: CC BY-SA 4.0
---

# Intel DQ77MK

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## General Information

The Intel DQ77MK is a motherboard with an Intel LGA1155 socket and Intel Q77 Express Chipset.

[Official Intel Page](https://www.intel.com/content/www/us/en/motherboards/desktop-motherboards/desktop-board-dq77mk.html)
[Intel ARK Page](https://ark.intel.com/products/59044/)

### Technical specifications

| CPU Socket | Intel LGA1155 | 
|---|---|
| Memory | 4 x Unbuffered DIMM, Max. 32 GB, DDR3 1600/1333/1066 MHz | 
| Chipset | Intel Q77 Express Chipset | 
| Expansion Slots | 1x PCIe 3.0 x16 slot + 1x PCIe 2.0 x4 + 1x PCIe 2.0 x1t + 1x PCI + 1x Mini PCIe (supporting mSATA, Wireless Intel AMT) | 
| Storage | 2x SATA 6.0 Gb/s + 2x SATA 3.0 Gb/s + 1x SATA 3.0 Gb/s (multiplexed with an mSATA port, routed to Mini PCIe slot) + 1x eSATA 3.0 Gb/s | 
| Video Output | DP (2560 x 1600 at 60Hz) + DVI-I (1920 x 1200 at 60Hz) + DVI-D (1920 x 1200 at 60Hz) | 
| Integrated NIC | 2x Gigabit (Intel 82579LM + Intel 82574L) | 
| Integrated Audio | Realtek ALC892 8-channel | 
| USB | 4x USB 3.0 ports (2x back panel, 2x internal) + 10x USB 2.0 ports (4x back panel, 4x internal, 2x routed to Mini PCIe slot) | 
| IEE1394 (FireWire) | 2x 1394a ports (1x back panel, 1x internal) | 

## Kernel Configuration

### SATA

Select "NVIDIA SATA support" module.

**SATA on Intel DQ77MK**

### Sound

The soundchip works with the snd-hda-intel sound module. The corresponding kernel options are:

**Soundchip on Intel DQ77MK**

### USB

**USB on Intel DQ77MK**

### Network

The NIC's are a Intel 82579LM (Intel AMT) and Intel 82574L.

**NIC on Intel DQ77MK**

## Appendices

### Lspci Output

### Disclaimer

Everything been tested working on
