<!-- source: https://wiki.gentoo.org/wiki/Lenovo_Thinkpad_R500 | group: Gentoo Wiki (Main) | wiki-title: Lenovo Thinkpad R500 -->
---
title: Lenovo Thinkpad R500
url: https://wiki.gentoo.org/wiki/Lenovo_Thinkpad_R500
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-20"
fingerprint: "7f089e24c7aa706a"
license: CC BY-SA 4.0
---

# Lenovo Thinkpad R500

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | N/A |  | N/A | N/A | N/A |  | 
| GPU | Intel Corporation Mobile 4 Series Chipset Integrated Graphics Controller |  | N/A | N/A | N/A |  | 
| Ethernet | Broadcom Corporation NetLink BCM5787M Gigabit Ethernet PCI Express |  | N/A | N/A | N/A |  | 
| Wi-Fi | Intel Corporation PRO/Wireless 5100 AGN \[Shiloh\] Network Connection |  | N/A | N/A | N/A |  | 
| SD Card Reader | Ricoh Co Ltd R5C822 SD/SDIO/MMC/MS/MSPro Host Adapter |  | N/A | N/A | N/A |  | 
| Webcam | N/A |  | N/A | N/A | N/A |  | 
| Fingerprint reader | N/A |  | N/A | N/A | N/A |  | 

`root #``lspci`
00:00.0 Host bridge: Intel Corporation Mobile 4 Series Chipset Memory Controller Hub (rev 07)
00:02.0 VGA compatible controller: Intel Corporation Mobile 4 Series Chipset Integrated Graphics Controller (rev 07)
00:02.1 Display controller: Intel Corporation Mobile 4 Series Chipset Integrated Graphics Controller (rev 07)
00:03.0 Communication controller: Intel Corporation Mobile 4 Series Chipset MEI Controller (rev 07)
00:1a.0 USB controller: Intel Corporation 82801I (ICH9 Family) USB UHCI Controller #4 (rev 03)
00:1a.1 USB controller: Intel Corporation 82801I (ICH9 Family) USB UHCI Controller #5 (rev 03)
00:1a.2 USB controller: Intel Corporation 82801I (ICH9 Family) USB UHCI Controller #6 (rev 03)
00:1a.7 USB controller: Intel Corporation 82801I (ICH9 Family) USB2 EHCI Controller #2 (rev 03)
00:1b.0 Audio device: Intel Corporation 82801I (ICH9 Family) HD Audio Controller (rev 03)
00:1c.0 PCI bridge: Intel Corporation 82801I (ICH9 Family) PCI Express Port 1 (rev 03)
00:1c.1 PCI bridge: Intel Corporation 82801I (ICH9 Family) PCI Express Port 2 (rev 03)
00:1c.3 PCI bridge: Intel Corporation 82801I (ICH9 Family) PCI Express Port 4 (rev 03)
00:1c.4 PCI bridge: Intel Corporation 82801I (ICH9 Family) PCI Express Port 5 (rev 03)
00:1c.5 PCI bridge: Intel Corporation 82801I (ICH9 Family) PCI Express Port 6 (rev 03)
00:1d.0 USB controller: Intel Corporation 82801I (ICH9 Family) USB UHCI Controller #1 (rev 03)
00:1d.1 USB controller: Intel Corporation 82801I (ICH9 Family) USB UHCI Controller #2 (rev 03)
00:1d.2 USB controller: Intel Corporation 82801I (ICH9 Family) USB UHCI Controller #3 (rev 03)
00:1d.7 USB controller: Intel Corporation 82801I (ICH9 Family) USB2 EHCI Controller #1 (rev 03)
00:1e.0 PCI bridge: Intel Corporation 82801 Mobile PCI Bridge (rev 93)
00:1f.0 ISA bridge: Intel Corporation ICH9M LPC Interface Controller (rev 03)
00:1f.2 SATA controller: Intel Corporation 82801IBM/IEM (ICH9M/ICH9M-E) 4 port SATA Controller \[AHCI mode\] (rev 03)
00:1f.3 SMBus: Intel Corporation 82801I (ICH9 Family) SMBus Controller (rev 03)
03:00.0 Network controller: Intel Corporation PRO/Wireless 5100 AGN \[Shiloh\] Network Connection
04:00.0 Ethernet controller: Broadcom Corporation NetLink BCM5787M Gigabit Ethernet PCI Express (rev 02)
15:00.0 CardBus bridge: Ricoh Co Ltd RL5c476 II (rev ba)
15:00.1 FireWire (IEEE 1394): Ricoh Co Ltd R5C832 IEEE 1394 Controller (rev 04)
15:00.2 SD Host controller: Ricoh Co Ltd R5C822 SD/SDIO/MMC/MS/MSPro Host Adapter (rev 21)
15:00.3 System peripheral: Ricoh Co Ltd R5C843 MMC Host Controller (rev 11)
15:00.4 System peripheral: Ricoh Co Ltd R5C592 Memory Stick Bus Host Adapter (rev 11)
15:00.5 System peripheral: Ricoh Co Ltd xD-Picture Card Controller (rev 11)

## Installation

### Firmware

Install the firmware for Wi-Fi:

`root #``emerge --ask sys-kernel/linux-firmware`
### Kernel

KERNEL **Ethernet and Wi-Fi**

KERNEL **Sensors**

KERNEL **Watchdog**

KERNEL **Random number generator**

KERNEL **Memory card reader**

KERNEL **Webcam**

### Emerge

FILE **`/etc/portage/package.use/00input`**

```
 INPUT_DEVICES: libinput evdev synaptics
```
FILE **`/etc/portage/package.use/00video`**

```
 VIDEO_CARDS: -* intel # or "radeon" in case of AMD/ATi integrated graphics
```
Install the sensor monitoring program:

`root #``emerge --ask sys-apps/lm-sensors`
## Configuration

### Trackpoint

Enable 3-button scroll:

FILE **`/usr/share/X11/xorg.conf.d/11-evdev-trackpoint.conf`**

### Power saving

The following configuration enables aggressive power saving:

FILE **`/etc/udev/rules.d/10-local-powersave.rules`**
