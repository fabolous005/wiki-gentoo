<!-- source: https://wiki.gentoo.org/wiki/Huawei_MateBook_D_15_AMD_2019 | group: Gentoo Wiki (Main) | wiki-title: Huawei MateBook D 15 AMD 2019 -->
---
title: Huawei MateBook D 15 AMD 2019
url: https://wiki.gentoo.org/wiki/Huawei_MateBook_D_15_AMD_2019
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "1f562cd1e3b224ed"
license: CC BY-SA 4.0
---

# Huawei MateBook D 15 AMD 2019

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

There are different Huawei laptops models for MateBook series. In this page, the model will be AMD version of Huawei Laptop D 15 released on 2019.

## Hardware

### CPU

There are 2 versions available: AMD Ryzen 5 3500U (used here) and AMD Ryzen 7 3700U.

`user $``lscpu`
Architecture:                       x86\_64
CPU op-mode(s):                     32-bit, 64-bit
Address sizes:                      43 bits physical, 48 bits virtual
Byte Order:                         Little Endian
CPU(s):                             8
On-line CPU(s) list:                0-7
Vendor ID:                          AuthenticAMD
BIOS Vendor ID:                     Advanced Micro Devices, Inc.
Model name:                         AMD Ryzen 5 3500U with Radeon Vega Mobile Gfx
BIOS Model name:                    AMD Ryzen 5 3500U with Radeon Vega Mobile Gfx   NULL CPU @ 2.1GHz
BIOS CPU family:                    107
CPU family:                         23
Model:                              24
Thread(s) per core:                 2
Core(s) per socket:                 4
Socket(s):                          1
Stepping:                           1
Frequency boost:                    enabled
CPU(s) scaling MHz:                 65%
CPU max MHz:                        2100.0000
CPU min MHz:                        1400.0000
BogoMIPS:                           4193.78
Flags:                              fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx mmxext fxsr\_opt pdpe1gb rdtscp lm constant\_tsc rep\_good nopl nonstop\_tsc cpuid extd\_apicid aperfmperf rapl pni pclmulqdq monitor ssse3 fma cx16 sse4\_1 sse4\_2 movbe popcnt aes xsave avx f16c rdrand lahf\_lm cmp\_legacy svm extapic cr8\_legacy abm sse4a misalignsse 3dnowprefetch osvw skinit wdt tce topoext perfctr\_core perfctr\_nb bpext perfctr\_llc mwaitx cpb hw\_pstate ssbd ibpb vmmcall fsgsbase bmi1 avx2 smep bmi2 rdseed adx smap clflushopt sha\_ni xsaveopt xsavec xgetbv1 clzero irperf xsaveerptr arat npt lbrv svm\_lock nrip\_save tsc\_scale vmcb\_clean flushbyasid decodeassists pausefilter pfthreshold avic v\_vmsave\_vmload vgif overflow\_recov succor smca sev sev\_es

### GPU

There are 2 versions available: Radeon Vega 8 Graphics (used here) and Radeon Vega 10 Graphics.

`root #``lscpi -k -s 03:00.0`
03:00.0 VGA compatible controller \[0300\]: Advanced Micro Devices, Inc. \[AMD/ATI\] Picasso/Raven 2 \[Radeon Vega Series / Radeon Vega Mobile Series\] \[1002:15d8\] (rev c2)
	Subsystem: Huawei Technologies Co., Ltd. Picasso/Raven 2 \[Radeon Vega Series / Radeon Vega Mobile Series\] \[19e5:3e18\]
	Kernel driver in use: amdgpu
	Kernel modules: amdgpu

### RAM

RAM is DDR4 and the RAM modules are not upgradable.

The capacity of RAM modules:

`root #``dmidecode -t memory | grep -i size`
Size: 4 GB
	Size: 4 GB

The frequency of RAM modulesː

`root #``dmidecode -t memory | grep 'Memory Speed'`
Configured Memory Speed: 2400 MT/s
	Configured Memory Speed: 2400 MT/s

### SSD

The storage slot is PCIe 3.0 NVMe SSD.

The original storage was changed.

`root #``lscpi -k -s 01:00.0`
01:00.0 Non-Volatile memory controller \[0108\]: Samsung Electronics Co Ltd NVMe SSD Controller SM981/PM981/PM983 \[144d:a808\]
	Subsystem: Samsung Electronics Co Ltd NVMe SSD Controller SM981/PM981/PM983 (SSD 970 EVO) \[144d:a801\]
	Kernel driver in use: nvme
	Kernel modules: nvme

### Wireless

The original Mini PCIe card was changed.

`root #``lscpi -k -s 02:00.0`
02:00.0 Network controller \[0280\]: Qualcomm Atheros AR9462 Wireless Network Adapter \[168c:0034\] (rev 01)
	Subsystem: AzureWave AR9462 Wireless Network Adapter \[1a3b:2234\]
	Kernel driver in use: ath9k
	Kernel modules: ath9k

### Sound

For laptop speakers and HDMI/DP is used Realtek HD Audio Controller.

`root #``lscpi -k -s 03:00.6`
03:00.6 Audio device \[0403\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h/19h HD Audio Controller \[1022:15e3\]
	Subsystem: Huawei Technologies Co., Ltd. Family 17h/19h HD Audio Controller \[19e5:3e18\]
	Kernel driver in use: snd\_hda\_intel
	Kernel modules: snd\_hda\_intel

`root #``lscpi -k -s 03:00.1`
03:00.1 Audio device \[0403\]: Advanced Micro Devices, Inc. \[AMD/ATI\] Raven/Raven2/Fenghuang HDMI/DP Audio Controller \[1002:15de\]
	Subsystem: Huawei Technologies Co., Ltd. Raven/Raven2/Fenghuang HDMI/DP Audio Controller \[19e5:3e18\]
	Kernel driver in use: snd\_hda\_intel
	Kernel modules: snd\_hda\_intel

### lspci

`root #``lspci -nnk`
00:00.0 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Root Complex \[1022:15d0\]
	Subsystem: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Root Complex \[1022:15d0\]
00:00.2 IOMMU \[0806\]: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 IOMMU \[1022:15d1\]
	Subsystem: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 IOMMU \[1022:15d1\]
00:01.0 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h (Models 00h-1fh) PCIe Dummy Host Bridge \[1022:1452\]
00:01.3 PCI bridge \[0604\]: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 PCIe GPP Bridge \[6:0\] \[1022:15d3\]
	Subsystem: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 PCIe GPP Bridge \[6:0\] \[1022:1453\]
	Kernel driver in use: pcieport
00:01.7 PCI bridge \[0604\]: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 PCIe GPP Bridge \[6:0\] \[1022:15d3\]
	Subsystem: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 PCIe GPP Bridge \[6:0\] \[1022:1453\]
	Kernel driver in use: pcieport
00:08.0 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h (Models 00h-1fh) PCIe Dummy Host Bridge \[1022:1452\]
00:08.1 PCI bridge \[0604\]: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Internal PCIe GPP Bridge 0 to Bus A \[1022:15db\]
	Subsystem: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Internal PCIe GPP Bridge 0 to Bus A \[1022:0000\]
	Kernel driver in use: pcieport
00:14.0 SMBus \[0c05\]: Advanced Micro Devices, Inc. \[AMD\] FCH SMBus Controller \[1022:790b\] (rev 61)
	Subsystem: Huawei Technologies Co., Ltd. FCH SMBus Controller \[19e5:3e18\]
	Kernel driver in use: piix4\_smbus
	Kernel modules: i2c\_piix4, sp5100\_tco
00:14.3 ISA bridge \[0601\]: Advanced Micro Devices, Inc. \[AMD\] FCH LPC Bridge \[1022:790e\] (rev 51)
	Subsystem: Huawei Technologies Co., Ltd. FCH LPC Bridge \[19e5:3e18\]
00:18.0 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Device 24: Function 0 \[1022:15e8\]
00:18.1 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Device 24: Function 1 \[1022:15e9\]
00:18.2 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Device 24: Function 2 \[1022:15ea\]
00:18.3 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Device 24: Function 3 \[1022:15eb\]
	Kernel driver in use: k10temp
	Kernel modules: k10temp
00:18.4 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Device 24: Function 4 \[1022:15ec\]
00:18.5 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Device 24: Function 5 \[1022:15ed\]
00:18.6 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Device 24: Function 6 \[1022:15ee\]
00:18.7 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Device 24: Function 7 \[1022:15ef\]
01:00.0 Non-Volatile memory controller \[0108\]: Samsung Electronics Co Ltd NVMe SSD Controller SM981/PM981/PM983 \[144d:a808\]
	Subsystem: Samsung Electronics Co Ltd NVMe SSD Controller SM981/PM981/PM983 (SSD 970 EVO) \[144d:a801\]
	Kernel driver in use: nvme
	Kernel modules: nvme
02:00.0 Network controller \[0280\]: Qualcomm Atheros AR9462 Wireless Network Adapter \[168c:0034\] (rev 01)
	Subsystem: AzureWave AR9462 Wireless Network Adapter \[1a3b:2234\]
	Kernel driver in use: ath9k
	Kernel modules: ath9k
03:00.0 VGA compatible controller \[0300\]: Advanced Micro Devices, Inc. \[AMD/ATI\] Picasso/Raven 2 \[Radeon Vega Series / Radeon Vega Mobile Series\] \[1002:15d8\] (rev c2)
	Subsystem: Huawei Technologies Co., Ltd. Picasso/Raven 2 \[Radeon Vega Series / Radeon Vega Mobile Series\] \[19e5:3e18\]
	Kernel driver in use: amdgpu
	Kernel modules: amdgpu
03:00.1 Audio device \[0403\]: Advanced Micro Devices, Inc. \[AMD/ATI\] Raven/Raven2/Fenghuang HDMI/DP Audio Controller \[1002:15de\]
	Subsystem: Huawei Technologies Co., Ltd. Raven/Raven2/Fenghuang HDMI/DP Audio Controller \[19e5:3e18\]
	Kernel driver in use: snd\_hda\_intel
	Kernel modules: snd\_hda\_intel
03:00.2 Encryption controller \[1080\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h (Models 10h-1fh) Platform Security Processor \[1022:15df\]
	Subsystem: Huawei Technologies Co., Ltd. Family 17h (Models 10h-1fh) Platform Security Processor \[19e5:3e18\]
	Kernel driver in use: ccp
	Kernel modules: ccp
03:00.3 USB controller \[0c03\]: Advanced Micro Devices, Inc. \[AMD\] Raven USB 3.1 \[1022:15e0\]
	Subsystem: Huawei Technologies Co., Ltd. Raven USB 3.1 \[19e5:3e18\]
	Kernel driver in use: xhci\_hcd
	Kernel modules: xhci\_pci
03:00.4 USB controller \[0c03\]: Advanced Micro Devices, Inc. \[AMD\] Raven USB 3.1 \[1022:15e1\]
	Subsystem: Huawei Technologies Co., Ltd. Raven USB 3.1 \[19e5:3e18\]
	Kernel driver in use: xhci\_hcd
	Kernel modules: xhci\_pci
03:00.5 Multimedia controller \[0480\]: Advanced Micro Devices, Inc. \[AMD\] ACP/ACP3X/ACP6x Audio Coprocessor \[1022:15e2\]
	Subsystem: Huawei Technologies Co., Ltd. ACP/ACP3X/ACP6x Audio Coprocessor \[19e5:3e18\]
	Kernel driver in use: snd\_pci\_acp3x
	Kernel modules: snd\_pci\_acp3x, snd\_rn\_pci\_acp3x
03:00.6 Audio device \[0403\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h/19h HD Audio Controller \[1022:15e3\]
	Subsystem: Huawei Technologies Co., Ltd. Family 17h/19h HD Audio Controller \[19e5:3e18\]
	Kernel driver in use: snd\_hda\_intel
	Kernel modules: snd\_hda\_intel

### lsusb

`user $``lsusb`
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 002: ID 05e3:0610 Genesys Logic, Inc. Hub
Bus 001 Device 003: ID 27c6:5110 Shenzhen Goodix Technology Co.,Ltd. Goodix Fingerprint Device 
Bus 001 Device 004: ID 13d3:56f9 IMC Networks ov9734\_azurewave\_camera
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub

## Blobs

### AMDGPU

Firmware blobs required for GPU are:

`user $``echo amdgpu/*`
amdgpu/picasso\_asd.bin amdgpu/picasso\_ce.bin amdgpu/picasso\_gpu\_info.bin amdgpu/picasso\_me.bin amdgpu/picasso\_mec2.bin amdgpu/picasso\_mec.bin amdgpu/picasso\_pfp.bin amdgpu/picasso\_rlc.bin amdgpu/picasso\_sdma.bin amdgpu/picasso\_ta.bin amdgpu/picasso\_vcn.bin amdgpu/raven\_dmcu.bin

## Kernel

### Graphics

For Radeon Vega 8 Graphics or Radeon Vega 10 Graphics:

### Sound

The motherboard uses Realtek HD Audio.

### Wireless

The motheboard does not have whitelist for Mini PCIe wireless card.

For Qualcomm Atheros AR9462 (the original Mini PCIe wireless card was replaced by a blobless wireless card):

### Keyboard

The keyboard is connected as AT keyboard.

### Touchpad

The touchpad is an ELAN 2204 model, it is connected by i2c protocol.
