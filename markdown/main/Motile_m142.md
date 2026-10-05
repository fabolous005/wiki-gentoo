<!-- source: https://wiki.gentoo.org/wiki/Motile_m142 | group: Gentoo Wiki (Main) | wiki-title: Motile m142 -->
---
title: Motile m142
url: https://wiki.gentoo.org/wiki/Motile_m142
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-02-27"
fingerprint: ff462cc1a33a05ad
license: CC BY-SA 4.0
---

# Motile m142

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Hardware

`root #``lspci`
00:00.0 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Root Complex
00:00.2 IOMMU: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 IOMMU
00:01.0 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Family 17h (Models 00h-1fh) PCIe Dummy Host Bridge
00:01.2 PCI bridge: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 PCIe GPP Bridge \[6:0\]
00:01.4 PCI bridge: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 PCIe GPP Bridge \[6:0\]
00:01.5 PCI bridge: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 PCIe GPP Bridge \[6:0\]
00:08.0 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Family 17h (Models 00h-1fh) PCIe Dummy Host Bridge
00:08.1 PCI bridge: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Internal PCIe GPP Bridge 0 to Bus A
00:08.2 PCI bridge: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Internal PCIe GPP Bridge 0 to Bus B
00:14.0 SMBus: Advanced Micro Devices, Inc. \[AMD\] FCH SMBus Controller (rev 61)
00:14.3 ISA bridge: Advanced Micro Devices, Inc. \[AMD\] FCH LPC Bridge (rev 51)
00:18.0 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Device 24: Function 0
00:18.1 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Device 24: Function 1
00:18.2 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Device 24: Function 2
00:18.3 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Device 24: Function 3
00:18.4 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Device 24: Function 4
00:18.5 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Device 24: Function 5
00:18.6 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Device 24: Function 6
00:18.7 Host bridge: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2 Device 24: Function 7
01:00.0 Non-Volatile memory controller: Micron/Crucial Technology Device 2263 (rev 03)
02:00.0 Ethernet controller: Realtek Semiconductor Co., Ltd. RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller (rev 15)
03:00.0 Network controller: Intel Corporation Dual Band Wireless-AC 3168NGW \[Stone Peak\] (rev 10)
04:00.0 VGA compatible controller: Advanced Micro Devices, Inc. \[AMD/ATI\] Picasso (rev c2)
04:00.1 Audio device: Advanced Micro Devices, Inc. \[AMD/ATI\] Raven/Raven2/Fenghuang HDMI/DP Audio Controller
04:00.2 Encryption controller: Advanced Micro Devices, Inc. \[AMD\] Family 17h (Models 10h-1fh) Platform Security Processor
04:00.3 USB controller: Advanced Micro Devices, Inc. \[AMD\] Raven USB 3.1
04:00.4 USB controller: Advanced Micro Devices, Inc. \[AMD\] Raven USB 3.1
04:00.6 Audio device: Advanced Micro Devices, Inc. \[AMD\] Family 17h (Models 10h-1fh) HD Audio Controller
05:00.0 SATA controller: Advanced Micro Devices, Inc. \[AMD\] FCH SATA Controller \[AHCI mode\] (rev 61)

### BIOS

### CPU

The CPU is an AMD Ryzen 5 3500U with four cores.

`user $``cat /proc/cpuinfo`
processor	: 0
vendor\_id	: AuthenticAMD
cpu family	: 23
model		: 24
model name	: AMD Ryzen 5 3500U with Radeon Vega Mobile Gfx
stepping	: 1
microcode	: 0x8108102
cpu MHz		: 1506.542
cache size	: 512 KB
physical id	: 0
siblings	: 8
core id		: 0
cpu cores	: 4
apicid		: 0
initial apicid	: 0
fpu		: yes
fpu\_exception	: yes
cpuid level	: 13
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx mmxext fxsr\_opt pdpe1gb rdtscp lm constant\_tsc rep\_good nopl nonstop\_tsc cpuid extd\_apicid aperfmperf pni pclmulqdq monitor ssse3 fma cx16 sse4\_1 sse4\_2 movbe popcnt aes xsave avx f16c rdrand lahf\_lm cmp\_legacy svm extapic cr8\_legacy abm sse4a misalignsse 3dnowprefetch osvw skinit wdt tce topoext perfctr\_core perfctr\_nb bpext perfctr\_llc mwaitx cpb hw\_pstate sme ssbd sev ibpb vmmcall fsgsbase bmi1 avx2 smep bmi2 rdseed adx smap clflushopt sha\_ni xsaveopt xsavec xgetbv1 xsaves clzero irperf xsaveerptr arat npt lbrv svm\_lock nrip\_save tsc\_scale vmcb\_clean flushbyasid decodeassists pausefilter pfthreshold avic v\_vmsave\_vmload vgif overflow\_recov succor smca
bugs		: sysret\_ss\_attrs null\_seg spectre\_v1 spectre\_v2 spec\_store\_bypass
bogomips	: 4191.95
TLB size	: 2560 4K pages
clflush size	: 64
cache\_alignment	: 64
address sizes	: 43 bits physical, 48 bits virtual
power management: ts ttp tm hwpstate eff\_freq\_ro \[13\] \[14\]

### Graphics

The CPU provides Radeon Vega Mobile graphics, which claims 2Gb of main memory for video. Follow the instructions in the [AMDGPU](https://wiki.gentoo.org/wiki/AMDGPU) article.

`root #``lspci -k -s 04:00.0````
04:00.0 VGA compatible controller: Advanced Micro Devices, Inc. [AMD/ATI] Picasso (rev c2)
        Subsystem: Tongfang Hongkong Limited Picasso
        Kernel driver in use: amdgpu
        Kernel modules: amdgpu
```
### Ethernet

### ACPI

### Wireless

`root #``lspci -k -s 03:00.0````
03:00.0 Network controller: Intel Corporation Dual Band Wireless-AC 3168NGW [Stone Peak] (rev 10)
        Subsystem: Intel Corporation Dual Band Wireless-AC 3168NGW [Stone Peak]
        Kernel driver in use: iwlwifi
        Kernel modules: iwlwifi
```
The AC3168NGW wifi works with kernel version 5.8 and 5.4. The disable wireless key (Fn-F4) works through uefi and does not require kernel support.

### Sound

The laptop features AMD/Realtek HD audio.

`root #``lspci -k -s 04:00.1````
04:00.1 Audio device: Advanced Micro Devices, Inc. [AMD/ATI] Raven/Raven2/Fenghuang HDMI/DP Audio Controller
        Subsystem: Tongfang Hongkong Limited Raven/Raven2/Fenghuang HDMI/DP Audio Controller
        Kernel driver in use: snd_hda_intel
```
Configure [ALSA](https://wiki.gentoo.org/wiki/ALSA) with the following driver settings:

### Touchpad

The laptop's touchpad will probably be a Tongfang model, similar to Elan touchpads, and may not be detected by system tools, including lspci.

To enable it, use the following kernel configuration settings:

### USB

### SATA, PCI

### NVME SSD

### Webcam

### Bluetooth

### Keyboard

Keyboard backlight adjustment is handled through uefi, and does not require kernel support.
