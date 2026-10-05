<!-- source: https://wiki.gentoo.org/wiki/Dell_DA310 | group: Gentoo Wiki (Main) | wiki-title: Dell DA310 -->
---
title: Dell DA310
url: https://wiki.gentoo.org/wiki/Dell_DA310
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-11-12"
fingerprint: "57d08b2d09b3730f"
license: CC BY-SA 4.0
---

# Dell DA310

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The DA310 is a compact 7-port USB-C to USB-C, HDMI, DisplayPort, VGA, Ethernet and USB 3.2 adapter by Dell. It is fully compatible with the latest Linux kernels.

## Status

| **Port/Functionality** | **Status** | 
| Male USB Type-C |  | 
| USB Type-C |  | 
| Thunderbolt 3 Hotplug |  | 
| USB 3.2 |  | 
| Ethernet |  | 
| HDMI |  | 
| DisplayPort |  | 
| VGA |  | 

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Firmware | Kernel version | Notes | 
|---|---|---|---|---|---|---|---|
| Gigabit Ethernet Adapter | [Realtek RTL8153](https://www.realtek.com/en/products/communications-network-ics/item/rtl8153) |  | [0bda:8153](https://cateee.net/lkddb/web-lkddb/USB_RTL8152.html) | RTL8152 | rtl8153b-2 | 6.16.12 | Enable kernel option `USB_RTL8152` in the kernel. | 
| USB 2.0 Hub | Fresco Logic Frescologic |  | [1d5c:5510](https://linux-hardware.org/index.php?id=usb:1d5c-5510) | HUB (usbcore) | N/A | 6.16.12 |  | 
| USB 3.1 Gen. 2 Hub | Fresco Logic Frescologic |  | [1d5c:5500](https://linux-hardware.org/index.php?id=usb:1d5c-5500) | HUB (usbcore) | N/A | 6.16.12 |  | 
| Dell DA310 | Dell Computer Corp. |  | [413c:c010](https://linux-hardware.org/?id=usb:413c-c010) | usbhid | N/A | 6.16.12 | Enable kernel option `USB_HID` in the kernel. | 

## Installation

### Kernel

### Emerge

In order to enable the security levels for Thunderbolt 3, [sys-apps/bolt](https://packages.gentoo.org/packages/sys-apps/bolt) needs to be installed:

`root #``emerge --ask sys-apps/bolt`
