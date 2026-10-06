<!-- source: https://wiki.gentoo.org/wiki/ASUS_M4A79T_Deluxe | group: Gentoo Wiki (Main) | wiki-title: ASUS M4A79T Deluxe -->
---
title: ASUS M4A79T Deluxe
url: https://wiki.gentoo.org/wiki/ASUS_M4A79T_Deluxe
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-22"
fingerprint: "5e008916c5a6396c"
license: CC BY-SA 4.0
---

# ASUS M4A79T Deluxe

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

The ASUS M4A79T Deluxe is quite content running Gentoo. No noticeable issues with a standard MBR (BIOS) installation.

## Hardware

### Standard

| Device | Make/model | Status | Bus ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | AMD Phenom(tm) II X4 955 | Works | N/A | N/A | 4.3.3-gentoo |  | 
| Front side bus | RX780/RD790 Host Bridge | Works | 1002:5956 | pcieport | 4.3.3-gentoo |  | 
| SMBus controller | SBx00 SMBus | Works | 1002:4385 | N/A | 4.3.3-gentoo |  | 
| IDE controller | SB7x0/SB8x0/SB9x0 | Works | 1002:439c | pata\_atiixp | 4.3.3-gentoo |  | 
| Audio device (on board) | SBx00 Azalia (Intel HDA) | Works | 1002:4383 | snd\_hda\_intel | 4.3.3-gentoo |  | 
| USB (OHCI0) controller | SB7x0/SB8x0/SB9x0 | Works | 1002:4397 | ohci-pci | 4.3.3-gentoo |  | 
| SATA controller | SB7x0/SB8x0/SB9x0 | Works | 1002:4391 | ahci | 4.3.3-gentoo |  | 
| Firewire controller | VIA Technologies, Inc. VT6315 Series | Works | 1106:3403 | firewire\_ohci | 4.3.3-gentoo |  | 
| Ethernet controller | RTL8111/8168/8411 PCI Express Gigabit Ethernet | Works | 10ec:8168 | r8169 | 4.3.3-gentoo |  | 

### Accessories

| Device | Make/model | Status | Bus ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| VGA controller | NVIDIA Corporation NV18 \[GeForce4 MX 4000\] | Works | 10de:0185 | nouveau | 4.3.3-gentoo |  | 

## Installation

Simply follow MBR instructions in the [AMD64 Handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64) and configure the kernel with the drivers listed above. See the Kernel section below.

### Firmware

No firmware is necessary for this device. Everything is available in the Linux kernel.

### Kernel

KERNEL **Enable support for standard hardware drivers (listed above)**

```
Bus options (PCI etc.) --->
   [*] PCI support
Device Drivers --->
   IEEE 1394 (FireWire) support  --->
      <*> FireWire driver stack
   [*] Network device support  --->
      [*] Ethernet driver support  --->
         [*] Realtek devices
            <*> Realtek 8169 gigabit ethernet support
   <*> Serial ATA and Parallel ATA drivers  --->
      <*> AHCI SATA support
      [*] ATA SFF support (for legacy IDE and PATA)
         <M> ATA BMDMA support
   <*> Sound card support  --->
      <*> Advanced Linux Sound Architecture  --->
         HD Audio --->
            <*> HD Audio PCI
   [*] USB support  ---> 
      <*> OHCI HCD (USB 1.1) support
```
## See also

- [Lenovo Thinkpad W530](https://wiki.gentoo.org/wiki/Lenovo_Thinkpad_W530) - Another one of  [Matthew Marchese (maffblaster)](https://wiki.gentoo.org/wiki/User:Maffblaster) 's machines.
