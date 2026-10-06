<!-- source: https://wiki.gentoo.org/wiki/Dell_DA200 | group: Gentoo Wiki (Main) | wiki-title: Dell DA200 -->
---
title: Dell DA200
url: https://wiki.gentoo.org/wiki/Dell_DA200
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-11-12"
fingerprint: "1f04891589a63b0c"
license: CC BY-SA 4.0
---

# Dell DA200

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The DA200 is a compact 4-port USB-C to HDMI, VGA, Ethernet and USB 3.0 adapter by Dell. It is fully compatible with the latest Linux kernels.

## Status

| **Port/Functionality** | **Status** | 
| Male USB Type-C | Works | 
| Thunderbolt 3 Hotplug | Works | 
| USB 3.0 | Works | 
| Ethernet | Works | 
| HDMI | Works | 
| VGA | Works | 

## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Firmware | Kernel version | Notes | 
|---|---|---|---|---|---|---|---|
| Gigabit Ethernet Adapter | [Realtek RTL8153](https://www.realtek.com/en/products/communications-network-ics/item/rtl8153) | Works | [0bda:8153](https://cateee.net/lkddb/web-lkddb/USB_RTL8152.html) | RTL8152 | rtl8153b-2 | 6.16.12 | Enable kernel option `USB_RTL8152` in the kernel. | 
| 4-port USB 2.0 Hub | [Genesys Logic, Inc.](http://www.genesyslogic.com/en/product_list.php?1st=3&2nd=10) | Works | `05e3:0610` | HUB (usbcore) | N/A | 6.16.12 |  | 
| 4-port USB 3.1 Hub | [Genesys Logic, Inc.](http://www.genesyslogic.com/en/product_list.php?1st=3&2nd=10) | Works | `05e3:0617` | HUB (usbcore) | N/A | 6.16.12 |  | 
| EFM8 HID ISP | [Cygnal Integrated Products, Inc.](https://www.silabs.com/mcu/8-bit) | Works | `10c4:f407` | usbhid | N/A | 6.16.12 | Enable kernel option `USB_HID` in the kernel. | 

## Installation

### Kernel

KERNEL

```
Device Drivers --->
  Generic Driver Options  --->
    Firmware loader --->
      -*- Firmware loading facility
      () Build named firmware blobs into the kernel binary
      (/lib/firmware) Firmware blobs root directory
  [*] HID support  --->
      HID support  --->
      <*>   USB HID transport layer
  [*] PCI support  --->
    [*]   PCI Express Port Bus support
    [*]     PCI Express Hotplug driver
    [*] Support for PCI Hotplug  --->
        [*]   ACPI PCI Hotplug driver
  [*] USB support  --->
    <*>   USB Type-C Support  --->
      <*>   USB Type-C Port Controller Manager 
      <*>     Type-C Port Controller Interface driver
      <*>   USB Type-C Connector System Software Interface driver 
      <*>     UCSI ACPI Interface Driver
            USB Type-C Alternate Mode drivers  --->
        <*> Thunderbolt3 Alternate Mode driver
  [*] Network device support --->
    <*> USB Network Adapters --->
      <*>   Realtek RTL8152/RTL8153 Based USB Ethernet Adapters
```
### Emerge

In order to enable the security levels for Thunderbolt 3, [sys-apps/bolt](https://packages.gentoo.org/packages/sys-apps/bolt) needs to be installed:

`root #``emerge --ask sys-apps/bolt`
## See also

## External resources

- [https://gitlab.freedesktop.org/bolt/bolt](https://gitlab.freedesktop.org/bolt/bolt)
- [https://www.kernel.org/doc/html/latest/admin-guide/kernel-parameters.html?highlight=usb\_quirk\_no\_lpm](https://www.kernel.org/doc/html/latest/admin-guide/kernel-parameters.html?highlight=usb_quirk_no_lpm)
