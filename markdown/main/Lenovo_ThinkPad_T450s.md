<!-- source: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_T450s | group: Gentoo Wiki (Main) | wiki-title: Lenovo ThinkPad T450s -->
---
title: Lenovo ThinkPad T450s
url: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_T450s
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "7f5087052baa526c"
license: CC BY-SA 4.0
---

# Lenovo ThinkPad T450s

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

This article describes hardware specific setup steps for the Lenovo ThinkPad T450s on Gentoo.

## Hardware

| Device | Make/model | Status | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|
| CPU | Intel(R) i7-5600U Processor (4M Cache, 2.6GHz) |  | N/A | 4.19.97,5.4.28 | Microcode update recommended | 
| Video card | Intel Corporation HD Graphics 5500 (rev 09) |  | i915 | 4.19.97,5.4.28 | N/A | 
| Ethernet controller | Intel Corporation Ethernet Connection (3) I218-LM (rev 03) |  | e1000e | 4.19.97,5.4.28 | N/A | 
| Audio device | Intel Corporation Wildcat Point-LP High Definition Audio Controller (rev 03) |  | snd\_hda\_intel | 4.19.97,5.4.28 | N/A | 
| Wireless controller | Intel Corporation Wireless 7265 (rev 99) |  | iwlwifi | 4.19.97,5.4.28 | N/A | 
| SD card reader | Realtek Semiconductor Co., Ltd. RTS5227 PCI Express Card Reader (rev 01) |  | rtsx\_pci | 4.19.97,5.4.28 | N/A | 
| Integrated camera | Chicony Electronics |  | uvcvideo | 4.19.97,5.4.28 | N/A | 
| Fingerprint sensor | Validity Sensors, Inc. VFS 5011 |  | N/A | 4.19.97,5.4.28 | N/A | 
| EHCI controller | Intel Corporation Wildcat Point-LP USB EHCI Controller (rev 03) |  | ehci\_pci | 4.19.97,5.4.28 | N/A | 
| xHCI controller | Intel Corporation Wildcat Point-LP USB xHCI Controller (rev 03) |  | xhci\_pci | 4.19.97,5.4.28 | N/A | 
| MEI controller | Intel Corporation Wildcat Point-LP MEI Controller #1 (rev 03) |  | mei\_me | 4.19.97,5.4.28 | N/A | 
| PCI Express controller | Intel Corporation Wildcat Point-LP PCI Express Root Port #3 (rev e3) |  | pcieport | 4.19.97,5.4.28 | N/A | 
| SATA controller | Intel Corporation Wildcat Point-LP SATA Controller \[AHCI Mode\] (rev 03) |  | ahci | 4.19.97,5.4.28 | N/A | 
| LPC controller | Intel Corporation Wildcat Point-LP LPC Controller (rev 03) |  | lpc\_ich | 4.19.97,5.4.28 | N/A | 
| SMBus controller | Intel Corporation Wildcat Point-LP SMBus Controller (rev 03) |  | ic2\_i801 | 4.19.97,5.4.28 | N/A | 
| Host bridge | Intel Corporation Broadwell-U Host Bridge -OPI (rev 09) |  | bdw\_uncore | 4.19.97,5.4.28 | N/A | 

## Installation

It is a good idea to update to the latest BIOS available, check [the official page](https://support.lenovo.com/au/en/downloads/ds102109).

The installation procedure from a Gentoo installation media works perfectly as described [in the Handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64/Full/Installation).

Make sure to boot in **UEFI** mode (*BIOS, Startup, UEFI/Legacy Boot must be set to \[UEFI Only\]*) and follow the UEFI variants in the guide. [Genkernel](https://wiki.gentoo.org/wiki/Genkernel) should generate a working kernel with pretty much everything working out of the box.

To customize the kernel, the section below describes the needed modules.

## Configuration

### CPU

It is recommended to enable the microcode update support as explained in [Intel microcode](https://wiki.gentoo.org/wiki/Intel_microcode).

### Kernel modules

These sections contain only the specific portion needed to enable the device. Most of the drivers can be compiled as modules or built directly into the kernel, it is up to the reader to choose the preferred way of compilation.

#### ACPI

**ACPI**

#### Hardware monitoring

**Hardware monitoring**

#### Video card

**Video card**

#### Ethernet controller

**Ethernet controller**

#### Wireless controller

**Wireless controller**

#### Integrated camera

**Integrated camera**

#### Audio device

**Audio device**

#### EHCI controller

**EHCI controller**

#### xHCI controller

**xHCI controller**

#### MEI controller

**MEI controller**

#### PCI Express controller

**PCI Express controller**

#### SATA controller

**SATA controller**

#### LPC controller

**LPC controller**

#### SMBus controller

**SMBus controller**

#### Host bridge

**Host bridge**

### Hotkeys

If you have a recent BIOS installed all the Fn keys should work out of the box.

Bear in mind there's a BIOS option to alternate between standard F1-F12 functions and media functions: *BIOS, Config, Keyboard/Mouse, F1-F12 as Primary Function*.
