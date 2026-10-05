<!-- source: https://wiki.gentoo.org/wiki/PCSpecialist_Optimus_XI | group: Gentoo Wiki (Main) | wiki-title: PCSpecialist Optimus XI -->
---
title: PCSpecialist Optimus XI
url: https://wiki.gentoo.org/wiki/PCSpecialist_Optimus_XI
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "7f58af6e5b2ab169"
license: CC BY-SA 4.0
---

# PCSpecialist Optimus XI

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Hardware information

### lscpu

`root #``lscpu`
Architecture:                    x86\_64
CPU op-mode(s):                  32-bit, 64-bit
Byte Order:                      Little Endian
Address sizes:                   39 bits physical, 48 bits virtual
CPU(s):                          12
On-line CPU(s) list:             0-11
Thread(s) per core:              2
Core(s) per socket:              6
Socket(s):                       1
NUMA node(s):                    1
Vendor ID:                       GenuineIntel
CPU family:                      6
Model:                           165
Model name:                      Intel(R) Core(TM) i7-10750H CPU @ 2.60GHz
Stepping:                        2
CPU MHz:                         900.213
CPU max MHz:                     5000.0000
CPU min MHz:                     800.0000
BogoMIPS:                        5199.98
Virtualization:                  VT-x
L1d cache:                       192 KiB
L1i cache:                       192 KiB
L2 cache:                        1.5 MiB
L3 cache:                        12 MiB
NUMA node0 CPU(s):               0-11
Vulnerability Itlb multihit:     Processor vulnerable
Vulnerability L1tf:              Not affected
Vulnerability Mds:               Not affected
Vulnerability Meltdown:          Not affected
Vulnerability Spec store bypass: Mitigation; Speculative Store Bypass disabled via prctl and seccomp
Vulnerability Spectre v1:        Mitigation; usercopy/swapgs barriers and \_\_user pointer sanitization
Vulnerability Spectre v2:        Mitigation; Enhanced IBRS, IBPB conditional, RSB filling
Vulnerability Srbds:             Not affected
Vulnerability Tsx async abort:   Not affected
Flags:                           fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc art arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc cpuid aperfmperf pni pclmulqdq dtes64 monitor ds\_cpl vmx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm 3dnowprefetch cpuid\_fault epb invpcid\_single ssbd ibrs ibpb stibp ibrs\_enhanced tpr\_shadow vnmi flexpriority ept vpid ept\_ad fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid mpx rdseed adx smap clflushopt intel\_pt xsaveopt xsavec xgetbv1 xsaves dtherm ida arat pln pts hwp hwp\_notify hwp\_act\_window hwp\_epp pku ospke md\_clear flush\_l1d arch\_capabilities

### lspci

`root #``lspci -nnk`
00:00.0 Host bridge \[0600\]: Intel Corporation Device \[8086:9b54\] (rev 02)
	Subsystem: CLEVO/KAPOK Computer Device \[1558:8520\]
00:01.0 PCI bridge \[0604\]: Intel Corporation Xeon E3-1200 v5/E3-1500 v5/6th Gen Core Processor PCIe Controller (x16) \[8086:1901\] (rev 02)
	Kernel driver in use: pcieport
00:02.0 VGA compatible controller \[0300\]: Intel Corporation Device \[8086:9bc4\] (rev 05)
	Subsystem: CLEVO/KAPOK Computer Device \[1558:8520\]
	Kernel driver in use: i915
00:12.0 Signal processing controller \[1180\]: Intel Corporation Device \[8086:06f9\]
	Subsystem: CLEVO/KAPOK Computer Device \[1558:8520\]
00:14.0 USB controller \[0c03\]: Intel Corporation Device \[8086:06ed\]
	Subsystem: CLEVO/KAPOK Computer Device \[1558:8520\]
	Kernel driver in use: xhci\_hcd
00:14.2 RAM memory \[0500\]: Intel Corporation Device \[8086:06ef\]
	Subsystem: CLEVO/KAPOK Computer Device \[1558:8520\]
00:15.0 Serial bus controller \[0c80\]: Intel Corporation Device \[8086:06e8\]
	Subsystem: CLEVO/KAPOK Computer Device \[1558:8520\]
00:15.1 Serial bus controller \[0c80\]: Intel Corporation Device \[8086:06e9\]
	Subsystem: CLEVO/KAPOK Computer Device \[1558:8520\]
00:16.0 Communication controller \[0780\]: Intel Corporation Device \[8086:06e0\]
	Subsystem: CLEVO/KAPOK Computer Device \[1558:8520\]
00:17.0 SATA controller \[0106\]: Intel Corporation Device \[8086:06d3\]
	Subsystem: CLEVO/KAPOK Computer Device \[1558:8520\]
	Kernel driver in use: ahci
00:1d.0 PCI bridge \[0604\]: Intel Corporation Device \[8086:06b0\] (rev f0)
	Kernel driver in use: pcieport
00:1d.5 PCI bridge \[0604\]: Intel Corporation Device \[8086:06b5\] (rev f0)
	Kernel driver in use: pcieport
00:1d.6 PCI bridge \[0604\]: Intel Corporation Device \[8086:06b6\] (rev f0)
	Kernel driver in use: pcieport
00:1f.0 ISA bridge \[0601\]: Intel Corporation Device \[8086:068d\]
	Subsystem: CLEVO/KAPOK Computer Device \[1558:8520\]
00:1f.3 Audio device \[0403\]: Intel Corporation Device \[8086:06c8\]
	Subsystem: CLEVO/KAPOK Computer Device \[1558:8520\]
	Kernel driver in use: snd\_hda\_intel
00:1f.4 SMBus \[0c05\]: Intel Corporation Device \[8086:06a3\]
	Subsystem: CLEVO/KAPOK Computer Device \[1558:8520\]
00:1f.5 Serial bus controller \[0c80\]: Intel Corporation Device \[8086:06a4\]
	Subsystem: CLEVO/KAPOK Computer Device \[1558:8520\]
01:00.0 VGA compatible controller \[0300\]: NVIDIA Corporation TU116M \[GeForce GTX 1660 Ti Mobile\] \[10de:2191\] (rev a1)
	Subsystem: CLEVO/KAPOK Computer TU116M \[GeForce GTX 1660 Ti Mobile\] \[1558:8520\]
	Kernel driver in use: nvidia
	Kernel modules: nvidia\_drm, nvidia
01:00.1 Audio device \[0403\]: NVIDIA Corporation TU116 High Definition Audio Controller \[10de:1aeb\] (rev a1)
	Subsystem: NVIDIA Corporation TU116 High Definition Audio Controller \[10de:0000\]
	Kernel driver in use: snd\_hda\_intel
01:00.2 USB controller \[0c03\]: NVIDIA Corporation Device \[10de:1aec\] (rev a1)
	Subsystem: NVIDIA Corporation Device \[10de:0000\]
	Kernel driver in use: xhci\_hcd
01:00.3 Serial bus controller \[0c80\]: NVIDIA Corporation TU116 \[GeForce GTX 1650 SUPER\] \[10de:1aed\] (rev a1)
	Subsystem: NVIDIA Corporation TU116 \[GeForce GTX 1650 SUPER\] \[10de:0000\]
06:00.0 Non-Volatile memory controller \[0108\]: Realtek Semiconductor Co., Ltd. Device \[10ec:5763\] (rev 01)
	Subsystem: Realtek Semiconductor Co., Ltd. Device \[10ec:5763\]
	Kernel driver in use: nvme
07:00.0 Network controller \[0280\]: Intel Corporation Wi-Fi 6 AX200 \[8086:2723\] (rev 1a)
	Subsystem: Intel Corporation Wi-Fi 6 AX200 \[8086:0080\]
	Kernel driver in use: iwlwifi
	Kernel modules: iwlwifi
08:00.0 Unassigned class \[ff00\]: Realtek Semiconductor Co., Ltd. RTL8411B PCI Express Card Reader \[10ec:5287\] (rev 01)
	Subsystem: CLEVO/KAPOK Computer RTL8411B PCI Express Card Reader \[1558:8520\]
	Kernel driver in use: rtsx\_pci
08:00.1 Ethernet controller \[0200\]: Realtek Semiconductor Co., Ltd. RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller \[10ec:8168\] (rev 12)
	Subsystem: CLEVO/KAPOK Computer RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller \[1558:8520\]
	Kernel driver in use: r8169

### lsusb

`root #``lsusb`
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 001 Device 004: ID 5986:9102 Acer, Inc BisonCam,NB Pro
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 003: ID 8087:0029 Intel Corp. 
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

## Installation

### Kernel

#### EFI stub

The kernel can be [booted with EFI stub](https://wiki.gentoo.org/wiki/EFI_stub), however the path has to be /EFI/Boot/bootx64.efi. I haven't been able to register custom entries with [efibootmgr](https://wiki.gentoo.org/wiki/Efibootmgr) (the UEFI firmware seems to reset the entries).

#### AHCI

AHCI has to be enabled.

**Based on 5.4.48:**

#### SSD drive

If you have a SSD drive, enable [NVMe](https://wiki.gentoo.org/wiki/NVMe) support:

**Based on 5.4.48:**

#### Ethernet

Enable r8169:

**Based on 5.4.48:**

#### WiFi

The driver is [iwlwifi](https://wiki.gentoo.org/wiki/Iwlwifi):

**Based on 5.4.48:**

#### Sound

Enable snd\_hda\_intel:

**Based on 5.4.48:**

#### USB

**Based on 5.4.48:**

#### SD card

Enable rtsx\_pci:

**Based on 5.4.48:**

#### Webcam

**Based on 5.4.48:**

#### Keyboard backlight

First, enable WMI in the kernel:

**Based on 5.4.48:**

Then, install the kernel module [app-laptop/tuxedo-keyboard](https://packages.gentoo.org/packages/app-laptop/tuxedo-keyboard). This will make the keyboard backlight Fn keys work, and allow configuration via /sys/devices/platform/tuxedo\_keyboard.

### Optimus graphics

#### Proprietary NVIDIA driver

With the proprietary NVIDIA driver, one can use either [bumblebee](https://wiki.gentoo.org/wiki/NVIDIA/Bumblebee) or [PRIME render offload](https://download.nvidia.com/XFree86/Linux-x86_64/435.17/README/primerenderoffload.html).

For the bumblebee method, bbswitch has to be disabled, as it will freeze the system. To do so, set `PMMethod=none` in your /etc/bumblebee/bumblebee.conf. Note that this will disable powering off the card when unused; I haven't been able to make linux PM work.

For PRIME render offload, make sure that the `libglvnd` is enabled and `nvidia` is in VIDEO\_CARDS, and add `options nvidia_drm modeset=1` to /etc/modprobe.d/nvidia.conf. To enable power management in the NVIDIA driver, add `NVreg_DynamicPowerManagement=0x02` in the module options in /etc/modprobe.d/nvidia.conf. This is described in the [driver documentation](https://download.nvidia.com/XFree86/Linux-x86_64/435.17/README/dynamicpowermanagement.html).

#### Nouveau

Untested.
