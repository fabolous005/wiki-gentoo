<!-- source: https://wiki.gentoo.org/wiki/USB_to_Ethernet_Adapter | group: Gentoo Wiki (Main) | wiki-title: USB to Ethernet Adapter -->
---
title: USB to Ethernet Adapter
url: https://wiki.gentoo.org/wiki/USB_to_Ethernet_Adapter
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-31"
fingerprint: edd02a35bdc4d39e
license: CC BY-SA 4.0
---

# USB to Ethernet Adapter

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page lists common USB to Ethernet network adapters and provides details for enabling Linux kernel support.

## Installation

### Kernel configuration

**`CONFIG_USB_USBNET`**

If there is an USB HUB inside the adapter, this should work directly if your USB Controller is configured correctly (see the [USB Guide](https://wiki.gentoo.org/wiki/USB/Guide) article).

## Working devices

The table below lists USB Fast Ethernet adapters working with kernel-modules:

| ID | Product Name | lsusb Name | Chipset | Module | 
|---|---|---|---|---|
| 0fe6:9700 | 1 Port USB Network with 3 Port USB HUB | Kontron DM9601 Fast Ethernet Adapter | Davicom DM9601 | USB\_NET\_DM9601 | 
| 2357:0602 | TP-Link UE200 USB 2.0 to 100Mbps adapter | TP-Link | Realtek RTL8152B | CONFIG\_USB\_RTL8152 | 
| 0bda:8153 | Anker 3-Port USB 3.0 Hub with Ethernet | Realtek Semiconductor Corp. RTL8153 Gigabit Ethernet Adapter | Realtek RTL8153 | CONFIG\_USB\_RTL8152 | 
| 0b95:1790 | TP-Link UE300C USB Type-C to RJ45 Gigabit Ethernet | ASIX Electronics Corp. AX88179 Gigabit Ethernet | Realtek RTL8153 | CONFIG\_USB\_NET\_AX88179\_178A | 

## See also

- [USB Audio](https://wiki.gentoo.org/wiki/USB_Audio) — details the necessary system configuration to support **speakers** and **microphones** connected to the system via USB.
