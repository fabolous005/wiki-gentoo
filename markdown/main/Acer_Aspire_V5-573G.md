<!-- source: https://wiki.gentoo.org/wiki/Acer_Aspire_V5-573G | group: Gentoo Wiki (Main) | wiki-title: Acer Aspire V5-573G -->
---
title: Acer Aspire V5-573G
url: https://wiki.gentoo.org/wiki/Acer_Aspire_V5-573G
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "6f5e1dd6efb2126b"
license: CC BY-SA 4.0
---

# Acer Aspire V5-573G

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

![](https://wiki.gentoo.org/images/thumb/7/70/Acer_V5-573G.png/300px-Acer_V5-573G.png)

The **Acer Aspire V5-573G** is a laptop with a 15.7" screen, hybrid graphics, and DDR3 PC3L-12800 (1600MHz) 1.35V SO-DIMM up to 12 GB.

## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | Intel® Core™ i5-4200U | Works | N/A | N/A | N/A |  | 
| iGPU | Intel Corporation Haswell-ULT Integrated Graphics Controller (rev 09) | Works | N/A | i915 | N/A |  | 
| dGPU | NVIDIA Corporation GK107M \[GeForce GT 750M\] (rev ff) | Works | N/A | nouveau, nvidia | N/A |  | 
| Ethernet | Realtek Semiconductor Co., Ltd. RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller (rev 14) | Works | N/A | r8169 | N/A |  | 
| Wi-Fi | Qualcomm Atheros AR9462 Wireless Network Adapter (rev 01) | Works | N/A | ath9k | N/A |  | 
| Bluetooth | Qualcomm Atheros AR9462 Wireless Network Adapter (rev 01) | Works | N/A | ath3k | N/A |  | 
| Sound | Intel Corporation Haswell-ULT HD Audio Controller (rev 09) | Works | N/A | snd\_hda\_intel | N/A |  | 
| Webcam | N/A | Works | N/A | uvcvideo | N/A |  | 

### Detailed information

`user $``cat /proc/cpuinfo`
processor	: 0
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 69
model name	: Intel(R) Core(TM) i5-4200U CPU @ 1.60GHz
stepping	: 1
microcode	: 0x17
cpu MHz		: 1526.714
cache size	: 3072 KB
physical id	: 0
siblings	: 4
core id		: 0
cpu cores	: 2
apicid		: 0
initial apicid	: 0
fpu		: yes
fpu\_exception	: yes
cpuid level	: 13
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc aperfmperf eagerfpu pni pclmulqdq dtes64 monitor ds\_cpl vmx est tm2 ssse3 fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm ida arat epb xsaveopt pln pts dtherm tpr\_shadow vnmi flexpriority ept vpid fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid
bogomips	: 4589.50
clflush size	: 64
cache\_alignment	: 64
address sizes	: 39 bits physical, 48 bits virtual
power management:
processor	: 1
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 69
model name	: Intel(R) Core(TM) i5-4200U CPU @ 1.60GHz
stepping	: 1
microcode	: 0x17
cpu MHz		: 1597.781
cache size	: 3072 KB
physical id	: 0
siblings	: 4
core id		: 0
cpu cores	: 2
apicid		: 1
initial apicid	: 1
fpu		: yes
fpu\_exception	: yes
cpuid level	: 13
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc aperfmperf eagerfpu pni pclmulqdq dtes64 monitor ds\_cpl vmx est tm2 ssse3 fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm ida arat epb xsaveopt pln pts dtherm tpr\_shadow vnmi flexpriority ept vpid fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid
bogomips	: 4589.50
clflush size	: 64
cache\_alignment	: 64
address sizes	: 39 bits physical, 48 bits virtual
power management:
processor	: 2
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 69
model name	: Intel(R) Core(TM) i5-4200U CPU @ 1.60GHz
stepping	: 1
microcode	: 0x17
cpu MHz		: 1486.375
cache size	: 3072 KB
physical id	: 0
siblings	: 4
core id		: 1
cpu cores	: 2
apicid		: 2
initial apicid	: 2
fpu		: yes
fpu\_exception	: yes
cpuid level	: 13
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc aperfmperf eagerfpu pni pclmulqdq dtes64 monitor ds\_cpl vmx est tm2 ssse3 fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm ida arat epb xsaveopt pln pts dtherm tpr\_shadow vnmi flexpriority ept vpid fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid
bogomips	: 4589.50
clflush size	: 64
cache\_alignment	: 64
address sizes	: 39 bits physical, 48 bits virtual
power management:
processor	: 3
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 69
model name	: Intel(R) Core(TM) i5-4200U CPU @ 1.60GHz
stepping	: 1
microcode	: 0x17
cpu MHz		: 1599.937
cache size	: 3072 KB
physical id	: 0
siblings	: 4
core id		: 1
cpu cores	: 2
apicid		: 3
initial apicid	: 3
fpu		: yes
fpu\_exception	: yes
cpuid level	: 13
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc aperfmperf eagerfpu pni pclmulqdq dtes64 monitor ds\_cpl vmx est tm2 ssse3 fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm ida arat epb xsaveopt pln pts dtherm tpr\_shadow vnmi flexpriority ept vpid fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid
bogomips	: 4589.50
clflush size	: 64
cache\_alignment	: 64
address sizes	: 39 bits physical, 48 bits virtual
power management:

`user $``lscpu`
Architecture:          x86\_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                4
On-line CPU(s) list:   0-3
Thread(s) per core:    2
Core(s) per socket:    2
Socket(s):             1
Vendor ID:             GenuineIntel
CPU family:            6
Model:                 69
Model name:            Intel(R) Core(TM) i5-4200U CPU @ 1.60GHz
Stepping:              1
CPU MHz:               1600.386
CPU max MHz:           2600.0000
CPU min MHz:           800.0000
BogoMIPS:              4589.50
Virtualization:        VT-x
L1d cache:             32K
L1i cache:             32K
L2 cache:              256K
L3 cache:              3072K

`root #``lspci -k (partial output, obtained by merging)`
00:00.0 Host bridge: Intel Corporation Haswell-ULT DRAM Controller (rev 09)
00:02.0 VGA compatible controller: Intel Corporation Haswell-ULT Integrated Graphics Controller (rev 09)
	Subsystem: Acer Incorporated \[ALI\] Device 079b
	Kernel driver in use: i915
	Kernel modules: i915
00:03.0 Audio device: Intel Corporation Haswell-ULT HD Audio Controller (rev 09)
	Subsystem: Acer Incorporated \[ALI\] Device 079b
	Kernel driver in use: snd\_hda\_intel
	Kernel modules: snd\_hda\_intel
00:14.0 USB controller: Intel Corporation 8 Series USB xHCI HC (rev 04)
	Subsystem: Acer Incorporated \[ALI\] Device 079b
	Kernel driver in use: xhci\_hcd
	Kernel modules: xhci\_hcd
00:16.0 Communication controller: Intel Corporation 8 Series HECI #0 (rev 04)
00:1b.0 Audio device: Intel Corporation 8 Series HD Audio Controller (rev 04)
	Subsystem: Acer Incorporated \[ALI\] Device 079b
	Kernel driver in use: snd\_hda\_intel
	Kernel modules: snd\_hda\_intel
00:1c.0 PCI bridge: Intel Corporation 8 Series PCI Express Root Port 1 (rev e4)
00:1c.2 PCI bridge: Intel Corporation 8 Series PCI Express Root Port 3 (rev e4)
00:1c.3 PCI bridge: Intel Corporation 8 Series PCI Express Root Port 4 (rev e4)
00:1c.4 PCI bridge: Intel Corporation 8 Series PCI Express Root Port 5 (rev e4)
00:1d.0 USB controller: Intel Corporation 8 Series USB EHCI #1 (rev 04)
	Subsystem: Acer Incorporated \[ALI\] Device 079b
	Kernel driver in use: ehci-pci
	Kernel modules: ehci\_pci
00:1f.0 ISA bridge: Intel Corporation 8 Series LPC Controller (rev 04)
00:1f.2 SATA controller: Intel Corporation 8 Series SATA Controller 1 \[AHCI mode\] (rev 04)
	Subsystem: Acer Incorporated \[ALI\] Device 079b
	Kernel driver in use: ahci
	Kernel modules: ahci
00:1f.3 SMBus: Intel Corporation 8 Series SMBus Controller (rev 04)
01:00.0 3D controller: NVIDIA Corporation GK107M \[GeForce GT 750M\] (rev ff)
	Kernel modules: nouveau, nvidia
04:00.0 Network controller: Qualcomm Atheros AR9462 Wireless Network Adapter (rev 01)
	Subsystem: Foxconn International, Inc. Device e052
	Kernel driver in use: ath9k
	Kernel modules: ath9k
05:00.0 Unassigned class \[ff00\]: Realtek Semiconductor Co., Ltd. Device 5287 (rev 01)
05:00.1 Ethernet controller: Realtek Semiconductor Co., Ltd. RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller (rev 14)
	Subsystem: Acer Incorporated \[ALI\] Device 079b
	Kernel driver in use: r8169
	Kernel modules: r8169

`root #``grep ath9k /proc/modules`
ath9k 87394 0 - Live 0xffffffffa0ca5000
ath9k\_common 13706 1 ath9k, Live 0xffffffffa0c9c000
ath9k\_hw 392489 2 ath9k,ath9k\_common, Live 0xffffffffa0c2c000
ath 19291 3 ath9k,ath9k\_common,ath9k\_hw, Live 0xffffffffa0c23000
mac80211 495382 1 ath9k, Live 0xffffffffa0b83000
cfg80211 429729 4 ath9k,ath9k\_common,ath,mac80211, Live 0xffffffffa0aee000

`root #``grep ath3k /proc/modules`
ath3k 7973 0 - Live 0xffffffffa0ef1000
bluetooth 367977 24 bnep,ath3k,btusb, Live 0xffffffffa0ddb000
usbcore 191192 7 ath3k,uvcvideo,btusb,usbhid,ehci\_pci,xhci\_hcd,ehci\_hcd, Live 0xffffffffa0199000

`root #``grep uvc /proc/modules`
uvcvideo 72616 0 - Live 0xffffffffa0ed5000
videobuf2\_vmalloc 3304 1 uvcvideo, Live 0xffffffffa0ed1000
videobuf2\_core 39435 1 uvcvideo, Live 0xffffffffa0ebc000
videodev 122795 3 uvcvideo,videobuf2\_core,v4l2\_common, Live 0xffffffffa0e8a000
media 12229 2 uvcvideo,videodev, Live 0xffffffffa0e82000
usbcore 191192 7 ath3k,uvcvideo,btusb,usbhid,ehci\_pci,xhci\_hcd,ehci\_hcd, Live 0xffffffffa0199000
