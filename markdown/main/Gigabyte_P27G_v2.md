<!-- source: https://wiki.gentoo.org/wiki/Gigabyte_P27G_v2 | group: Gentoo Wiki (Main) | wiki-title: Gigabyte P27G v2 -->
---
title: Gigabyte P27G v2
url: https://wiki.gentoo.org/wiki/Gigabyte_P27G_v2
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "7f589f06dfa23168"
license: CC BY-SA 4.0
---

# Gigabyte P27G v2

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Hardware

`root #``lspci`
00:00.0 Host bridge: Intel Corporation Xeon E3-1200 v3/4th Gen Core Processor DRAM Controller (rev 06)
	Subsystem: CLEVO/KAPOK Computer Device 3501
00:01.0 PCI bridge: Intel Corporation Xeon E3-1200 v3/4th Gen Core Processor PCI Express x16 Controller (rev 06)
	Kernel driver in use: pcieport
00:02.0 VGA compatible controller: Intel Corporation 4th Gen Core Processor Integrated Graphics Controller (rev 06)
	Subsystem: CLEVO/KAPOK Computer Device 3501
	Kernel driver in use: i915
00:03.0 Audio device: Intel Corporation Xeon E3-1200 v3/4th Gen Core Processor HD Audio Controller (rev 06)
	Subsystem: CLEVO/KAPOK Computer Device 3501
	Kernel driver in use: snd\_hda\_intel
	Kernel modules: snd\_hda\_intel
00:14.0 USB controller: Intel Corporation 8 Series/C220 Series Chipset Family USB xHCI (rev 05)
	Subsystem: CLEVO/KAPOK Computer Device 3501
	Kernel driver in use: xhci\_hcd
	Kernel modules: xhci\_pci
00:16.0 Communication controller: Intel Corporation 8 Series/C220 Series Chipset Family MEI Controller #1 (rev 04)
	Subsystem: CLEVO/KAPOK Computer Device 3501
00:1a.0 USB controller: Intel Corporation 8 Series/C220 Series Chipset Family USB EHCI #2 (rev 05)
	Subsystem: CLEVO/KAPOK Computer Device 3501
	Kernel driver in use: ehci-pci
	Kernel modules: ehci\_pci
00:1b.0 Audio device: Intel Corporation 8 Series/C220 Series Chipset High Definition Audio Controller (rev 05)
	Subsystem: CLEVO/KAPOK Computer Device 3501
	Kernel driver in use: snd\_hda\_intel
	Kernel modules: snd\_hda\_intel
00:1c.0 PCI bridge: Intel Corporation 8 Series/C220 Series Chipset Family PCI Express Root Port #1 (rev d5)
	Kernel driver in use: pcieport
00:1c.2 PCI bridge: Intel Corporation 8 Series/C220 Series Chipset Family PCI Express Root Port #3 (rev d5)
	Kernel driver in use: pcieport
00:1c.3 PCI bridge: Intel Corporation 8 Series/C220 Series Chipset Family PCI Express Root Port #4 (rev d5)
	Kernel driver in use: pcieport
00:1d.0 USB controller: Intel Corporation 8 Series/C220 Series Chipset Family USB EHCI #1 (rev 05)
	Subsystem: CLEVO/KAPOK Computer Device 3501
	Kernel driver in use: ehci-pci
	Kernel modules: ehci\_pci
00:1f.0 ISA bridge: Intel Corporation HM87 Express LPC Controller (rev 05)
	Subsystem: CLEVO/KAPOK Computer Device 3501
00:1f.2 SATA controller: Intel Corporation 8 Series/C220 Series Chipset Family 6-port SATA Controller 1 \[AHCI mode\] (rev 05)
	Subsystem: CLEVO/KAPOK Computer Device 3501
	Kernel driver in use: ahci
	Kernel modules: ahci
00:1f.3 SMBus: Intel Corporation 8 Series/C220 Series Chipset Family SMBus Controller (rev 05)
	Subsystem: CLEVO/KAPOK Computer Device 3501
	Kernel modules: i2c\_i801
01:00.0 3D controller: NVIDIA Corporation GM107M \[GeForce GTX 860M\] (rev a2)
	Subsystem: CLEVO/KAPOK Computer Device 3501
03:00.0 Network controller: Realtek Semiconductor Co., Ltd. RTL8723BE PCIe Wireless Network Adapter
	Subsystem: Realtek Semiconductor Co., Ltd. Device b729
	Kernel driver in use: rtl8723be
	Kernel modules: rtl8723be
04:00.0 Unassigned class \[ff00\]: Realtek Semiconductor Co., Ltd. Device 5287 (rev 01)
	Subsystem: CLEVO/KAPOK Computer Device 3501
04:00.1 Ethernet controller: Realtek Semiconductor Co., Ltd. RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller (rev 12)
	Subsystem: CLEVO/KAPOK Computer Device 3501
	Kernel driver in use: r8169
	Kernel modules: r8169

## Kernel configuration

KERNEL **Enabling evdev in the kernel**

KERNEL **Configuring framebuffers**

KERNEL **Intel settings**

KERNEL

KERNEL

External firmware is required for the wireless card:

`root #``emerge --ask linux-firmware`
