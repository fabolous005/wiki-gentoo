<!-- source: https://wiki.gentoo.org/wiki/Asus_P5N32-E_SLI_Plus | group: Gentoo Wiki (Main) | wiki-title: Asus P5N32-E SLI Plus -->
---
title: Asus P5N32-E SLI Plus
url: https://wiki.gentoo.org/wiki/Asus_P5N32-E_SLI_Plus
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-22"
fingerprint: "7f00015cd4a33969"
license: CC BY-SA 4.0
---

# Asus P5N32-E SLI Plus

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## General Information

The Asus P5N32-E SLI Plus is a motherboard with an Intel LGA775 socket, compatible with 45nm multi-core CPUS.

### Technical specifications

| CPU-socket | Intel LGA775 | 
|---|---|
| Front Side Bus | 1333/1066/800/533/ MHz | 
| Memory | 4 x DIMM, Max. 8 GB, DDR2 800/667/533 | 
| Northbridge | C55 a.k.a. nForce®650i SLI | 
| Expansion Slots | 2 x PCIe x16 slot (SLI) + 2 x PCI 2.2 + 1 x PCI Express x16, at x8 speed | 
| Southbridge | MCP55P a.k.a. nForce®570 SLI | 
| Storage | 1 xUltraDMA + 6 xSATA 3 Gb/s | 
| Integrated NIC | Dual Gigabit/ MAC with external Marvell PHY Support | 
| Integrated Audio | ADI 1988B 8 -Channel | 
| USB | 10 USB 2.0 ports (6 ports at mid-board, 4 ports at back panel) | 
| IEE1394 (Firewire) | VIA6308P controller supports 2 x 1394a ports | 

## Kernel Configuration

### SATA

Select "NVIDIA SATA support" module.

KERNEL **SATA on P5N32-E SLI Plus**

```
Device Drivers --->
  Serial ATA (prod) and Parallel ATA (experimental) drivers  --->
   <*>   ATA SFF support
    <*>   NVIDIA SATA support
```
### Sound

The soundchip works with the snd-hda-intel sound module. The corresponding kernel options are:

KERNEL **Soundchip on P5N32-E SLI Plus**

```
Device Drivers --->
  Sound --->
   Advanced Linux Sound Architecture --->
    PCI sound devices --->
     <*> Intel HD Audio
      <*> Build Analog Device HD-audio codec support
```
### USB

KERNEL **USB on P5N32-E SLI Plus**

```
Device Drivers --->
  USB support --->
   <*> EHCI HCD (USB 2.0) support
   <*> OHCI HCD support
```
### Network

The NIC is a NVIDIA Dual Gigabit MAC with external Marvell PHY.

KERNEL **NIC on P5N32-E SLI Plus**

```
Device Drivers --->
  [*] Network device support --->
   [*] Ethernet (10 or 100Mbit)  --->  
    <*> nForce Ethernet support
     [*] Use Rx Polling
   <*> PHY Device support and infrastructure  --->
    <*>   Drivers for Marvell PHYs
```
## Appendices

### Lspci Output

### Disclaimer

Everything been tested working on
