<!-- source: https://wiki.gentoo.org/wiki/ASUS_TUF_GAMING_B550M-PLUS | group: Gentoo Wiki (Main) | wiki-title: ASUS TUF GAMING B550M-PLUS -->
---
title: ASUS TUF GAMING B550M-PLUS
url: https://wiki.gentoo.org/wiki/ASUS_TUF_GAMING_B550M-PLUS
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-22"
fingerprint: ff51af55996a35ec
license: CC BY-SA 4.0
---

# ASUS TUF GAMING B550M-PLUS

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article details the ASUS TUF GAMING B550M-PLUS motherboard; providing Linux kernel configuration hints and workarounds. The motherboard has an B550 chipset and AM4 CPU socket compatible with Ryzen CPUs.

## Hardware

### Standard

| Device | Make/model | Status | Bus ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | AMD Ryzen 7 3800X 8-Core Processor |  | N/A | N/A | 5.8.6-gentoo | This CPU rocks! | 
| Audio device (on board) | Starship/Matisse HD Audio Controller |  | 09:00.4 | snd\_hda\_codec\_realtek | 5.8.6-gentoo |  | 
| USB controller | USB controller: Advanced Micro Devices, Inc. \[AMD\] Device 43ee |  | 09:00.3 | ohci-pci | 5.8.6-gentoo |  | 
| SATA controller | SATA controller: Advanced Micro Devices, Inc. \[AMD\] Device 43eb |  | 02:00.1 | ahci | 5.8.6-gentoo |  | 
| NVMe disk | Non-Volatile memory controller: Kingston Technology Company, Inc. Device 2263 (rev 03) |  | 01:00.0 | nvme | 5.8.6-gentoo | KINGSTON A2000 SA2000M8/500G 500ГБ, M.2 2280, PCI-E x4, NVMe on M2\_1 | 
| Ethernet controller | Ethernet controller: Realtek Semiconductor Co., Ltd. RTL8125 2.5GbE Controller (rev 04) |  | 06:00.0 | r8169 | 5.9.6-gentoo | Kernel 5.9.6 and >=sys-kernel/linux-firmware-20201022-r2 for RTL8125(B) support. | 
| Video card | NVIDIA Corporation TU116 \[GeForce GTX 1660 SUPER\] |  | 10de:0185 | x11-drivers/nvidia-drivers | 5.8.6-gentoo |  | 
| Memory | CRUCIAL BL16G32C16U4B.M16FE |  |  |  | 5.8.6-gentoo | 2 modules | 

## Installation

Network is not working in standard Gentoo image. I was using [USB WiFi TP-LINK Archer  T1U](https://www.tp-link.com/us/home-networking/usb-adapter/archer-t1u/) during installation:

### Kernel

The kernel configuration described in

- [Ryzen](https://wiki.gentoo.org/wiki/Ryzen)
- [Gigabyte\_X570-UD](https://wiki.gentoo.org/wiki/Gigabyte_X570-UD) is more specific

You should except from [Gigabyte\_X570-UD](https://wiki.gentoo.org/wiki/Gigabyte_X570-UD) this items:

- Ethernet driver section
- Do not enable \`AMD Secure Memory Encryption (SME) support\` because with this option kernel doesn't boot



**Network requires Kernel 5.9.8**

## Performance and CPU temperature

The [my kernel config](https://github.com/sergeygalkin/cookbook/blob/master/gentoo/kernel_config/home/config) build time (after make clean) is about 3 minutes on this hardware

- disk is INTEL SSDSC2CW240A3
- memory is 2 x CRUCIAL BL16G32C16U4B.M16FE with Configured Memory Speed: 2666 MT/s

`root #``time  make -sj 17`
real	2m58,980s
user	38m32,045s
sys	4m14,134s

The maximum temperature is about 81°C during kernel build with standard AMD Box cooler without overclocking

`user $``sensors`
k10temp-pci-00c3
Adapter: PCI adapter
Vcore:         1.38 V  
Vsoc:          1.10 V  
Tctl:         +81.5°C  
Tdie:         +81.5°C  
Tccd1:        +80.0°C  
Icore:        60.00 A  
Isoc:          6.50 A

### Known hardware issues

#### Ethernet timeouts

- use fresh kernel, 5.14+
- disable scatter-gather on every boot via \`/usr/sbin/ethtool -K eth0 sg off\`

#### Bridge interface is not working with kvm

Found on 5.8.8 kernel. This bridge is standard bridge interface:

`root #``brctl show`
bridge name	bridge id		STP enabled	interfaces
br10		8000.10c37b6d02a3	no		eth0

With kvm interface like this:

Network stop working until kvm stopped. Error is
