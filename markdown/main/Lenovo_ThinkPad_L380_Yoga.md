<!-- source: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_L380_Yoga | group: Gentoo Wiki (Main) | wiki-title: Lenovo ThinkPad L380 Yoga -->
---
title: Lenovo ThinkPad L380 Yoga
url: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_L380_Yoga
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "3f588726db28546b"
license: CC BY-SA 4.0
---

# Lenovo ThinkPad L380 Yoga

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

The article is based on Lenovo ThinkPad L380 Yoga model 20M7000MUS.

## Hardware

### Standard

| Device | Make/model | Status | Bus ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| Ethernet controller | Intel Corporation Ethernet Connection (4) I219-V (rev 21) |  | 8086:15d8 | e1000e | 5.10.61-gentoo | — | 
| Wireless network controller | Intel Corporation Wi-Fi 6 AX200 (rev 1a) |  | 8086:4239 | iwlwifi | 5.10.61-gentoo | — | 
| Audio device | Intel Corporation Sunrise Point-LP HD Audio (rev 21) |  | 8086:9d71 | snd-hda-intel | 5.10.61-gentoo | Uses Realtek HD-audio codec | 
| Card reader | Realtek Semiconductor Co., Ltd. RTS522A PCI Express Card Reader (rev 01) |  | 10ec:522a | rtsx\_pci\_sdmmc | 5.10.61-gentoo | — | 
| Touchscreen | Wacom Co., Ltd Pen and multitouch sensor |  | 056a:515a | usbhid | 5.10.61-gentoo | — | 
| Bluetooth | Intel Corp. AX200 Bluetooth |  | 8087:0029 | btintel | 5.10.61-gentoo | — | 
| Web-camera | Lite-On Technology Corp. Integrated Camera |  | 04ca:7070 | uvcvideo | 5.10.61-gentoo | — | 
| Fingerprint reader | Synaptics, Inc. Metallica MOH Touch Fingerprint Reader |  | 06cb:00a2 | — | — | No driver available yet | 

### Device listing

`user $``lspci -nn`
00:00.0 Host bridge \[0600\]: Intel Corporation Xeon E3-1200 v6/7th Gen Core Processor Host Bridge/DRAM Registers \[8086:5914\] (rev 08)
00:02.0 VGA compatible controller \[0300\]: Intel Corporation UHD Graphics 620 \[8086:5917\] (rev 07)
00:04.0 Signal processing controller \[1180\]: Intel Corporation Xeon E3-1200 v5/E3-1500 v5/6th Gen Core Processor Thermal Subsystem \[8086:1903\] (rev 08)
00:13.0 Non-VGA unclassified device \[0000\]: Intel Corporation Sunrise Point-LP Integrated Sensor Hub \[8086:9d35\] (rev 21)
00:14.0 USB controller \[0c03\]: Intel Corporation Sunrise Point-LP USB 3.0 xHCI Controller \[8086:9d2f\] (rev 21)
00:14.2 Signal processing controller \[1180\]: Intel Corporation Sunrise Point-LP Thermal subsystem \[8086:9d31\] (rev 21)
00:15.0 Signal processing controller \[1180\]: Intel Corporation Sunrise Point-LP Serial IO I2C Controller #0 \[8086:9d60\] (rev 21)
00:15.1 Signal processing controller \[1180\]: Intel Corporation Sunrise Point-LP Serial IO I2C Controller #1 \[8086:9d61\] (rev 21)
00:16.0 Communication controller \[0780\]: Intel Corporation Sunrise Point-LP CSME HECI #1 \[8086:9d3a\] (rev 21)
00:1c.0 PCI bridge \[0604\]: Intel Corporation Sunrise Point-LP PCI Express Root Port #1 \[8086:9d10\] (rev f1)
00:1c.2 PCI bridge \[0604\]: Intel Corporation Sunrise Point-LP PCI Express Root Port #3 \[8086:9d12\] (rev f1)
00:1c.4 PCI bridge \[0604\]: Intel Corporation Sunrise Point-LP PCI Express Root Port #5 \[8086:9d14\] (rev f1)
00:1d.0 PCI bridge \[0604\]: Intel Corporation Sunrise Point-LP PCI Express Root Port #9 \[8086:9d18\] (rev f1)
00:1f.0 ISA bridge \[0601\]: Intel Corporation Sunrise Point LPC Controller/eSPI Controller \[8086:9d4e\] (rev 21)
00:1f.2 Memory controller \[0580\]: Intel Corporation Sunrise Point-LP PMC \[8086:9d21\] (rev 21)
00:1f.3 Audio device \[0403\]: Intel Corporation Sunrise Point-LP HD Audio \[8086:9d71\] (rev 21)
00:1f.4 SMBus \[0c05\]: Intel Corporation Sunrise Point-LP SMBus \[8086:9d23\] (rev 21)
00:1f.6 Ethernet controller \[0200\]: Intel Corporation Ethernet Connection (4) I219-V \[8086:15d8\] (rev 21)
02:00.0 Network controller \[0280\]: Intel Corporation Wi-Fi 6 AX200 \[8086:2723\] (rev 1a)
03:00.0 Non-Volatile memory controller \[0108\]: Toshiba Corporation Device \[1179:0116\]
04:00.0 Unassigned class \[ff00\]: Realtek Semiconductor Co., Ltd. RTS522A PCI Express Card Reader \[10ec:522a\] (rev 01)

`user $``lsusb`
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 001 Device 005: ID 06cb:00a2 Synaptics, Inc. Metallica MOH Touch Fingerprint Reader
Bus 001 Device 004: ID 8087:0029 Intel Corp. AX200 Bluetooth
Bus 001 Device 003: ID 056a:515a Wacom Co., Ltd Pen and multitouch sensor
Bus 001 Device 002: ID 04ca:7070 Lite-On Technology Corp. Integrated Camera
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

## Installation

FILE **`/etc/portage/make.conf`**

### Firmware

`root #``emerge --ask sys-kernel/linux-firmware`
### Kernel

KERNEL **Ethernet controller**

KERNEL **Audio device**

KERNEL **Card reader**

KERNEL **Touchscreen**

KERNEL **Bluetooth**

KERNEL **Web-camera**

Building the following device drivers as module may be appropriate as they require firmware during initialization.

KERNEL **Wireless network controller**
