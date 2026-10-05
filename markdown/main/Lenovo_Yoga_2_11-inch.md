<!-- source: https://wiki.gentoo.org/wiki/Lenovo_Yoga_2_11-inch | group: Gentoo Wiki (Main) | wiki-title: Lenovo Yoga 2 11-inch -->
---
title: Lenovo Yoga 2 11-inch
url: https://wiki.gentoo.org/wiki/Lenovo_Yoga_2_11-inch
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "3f59bd167bb0324b"
license: CC BY-SA 4.0
---

# Lenovo Yoga 2 11-inch

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

Information contained herein was acquired from model no. 20332, but may work with variants.

## Hardware

### Laptop Specifications

| Device | Model | Works | Notes | 
|---|---|---|---|
| Intel® Pentium™ | N3520, 2.42GHz, 4T, 2M (BayTrail) |  |  | 
| Intel® HD Graphics | 4200 |  | i915 kernel driver | 
| 11.6" 1366x768 IPS TFT LCD |  |  |  | 
| Wireless | Qualcomm Atheros QCA9565 |  | ath9k kernel driver | 
| Bluetooth |  |  |  | 
| Camera |  |  | uvcvideo | 
| Card Reader |  |  |  | 
| ELantech Touchpad | ETPS/2 |  | PS/2 Mouse as module + Elantech PS/2 protocol extension | 
| Intel HD Audio |  |  | HD Audio PCI (snd-hda-intel) | 
| Digitizer | Atmel maXTouch |  |  | 

#### Hardware

`root #``lspci -k`
00:00.0 Host bridge: Intel Corporation Atom Processor Z36xxx/Z37xxx Series SoC Transaction Register (rev 0e)
	Subsystem: Lenovo Atom Processor Z36xxx/Z37xxx Series SoC Transaction Register
	Kernel driver in use: iosf\_mbi\_pci
00:02.0 VGA compatible controller: Intel Corporation Atom Processor Z36xxx/Z37xxx Series Graphics & Display (rev 0e)
	Subsystem: Lenovo Atom Processor Z36xxx/Z37xxx Series Graphics & Display
	Kernel driver in use: i915
	Kernel modules: i915
00:13.0 SATA controller: Intel Corporation Atom Processor E3800 Series SATA AHCI Controller (rev 0e)
	Subsystem: Lenovo Atom Processor E3800 Series SATA AHCI Controller
	Kernel driver in use: ahci
00:14.0 USB controller: Intel Corporation Atom Processor Z36xxx/Z37xxx, Celeron N2000 Series USB xHCI (rev 0e)
	Subsystem: Lenovo Atom Processor Z36xxx/Z37xxx, Celeron N2000 Series USB xHCI
	Kernel driver in use: xhci\_hcd
00:1a.0 Encryption controller: Intel Corporation Atom Processor Z36xxx/Z37xxx Series Trusted Execution Engine (rev 0e)
	Subsystem: Lenovo Atom Processor Z36xxx/Z37xxx Series Trusted Execution Engine
	Kernel driver in use: mei\_txe
	Kernel modules: mei\_txe
00:1b.0 Audio device: Intel Corporation Atom Processor Z36xxx/Z37xxx Series High Definition Audio Controller (rev 0e)
	Subsystem: Lenovo Atom Processor Z36xxx/Z37xxx Series High Definition Audio Controller
00:1c.0 PCI bridge: Intel Corporation Atom Processor E3800 Series PCI Express Root Port 1 (rev 0e)
	Kernel driver in use: pcieport
	Kernel modules: shpchp
00:1f.0 ISA bridge: Intel Corporation Atom Processor Z36xxx/Z37xxx Series Power Control Unit (rev 0e)
	Subsystem: Lenovo Atom Processor Z36xxx/Z37xxx Series Power Control Unit
	Kernel driver in use: lpc\_ich
	Kernel modules: lpc\_ich
00:1f.3 SMBus: Intel Corporation Atom Processor E3800 Series SMBus Controller (rev 0e)
	Subsystem: Lenovo Atom Processor E3800 Series SMBus Controller
	Kernel driver in use: i801\_smbus
	Kernel modules: i2c\_i801
01:00.0 Network controller: Qualcomm Atheros QCA9565 / AR9565 Wireless Network Adapter (rev 01)
	Subsystem: Lenovo QCA9565 / AR9565 Wireless Network Adapter
	Kernel driver in use: ath9k
	Kernel modules: ath9k

`root #``lsusb`
Bus 001 Device 005: ID 048d:8386 Integrated Technology Express, Inc. 
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 001 Device 006: ID 1bcf:2c66 Sunplus Innovation Technology Inc. 
Bus 001 Device 008: ID 0cf3:3004 Qualcomm Atheros Communications AR3012 Bluetooth 4.0
Bus 001 Device 003: ID 03eb:8c1d Atmel Corp. 
Bus 001 Device 004: ID 0781:5581 SanDisk Corp. Ultra
Bus 001 Device 002: ID 05e3:0608 Genesys Logic, Inc. Hub
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

## Configuration details

### Graphics

See [Intel](https://wiki.gentoo.org/wiki/Intel).

### Touchpad

This laptop has an Elantech PS/2 touchpad:

KERNEL

### make.conf

It is recommended to add or replace the following lines

FILE **`/etc/portage/make.conf`**

```
VIDEO_CARDS="intel i965"
CPU_FLAGS_X86="mmx mmxext popcnt sse sse2 sse3 sse4_1 sse4_2 ssse3"
INPUT_DEVICES="evdev synaptics"
```
