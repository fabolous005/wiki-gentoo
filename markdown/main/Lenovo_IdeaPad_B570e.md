<!-- source: https://wiki.gentoo.org/wiki/Lenovo_IdeaPad_B570e | group: Gentoo Wiki (Main) | wiki-title: Lenovo IdeaPad B570e -->
---
title: Lenovo IdeaPad B570e
url: https://wiki.gentoo.org/wiki/Lenovo_IdeaPad_B570e
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "7f5a870ddbaad26a"
license: CC BY-SA 4.0
---

# Lenovo IdeaPad B570e

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | Intel Celeron B800 |  | N/A | N/A | N/A |  | 
| GPU | Intel Corporation 2nd Generation Core Processor Family DRAM Controller |  | 8086:0104 | N/A | N/A |  | 
| Sound | Intel Corporation 6 Series/C200 Series Chipset Family High Definition Audio Controller |  | 8086:1c20 | N/A | N/A |  | 
| Ethernet | Realtek Semiconductor Co., Ltd. RTL8111/8168 PCI Express Gigabit Ethernet controller |  | 10ec:8168 | N/A | N/A |  | 
| Wi-Fi | Broadcom Corporation BCM4313 802.11b/g/n Wireless LAN Controller |  | 14e4:4727 | N/A | N/A |  | 
| Bluetooth | Broadcom Corporation BCM4313 802.11b/g/n Wireless LAN Controller |  | N/A | N/A | N/A |  | 
| Webcam | Chicony Electronics Co., Ltd Lenovo EasyCamera |  | 04f2:b272 | N/A | N/A |  | 
| Card reader | Realtek Semiconductor Corp. RTS5139 Card Reader Controller |  | 0bda:0139 | N/A | N/A |  | 

### Fn keys

| Function | Keys | Status | Notes | 
|---|---|---|---|
| Camera | Fn-Esc |  |  | 
| Sleep | Fn-F1 |  |  | 
| LCD On/Off | Fn-F2 |  |  | 
| VGA/LCD | Fn-F3 |  |  | 
| N/A | Fn-F4 |  |  | 
| Wireless | Fn-F5 |  |  | 
| Touchpad On/Off | Fn-F6 |  |  | 
| Volume + | Fn-Right |  |  | 
| Volume - | Fn-Left |  |  | 
| Brightness + | Fn-Up |  |  | 
| Brightness - | Fn-Down |  |  | 

### Detailed information

`root #``lspci -nn`
00:00.0 Host bridge \[0600\]: Intel Corporation 2nd Generation Core Processor Family DRAM Controller \[8086:0104\] (rev 09)
00:02.0 VGA compatible controller \[0300\]: Intel Corporation 2nd Generation Core Processor Family Integrated Graphics Controller \[8086:0106\] (rev 09)
00:16.0 Communication controller \[0780\]: Intel Corporation 6 Series/C200 Series Chipset Family MEI Controller #1 \[8086:1c3a\] (rev 04)
00:1a.0 USB controller \[0c03\]: Intel Corporation 6 Series/C200 Series Chipset Family USB Enhanced Host Controller #2 \[8086:1c2d\] (rev 05)
00:1b.0 Audio device \[0403\]: Intel Corporation 6 Series/C200 Series Chipset Family High Definition Audio Controller \[8086:1c20\] (rev 05)
00:1c.0 PCI bridge \[0604\]: Intel Corporation 6 Series/C200 Series Chipset Family PCI Express Root Port 1 \[8086:1c10\] (rev b5)
00:1c.1 PCI bridge \[0604\]: Intel Corporation 6 Series/C200 Series Chipset Family PCI Express Root Port 2 \[8086:1c12\] (rev b5)
00:1c.3 PCI bridge \[0604\]: Intel Corporation 6 Series/C200 Series Chipset Family PCI Express Root Port 4 \[8086:1c16\] (rev b5)
00:1d.0 USB controller \[0c03\]: Intel Corporation 6 Series/C200 Series Chipset Family USB Enhanced Host Controller #1 \[8086:1c26\] (rev 05)
00:1f.0 ISA bridge \[0601\]: Intel Corporation HM65 Express Chipset Family LPC Controller \[8086:1c49\] (rev 05)
00:1f.2 SATA controller \[0106\]: Intel Corporation 6 Series/C200 Series Chipset Family 6 port SATA AHCI Controller \[8086:1c03\] (rev 05)
00:1f.3 SMBus \[0c05\]: Intel Corporation 6 Series/C200 Series Chipset Family SMBus Controller \[8086:1c22\] (rev 05)
02:00.0 Network controller \[0280\]: Broadcom Corporation BCM4313 802.11b/g/n Wireless LAN Controller \[14e4:4727\] (rev 01)
03:00.0 Ethernet controller \[0200\]: Realtek Semiconductor Co., Ltd. RTL8111/8168 PCI Express Gigabit Ethernet controller \[10ec:8168\] (rev 06)

`root #``lsusb`
Bus 001 Device 002: ID 8087:0024 Intel Corp. Integrated Rate Matching Hub
Bus 002 Device 002: ID 8087:0024 Intel Corp. Integrated Rate Matching Hub
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 003: ID 04f2:b272 Chicony Electronics Co., Ltd Lenovo EasyCamera
Bus 002 Device 003: ID 0bda:0139 Realtek Semiconductor Corp. RTS5139 Card Reader Controller

## Installation

### DSDT

Extract ACPI tables:

cat /sys/firmware/acpi/tables/DSDT > dsdt.dat

Decompile:

iasl -d dsdt.dat

Recompile:

iasl -tc dsdt.dsl

Errors, Remarks and Warnings:

Intel ACPI Component Architecture
ASL Optimizing Compiler version 20130117-32 \[Oct  3 2013\]
Copyright (c) 2000 - 2013 Intel Corporation
dsdt.dsl   4461:                             Name (\_T\_0, Zero)  
Remark   5011 -        Use of compiler reserved name ^  (\_T\_0)
...
dsdt.dsl   7854:                 Method (\_CRS, 0, NotSerialized)  
Warning  1114 -                            ^ Not all control paths return a value (\_CRS)
...
dsdt.dsl  10690:                     Name (\_PLD, Buffer (0x10)  
Error    4105 -      Invalid object type for reserved name ^  (\_PLD: found BUFFER, requires Package)
...
Compilation complete. 7 Errors, 10 Warnings, 7 Remarks, 60 Optimizations

To remove Remarks change:

Name (\_T\_0, Zero)

to:

Name (T\_0, Zero)

To remove Errors change:

```
Name (_PLD, Buffer (0x10)
 {
   ...
}
```
to:

```
Name (_PLD, Package (0x01) { Buffer (0x10)
 {
   ...
 }
}
```
To remove Warnings, `Return (0)` should be added at the end of the function:

```
Method (_CRS, 0, NotSerialized) {
...
Return (0)
}
```
Enable custom DSDT in kernel:

**DSDT**

### Firmware

The wireless card requires [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) to be installed:

`root #``emerge --ask sys-kernel/linux-firmware`
### Kernel

**Sound**

**Ethernet**

**Wi-Fi**

**Bluetooth**

**Fn keys**

**Webcam**

**Card reader**
