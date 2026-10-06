<!-- source: https://wiki.gentoo.org/wiki/Dell_SD25 | group: Gentoo Wiki (Main) | wiki-title: Dell SD25 -->
---
title: Dell SD25
url: https://wiki.gentoo.org/wiki/Dell_SD25
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-11-10"
fingerprint: "1f06891589a739ac"
license: CC BY-SA 4.0
---

# Dell SD25

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The Dell Pro Smart Dock SD25 is an USB-C dock with HDMI, 2xDisplayPort, Ethernet, 4xUSB 3.2 and 2xUSB-C connections by Dell. It is fully compatible with the latest Linux kernels.

## Status

| **Port/Functionality** | **Status** | 
| Male USB Type-C | Works | 
| Thunderbolt 3 Hotplug | Works | 
| USB 3.2 | Works | 
| Ethernet | Works | 
| HDMI | Works | 
| DisplayPort | Works | 

## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Firmware | Kernel version | Notes | 
|---|---|---|---|---|---|---|---|
| USB 10/100/1G/2.5G LAN | Realtek Semiconductor Corp. | Works | [0bda:8156](https://linux-hardware.org/index.php?id=usb:0bda-8156) | RTL8152 | rtl8156b-2 | 6.16.12 | Enable kernel option `USB_RTL8152` in the kernel. | 
| USB 2.1 Hub | Realtek Semiconductor Corp. | Works | [0bda:5480](https://linux-hardware.org/?id=usb:0bda-5480) [0bda:5481](https://linux-hardware.org/?id=usb:0bda-5481) [0bda:5485](https://linux-hardware.org/?id=usb:0bda-5485) | HUB (usbcore) | N/A | 6.16.12 |  | 
| USB 3.2 Hub | Realtek Semiconductor Corp. | Works | [0bda:0480](https://linux-hardware.org/?id=usb:0bda-0480) [0bda:0481](https://linux-hardware.org/?id=usb:0bda-0481) [0bda:0485](https://linux-hardware.org/?id=usb:0bda-0485) | HUB (usbcore) | N/A | 6.16.12 |  | 
| HID Device | Dell Computer Corp. | Works | [413c:b0a1](https://linux-hardware.org/?id=usb:413c-b0a1) [413c:b0a2](https://linux-hardware.org/?id=usb:413c-b0a2) [413c:b0a3](https://linux-hardware.org/?id=usb:413c-b0a3) [413c:b0a5](https://linux-hardware.org/?id=usb:413c-b0a5) [413c:b06e](https://linux-hardware.org/?id=usb:413c-b06e) | usbhid | N/A | 6.16.12 | Enable kernel option `USB_HID` in the kernel. | 

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
