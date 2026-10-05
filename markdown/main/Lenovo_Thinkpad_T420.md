<!-- source: https://wiki.gentoo.org/wiki/Lenovo_Thinkpad_T420 | group: Gentoo Wiki (Main) | wiki-title: Lenovo Thinkpad T420 -->
---
title: Lenovo Thinkpad T420
url: https://wiki.gentoo.org/wiki/Lenovo_Thinkpad_T420
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "7d1abf1fd3aa30ed"
license: CC BY-SA 4.0
---

# Lenovo Thinkpad T420

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

After proper configuration, every part of the Thinkpad T420 is Linux friendly.

## Hardware

### lspci

`root #``lspci -nnk`
00:00.0 Host bridge \[0600\]: Intel Corporation 2nd Generation Core Processor Family DRAM Controller \[8086:0104\] (rev 09)
	Subsystem: Lenovo 2nd Generation Core Processor Family DRAM Controller \[17aa:21ce\]
00:02.0 VGA compatible controller \[0300\]: Intel Corporation 2nd Generation Core Processor Family Integrated Graphics Controller \[8086:0126\] (rev 09)
	Subsystem: Lenovo 2nd Generation Core Processor Family Integrated Graphics Controller \[17aa:21ce\]
	Kernel driver in use: i915
00:16.0 Communication controller \[0780\]: Intel Corporation 6 Series/C200 Series Chipset Family MEI Controller #1 \[8086:1c3a\] (rev 04)
	Subsystem: Lenovo 6 Series/C200 Series Chipset Family MEI Controller \[17aa:21ce\]
00:16.3 Serial controller \[0700\]: Intel Corporation 6 Series/C200 Series Chipset Family KT Controller \[8086:1c3d\] (rev 04)
	Subsystem: Lenovo 6 Series/C200 Series Chipset Family KT Controller \[17aa:21ce\]
	Kernel driver in use: serial
00:19.0 Ethernet controller \[0200\]: Intel Corporation 82579LM Gigabit Network Connection \[8086:1502\] (rev 04)
	Subsystem: Lenovo 82579LM Gigabit Network Connection \[17aa:21ce\]
	Kernel driver in use: e1000e
00:1a.0 USB controller \[0c03\]: Intel Corporation 6 Series/C200 Series Chipset Family USB Enhanced Host Controller #2 \[8086:1c2d\] (rev 04)
	Subsystem: Lenovo 6 Series/C200 Series Chipset Family USB Enhanced Host Controller \[17aa:21ce\]
	Kernel driver in use: ehci-pci
00:1b.0 Audio device \[0403\]: Intel Corporation 6 Series/C200 Series Chipset Family High Definition Audio Controller \[8086:1c20\] (rev 04)
	Subsystem: Lenovo 6 Series/C200 Series Chipset Family High Definition Audio Controller \[17aa:21ce\]
	Kernel driver in use: snd\_hda\_intel
00:1c.0 PCI bridge \[0604\]: Intel Corporation 6 Series/C200 Series Chipset Family PCI Express Root Port 1 \[8086:1c10\] (rev b4)
	Kernel driver in use: pcieport
00:1c.1 PCI bridge \[0604\]: Intel Corporation 6 Series/C200 Series Chipset Family PCI Express Root Port 2 \[8086:1c12\] (rev b4)
	Kernel driver in use: pcieport
00:1c.3 PCI bridge \[0604\]: Intel Corporation 6 Series/C200 Series Chipset Family PCI Express Root Port 4 \[8086:1c16\] (rev b4)
	Kernel driver in use: pcieport
00:1c.4 PCI bridge \[0604\]: Intel Corporation 6 Series/C200 Series Chipset Family PCI Express Root Port 5 \[8086:1c18\] (rev b4)
	Kernel driver in use: pcieport
00:1d.0 USB controller \[0c03\]: Intel Corporation 6 Series/C200 Series Chipset Family USB Enhanced Host Controller #1 \[8086:1c26\] (rev 04)
	Subsystem: Lenovo 6 Series/C200 Series Chipset Family USB Enhanced Host Controller \[17aa:21ce\]
	Kernel driver in use: ehci-pci
00:1f.0 ISA bridge \[0601\]: Intel Corporation QM67 Express Chipset Family LPC Controller \[8086:1c4f\] (rev 04)
	Subsystem: Lenovo QM67 Express Chipset Family LPC Controller \[17aa:21ce\]
00:1f.2 SATA controller \[0106\]: Intel Corporation 6 Series/C200 Series Chipset Family 6 port SATA AHCI Controller \[8086:1c03\] (rev 04)
	Subsystem: Lenovo 6 Series/C200 Series Chipset Family 6 port SATA AHCI Controller \[17aa:21ce\]
	Kernel driver in use: ahci
	Kernel modules: ahci
00:1f.3 SMBus \[0c05\]: Intel Corporation 6 Series/C200 Series Chipset Family SMBus Controller \[8086:1c22\] (rev 04)
	Subsystem: Lenovo 6 Series/C200 Series Chipset Family SMBus Controller \[17aa:21ce\]
	Kernel driver in use: i801\_smbus
	Kernel modules: i2c\_i801
03:00.0 Network controller \[0280\]: Intel Corporation Centrino Advanced-N 6205 \[Taylor Peak\] \[8086:0085\] (rev 34)
	Subsystem: Intel Corporation Centrino Advanced-N 6205 AGN \[8086:1311\]
	Kernel driver in use: iwlwifi
0d:00.0 System peripheral \[0880\]: Ricoh Co Ltd PCIe SDXC/MMC Host Controller \[1180:e823\] (rev 08)
	Subsystem: Lenovo PCIe SDXC/MMC Host Controller \[17aa:21ce\]
	Kernel driver in use: sdhci-pci

### lsusb

`root #``lsusb`
Bus 002 Device 003: ID 17ef:1003 Lenovo Integrated Smart Card Reader
Bus 002 Device 004: ID 05dc:a838 Lexar Media, Inc. 
Bus 002 Device 002: ID 8087:0024 Intel Corp. Integrated Rate Matching Hub
Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 004: ID 04f2:b221 Chicony Electronics Co., Ltd integrated camera
Bus 001 Device 003: ID 0a5c:217f Broadcom Corp. BCM2045B (BDC-2.1)
Bus 001 Device 002: ID 8087:0024 Intel Corp. Integrated Rate Matching Hub
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

## Installation

### Firmware

The Linux firmware package is needed since the Centrino Advanced-N 6205 AGN wifi adapter requires a specific firmware file.

The only file that is needed is the iwlwifi-6000g2a-6.ucode firmware file. It must be compiled directly into the kernel binary. Obtain the file by installing the Linux firmware package:

`root #``emerge --ask sys-kernel/linux-firmware`
### Kernel

Here's a link to a pre-configured, hand-selected, Gentoo specific [4.10.10-gentoo kernel config file](https://gist.github.com/maffblaster/9ae348179f970bd4614788ca0aed3d1b).

**Enable support for these hardware drivers:**

**Webcam**

## See also

## External resources

- [http://www.thinkwiki.org/wiki/Installing\_Gentoo\_on\_a\_ThinkPad\_T420](http://www.thinkwiki.org/wiki/Installing_Gentoo_on_a_ThinkPad_T420) - Another guide to installing Gentoo on the Lenovo Thinkpad T420.
