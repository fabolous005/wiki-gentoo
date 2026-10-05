<!-- source: https://wiki.gentoo.org/wiki/Dell_DA200 | group: Gentoo Wiki (Main) | wiki-title: Dell DA200 -->
---
title: Dell DA200
url: https://wiki.gentoo.org/wiki/Dell_DA200
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-11-12"
fingerprint: "17b0292ca9a37767"
license: CC BY-SA 4.0
---

# Dell DA200

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The DA200 is a compact 4-port USB-C to HDMI, VGA, Ethernet and USB 3.0 adapter by Dell. It is fully compatible with the latest Linux kernels.

## Status

| **Port/Functionality** | **Status** | 
| Male USB Type-C |  | 
| Thunderbolt 3 Hotplug |  | 
| USB 3.0 |  | 
| Ethernet |  | 
| HDMI |  | 
| VGA |  | 

## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Firmware | Kernel version | Notes | 
|---|---|---|---|---|---|---|---|
| Gigabit Ethernet Adapter | [Realtek RTL8153](https://www.realtek.com/en/products/communications-network-ics/item/rtl8153) |  | [0bda:8153](https://cateee.net/lkddb/web-lkddb/USB_RTL8152.html) | RTL8152 | rtl8153b-2 | 6.16.12 | Enable kernel option `USB_RTL8152` in the kernel. | 
| 4-port USB 2.0 Hub | [Genesys Logic, Inc.](http://www.genesyslogic.com/en/product_list.php?1st=3&2nd=10) |  | `05e3:0610` | HUB (usbcore) | N/A | 6.16.12 |  | 
| 4-port USB 3.1 Hub | [Genesys Logic, Inc.](http://www.genesyslogic.com/en/product_list.php?1st=3&2nd=10) |  | `05e3:0617` | HUB (usbcore) | N/A | 6.16.12 |  | 
| EFM8 HID ISP | [Cygnal Integrated Products, Inc.](https://www.silabs.com/mcu/8-bit) |  | `10c4:f407` | usbhid | N/A | 6.16.12 | Enable kernel option `USB_HID` in the kernel. | 

## Installation

### Kernel

KERNEL

### Emerge

In order to enable the security levels for Thunderbolt 3, [sys-apps/bolt](https://packages.gentoo.org/packages/sys-apps/bolt) needs to be installed:

`root #``emerge --ask sys-apps/bolt`
## See also

## External resources

- [https://gitlab.freedesktop.org/bolt/bolt](https://gitlab.freedesktop.org/bolt/bolt)
- [https://www.kernel.org/doc/html/latest/admin-guide/kernel-parameters.html?highlight=usb\_quirk\_no\_lpm](https://www.kernel.org/doc/html/latest/admin-guide/kernel-parameters.html?highlight=usb_quirk_no_lpm)
