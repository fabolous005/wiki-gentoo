<!-- source: https://wiki.gentoo.org/wiki/Lenovo_Thinkpad_E550 | group: Gentoo Wiki (Main) | wiki-title: Lenovo Thinkpad E550 -->
---
title: Lenovo Thinkpad E550
url: https://wiki.gentoo.org/wiki/Lenovo_Thinkpad_E550
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-22"
fingerprint: "7f5a0727d92a526a"
license: CC BY-SA 4.0
---

# Lenovo Thinkpad E550

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

## Hardware

`root #``lspci`
00:00.0 Host bridge: Intel Corporation Broadwell-U Host Bridge -OPI (rev 09)
00:02.0 VGA compatible controller: Intel Corporation Broadwell-U Integrated Graphics (rev 09)
00:03.0 Audio device: Intel Corporation Broadwell-U Audio Controller (rev 09)
00:14.0 USB controller: Intel Corporation Wildcat Point-LP USB xHCI Controller (rev 03)
00:16.0 Communication controller: Intel Corporation Wildcat Point-LP MEI Controller #1 (rev 03)
00:19.0 Ethernet controller: Intel Corporation Ethernet Connection (3) I218-V (rev 03)
00:1b.0 Audio device: Intel Corporation Wildcat Point-LP High Definition Audio Controller (rev 03)
00:1c.0 PCI bridge: Intel Corporation Wildcat Point-LP PCI Express Root Port #1 (rev e3)
00:1c.2 PCI bridge: Intel Corporation Wildcat Point-LP PCI Express Root Port #3 (rev e3)
00:1c.4 PCI bridge: Intel Corporation Wildcat Point-LP PCI Express Root Port #5 (rev e3)
00:1c.5 PCI bridge: Intel Corporation Wildcat Point-LP PCI Express Root Port #6 (rev e3)
00:1d.0 USB controller: Intel Corporation Wildcat Point-LP USB EHCI Controller (rev 03)
00:1f.0 ISA bridge: Intel Corporation Wildcat Point-LP LPC Controller (rev 03)
00:1f.2 SATA controller: Intel Corporation Wildcat Point-LP SATA Controller \[AHCI Mode\] (rev 03)
00:1f.3 SMBus: Intel Corporation Wildcat Point-LP SMBus Controller (rev 03)
00:1f.6 Signal processing controller: Intel Corporation Wildcat Point-LP Thermal Management Controller (rev 03)
04:00.0 Network controller: Intel Corporation Wireless 7265 (rev 61)
05:00.0 Display controller: Advanced Micro Devices, Inc. \[AMD/ATI\] Opal XT \[Radeon R7 M265\]
06:00.0 Unassigned class \[ff00\]: Realtek Semiconductor Co., Ltd. RTS5227 PCI Express Card Reader (rev 01)

`root #``lsusb`
Bus 003 Device 002: ID 8087:8001 Intel Corp. 
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 001 Device 004: ID 5986:055a Acer, Inc
Bus 001 Device 002: ID 04f2:b444 Chicony Electronics Co., Ltd
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

### Status

| Device | Works | Notes |  | 
|---|---|---|---|
| CPU: Intel Core i5-5200U or i7-5500U |  |  |  | 
| Video: Intel HD Graphics 5500 |  |  |  | 
| Video: AMD Radeon R7 M265 |  |  |  | 
| SATA: Intel Corporation Wildcat Point-LP SATA Controller |  |  |  | 
| Audio: Intel HD Audio |  |  |  | 
| Ethernet: Intel I218-V PCI Express Gigabit Ethernet |  |  |  | 
| Wireless LAN: Intel Wireless 7265 802.11ac |  |  |  | 
| Wireless LAN: Intel Wireless 3160 802.11ac |  |  |  | 
| Bluetooth: Intel Wireless 7265 802.11ac |  |  |  | 
| Bluetooth: Intel Wireless 3160 802.11ac |  |  |  | 
| Camera: Chicony Electronics Co., Ltd USB webcam |  |  |  | 
| Camera: Acer, Inc |  |  |  | 
| Fingerprint reader: Validity VFS5017 USB fingerprint reader |  | Works with sys-auth/libfprint-0.5.1-r1 |  | 
| SD card reader: Realtek RTS5227 PCI-E card reader |  |  |  | 
| SynPS/2 Synaptics TouchPad |  |  |  | 
| Hardware monitoring |  |  |  | 
| Hotkeys |  | Some have to be manually configured | See also [http://www.thinkwiki.org/wiki/Thinkpad-acpi#Supported\_ThinkPads](http://www.thinkwiki.org/wiki/Thinkpad-acpi#Supported_ThinkPads) | 
| Discrete TPM (TPM 1.2) |  | Must be activated through BIOS, disables access to Intel PTT |  | 
| Intel PTT (TPM 2.0) |  | A bug (allegedly from the BIOS) prevents use when IOMMU is enabled, pass intel\_iommu=off at boot time to disable it |  | 

### ACPI / Power Management

| Function | Works | Notes |  | 
|---|---|---|---|
| CPU frequency scaling |  | With laptop-mode-tools |  | 
| Suspend to RAM |  |  |  | 
| Suspend to disk (hibernate) |  |  |  | 
| Display backlight control |  |  |  | 

## Installation

### Firmware

The wireless card requires external firmware:

`root #``emerge --ask sys-kernel/linux-firmware`
### Kernel

KERNEL

### Emerge

FILE **`/etc/portage/package.use/00input`**

```
 INPUT_DEVICES: evdev synaptics
```
FILE **`/etc/portage/package.use/00video`**

```
 VIDEO_CARDS: -* intel i965 radeon
```
FILE **`/etc/portage/package.use/00cpu-flags`**

```
  CPU_FLAGS_X86: aes avx avx2 fma3 mmx mmxext popcnt sse sse2 sse3 sse4_1 sse4_2 ssse3
```
For the card reader ([sys-apps/pcsc-tools](https://packages.gentoo.org/packages/sys-apps/pcsc-tools)):

`root #``emerge --ask sys-apps/pcsc-tools`
For the fingerprint reader ([sys-auth/libfprint](https://packages.gentoo.org/packages/sys-auth/libfprint)):

`root #``emerge --ask sys-auth/libfprint`
