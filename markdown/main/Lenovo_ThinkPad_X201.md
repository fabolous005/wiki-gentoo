<!-- source: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_X201 | group: Gentoo Wiki (Main) | wiki-title: Lenovo ThinkPad X201 -->
---
title: Lenovo ThinkPad X201
url: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_X201
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-20"
fingerprint: "6f5e0d35c33e4a6a"
license: CC BY-SA 4.0
---

# Lenovo ThinkPad X201

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

### Configuration

FILE **`/etc/portage/make.conf`**

FILE **`/etc/portage/package.use/00input`**

```
 INPUT_DEVICES: evdev synaptics
```
FILE **`/etc/portage/package.use/00video`**

```
 VIDEO_CARDS: -* intel i965
```
## Graphics

KERNEL

## Hardware details

`root #``lspci -nn`
00:00.0 Host bridge \[0600\]: Intel Corporation Core Processor DRAM Controller \[8086:0044\] (rev 02)
00:02.0 VGA compatible controller \[0300\]: Intel Corporation Core Processor Integrated Graphics Controller \[8086:0046\] (rev 02)
00:16.0 Communication controller \[0780\]: Intel Corporation 5 Series/3400 Series Chipset HECI Controller \[8086:3b64\] (rev 06)
00:16.3 Serial controller \[0700\]: Intel Corporation 5 Series/3400 Series Chipset KT Controller \[8086:3b67\] (rev 06)
00:19.0 Ethernet controller \[0200\]: Intel Corporation 82577LM Gigabit Network Connection \[8086:10ea\] (rev 06)
00:1a.0 USB controller \[0c03\]: Intel Corporation 5 Series/3400 Series Chipset USB2 Enhanced Host Controller \[8086:3b3c\] (rev 06)
00:1b.0 Audio device \[0403\]: Intel Corporation 5 Series/3400 Series Chipset High Definition Audio \[8086:3b56\] (rev 06)
00:1c.0 PCI bridge \[0604\]: Intel Corporation 5 Series/3400 Series Chipset PCI Express Root Port 1 \[8086:3b42\] (rev 06)
00:1c.3 PCI bridge \[0604\]: Intel Corporation 5 Series/3400 Series Chipset PCI Express Root Port 4 \[8086:3b48\] (rev 06)
00:1c.4 PCI bridge \[0604\]: Intel Corporation 5 Series/3400 Series Chipset PCI Express Root Port 5 \[8086:3b4a\] (rev 06)
00:1d.0 USB controller \[0c03\]: Intel Corporation 5 Series/3400 Series Chipset USB2 Enhanced Host Controller \[8086:3b34\] (rev 06)
00:1e.0 PCI bridge \[0604\]: Intel Corporation 82801 Mobile PCI Bridge \[8086:2448\] (rev a6)
00:1f.0 ISA bridge \[0601\]: Intel Corporation Mobile 5 Series Chipset LPC Interface Controller \[8086:3b07\] (rev 06)
00:1f.2 SATA controller \[0106\]: Intel Corporation 5 Series/3400 Series Chipset 6 port SATA AHCI Controller \[8086:3b2f\] (rev 06)
00:1f.3 SMBus \[0c05\]: Intel Corporation 5 Series/3400 Series Chipset SMBus Controller \[8086:3b30\] (rev 06)
00:1f.6 Signal processing controller \[1180\]: Intel Corporation 5 Series/3400 Series Chipset Thermal Subsystem \[8086:3b32\] (rev 06)
02:00.0 Network controller \[0280\]: Intel Corporation Centrino Ultimate-N 6300 \[8086:4238\] (rev 35)
ff:00.0 Host bridge \[0600\]: Intel Corporation Core Processor QuickPath Architecture Generic Non-core Registers \[8086:2c62\] (rev 02)
ff:00.1 Host bridge \[0600\]: Intel Corporation Core Processor QuickPath Architecture System Address Decoder \[8086:2d01\] (rev 02)
ff:02.0 Host bridge \[0600\]: Intel Corporation Core Processor QPI Link 0 \[8086:2d10\] (rev 02)
ff:02.1 Host bridge \[0600\]: Intel Corporation Core Processor QPI Physical 0 \[8086:2d11\] (rev 02)
ff:02.2 Host bridge \[0600\]: Intel Corporation Core Processor Reserved \[8086:2d12\] (rev 02)
ff:02.3 Host bridge \[0600\]: Intel Corporation Core Processor Reserved \[8086:2d13\] (rev 02)

`root #``lsusb`
Bus 001 Device 002: ID 8087:0020 Intel Corp. Integrated Rate Matching Hub
Bus 002 Device 002: ID 8087:0020 Intel Corp. Integrated Rate Matching Hub
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 003: ID 147e:2016 Upek Biometric Touchchip/Touchstrip Fingerprint Sensor

`root #``cat /proc/cpuinfo`
processor	: 0
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 37
model name	: Intel(R) Core(TM) i5 CPU       M 520  @ 2.40GHz
stepping	: 5
microcode	: 0x3
cpu MHz		: 2400.000
cache size	: 3072 KB
physical id	: 0
siblings	: 4
core id		: 0
cpu cores	: 2
apicid		: 0
initial apicid	: 0
fpu		: yes
fpu\_exception	: yes
cpuid level	: 11
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx rdtscp lm constant\_tsc arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc aperfmperf pni pclmulqdq dtes64 monitor ds\_cpl vmx smx est tm2 ssse3 cx16 xtpr pdcm pcid sse4\_1 sse4\_2 popcnt aes lahf\_lm ida arat dtherm tpr\_shadow vnmi flexpriority ept vpid
bogomips	: 4788.10
clflush size	: 64
cache\_alignment	: 64
address sizes	: 36 bits physical, 48 bits virtual
power management:
processor	: 1
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 37
model name	: Intel(R) Core(TM) i5 CPU       M 520  @ 2.40GHz
stepping	: 5
microcode	: 0x3
cpu MHz		: 2400.000
cache size	: 3072 KB
physical id	: 0
siblings	: 4
core id		: 0
cpu cores	: 2
apicid		: 1
initial apicid	: 1
fpu		: yes
fpu\_exception	: yes
cpuid level	: 11
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx rdtscp lm constant\_tsc arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc aperfmperf pni pclmulqdq dtes64 monitor ds\_cpl vmx smx est tm2 ssse3 cx16 xtpr pdcm pcid sse4\_1 sse4\_2 popcnt aes lahf\_lm ida arat dtherm tpr\_shadow vnmi flexpriority ept vpid
bogomips	: 4788.10
clflush size	: 64
cache\_alignment	: 64
address sizes	: 36 bits physical, 48 bits virtual
power management:
processor	: 2
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 37
model name	: Intel(R) Core(TM) i5 CPU       M 520  @ 2.40GHz
stepping	: 5
microcode	: 0x3
cpu MHz		: 2400.000
cache size	: 3072 KB
physical id	: 0
siblings	: 4
core id		: 2
cpu cores	: 2
apicid		: 4
initial apicid	: 4
fpu		: yes
fpu\_exception	: yes
cpuid level	: 11
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx rdtscp lm constant\_tsc arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc aperfmperf pni pclmulqdq dtes64 monitor ds\_cpl vmx smx est tm2 ssse3 cx16 xtpr pdcm pcid sse4\_1 sse4\_2 popcnt aes lahf\_lm ida arat dtherm tpr\_shadow vnmi flexpriority ept vpid
bogomips	: 4788.10
clflush size	: 64
cache\_alignment	: 64
address sizes	: 36 bits physical, 48 bits virtual
power management:
processor	: 3
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 37
model name	: Intel(R) Core(TM) i5 CPU       M 520  @ 2.40GHz
stepping	: 5
microcode	: 0x3
cpu MHz		: 2400.000
cache size	: 3072 KB
physical id	: 0
siblings	: 4
core id		: 2
cpu cores	: 2
apicid		: 5
initial apicid	: 5
fpu		: yes
fpu\_exception	: yes
cpuid level	: 11
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx rdtscp lm constant\_tsc arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc aperfmperf pni pclmulqdq dtes64 monitor ds\_cpl vmx smx est tm2 ssse3 cx16 xtpr pdcm pcid sse4\_1 sse4\_2 popcnt aes lahf\_lm ida arat dtherm tpr\_shadow vnmi flexpriority ept vpid
bogomips	: 4788.10
clflush size	: 64
cache\_alignment	: 64
address sizes	: 36 bits physical, 48 bits virtual
power management:
