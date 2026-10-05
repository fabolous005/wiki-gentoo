<!-- source: https://wiki.gentoo.org/wiki/Asrock_Z97_Extreme4 | group: Gentoo Wiki (Main) | wiki-title: Asrock Z97 Extreme4 -->
---
title: Asrock Z97 Extreme4
url: https://wiki.gentoo.org/wiki/Asrock_Z97_Extreme4
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-22"
fingerprint: "2dccd357de9ff16b"
license: CC BY-SA 4.0
---

# Asrock Z97 Extreme4

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## General Information

Asrock Z97 Extreme4 is a motherboard with an Intel LGA1150 socket and Intel Z97 Express Chipset.

### Technical specifications

| CPU-socket | Intel LGA1150 | 
|---|---|
| Chipset | Z97 | 
| Memory | 4 x DIMM, Max. 32 GB, DDR3/DDR3L 3200+(OC)/2933(OC)/2800(OC)/2400(OC)/2133(OC)/1866(OC)/1600/1333/1066 non-ECC, un-buffered memory | 
| Expansion Slots | 3 x PCIe 3.0 x16 slot + 3 x PCIe 2.0 x1 | 
| Storage | 6 xSATA 6 Gb/s - RAID 0, RAID 1, RAID 5, RAID 10 + 2 x SATA3 6.0 Gb/s Connectors by ASMedia ASM1061 | 
| Integrated NIC | Giga PHY Intel® I218V Gigabit LAN 10/100/1000 Mb/s | 
| Integrated Audio | Realtek ALC1150 | 
| USB | 4 x USB 3.0 Ports (Intel® Z97) 2 x USB 2.0 Ports 2 x USB 2.0 Ports (ASMedia ASM1042AE) | 

## Kernel Configuration

### Sata

Select "AHCI SATA support" module.

KERNEL **SATA on Asrock Z97 Extreme4**

### Sound

KERNEL **Sound on Asrock Z97 Extreme4**

### USB

KERNEL **USB on Asrock Z97 Extreme4**

### Network

KERNEL **NIC on Asrock Z97 Extreme**

### Intel MEI

KERNEL **MEI on Asrock Z97 Extreme**

### Sensor

KERNEL **Sensor on Asrock Z97 Extreme**

### PCI Express

KERNEL **PCI Express on Asrock Z97 Extreme**

### SMBus

KERNEL **SMBus on Asrock Z97 Extreme**

### Warning

All tested and working very well.

`user $``uname -mvrpo`
4.0.5-gentoo #4 SMP PREEMPT Thu Jun 11 11:40:18 GMT 2015 x86\_64 Intel(R) Core(TM) i7-4790K CPU @ 4.00GHz GNU/Linux
