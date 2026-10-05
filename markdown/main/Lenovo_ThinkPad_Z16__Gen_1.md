<!-- source: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_Z16_(Gen_1) | group: Gentoo Wiki (Main) | wiki-title: Lenovo ThinkPad Z16 (Gen 1) -->
---
title: Lenovo ThinkPad Z16 (Gen 1)
url: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_Z16_(Gen_1)
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: ff0a95dd47fa9829
license: CC BY-SA 4.0
---

# Lenovo ThinkPad Z16 (Gen 1)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

The Lenovo ThinkPad Z16 is a new line of ThinkPads (Z series 2022). It features an AMD 6000-series CPU, optionally an AMD 6500M discrete GPU, up to 32GB of soldered dual channel LPDDR5 RAM and up to 2TB of PCIe Gen 4 SSD storage. All the laptop's features work with Linux, though, like most modern hardware, it requires non-free firmware, specifically for the Wi-Fi 6E chip, the AMD GPUs and the sound card. All needed firmware are available in the [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) package.

[fwupd](https://wiki.gentoo.org/wiki/Fwupd) is compatible with this laptop.

## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | AMD 6000H series |  | N/A | N/A | 6.1.12 | N/A | 
| Video card | AMD Rembrandt \[Radeon 680M\] |  | 1002:1681 | amdgpu | 6.1.12 | N/A | 
| Video card | AMD Navi 24 \[Radeon 6500M\] |  | 1002:743f | amdgpu | 6.1.12 | N/A | 
| Touchpad | Elan |  | 04f3:3232 | i2c\_hid | 6.1.12 | N/A | 
| TrackPoint | Elan |  | N/A | psmouse | 6.1.12 | N/A | 
| Touch Screen | Wacom WACF2200 |  | 056a:52f5 | hid-multitouch | 6.1.12 | N/A | 
| Fingerprint Reader | Shenzhen Goodix |  | 27c6:658c | N/A | 6.1.12 | May require clearing fingerprint data in the BIOS | 
| Webcam | Integrated Camera |  | 04f2:b78c | uvcvideo | 6.1.12 | N/A | 
| Microphone | ALC287 |  | 04f2:b78c | snd | 6.1.12 | N/A | 
| Wi-Fi | WCN6855 |  | 04f2:b78c | ath11k\_pci | 6.1.12 | N/A | 
| TPM | Integrated Camera |  | N/A | tpm\_tis, tpm\_crb | 6.1.12 | N/A | 

### Accessories

| Device | Make/model | Status | Bus ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| Dock | Dell WD19TB |  | N/A | N/A | 6.1.12 | N/A | 

### lspci

`root #``lspci -vnnnk`
00:00.0 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h-19h PCIe Root Complex \[1022:14b5\] (rev 01)
	Subsystem: Lenovo Device \[17aa:22f2\]
	Flags: fast devsel
00:00.2 IOMMU \[0806\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h-19h IOMMU \[1022:14b6\]
	Subsystem: Lenovo Device \[17aa:22f2\]
	Flags: bus master, fast devsel, latency 0, IRQ 25
	Capabilities: \[40\] Secure device \<?>
	Capabilities: \[64\] MSI: Enable+ Count=1/4 Maskable- 64bit+
	Capabilities: \[74\] HyperTransport: MSI Mapping Enable+ Fixed+
00:01.0 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h-19h PCIe Dummy Host Bridge \[1022:14b7\] (rev 01)
	Flags: fast devsel, IOMMU group 0
00:01.1 PCI bridge \[0604\]: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:14b8\] (prog-if 00 \[Normal decode\])
	Subsystem: Lenovo Device \[17aa:22f2\]
	Flags: bus master, fast devsel, latency 0, IRQ 69, IOMMU group 1
	Bus: primary=00, secondary=01, subordinate=03, sec-latency=0
	I/O behind bridge: 6000-6fff \[size=4K\] \[16-bit\]
	Memory behind bridge: b0d00000-b0efffff \[size=2M\] \[32-bit\]
	Prefetchable memory behind bridge: 900000000-a0fffffff \[size=4352M\] \[32-bit\]
	Capabilities: \[50\] Power Management version 3
	Capabilities: \[58\] Express Root Port (Slot+), MSI 00
	Capabilities: \[a0\] MSI: Enable+ Count=1/1 Maskable- 64bit+
	Capabilities: \[c0\] Subsystem: Lenovo Device \[17aa:22f2\]
	Capabilities: \[c8\] HyperTransport: MSI Mapping Enable+ Fixed+
	Capabilities: \[100\] Vendor Specific Information: ID=0001 Rev=1 Len=010 \<?>
	Capabilities: \[150\] Advanced Error Reporting
	Capabilities: \[270\] Secondary PCI Express
	Capabilities: \[2a0\] Access Control Services
	Capabilities: \[370\] L1 PM Substates
	Capabilities: \[400\] Data Link Feature \<?>
	Capabilities: \[410\] Physical Layer 16.0 GT/s \<?>
	Capabilities: \[440\] Lane Margining at the Receiver \<?>
	Kernel driver in use: pcieport
00:01.2 PCI bridge \[0604\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h-19h PCIe GPP Bridge \[1022:14ba\] (prog-if 00 \[Normal decode\])
	Subsystem: Lenovo Device \[17aa:22f2\]
	Flags: bus master, fast devsel, latency 0, IRQ 70, IOMMU group 2
	Bus: primary=00, secondary=04, subordinate=04, sec-latency=0
	I/O behind bridge: \[disabled\] \[32-bit\]
	Memory behind bridge: b0000000-b01fffff \[size=2M\] \[32-bit\]
	Prefetchable memory behind bridge: \[disabled\] \[64-bit\]
	Capabilities: \[50\] Power Management version 3
	Capabilities: \[58\] Express Root Port (Slot+), MSI 00
	Capabilities: \[a0\] MSI: Enable+ Count=1/1 Maskable- 64bit+
	Capabilities: \[c0\] Subsystem: Lenovo Device \[17aa:22f2\]
	Capabilities: \[c8\] HyperTransport: MSI Mapping Enable+ Fixed+
	Capabilities: \[100\] Vendor Specific Information: ID=0001 Rev=1 Len=010 \<?>
	Capabilities: \[150\] Advanced Error Reporting
	Capabilities: \[270\] Secondary PCI Express
	Capabilities: \[2a0\] Access Control Services
	Capabilities: \[370\] L1 PM Substates
	Capabilities: \[400\] Data Link Feature \<?>
	Capabilities: \[410\] Physical Layer 16.0 GT/s \<?>
	Capabilities: \[440\] Lane Margining at the Receiver \<?>
	Kernel driver in use: pcieport
00:02.0 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h-19h PCIe Dummy Host Bridge \[1022:14b7\] (rev 01)
	Flags: fast devsel, IOMMU group 3
00:02.1 PCI bridge \[0604\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h-19h PCIe GPP Bridge \[1022:14ba\] (prog-if 00 \[Normal decode\])
	Subsystem: Lenovo Device \[17aa:22f2\]
	Flags: bus master, fast devsel, latency 0, IRQ 71, IOMMU group 4
	Bus: primary=00, secondary=05, subordinate=05, sec-latency=0
	I/O behind bridge: 7000-7fff \[size=4K\] \[16-bit\]
	Memory behind bridge: b0c00000-b0cfffff \[size=1M\] \[32-bit\]
	Prefetchable memory behind bridge: 8a0200000-8a03fffff \[size=2M\] \[32-bit\]
	Capabilities: \[50\] Power Management version 3
	Capabilities: \[58\] Express Root Port (Slot+), MSI 00
	Capabilities: \[a0\] MSI: Enable+ Count=1/1 Maskable- 64bit+
	Capabilities: \[c0\] Subsystem: Lenovo Device \[17aa:22f2\]
	Capabilities: \[c8\] HyperTransport: MSI Mapping Enable+ Fixed+
	Capabilities: \[100\] Vendor Specific Information: ID=0001 Rev=1 Len=010 \<?>
	Capabilities: \[150\] Advanced Error Reporting
	Capabilities: \[270\] Secondary PCI Express
	Capabilities: \[2a0\] Access Control Services
	Capabilities: \[400\] Data Link Feature \<?>
	Capabilities: \[410\] Physical Layer 16.0 GT/s \<?>
	Capabilities: \[440\] Lane Margining at the Receiver \<?>
	Kernel driver in use: pcieport
00:02.4 PCI bridge \[0604\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h-19h PCIe GPP Bridge \[1022:14ba\] (prog-if 00 \[Normal decode\])
	Subsystem: Lenovo Device \[17aa:22f2\]
	Flags: bus master, fast devsel, latency 0, IRQ 72, IOMMU group 5
	Bus: primary=00, secondary=06, subordinate=06, sec-latency=0
	I/O behind bridge: \[disabled\] \[32-bit\]
	Memory behind bridge: b0b00000-b0bfffff \[size=1M\] \[32-bit\]
	Prefetchable memory behind bridge: \[disabled\] \[64-bit\]
	Capabilities: \[50\] Power Management version 3
	Capabilities: \[58\] Express Root Port (Slot+), MSI 00
	Capabilities: \[a0\] MSI: Enable+ Count=1/1 Maskable- 64bit+
	Capabilities: \[c0\] Subsystem: Lenovo Device \[17aa:22f2\]
	Capabilities: \[c8\] HyperTransport: MSI Mapping Enable+ Fixed+
	Capabilities: \[100\] Vendor Specific Information: ID=0001 Rev=1 Len=010 \<?>
	Capabilities: \[150\] Advanced Error Reporting
	Capabilities: \[270\] Secondary PCI Express
	Capabilities: \[2a0\] Access Control Services
	Capabilities: \[370\] L1 PM Substates
	Capabilities: \[400\] Data Link Feature \<?>
	Capabilities: \[410\] Physical Layer 16.0 GT/s \<?>
	Capabilities: \[440\] Lane Margining at the Receiver \<?>
	Kernel driver in use: pcieport
00:03.0 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h-19h PCIe Dummy Host Bridge \[1022:14b7\] (rev 01)
	Flags: fast devsel, IOMMU group 6
00:03.1 PCI bridge \[0604\]: Advanced Micro Devices, Inc. \[AMD\] Family 19h USB4/Thunderbolt PCIe tunnel \[1022:14cd\] (prog-if 00 \[Normal decode\])
	Subsystem: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:1453\]
	Flags: bus master, fast devsel, latency 0, IRQ 73, IOMMU group 6
	Bus: primary=00, secondary=07, subordinate=36, sec-latency=0
	I/O behind bridge: 4000-5fff \[size=8K\] \[16-bit\]
	Memory behind bridge: 98000000-afffffff \[size=384M\] \[32-bit\]
	Prefetchable memory behind bridge: a40000000-a67ffffff \[size=640M\] \[32-bit\]
	Capabilities: \[50\] Power Management version 3
	Capabilities: \[58\] Express Root Port (Slot+), MSI 00
	Capabilities: \[a0\] MSI: Enable+ Count=1/1 Maskable- 64bit+
	Capabilities: \[c0\] Subsystem: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:1453\]
	Capabilities: \[c8\] HyperTransport: MSI Mapping Enable+ Fixed+
	Capabilities: \[100\] Vendor Specific Information: ID=0001 Rev=1 Len=010 \<?>
	Capabilities: \[150\] Advanced Error Reporting
	Capabilities: \[270\] Secondary PCI Express
	Capabilities: \[400\] Data Link Feature \<?>
	Kernel driver in use: pcieport
00:04.0 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h-19h PCIe Dummy Host Bridge \[1022:14b7\] (rev 01)
	Flags: fast devsel, IOMMU group 7
00:04.1 PCI bridge \[0604\]: Advanced Micro Devices, Inc. \[AMD\] Family 19h USB4/Thunderbolt PCIe tunnel \[1022:14cd\] (prog-if 00 \[Normal decode\])
	Subsystem: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:1453\]
	Flags: bus master, fast devsel, latency 0, IRQ 74, IOMMU group 7
	Bus: primary=00, secondary=37, subordinate=66, sec-latency=0
	I/O behind bridge: 2000-3fff \[size=8K\] \[16-bit\]
	Memory behind bridge: 80000000-97ffffff \[size=384M\] \[32-bit\]
	Prefetchable memory behind bridge: a10000000-a37ffffff \[size=640M\] \[32-bit\]
	Capabilities: \[50\] Power Management version 3
	Capabilities: \[58\] Express Root Port (Slot+), MSI 00
	Capabilities: \[a0\] MSI: Enable+ Count=1/1 Maskable- 64bit+
	Capabilities: \[c0\] Subsystem: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:1453\]
	Capabilities: \[c8\] HyperTransport: MSI Mapping Enable+ Fixed+
	Capabilities: \[100\] Vendor Specific Information: ID=0001 Rev=1 Len=010 \<?>
	Capabilities: \[150\] Advanced Error Reporting
	Capabilities: \[270\] Secondary PCI Express
	Capabilities: \[400\] Data Link Feature \<?>
	Kernel driver in use: pcieport
00:08.0 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h-19h PCIe Dummy Host Bridge \[1022:14b7\] (rev 01)
	Flags: fast devsel, IOMMU group 8
00:08.1 PCI bridge \[0604\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h-19h Internal PCIe GPP Bridge \[1022:14b9\] (rev 10) (prog-if 00 \[Normal decode\])
	Subsystem: Lenovo Device \[17aa:22f2\]
	Flags: bus master, fast devsel, latency 0, IRQ 75, IOMMU group 9
	Bus: primary=00, secondary=67, subordinate=67, sec-latency=0
	I/O behind bridge: 1000-1fff \[size=4K\] \[16-bit\]
	Memory behind bridge: b0600000-b0afffff \[size=5M\] \[32-bit\]
	Prefetchable memory behind bridge: a70000000-a801fffff \[size=258M\] \[32-bit\]
	Capabilities: \[50\] Power Management version 3
	Capabilities: \[58\] Express Root Port (Slot-), MSI 00
	Capabilities: \[a0\] MSI: Enable+ Count=1/1 Maskable- 64bit+
	Capabilities: \[c0\] Subsystem: Lenovo Device \[17aa:22f2\]
	Capabilities: \[100\] Vendor Specific Information: ID=0001 Rev=1 Len=010 \<?>
	Capabilities: \[270\] Secondary PCI Express
	Capabilities: \[2a0\] Access Control Services
	Capabilities: \[400\] Data Link Feature \<?>
	Capabilities: \[410\] Physical Layer 16.0 GT/s \<?>
	Capabilities: \[450\] Lane Margining at the Receiver \<?>
	Kernel driver in use: pcieport
00:08.3 PCI bridge \[0604\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h-19h Internal PCIe GPP Bridge \[1022:14b9\] (rev 10) (prog-if 00 \[Normal decode\])
pcilib: Error reading /sys/bus/pci/devices/0000:00:08.3/label: Operation not permitted
	Subsystem: Lenovo Device \[17aa:22f2\]
	Flags: bus master, fast devsel, latency 0, IRQ 76, IOMMU group 10
	Bus: primary=00, secondary=68, subordinate=68, sec-latency=0
	I/O behind bridge: \[disabled\] \[32-bit\]
	Memory behind bridge: b0200000-b05fffff \[size=4M\] \[32-bit\]
	Prefetchable memory behind bridge: \[disabled\] \[64-bit\]
	Capabilities: \[50\] Power Management version 3
	Capabilities: \[58\] Express Root Port (Slot-), MSI 00
	Capabilities: \[a0\] MSI: Enable+ Count=1/1 Maskable- 64bit+
	Capabilities: \[c0\] Subsystem: Lenovo Device \[17aa:22f2\]
	Capabilities: \[100\] Vendor Specific Information: ID=0001 Rev=1 Len=010 \<?>
	Capabilities: \[150\] Advanced Error Reporting
	Capabilities: \[270\] Secondary PCI Express
	Capabilities: \[2a0\] Access Control Services
	Capabilities: \[400\] Data Link Feature \<?>
	Capabilities: \[410\] Physical Layer 16.0 GT/s \<?>
	Capabilities: \[450\] Lane Margining at the Receiver \<?>
	Kernel driver in use: pcieport
00:14.0 SMBus \[0c05\]: Advanced Micro Devices, Inc. \[AMD\] FCH SMBus Controller \[1022:790b\] (rev 71)
	Subsystem: Lenovo FCH SMBus Controller \[17aa:22f2\]
	Flags: 66MHz, medium devsel, IOMMU group 11
00:14.3 ISA bridge \[0601\]: Advanced Micro Devices, Inc. \[AMD\] FCH LPC Bridge \[1022:790e\] (rev 51)
	Subsystem: Lenovo FCH LPC Bridge \[17aa:22f2\]
	Flags: bus master, 66MHz, medium devsel, latency 0, IOMMU group 11
00:18.0 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Rembrandt Data Fabric: Device 18h; Function 0 \[1022:1679\]
	Flags: fast devsel, IOMMU group 12
00:18.1 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Rembrandt Data Fabric: Device 18h; Function 1 \[1022:167a\]
	Flags: fast devsel, IOMMU group 12
00:18.2 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Rembrandt Data Fabric: Device 18h; Function 2 \[1022:167b\]
	Flags: fast devsel, IOMMU group 12
00:18.3 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Rembrandt Data Fabric: Device 18h; Function 3 \[1022:167c\]
	Flags: fast devsel, IOMMU group 12
	Kernel driver in use: k10temp
00:18.4 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Rembrandt Data Fabric: Device 18h; Function 4 \[1022:167d\]
	Flags: fast devsel, IOMMU group 12
00:18.5 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Rembrandt Data Fabric: Device 18h; Function 5 \[1022:167e\]
	Flags: fast devsel, IOMMU group 12
00:18.6 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Rembrandt Data Fabric: Device 18h; Function 6 \[1022:167f\]
	Flags: fast devsel, IOMMU group 12
00:18.7 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Rembrandt Data Fabric: Device 18h; Function 7 \[1022:1680\]
	Flags: fast devsel, IOMMU group 12
01:00.0 PCI bridge \[0604\]: Advanced Micro Devices, Inc. \[AMD/ATI\] Navi 10 XL Upstream Port of PCI Express Switch \[1002:1478\] (rev c3) (prog-if 00 \[Normal decode\])
	Flags: bus master, fast devsel, latency 0, IRQ 77, IOMMU group 13
	Memory at b0e00000 (32-bit, non-prefetchable) \[size=16K\]
	Bus: primary=01, secondary=02, subordinate=03, sec-latency=0
	I/O behind bridge: \[disabled\] \[32-bit\]
	Memory behind bridge: b0d00000-b0dfffff \[size=1M\] \[32-bit\]
	Prefetchable memory behind bridge: 900000000-a0fffffff \[size=4352M\] \[32-bit\]
	Capabilities: \[48\] Vendor Specific Information: Len=08 \<?>
	Capabilities: \[50\] Power Management version 3
	Capabilities: \[58\] Express Upstream Port, MSI 00
	Capabilities: \[a0\] MSI: Enable- Count=1/1 Maskable- 64bit+
	Capabilities: \[100\] Vendor Specific Information: ID=0001 Rev=1 Len=010 \<?>
	Capabilities: \[150\] Advanced Error Reporting
	Capabilities: \[270\] Secondary PCI Express
	Capabilities: \[320\] Latency Tolerance Reporting
	Capabilities: \[370\] L1 PM Substates
	Capabilities: \[400\] Data Link Feature \<?>
	Capabilities: \[410\] Physical Layer 16.0 GT/s \<?>
	Capabilities: \[440\] Lane Margining at the Receiver \<?>
	Kernel driver in use: pcieport
02:00.0 PCI bridge \[0604\]: Advanced Micro Devices, Inc. \[AMD/ATI\] Navi 10 XL Downstream Port of PCI Express Switch \[1002:1479\] (prog-if 00 \[Normal decode\])
	Subsystem: Advanced Micro Devices, Inc. \[AMD/ATI\] Navi 10 XL Downstream Port of PCI Express Switch \[1002:1479\]
	Flags: bus master, fast devsel, latency 0, IRQ 78, IOMMU group 14
	Bus: primary=02, secondary=03, subordinate=03, sec-latency=0
	I/O behind bridge: \[disabled\] \[32-bit\]
	Memory behind bridge: b0d00000-b0dfffff \[size=1M\] \[32-bit\]
	Prefetchable memory behind bridge: 900000000-a0fffffff \[size=4352M\] \[32-bit\]
	Capabilities: \[50\] Power Management version 3
	Capabilities: \[58\] Express Downstream Port (Slot-), MSI 00
	Capabilities: \[a0\] MSI: Enable+ Count=1/1 Maskable- 64bit+
	Capabilities: \[c0\] Subsystem: Advanced Micro Devices, Inc. \[AMD/ATI\] Navi 10 XL Downstream Port of PCI Express Switch \[1002:1479\]
	Capabilities: \[100\] Vendor Specific Information: ID=0001 Rev=1 Len=010 \<?>
	Capabilities: \[150\] Advanced Error Reporting
	Capabilities: \[270\] Secondary PCI Express
	Capabilities: \[2a0\] Access Control Services
	Capabilities: \[400\] Data Link Feature \<?>
	Capabilities: \[410\] Physical Layer 16.0 GT/s \<?>
	Capabilities: \[440\] Lane Margining at the Receiver \<?>
	Kernel driver in use: pcieport
03:00.0 Display controller \[0380\]: Advanced Micro Devices, Inc. \[AMD/ATI\] Navi 24 \[Radeon RX 6400/6500 XT/6500M\] \[1002:743f\] (rev c3)
	Subsystem: Lenovo Navi 24 \[Radeon RX 6400 / 6500 XT\] \[17aa:22f3\]
	Flags: bus master, fast devsel, latency 0, IRQ 79, IOMMU group 15
	Memory at 900000000 (64-bit, prefetchable) \[size=4G\]
	Memory at a00000000 (64-bit, prefetchable) \[size=256M\]
	Memory at b0d00000 (32-bit, non-prefetchable) \[size=1M\]
	Expansion ROM at \<ignored> \[disabled\]
	Capabilities: \[48\] Vendor Specific Information: Len=08 \<?>
	Capabilities: \[50\] Power Management version 3
	Capabilities: \[64\] Express Legacy Endpoint, MSI 00
	Capabilities: \[a0\] MSI: Enable+ Count=1/1 Maskable- 64bit+
	Capabilities: \[100\] Vendor Specific Information: ID=0001 Rev=1 Len=010 \<?>
	Capabilities: \[150\] Advanced Error Reporting
	Capabilities: \[200\] Physical Resizable BAR
	Capabilities: \[240\] Power Budgeting \<?>
	Capabilities: \[270\] Secondary PCI Express
	Capabilities: \[2a0\] Access Control Services
	Capabilities: \[2d0\] Process Address Space ID (PASID)
	Capabilities: \[320\] Latency Tolerance Reporting
	Capabilities: \[410\] Physical Layer 16.0 GT/s \<?>
	Capabilities: \[440\] Lane Margining at the Receiver \<?>
	Kernel driver in use: amdgpu
04:00.0 Network controller \[0280\]: Qualcomm QCNFA765 Wireless Network Adapter \[17cb:1103\] (rev 01)
	Subsystem: Lenovo QCNFA765 Wireless Network Adapter \[17aa:9309\]
	Flags: bus master, fast devsel, latency 0, IRQ 84, IOMMU group 16
	Memory at b0000000 (64-bit, non-prefetchable) \[size=2M\]
	Capabilities: \[40\] Power Management version 3
	Capabilities: \[50\] MSI: Enable+ Count=32/32 Maskable+ 64bit-
	Capabilities: \[70\] Express Endpoint, MSI 00
	Capabilities: \[100\] Advanced Error Reporting
	Capabilities: \[148\] Secondary PCI Express
	Capabilities: \[158\] Transaction Processing Hints
	Capabilities: \[1e4\] Latency Tolerance Reporting
	Capabilities: \[1ec\] L1 PM Substates
	Kernel driver in use: ath11k\_pci
05:00.0 SD Host controller \[0805\]: Genesys Logic, Inc GL9755 SD Host Controller \[17a0:9755\] (prog-if 01)
	Subsystem: Lenovo GL9755 SD Host Controller \[17aa:22f2\]
	Flags: fast devsel, IRQ 255, IOMMU group 17
	Memory at b0c00000 (32-bit, non-prefetchable) \[disabled\] \[size=4K\]
	Capabilities: \[80\] Express Endpoint, MSI 00
	Capabilities: \[e0\] MSI: Enable- Count=1/1 Maskable- 64bit+
	Capabilities: \[f8\] Power Management version 3
	Capabilities: \[100\] Vendor Specific Information: ID=17a0 Rev=1 Len=008 \<?>
	Capabilities: \[108\] Latency Tolerance Reporting
	Capabilities: \[110\] L1 PM Substates
	Capabilities: \[200\] Advanced Error Reporting
06:00.0 Non-Volatile memory controller \[0108\]: Sandisk Corp WD PC SN810 / Black SN850 NVMe SSD \[15b7:5011\] (rev 01) (prog-if 02 \[NVM Express\])
	Subsystem: Sandisk Corp WD PC SN810 / Black SN850 NVMe SSD \[15b7:5011\]
	Flags: bus master, fast devsel, latency 0, IRQ 82, IOMMU group 18
	Memory at b0b00000 (64-bit, non-prefetchable) \[size=16K\]
	Capabilities: \[80\] Power Management version 3
	Capabilities: \[90\] MSI: Enable- Count=1/32 Maskable- 64bit+
	Capabilities: \[b0\] MSI-X: Enable+ Count=65 Masked-
	Capabilities: \[c0\] Express Endpoint, MSI 00
	Capabilities: \[100\] Advanced Error Reporting
	Capabilities: \[1b8\] Latency Tolerance Reporting
	Capabilities: \[300\] Secondary PCI Express
	Capabilities: \[900\] L1 PM Substates
	Capabilities: \[910\] Data Link Feature \<?>
	Capabilities: \[920\] Lane Margining at the Receiver \<?>
	Capabilities: \[9c0\] Physical Layer 16.0 GT/s \<?>
	Kernel driver in use: nvme
67:00.0 VGA compatible controller \[0300\]: Advanced Micro Devices, Inc. \[AMD/ATI\] Rembrandt \[Radeon 680M\] \[1002:1681\] (rev 12) (prog-if 00 \[VGA controller\])
	Subsystem: Lenovo Rembrandt \[Radeon 680M\] \[17aa:22f2\]
	Flags: bus master, fast devsel, latency 0, IRQ 80, IOMMU group 19
	Memory at a70000000 (64-bit, prefetchable) \[size=256M\]
	Memory at a80000000 (64-bit, prefetchable) \[size=2M\]
	I/O ports at 1000 \[size=256\]
	Memory at b0a00000 (32-bit, non-prefetchable) \[size=512K\]
	Capabilities: \[48\] Vendor Specific Information: Len=08 \<?>
	Capabilities: \[50\] Power Management version 3
	Capabilities: \[64\] Express Legacy Endpoint, MSI 00
	Capabilities: \[a0\] MSI: Enable- Count=1/4 Maskable- 64bit+
	Capabilities: \[c0\] MSI-X: Enable+ Count=4 Masked-
	Capabilities: \[100\] Vendor Specific Information: ID=0001 Rev=1 Len=010 \<?>
	Capabilities: \[270\] Secondary PCI Express
	Capabilities: \[2a0\] Access Control Services
	Capabilities: \[2b0\] Address Translation Service (ATS)
	Capabilities: \[2c0\] Page Request Interface (PRI)
	Capabilities: \[2d0\] Process Address Space ID (PASID)
	Capabilities: \[410\] Physical Layer 16.0 GT/s \<?>
	Capabilities: \[450\] Lane Margining at the Receiver \<?>
	Kernel driver in use: amdgpu
67:00.1 Audio device \[0403\]: Advanced Micro Devices, Inc. \[AMD/ATI\] Rembrandt Radeon High Definition Audio Controller \[1002:1640\]
	Subsystem: Lenovo Rembrandt Radeon High Definition Audio Controller \[17aa:22f2\]
	Flags: bus master, fast devsel, latency 0, IRQ 157, IOMMU group 20
	Memory at b0ac8000 (32-bit, non-prefetchable) \[size=16K\]
	Capabilities: \[48\] Vendor Specific Information: Len=08 \<?>
	Capabilities: \[50\] Power Management version 3
	Capabilities: \[64\] Express Legacy Endpoint, MSI 00
	Capabilities: \[a0\] MSI: Enable+ Count=1/1 Maskable- 64bit+
	Capabilities: \[100\] Vendor Specific Information: ID=0001 Rev=1 Len=010 \<?>
	Capabilities: \[2a0\] Access Control Services
	Kernel driver in use: snd\_hda\_intel
67:00.2 Encryption controller \[1080\]: Advanced Micro Devices, Inc. \[AMD\] VanGogh PSP/CCP \[1022:1649\]
	Subsystem: Lenovo VanGogh PSP/CCP \[17aa:22f2\]
	Flags: bus master, fast devsel, latency 0, IRQ 24, IOMMU group 21
	Memory at b0900000 (32-bit, non-prefetchable) \[size=1M\]
	Memory at b0ace000 (32-bit, non-prefetchable) \[size=8K\]
	Capabilities: \[48\] Vendor Specific Information: Len=08 \<?>
	Capabilities: \[50\] Power Management version 3
	Capabilities: \[64\] Express Endpoint, MSI 00
	Capabilities: \[a0\] MSI: Enable- Count=1/2 Maskable- 64bit+
	Capabilities: \[c0\] MSI-X: Enable+ Count=2 Masked-
	Capabilities: \[100\] Vendor Specific Information: ID=0001 Rev=1 Len=010 \<?>
	Capabilities: \[2a0\] Access Control Services
	Kernel driver in use: ccp
67:00.3 USB controller \[0c03\]: Advanced Micro Devices, Inc. \[AMD\] Rembrandt USB4 XHCI controller #3 \[1022:161d\] (prog-if 30 \[XHCI\])
	Subsystem: Lenovo Device \[17aa:22f2\]
	Flags: bus master, fast devsel, latency 0, IRQ 133, IOMMU group 22
	Memory at b0600000 (64-bit, non-prefetchable) \[size=1M\]
	Capabilities: \[48\] Vendor Specific Information: Len=08 \<?>
	Capabilities: \[50\] Power Management version 3
	Capabilities: \[64\] Express Endpoint, MSI 00
	Capabilities: \[a0\] MSI: Enable+ Count=1/1 Maskable- 64bit+
	Capabilities: \[c0\] MSI-X: Enable- Count=1 Masked-
	Capabilities: \[100\] Vendor Specific Information: ID=0001 Rev=1 Len=010 \<?>
	Capabilities: \[2a0\] Access Control Services
	Kernel driver in use: xhci\_hcd
67:00.4 USB controller \[0c03\]: Advanced Micro Devices, Inc. \[AMD\] Rembrandt USB4 XHCI controller #4 \[1022:161e\] (prog-if 30 \[XHCI\])
	Subsystem: Lenovo Device \[17aa:22f2\]
	Flags: bus master, fast devsel, latency 0, IRQ 134, IOMMU group 23
	Memory at b0700000 (64-bit, non-prefetchable) \[size=1M\]
	Capabilities: \[48\] Vendor Specific Information: Len=08 \<?>
	Capabilities: \[50\] Power Management version 3
	Capabilities: \[64\] Express Endpoint, MSI 00
	Capabilities: \[a0\] MSI: Enable+ Count=1/1 Maskable- 64bit+
	Capabilities: \[c0\] MSI-X: Enable- Count=1 Masked-
	Capabilities: \[100\] Vendor Specific Information: ID=0001 Rev=1 Len=010 \<?>
	Capabilities: \[2a0\] Access Control Services
	Kernel driver in use: xhci\_hcd
67:00.5 Multimedia controller \[0480\]: Advanced Micro Devices, Inc. \[AMD\] ACP/ACP3X/ACP6x Audio Coprocessor \[1022:15e2\] (rev 60)
	Subsystem: Lenovo ACP/ACP3X/ACP6x Audio Coprocessor \[17aa:22f2\]
	Flags: bus master, fast devsel, latency 0, IRQ 156, IOMMU group 24
	Memory at b0a80000 (32-bit, non-prefetchable) \[size=256K\]
	Capabilities: \[48\] Vendor Specific Information: Len=08 \<?>
	Capabilities: \[50\] Power Management version 3
	Capabilities: \[64\] Express Endpoint, MSI 00
	Capabilities: \[a0\] MSI: Enable- Count=1/1 Maskable- 64bit+
	Capabilities: \[100\] Vendor Specific Information: ID=0001 Rev=1 Len=010 \<?>
	Capabilities: \[2a0\] Access Control Services
	Kernel driver in use: snd\_pci\_acp6x
67:00.6 Audio device \[0403\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h/19h HD Audio Controller \[1022:15e3\]
	Subsystem: Lenovo Family 17h/19h HD Audio Controller \[17aa:22f2\]
	Flags: bus master, fast devsel, latency 0, IRQ 158, IOMMU group 25
	Memory at b0ac0000 (32-bit, non-prefetchable) \[size=32K\]
	Capabilities: \[48\] Vendor Specific Information: Len=08 \<?>
	Capabilities: \[50\] Power Management version 3
	Capabilities: \[64\] Express Endpoint, MSI 00
	Capabilities: \[a0\] MSI: Enable+ Count=1/1 Maskable- 64bit+
	Capabilities: \[100\] Vendor Specific Information: ID=0001 Rev=1 Len=010 \<?>
	Capabilities: \[2a0\] Access Control Services
	Kernel driver in use: snd\_hda\_intel
67:00.7 Signal processing controller \[1180\]: Advanced Micro Devices, Inc. \[AMD\] Sensor Fusion Hub \[1022:15e4\]
	Subsystem: Lenovo Sensor Fusion Hub \[17aa:22f2\]
	Flags: fast devsel, IRQ 132, IOMMU group 26
	Memory at b0800000 (32-bit, non-prefetchable) \[size=1M\]
	Memory at b0acc000 (32-bit, non-prefetchable) \[size=8K\]
	Capabilities: \[48\] Vendor Specific Information: Len=08 \<?>
	Capabilities: \[50\] Power Management version 3
	Capabilities: \[64\] Express Endpoint, MSI 00
	Capabilities: \[a0\] MSI: Enable- Count=1/2 Maskable- 64bit+
	Capabilities: \[c0\] MSI-X: Enable- Count=2 Masked-
	Capabilities: \[100\] Vendor Specific Information: ID=0001 Rev=1 Len=010 \<?>
	Capabilities: \[2a0\] Access Control Services
	Kernel modules: amd\_sfh
68:00.0 USB controller \[0c03\]: Advanced Micro Devices, Inc. \[AMD\] Rembrandt USB4 XHCI controller #8 \[1022:161f\] (prog-if 30 \[XHCI\])
	Subsystem: Lenovo Device \[17aa:22f2\]
	Flags: bus master, fast devsel, latency 0, IRQ 135, IOMMU group 27
	Memory at b0200000 (64-bit, non-prefetchable) \[size=1M\]
	Capabilities: \[48\] Vendor Specific Information: Len=08 \<?>
	Capabilities: \[50\] Power Management version 3
	Capabilities: \[64\] Express Endpoint, MSI 00
	Capabilities: \[a0\] MSI: Enable+ Count=1/1 Maskable- 64bit+
	Capabilities: \[c0\] MSI-X: Enable- Count=1 Masked-
	Capabilities: \[100\] Vendor Specific Information: ID=0001 Rev=1 Len=010 \<?>
	Capabilities: \[270\] Secondary PCI Express
	Capabilities: \[2a0\] Access Control Services
	Capabilities: \[410\] Physical Layer 16.0 GT/s \<?>
	Capabilities: \[450\] Lane Margining at the Receiver \<?>
	Kernel driver in use: xhci\_hcd
68:00.3 USB controller \[0c03\]: Advanced Micro Devices, Inc. \[AMD\] Rembrandt USB4 XHCI controller #5 \[1022:15d6\] (prog-if 30 \[XHCI\])
	Subsystem: Advanced Micro Devices, Inc. \[AMD\] Rembrandt USB4 XHCI controller #5 \[1022:15d6\]
	Flags: bus master, fast devsel, latency 0, IRQ 136, IOMMU group 28
	Memory at b0300000 (64-bit, non-prefetchable) \[size=1M\]
	Capabilities: \[48\] Vendor Specific Information: Len=08 \<?>
	Capabilities: \[50\] Power Management version 3
	Capabilities: \[64\] Express Endpoint, MSI 00
	Capabilities: \[a0\] MSI: Enable+ Count=1/1 Maskable- 64bit+
	Capabilities: \[c0\] MSI-X: Enable- Count=1 Masked-
	Capabilities: \[100\] Vendor Specific Information: ID=0001 Rev=1 Len=010 \<?>
	Capabilities: \[2a0\] Access Control Services
	Kernel driver in use: xhci\_hcd
68:00.4 USB controller \[0c03\]: Advanced Micro Devices, Inc. \[AMD\] Rembrandt USB4 XHCI controller #6 \[1022:15d7\] (prog-if 30 \[XHCI\])
	Subsystem: Advanced Micro Devices, Inc. \[AMD\] Rembrandt USB4 XHCI controller #6 \[1022:15d7\]
	Flags: bus master, fast devsel, latency 0, IRQ 138, IOMMU group 29
	Memory at b0400000 (64-bit, non-prefetchable) \[size=1M\]
	Capabilities: \[48\] Vendor Specific Information: Len=08 \<?>
	Capabilities: \[50\] Power Management version 3
	Capabilities: \[64\] Express Endpoint, MSI 00
	Capabilities: \[a0\] MSI: Enable+ Count=1/1 Maskable- 64bit+
	Capabilities: \[c0\] MSI-X: Enable- Count=1 Masked-
	Capabilities: \[100\] Vendor Specific Information: ID=0001 Rev=1 Len=010 \<?>
	Capabilities: \[2a0\] Access Control Services
	Kernel driver in use: xhci\_hcd
68:00.5 USB controller \[0c03\]: Advanced Micro Devices, Inc. \[AMD\] Rembrandt USB4/Thunderbolt NHI controller #1 \[1022:162e\] (prog-if 40 \[USB4 Host Interface\])
	Subsystem: Advanced Micro Devices, Inc. \[AMD\] Rembrandt USB4/Thunderbolt NHI controller #1 \[1022:162e\]
	Flags: bus master, fast devsel, latency 0, IRQ 26, IOMMU group 30
	Memory at b0500000 (64-bit, non-prefetchable) \[size=512K\]
	Capabilities: \[48\] Vendor Specific Information: Len=08 \<?>
	Capabilities: \[50\] Power Management version 3
	Capabilities: \[64\] Express Endpoint, MSI 00
	Capabilities: \[a0\] MSI: Enable- Count=1/16 Maskable- 64bit+
	Capabilities: \[c0\] MSI-X: Enable+ Count=16 Masked-
	Capabilities: \[100\] Vendor Specific Information: ID=0001 Rev=1 Len=010 \<?>
	Capabilities: \[2a0\] Access Control Services
	Kernel driver in use: thunderbolt
68:00.6 USB controller \[0c03\]: Advanced Micro Devices, Inc. \[AMD\] Rembrandt USB4/Thunderbolt NHI controller #2 \[1022:162f\] (prog-if 40 \[USB4 Host Interface\])
	Subsystem: Advanced Micro Devices, Inc. \[AMD\] Rembrandt USB4/Thunderbolt NHI controller #2 \[1022:162f\]
	Flags: bus master, fast devsel, latency 0, IRQ 43, IOMMU group 31
	Memory at b0580000 (64-bit, non-prefetchable) \[size=512K\]
	Capabilities: \[48\] Vendor Specific Information: Len=08 \<?>
	Capabilities: \[50\] Power Management version 3
	Capabilities: \[64\] Express Endpoint, MSI 00
	Capabilities: \[a0\] MSI: Enable- Count=1/16 Maskable- 64bit+
	Capabilities: \[c0\] MSI-X: Enable+ Count=16 Masked-
	Capabilities: \[100\] Vendor Specific Information: ID=0001 Rev=1 Len=010 \<?>
	Capabilities: \[2a0\] Access Control Services
	Kernel driver in use: thunderbolt

### CPU

`root #``cat /proc/cpuinfo`
processor	: 0
vendor\_id	: AuthenticAMD
cpu family	: 25
model		: 68
model name	: AMD Ryzen 7 PRO 6850H with Radeon Graphics
stepping	: 1
microcode	: 0xa404102
cpu MHz		: 1773.722
cache size	: 512 KB
physical id	: 0
siblings	: 16
core id		: 0
cpu cores	: 8
apicid		: 0
initial apicid	: 0
fpu		: yes
fpu\_exception	: yes
cpuid level	: 16
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx mmxext fxsr\_opt pdpe1gb rdtscp lm constant\_tsc rep\_good nopl nonstop\_tsc cpuid extd\_apicid aperfmperf rapl pni pclmulqdq monitor ssse3 fma cx16 sse4\_1 sse4\_2 x2apic movbe popcnt aes xsave avx f16c rdrand lahf\_lm cmp\_legacy svm extapic cr8\_legacy abm sse4a misalignsse 3dnowprefetch osvw ibs skinit wdt tce topoext perfctr\_core perfctr\_nb bpext perfctr\_llc mwaitx cpb cat\_l3 cdp\_l3 hw\_pstate ssbd mba ibrs ibpb stibp vmmcall fsgsbase bmi1 avx2 smep bmi2 invpcid cqm rdt\_a rdseed adx smap clflushopt clwb sha\_ni xsaveopt xsavec xgetbv1 xsaves cqm\_llc cqm\_occup\_llc cqm\_mbm\_total cqm\_mbm\_local clzero irperf xsaveerptr rdpru wbnoinvd cppc arat npt lbrv svm\_lock nrip\_save tsc\_scale vmcb\_clean flushbyasid decodeassists pausefilter pfthreshold avic v\_vmsave\_vmload vgif v\_spec\_ctrl umip pku ospke vaes vpclmulqdq rdpid overflow\_recov succor smca
bugs		: sysret\_ss\_attrs spectre\_v1 spectre\_v2 spec\_store\_bypass
bogomips	: 6388.39
TLB size	: 2560 4K pages
clflush size	: 64
cache\_alignment	: 64
address sizes	: 48 bits physical, 48 bits virtual
power management: ts ttp tm hwpstate cpb eff\_freq\_ro \[13\] \[14\]

## Installation

### Firmware

The [AMDGPU](https://wiki.gentoo.org/wiki/AMDGPU) driver for the Radeon 680M GPU requires the following [firmware](https://wiki.gentoo.org/wiki/Linux_firmware):

- amdgpu/beige\_goby\_sos.bin
- amdgpu/beige\_goby\_ta.bin
- amdgpu/beige\_goby\_smc.bin
- amdgpu/beige\_goby\_dmcub.bin
- amdgpu/beige\_goby\_pfp.bin
- amdgpu/beige\_goby\_me.bin
- amdgpu/beige\_goby\_ce.bin
- amdgpu/beige\_goby\_rlc.bin
- amdgpu/beige\_goby\_mec.bin
- amdgpu/beige\_goby\_mec2.bin
- amdgpu/beige\_goby\_sdma.bin
- amdgpu/beige\_goby\_vcn.bin


The AMDGPU driver for the Radeon 6500M GPU requires the following firmware:

- amdgpu/yellow\_carp\_toc.bin
- amdgpu/yellow\_carp\_ta.bin
- amdgpu/yellow\_carp\_dmcub.bin
- amdgpu/yellow\_carp\_pfp.bin
- amdgpu/yellow\_carp\_me.bin
- amdgpu/yellow\_carp\_ce.bin
- amdgpu/yellow\_carp\_rlc.bin
- amdgpu/yellow\_carp\_mec.bin
- amdgpu/yellow\_carp\_mec2.bin
- amdgpu/yellow\_carp\_sdma.bin
- amdgpu/yellow\_carp\_vcn.bin


The wireless card requires the following firmware:

- ath11k/WCN6855/hw2.1/board-2.bin
- ath11k/WCN6855/hw2.1/regdb.bin
- ath11k/WCN6855/hw2.1/m3.bin
- ath11k/WCN6855/hw2.1/amss.bin
- regulatory.db
- regulatory.db.p7s


The sound card requires the following firmware:

- cirrus/cs35l41-dsp1-spk-prot-17aa22f2.wmfw
- cirrus/cs35l41-dsp1-spk-prot-17aa22f2-l0.bin
- cirrus/cs35l41-dsp1-spk-prot-17aa22f2-r0.bin


The [bluetooth](https://wiki.gentoo.org/wiki/Bluetooth) card requires the following firmware:

- qca/rampatch\_usb\_00130201.bin
- qca/nvm\_usb\_00130201\_gf.bin

### Kernel

#### Touchpad, trackpoint & touch screen

**Enable support for touchpad, trackpoint and touch screen**

#### Wi-Fi

For proper function, the DMAR\_TABLE and IRQ\_REMAP drivers need to be enabled, otherwise the Wi-Fi card will work intermitently and inconsistently.

**Enable support for wi-fi**

#### Bluetooth

**Enable support for bluetooth**

#### Sound & Microphone

The speakers on this laptop need the Cirrus CS35L41 driver and firmware, as well as serial bus multi instantiate driver. Without those, sound will work through AUX or USB, but the laptop speakers themselves will not.

**Enable support for sound & microphone**

#### Webcam

**Enable support for webcam**

#### Misc

The thinkpad\_acpi driver allows the Sound & Microphone mute LEDs to work (when sound/microphone are muted, their corresponding LED will light up). For proper function, it may be required to build the sound drivers in-kernel, rather than as modules. The media-sound/alsa-utils is also needed for proper function.

**Enable support for misc. features**

### Emerge

For GPU, sound and Wi-Fi functionality

`root #``emerge --ask sys-kernel/linux-firmware`
For [fingerprint reader](https://wiki.gentoo.org/wiki/Fingerprint_Reader) functionality

`root #``emerge --ask sys-auth/fprintd`
## Configuration

### Linux TTY font size

On the HiDPI 4K variant of the Z16, the font on the Linux terminal is too small for comfortable reading. To use bigger fonts, the following kernel parameter must be enabled:

**Use larger font in TTY**

Additionally, the [media-fonts/terminus-font](https://packages.gentoo.org/packages/media-fonts/terminus-font) must be installed.

Then, the consolefont service must be configured and enabled:

**`/etc/conf.d/consolefont`**

`root #``rc-update add consolefont boot`
Finally, the kernel command line must include "fbcon=font:TER16x32". This must be configured according to your [bootloader](https://wiki.gentoo.org/wiki/Bootloader).

`user $``cat /proc/cmdline`
<...> fbcon=font:TER16x32 <...>

## Troubleshooting

### Fingerprint Reader not working correctly

If the fingerprint reader (via [sys-auth/fprintd](https://packages.gentoo.org/packages/sys-auth/fprintd)) doesn't work correctly, such as losing the enrolled fingerprints, or the fingerprints never matching, try to reboot in the BIOS and clear the fingerprint data. After this, fprintd should work correctly.

### Fingerprint Reader works intermitently

If the fingerprint reader only works sometimes and other times does not recognize your fingerprint, try to restart fprintd:

`root #``killall fprintd`
### No sound on speakers

Make sure to enable the CONFIG\_SERIAL\_MULTI\_INSTANTIATE kernel driver.

### Wi-Fi works inconsistently

If there are no wifi devices detected at boot, or if the wifi device can't connect to a wifi network, a temporary workaround is to unload the ath11k\_pci driver and reload it. The permanent solution is to enable the CONFIG\_IRQ\_REMAP and CONFIG\_DMAR\_TABLE kernel configs.
