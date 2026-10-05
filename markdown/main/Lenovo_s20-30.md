<!-- source: https://wiki.gentoo.org/wiki/Lenovo_s20-30 | group: Gentoo Wiki (Main) | wiki-title: Lenovo s20-30 -->
---
title: Lenovo s20-30
url: https://wiki.gentoo.org/wiki/Lenovo_s20-30
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "7f5a072cdbe8326a"
license: CC BY-SA 4.0
---

# Lenovo s20-30

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This laptop has two versions, touch and non-touch. This article is based on the non-touch version.

## Hardware

| Class | Product | 
|---|---|
| Processor | Intel® Pentium® Processor N3530 | 
| Wi-Fi | Qualcomm Atheros QCA9565 / AR9565 | 
| Ethernet | Realtek | 
| Bluetooth | Atheros AR3012 | 
| Audio | Intel HD Audio | 
| Graphics | Intel HD Graphics | 

`root #``lspci -nn`
00:00.0 Host bridge \[0600\]: Intel Corporation Atom Processor Z36xxx/Z37xxx Series SoC Transaction Register \[8086:0f00\] (rev 0e)
00:02.0 VGA compatible controller \[0300\]: Intel Corporation Atom Processor Z36xxx/Z37xxx Series Graphics & Display \[8086:0f31\] (rev 0e)
00:13.0 SATA controller \[0106\]: Intel Corporation Device \[8086:0f23\] (rev 0e)
00:14.0 USB controller \[0c03\]: Intel Corporation Atom Processor Z36xxx/Z37xxx Series USB xHCI \[8086:0f35\] (rev 0e)
00:1a.0 Encryption controller \[1080\]: Intel Corporation Atom Processor Z36xxx/Z37xxx Series Trusted Execution Engine \[8086:0f18\] (rev 0e)
00:1b.0 Audio device \[0403\]: Intel Corporation Atom Processor Z36xxx/Z37xxx Series High Definition Audio Controller \[8086:0f04\] (rev 0e)
00:1c.0 PCI bridge \[0604\]: Intel Corporation Device \[8086:0f48\] (rev 0e)
00:1c.1 PCI bridge \[0604\]: Intel Corporation Device \[8086:0f4a\] (rev 0e)
00:1f.0 ISA bridge \[0601\]: Intel Corporation Atom Processor Z36xxx/Z37xxx Series Power Control Unit \[8086:0f1c\] (rev 0e)
00:1f.3 SMBus \[0c05\]: Intel Corporation Device \[8086:0f12\] (rev 0e)
01:00.0 Ethernet controller \[0200\]: Realtek Semiconductor Co., Ltd. RTL8101E/RTL8102E PCI Express Fast Ethernet controller \[10ec:8136\] (rev 08)
02:00.0 Network controller \[0280\]: Qualcomm Atheros QCA9565 / AR9565 Wireless Network Adapter \[168c:0036\] (rev 01)

`root #``lsusb`
Bus 001 Device 007: ID 0bda:0129 Realtek Semiconductor Corp. RTS5129 Card Reader Controller
Bus 001 Device 005: ID 0cf3:3004 Atheros Communications, Inc. AR3012 Bluetooth 4.0
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 001 Device 004: ID 05e3:0610 Genesys Logic, Inc. 4-port hub
Bus 001 Device 003: ID 5986:054a Acer, Inc 
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

### Wi-Fi

For Wi-Fi to work, enable the ath9k driver (ATH9K & ATH9K\_PCI) and enable the bluetooth coexistence for the bluetooth to work (ATH9K\_BTCOEX\_SUPPORT)

**Qualcomm Atheros QCA9565 / AR9565**

### Bluetooth

Enable ath3k, in kernel BT\_ATH3K

**Qualcomm Atheros QCA9565 / AR9565**

### Ethernet

Enable r8169

**RTL8101E/RTL8102E**

If built as module, it will be named r8169

### Graphics

Use intel HD graphics: [intel](https://wiki.gentoo.org/wiki/Intel)

### Smart card reader

### UEFI support

If you want to directly load the kernel from the UEFI:

- Enable UEFI-only loading (no legacy support)
- Set an administrator password
- Disable secure boot

Then follow the instructions from the wiki: [EFI stub](https://wiki.gentoo.org/wiki/EFI_stub)
