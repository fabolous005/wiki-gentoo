<!-- source: https://wiki.gentoo.org/wiki/Xiaomi_RedmiBook_14_Pro_2024 | group: Gentoo Wiki (Main) | wiki-title: Xiaomi RedmiBook 14 Pro 2024 -->
---
title: Xiaomi RedmiBook 14 Pro 2024
url: https://wiki.gentoo.org/wiki/Xiaomi_RedmiBook_14_Pro_2024
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-03-16"
fingerprint: "6ffe6c3cc11e5b69"
license: CC BY-SA 4.0
---

# Xiaomi RedmiBook 14 Pro 2024

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Laptop specifications

- Intel Core Ultra 5-125H or Intel Core Ultra 7-155H
- Intel AI Boost NPU 1.4 GHz
- 16-32GB LPDDR5x 7467MT/s single-channel
- Intel Arc (on-CPU)
- 14in Super Retina display 2880x1800
- 512-GB or 1TB SSD PCle 4.0 2242
- 80Wh battery with Thunderbolt 4 Type-C 100W
- x2 USB-A 3.2 Gen1, x1 USB-C, x1 Thunderbolt 4, x1 HDMI 2.1 TMDS, x1 3.5mm Jack

Printout of lspci:

`root #``lspci`
00:00.0 Host bridge: Intel Corporation Device 7d01 (rev 04)
00:02.0 VGA compatible controller: Intel Corporation Meteor Lake-P \[Intel Arc Graphics\] (rev 08)
00:04.0 Signal processing controller: Intel Corporation Meteor Lake-P Dynamic Tuning Technology (rev 04)
00:06.0 PCI bridge: Intel Corporation Device 7e4d (rev 20)
00:07.0 PCI bridge: Intel Corporation Meteor Lake-P Thunderbolt 4 PCI Express Root Port #1 (rev 10)
00:08.0 System peripheral: Intel Corporation Meteor Lake-P Gaussian & Neural-Network Accelerator (rev 20)
00:0a.0 Signal processing controller: Intel Corporation Meteor Lake-P Platform Monitoring Technology (rev 01)
00:0b.0 Processing accelerators: Intel Corporation Meteor Lake NPU (rev 04)
00:0d.0 USB controller: Intel Corporation Meteor Lake-P Thunderbolt 4 USB Controller (rev 10)
00:0d.2 USB controller: Intel Corporation Meteor Lake-P Thunderbolt 4 NHI #0 (rev 10)
00:12.0 Serial controller: Intel Corporation Meteor Lake-P Integrated Sensor Hub (rev 20)
00:14.0 USB controller: Intel Corporation Meteor Lake-P USB 3.2 Gen 2x1 xHCI Host Controller (rev 20)
00:14.2 RAM memory: Intel Corporation Device 7e7f (rev 20)
00:14.3 Network controller: Intel Corporation Meteor Lake PCH CNVi WiFi (rev 20)
00:15.0 Serial bus controller: Intel Corporation Meteor Lake-P Serial IO I2C Controller #0 (rev 20)
00:16.0 Communication controller: Intel Corporation Meteor Lake-P CSME HECI #1 (rev 20)
00:1f.0 ISA bridge: Intel Corporation Device 7e02 (rev 20)
00:1f.3 Multimedia audio controller: Intel Corporation Meteor Lake-P HD Audio Controller (rev 20)
00:1f.4 SMBus: Intel Corporation Meteor Lake-P SMBus Controller (rev 20)
00:1f.5 Serial bus controller: Intel Corporation Meteor Lake-P SPI Controller (rev 20)
01:00.0 Non-Volatile memory controller: Yangtze Memory Technologies Co.,Ltd PC300 NVMe SSD (DRAM-less) (rev 03)

Information from /proc/cpuinfo:

`root #``cat /proc/cpuinfo`
processor	: 0
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 170
model name	: Intel(R) Core(TM) Ultra 7 155H
stepping	: 4
microcode	: 0x17
cpu MHz		: 2800.000
cache size	: 24576 KB
physical id	: 0
siblings	: 22
core id		: 16
cpu cores	: 16
apicid		: 32
initial apicid	: 32
fpu		: yes
fpu\_exception	: yes
cpuid level	: 35
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc art arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc cpuid aperfmperf tsc\_known\_freq pni pclmulqdq dtes64 monitor ds\_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm 3dnowprefetch cpuid\_fault epb ssbd ibrs ibpb stibp ibrs\_enhanced tpr\_shadow flexpriority ept vpid ept\_ad fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid rdseed adx smap clflushopt clwb intel\_pt sha\_ni xsaveopt xsavec xgetbv1 xsaves split\_lock\_detect user\_shstk avx\_vnni dtherm ida arat pln pts hwp hwp\_notify hwp\_act\_window hwp\_epp hwp\_pkg\_req hfi vnmi umip pku ospke waitpkg gfni vaes vpclmulqdq rdpid bus\_lock\_detect movdiri movdir64b fsrm md\_clear serialize arch\_lbr ibt flush\_l1d arch\_capabilities
vmx flags	: vnmi preemption\_timer posted\_intr invvpid ept\_x\_only ept\_ad ept\_1gb flexpriority apicv tsc\_offset vtpr mtf vapic ept vpid unrestricted\_guest vapic\_reg vid ple shadow\_vmcs pml ept\_violation\_ve ept\_mode\_based\_exec tsc\_scaling usr\_wait\_pause notify\_vm\_exiting
bugs		: spectre\_v1 spectre\_v2 spec\_store\_bypass swapgs bhi
bogomips	: 5990.40
clflush size	: 64
cache\_alignment	: 64
address sizes	: 46 bits physical, 48 bits virtual
power management:
processor	: 1
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 170
model name	: Intel(R) Core(TM) Ultra 7 155H
stepping	: 4
microcode	: 0x17
cpu MHz		: 3000.030
cache size	: 24576 KB
physical id	: 0
siblings	: 22
core id		: 8
cpu cores	: 16
apicid		: 16
initial apicid	: 16
fpu		: yes
fpu\_exception	: yes
cpuid level	: 35
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc art arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc cpuid aperfmperf tsc\_known\_freq pni pclmulqdq dtes64 monitor ds\_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm 3dnowprefetch cpuid\_fault epb intel\_ppin ssbd ibrs ibpb stibp ibrs\_enhanced tpr\_shadow flexpriority ept vpid ept\_ad fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid rdseed adx smap clflushopt clwb intel\_pt sha\_ni xsaveopt xsavec xgetbv1 xsaves split\_lock\_detect user\_shstk avx\_vnni dtherm ida arat pln pts hwp hwp\_notify hwp\_act\_window hwp\_epp hwp\_pkg\_req hfi vnmi umip pku ospke waitpkg gfni vaes vpclmulqdq rdpid bus\_lock\_detect movdiri movdir64b fsrm md\_clear serialize arch\_lbr ibt flush\_l1d arch\_capabilities
vmx flags	: vnmi preemption\_timer posted\_intr invvpid ept\_x\_only ept\_ad ept\_1gb flexpriority apicv tsc\_offset vtpr mtf vapic ept vpid unrestricted\_guest vapic\_reg vid ple shadow\_vmcs pml ept\_violation\_ve ept\_mode\_based\_exec tsc\_scaling usr\_wait\_pause notify\_vm\_exiting
bugs		: spectre\_v1 spectre\_v2 spec\_store\_bypass swapgs bhi
bogomips	: 5990.40
clflush size	: 64
cache\_alignment	: 64
address sizes	: 46 bits physical, 48 bits virtual
power management:
processor	: 2
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 170
model name	: Intel(R) Core(TM) Ultra 7 155H
stepping	: 4
microcode	: 0x17
cpu MHz		: 3002.112
cache size	: 24576 KB
physical id	: 0
siblings	: 22
core id		: 8
cpu cores	: 16
apicid		: 17
initial apicid	: 17
fpu		: yes
fpu\_exception	: yes
cpuid level	: 35
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc art arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc cpuid aperfmperf tsc\_known\_freq pni pclmulqdq dtes64 monitor ds\_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm 3dnowprefetch cpuid\_fault epb intel\_ppin ssbd ibrs ibpb stibp ibrs\_enhanced tpr\_shadow flexpriority ept vpid ept\_ad fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid rdseed adx smap clflushopt clwb intel\_pt sha\_ni xsaveopt xsavec xgetbv1 xsaves split\_lock\_detect user\_shstk avx\_vnni dtherm ida arat pln pts hwp hwp\_notify hwp\_act\_window hwp\_epp hwp\_pkg\_req hfi vnmi umip pku ospke waitpkg gfni vaes vpclmulqdq rdpid bus\_lock\_detect movdiri movdir64b fsrm md\_clear serialize arch\_lbr ibt flush\_l1d arch\_capabilities
vmx flags	: vnmi preemption\_timer posted\_intr invvpid ept\_x\_only ept\_ad ept\_1gb flexpriority apicv tsc\_offset vtpr mtf vapic ept vpid unrestricted\_guest vapic\_reg vid ple shadow\_vmcs pml ept\_violation\_ve ept\_mode\_based\_exec tsc\_scaling usr\_wait\_pause notify\_vm\_exiting
bugs		: spectre\_v1 spectre\_v2 spec\_store\_bypass swapgs bhi
bogomips	: 5990.40
clflush size	: 64
cache\_alignment	: 64
address sizes	: 46 bits physical, 48 bits virtual
power management:
processor	: 3
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 170
model name	: Intel(R) Core(TM) Ultra 7 155H
stepping	: 4
microcode	: 0x17
cpu MHz		: 400.000
cache size	: 24576 KB
physical id	: 0
siblings	: 22
core id		: 12
cpu cores	: 16
apicid		: 24
initial apicid	: 24
fpu		: yes
fpu\_exception	: yes
cpuid level	: 35
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc art arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc cpuid aperfmperf tsc\_known\_freq pni pclmulqdq dtes64 monitor ds\_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm 3dnowprefetch cpuid\_fault epb intel\_ppin ssbd ibrs ibpb stibp ibrs\_enhanced tpr\_shadow flexpriority ept vpid ept\_ad fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid rdseed adx smap clflushopt clwb intel\_pt sha\_ni xsaveopt xsavec xgetbv1 xsaves split\_lock\_detect user\_shstk avx\_vnni dtherm ida arat pln pts hwp hwp\_notify hwp\_act\_window hwp\_epp hwp\_pkg\_req hfi vnmi umip pku ospke waitpkg gfni vaes vpclmulqdq rdpid bus\_lock\_detect movdiri movdir64b fsrm md\_clear serialize arch\_lbr ibt flush\_l1d arch\_capabilities
vmx flags	: vnmi preemption\_timer posted\_intr invvpid ept\_x\_only ept\_ad ept\_1gb flexpriority apicv tsc\_offset vtpr mtf vapic ept vpid unrestricted\_guest vapic\_reg vid ple shadow\_vmcs pml ept\_violation\_ve ept\_mode\_based\_exec tsc\_scaling usr\_wait\_pause notify\_vm\_exiting
bugs		: spectre\_v1 spectre\_v2 spec\_store\_bypass swapgs bhi
bogomips	: 5990.40
clflush size	: 64
cache\_alignment	: 64
address sizes	: 46 bits physical, 48 bits virtual
power management:
processor	: 4
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 170
model name	: Intel(R) Core(TM) Ultra 7 155H
stepping	: 4
microcode	: 0x17
cpu MHz		: 3000.000
cache size	: 24576 KB
physical id	: 0
siblings	: 22
core id		: 12
cpu cores	: 16
apicid		: 25
initial apicid	: 25
fpu		: yes
fpu\_exception	: yes
cpuid level	: 35
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc art arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc cpuid aperfmperf tsc\_known\_freq pni pclmulqdq dtes64 monitor ds\_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm 3dnowprefetch cpuid\_fault epb intel\_ppin ssbd ibrs ibpb stibp ibrs\_enhanced tpr\_shadow flexpriority ept vpid ept\_ad fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid rdseed adx smap clflushopt clwb intel\_pt sha\_ni xsaveopt xsavec xgetbv1 xsaves split\_lock\_detect user\_shstk avx\_vnni dtherm ida arat pln pts hwp hwp\_notify hwp\_act\_window hwp\_epp hwp\_pkg\_req hfi vnmi umip pku ospke waitpkg gfni vaes vpclmulqdq rdpid bus\_lock\_detect movdiri movdir64b fsrm md\_clear serialize arch\_lbr ibt flush\_l1d arch\_capabilities
vmx flags	: vnmi preemption\_timer posted\_intr invvpid ept\_x\_only ept\_ad ept\_1gb flexpriority apicv tsc\_offset vtpr mtf vapic ept vpid unrestricted\_guest vapic\_reg vid ple shadow\_vmcs pml ept\_violation\_ve ept\_mode\_based\_exec tsc\_scaling usr\_wait\_pause notify\_vm\_exiting
bugs		: spectre\_v1 spectre\_v2 spec\_store\_bypass swapgs bhi
bogomips	: 5990.40
clflush size	: 64
cache\_alignment	: 64
address sizes	: 46 bits physical, 48 bits virtual
power management:
processor	: 5
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 170
model name	: Intel(R) Core(TM) Ultra 7 155H
stepping	: 4
microcode	: 0x17
cpu MHz		: 2793.442
cache size	: 24576 KB
physical id	: 0
siblings	: 22
core id		: 16
cpu cores	: 16
apicid		: 33
initial apicid	: 33
fpu		: yes
fpu\_exception	: yes
cpuid level	: 35
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc art arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc cpuid aperfmperf tsc\_known\_freq pni pclmulqdq dtes64 monitor ds\_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm 3dnowprefetch cpuid\_fault epb intel\_ppin ssbd ibrs ibpb stibp ibrs\_enhanced tpr\_shadow flexpriority ept vpid ept\_ad fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid rdseed adx smap clflushopt clwb intel\_pt sha\_ni xsaveopt xsavec xgetbv1 xsaves split\_lock\_detect user\_shstk avx\_vnni dtherm ida arat pln pts hwp hwp\_notify hwp\_act\_window hwp\_epp hwp\_pkg\_req hfi vnmi umip pku ospke waitpkg gfni vaes vpclmulqdq rdpid bus\_lock\_detect movdiri movdir64b fsrm md\_clear serialize arch\_lbr ibt flush\_l1d arch\_capabilities
vmx flags	: vnmi preemption\_timer posted\_intr invvpid ept\_x\_only ept\_ad ept\_1gb flexpriority apicv tsc\_offset vtpr mtf vapic ept vpid unrestricted\_guest vapic\_reg vid ple shadow\_vmcs pml ept\_violation\_ve ept\_mode\_based\_exec tsc\_scaling usr\_wait\_pause notify\_vm\_exiting
bugs		: spectre\_v1 spectre\_v2 spec\_store\_bypass swapgs bhi
bogomips	: 5990.40
clflush size	: 64
cache\_alignment	: 64
address sizes	: 46 bits physical, 48 bits virtual
power management:
processor	: 6
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 170
model name	: Intel(R) Core(TM) Ultra 7 155H
stepping	: 4
microcode	: 0x17
cpu MHz		: 2900.000
cache size	: 24576 KB
physical id	: 0
siblings	: 22
core id		: 20
cpu cores	: 16
apicid		: 40
initial apicid	: 40
fpu		: yes
fpu\_exception	: yes
cpuid level	: 35
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc art arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc cpuid aperfmperf tsc\_known\_freq pni pclmulqdq dtes64 monitor ds\_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm 3dnowprefetch cpuid\_fault epb intel\_ppin ssbd ibrs ibpb stibp ibrs\_enhanced tpr\_shadow flexpriority ept vpid ept\_ad fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid rdseed adx smap clflushopt clwb intel\_pt sha\_ni xsaveopt xsavec xgetbv1 xsaves split\_lock\_detect user\_shstk avx\_vnni dtherm ida arat pln pts hwp hwp\_notify hwp\_act\_window hwp\_epp hwp\_pkg\_req hfi vnmi umip pku ospke waitpkg gfni vaes vpclmulqdq rdpid bus\_lock\_detect movdiri movdir64b fsrm md\_clear serialize arch\_lbr ibt flush\_l1d arch\_capabilities
vmx flags	: vnmi preemption\_timer posted\_intr invvpid ept\_x\_only ept\_ad ept\_1gb flexpriority apicv tsc\_offset vtpr mtf vapic ept vpid unrestricted\_guest vapic\_reg vid ple shadow\_vmcs pml ept\_violation\_ve ept\_mode\_based\_exec tsc\_scaling usr\_wait\_pause notify\_vm\_exiting
bugs		: spectre\_v1 spectre\_v2 spec\_store\_bypass swapgs bhi
bogomips	: 5990.40
clflush size	: 64
cache\_alignment	: 64
address sizes	: 46 bits physical, 48 bits virtual
power management:
processor	: 7
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 170
model name	: Intel(R) Core(TM) Ultra 7 155H
stepping	: 4
microcode	: 0x17
cpu MHz		: 400.000
cache size	: 24576 KB
physical id	: 0
siblings	: 22
core id		: 20
cpu cores	: 16
apicid		: 41
initial apicid	: 41
fpu		: yes
fpu\_exception	: yes
cpuid level	: 35
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc art arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc cpuid aperfmperf tsc\_known\_freq pni pclmulqdq dtes64 monitor ds\_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm 3dnowprefetch cpuid\_fault epb intel\_ppin ssbd ibrs ibpb stibp ibrs\_enhanced tpr\_shadow flexpriority ept vpid ept\_ad fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid rdseed adx smap clflushopt clwb intel\_pt sha\_ni xsaveopt xsavec xgetbv1 xsaves split\_lock\_detect user\_shstk avx\_vnni dtherm ida arat pln pts hwp hwp\_notify hwp\_act\_window hwp\_epp hwp\_pkg\_req hfi vnmi umip pku ospke waitpkg gfni vaes vpclmulqdq rdpid bus\_lock\_detect movdiri movdir64b fsrm md\_clear serialize arch\_lbr ibt flush\_l1d arch\_capabilities
vmx flags	: vnmi preemption\_timer posted\_intr invvpid ept\_x\_only ept\_ad ept\_1gb flexpriority apicv tsc\_offset vtpr mtf vapic ept vpid unrestricted\_guest vapic\_reg vid ple shadow\_vmcs pml ept\_violation\_ve ept\_mode\_based\_exec tsc\_scaling usr\_wait\_pause notify\_vm\_exiting
bugs		: spectre\_v1 spectre\_v2 spec\_store\_bypass swapgs bhi
bogomips	: 5990.40
clflush size	: 64
cache\_alignment	: 64
address sizes	: 46 bits physical, 48 bits virtual
power management:
processor	: 8
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 170
model name	: Intel(R) Core(TM) Ultra 7 155H
stepping	: 4
microcode	: 0x17
cpu MHz		: 1995.012
cache size	: 24576 KB
physical id	: 0
siblings	: 22
core id		: 24
cpu cores	: 16
apicid		: 48
initial apicid	: 48
fpu		: yes
fpu\_exception	: yes
cpuid level	: 35
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc art arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc cpuid aperfmperf tsc\_known\_freq pni pclmulqdq dtes64 monitor ds\_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm 3dnowprefetch cpuid\_fault epb intel\_ppin ssbd ibrs ibpb stibp ibrs\_enhanced tpr\_shadow flexpriority ept vpid ept\_ad fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid rdseed adx smap clflushopt clwb intel\_pt sha\_ni xsaveopt xsavec xgetbv1 xsaves split\_lock\_detect user\_shstk avx\_vnni dtherm ida arat pln pts hwp hwp\_notify hwp\_act\_window hwp\_epp hwp\_pkg\_req hfi vnmi umip pku ospke waitpkg gfni vaes vpclmulqdq rdpid bus\_lock\_detect movdiri movdir64b fsrm md\_clear serialize arch\_lbr ibt flush\_l1d arch\_capabilities
vmx flags	: vnmi preemption\_timer posted\_intr invvpid ept\_x\_only ept\_ad ept\_1gb flexpriority apicv tsc\_offset vtpr mtf vapic ept vpid unrestricted\_guest vapic\_reg vid ple shadow\_vmcs pml ept\_violation\_ve ept\_mode\_based\_exec tsc\_scaling usr\_wait\_pause notify\_vm\_exiting
bugs		: spectre\_v1 spectre\_v2 spec\_store\_bypass swapgs bhi
bogomips	: 5990.40
clflush size	: 64
cache\_alignment	: 64
address sizes	: 46 bits physical, 48 bits virtual
power management:
processor	: 9
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 170
model name	: Intel(R) Core(TM) Ultra 7 155H
stepping	: 4
microcode	: 0x17
cpu MHz		: 400.000
cache size	: 24576 KB
physical id	: 0
siblings	: 22
core id		: 24
cpu cores	: 16
apicid		: 49
initial apicid	: 49
fpu		: yes
fpu\_exception	: yes
cpuid level	: 35
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc art arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc cpuid aperfmperf tsc\_known\_freq pni pclmulqdq dtes64 monitor ds\_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm 3dnowprefetch cpuid\_fault epb intel\_ppin ssbd ibrs ibpb stibp ibrs\_enhanced tpr\_shadow flexpriority ept vpid ept\_ad fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid rdseed adx smap clflushopt clwb intel\_pt sha\_ni xsaveopt xsavec xgetbv1 xsaves split\_lock\_detect user\_shstk avx\_vnni dtherm ida arat pln pts hwp hwp\_notify hwp\_act\_window hwp\_epp hwp\_pkg\_req hfi vnmi umip pku ospke waitpkg gfni vaes vpclmulqdq rdpid bus\_lock\_detect movdiri movdir64b fsrm md\_clear serialize arch\_lbr ibt flush\_l1d arch\_capabilities
vmx flags	: vnmi preemption\_timer posted\_intr invvpid ept\_x\_only ept\_ad ept\_1gb flexpriority apicv tsc\_offset vtpr mtf vapic ept vpid unrestricted\_guest vapic\_reg vid ple shadow\_vmcs pml ept\_violation\_ve ept\_mode\_based\_exec tsc\_scaling usr\_wait\_pause notify\_vm\_exiting
bugs		: spectre\_v1 spectre\_v2 spec\_store\_bypass swapgs bhi
bogomips	: 5990.40
clflush size	: 64
cache\_alignment	: 64
address sizes	: 46 bits physical, 48 bits virtual
power management:
processor	: 10
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 170
model name	: Intel(R) Core(TM) Ultra 7 155H
stepping	: 4
microcode	: 0x17
cpu MHz		: 400.000
cache size	: 24576 KB
physical id	: 0
siblings	: 22
core id		: 28
cpu cores	: 16
apicid		: 56
initial apicid	: 56
fpu		: yes
fpu\_exception	: yes
cpuid level	: 35
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc art arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc cpuid aperfmperf tsc\_known\_freq pni pclmulqdq dtes64 monitor ds\_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm 3dnowprefetch cpuid\_fault epb intel\_ppin ssbd ibrs ibpb stibp ibrs\_enhanced tpr\_shadow flexpriority ept vpid ept\_ad fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid rdseed adx smap clflushopt clwb intel\_pt sha\_ni xsaveopt xsavec xgetbv1 xsaves split\_lock\_detect user\_shstk avx\_vnni dtherm ida arat pln pts hwp hwp\_notify hwp\_act\_window hwp\_epp hwp\_pkg\_req hfi vnmi umip pku ospke waitpkg gfni vaes vpclmulqdq rdpid bus\_lock\_detect movdiri movdir64b fsrm md\_clear serialize arch\_lbr ibt flush\_l1d arch\_capabilities
vmx flags	: vnmi preemption\_timer posted\_intr invvpid ept\_x\_only ept\_ad ept\_1gb flexpriority apicv tsc\_offset vtpr mtf vapic ept vpid unrestricted\_guest vapic\_reg vid ple shadow\_vmcs pml ept\_violation\_ve ept\_mode\_based\_exec tsc\_scaling usr\_wait\_pause notify\_vm\_exiting
bugs		: spectre\_v1 spectre\_v2 spec\_store\_bypass swapgs bhi
bogomips	: 5990.40
clflush size	: 64
cache\_alignment	: 64
address sizes	: 46 bits physical, 48 bits virtual
power management:
processor	: 11
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 170
model name	: Intel(R) Core(TM) Ultra 7 155H
stepping	: 4
microcode	: 0x17
cpu MHz		: 2800.000
cache size	: 24576 KB
physical id	: 0
siblings	: 22
core id		: 28
cpu cores	: 16
apicid		: 57
initial apicid	: 57
fpu		: yes
fpu\_exception	: yes
cpuid level	: 35
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc art arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc cpuid aperfmperf tsc\_known\_freq pni pclmulqdq dtes64 monitor ds\_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm 3dnowprefetch cpuid\_fault epb intel\_ppin ssbd ibrs ibpb stibp ibrs\_enhanced tpr\_shadow flexpriority ept vpid ept\_ad fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid rdseed adx smap clflushopt clwb intel\_pt sha\_ni xsaveopt xsavec xgetbv1 xsaves split\_lock\_detect user\_shstk avx\_vnni dtherm ida arat pln pts hwp hwp\_notify hwp\_act\_window hwp\_epp hwp\_pkg\_req hfi vnmi umip pku ospke waitpkg gfni vaes vpclmulqdq rdpid bus\_lock\_detect movdiri movdir64b fsrm md\_clear serialize arch\_lbr ibt flush\_l1d arch\_capabilities
vmx flags	: vnmi preemption\_timer posted\_intr invvpid ept\_x\_only ept\_ad ept\_1gb flexpriority apicv tsc\_offset vtpr mtf vapic ept vpid unrestricted\_guest vapic\_reg vid ple shadow\_vmcs pml ept\_violation\_ve ept\_mode\_based\_exec tsc\_scaling usr\_wait\_pause notify\_vm\_exiting
bugs		: spectre\_v1 spectre\_v2 spec\_store\_bypass swapgs bhi
bogomips	: 5990.40
clflush size	: 64
cache\_alignment	: 64
address sizes	: 46 bits physical, 48 bits virtual
power management:
processor	: 12
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 170
model name	: Intel(R) Core(TM) Ultra 7 155H
stepping	: 4
microcode	: 0x17
cpu MHz		: 2251.642
cache size	: 24576 KB
physical id	: 0
siblings	: 22
core id		: 0
cpu cores	: 16
apicid		: 0
initial apicid	: 0
fpu		: yes
fpu\_exception	: yes
cpuid level	: 35
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc art arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc cpuid aperfmperf tsc\_known\_freq pni pclmulqdq dtes64 monitor ds\_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm 3dnowprefetch cpuid\_fault epb intel\_ppin ssbd ibrs ibpb stibp ibrs\_enhanced tpr\_shadow flexpriority ept vpid ept\_ad fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid rdseed adx smap clflushopt clwb intel\_pt sha\_ni xsaveopt xsavec xgetbv1 xsaves split\_lock\_detect user\_shstk avx\_vnni dtherm ida arat pln pts hwp hwp\_notify hwp\_act\_window hwp\_epp hwp\_pkg\_req hfi vnmi umip pku ospke waitpkg gfni vaes vpclmulqdq rdpid bus\_lock\_detect movdiri movdir64b fsrm md\_clear serialize arch\_lbr ibt flush\_l1d arch\_capabilities
vmx flags	: vnmi preemption\_timer posted\_intr invvpid ept\_x\_only ept\_ad ept\_1gb flexpriority apicv tsc\_offset vtpr mtf vapic ept vpid unrestricted\_guest vapic\_reg vid ple shadow\_vmcs pml ept\_violation\_ve ept\_mode\_based\_exec tsc\_scaling usr\_wait\_pause notify\_vm\_exiting
bugs		: spectre\_v1 spectre\_v2 spec\_store\_bypass swapgs bhi
bogomips	: 5990.40
clflush size	: 64
cache\_alignment	: 64
address sizes	: 46 bits physical, 48 bits virtual
power management:
processor	: 13
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 170
model name	: Intel(R) Core(TM) Ultra 7 155H
stepping	: 4
microcode	: 0x17
cpu MHz		: 1639.430
cache size	: 24576 KB
physical id	: 0
siblings	: 22
core id		: 1
cpu cores	: 16
apicid		: 2
initial apicid	: 2
fpu		: yes
fpu\_exception	: yes
cpuid level	: 35
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc art arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc cpuid aperfmperf tsc\_known\_freq pni pclmulqdq dtes64 monitor ds\_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm 3dnowprefetch cpuid\_fault epb intel\_ppin ssbd ibrs ibpb stibp ibrs\_enhanced tpr\_shadow flexpriority ept vpid ept\_ad fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid rdseed adx smap clflushopt clwb intel\_pt sha\_ni xsaveopt xsavec xgetbv1 xsaves split\_lock\_detect user\_shstk avx\_vnni dtherm ida arat pln pts hwp hwp\_notify hwp\_act\_window hwp\_epp hwp\_pkg\_req hfi vnmi umip pku ospke waitpkg gfni vaes vpclmulqdq rdpid bus\_lock\_detect movdiri movdir64b fsrm md\_clear serialize arch\_lbr ibt flush\_l1d arch\_capabilities
vmx flags	: vnmi preemption\_timer posted\_intr invvpid ept\_x\_only ept\_ad ept\_1gb flexpriority apicv tsc\_offset vtpr mtf vapic ept vpid unrestricted\_guest vapic\_reg vid ple shadow\_vmcs pml ept\_violation\_ve ept\_mode\_based\_exec tsc\_scaling usr\_wait\_pause notify\_vm\_exiting
bugs		: spectre\_v1 spectre\_v2 spec\_store\_bypass swapgs bhi
bogomips	: 5990.40
clflush size	: 64
cache\_alignment	: 64
address sizes	: 46 bits physical, 48 bits virtual
power management:
processor	: 14
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 170
model name	: Intel(R) Core(TM) Ultra 7 155H
stepping	: 4
microcode	: 0x17
cpu MHz		: 1647.747
cache size	: 24576 KB
physical id	: 0
siblings	: 22
core id		: 2
cpu cores	: 16
apicid		: 4
initial apicid	: 4
fpu		: yes
fpu\_exception	: yes
cpuid level	: 35
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc art arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc cpuid aperfmperf tsc\_known\_freq pni pclmulqdq dtes64 monitor ds\_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm 3dnowprefetch cpuid\_fault epb intel\_ppin ssbd ibrs ibpb stibp ibrs\_enhanced tpr\_shadow flexpriority ept vpid ept\_ad fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid rdseed adx smap clflushopt clwb intel\_pt sha\_ni xsaveopt xsavec xgetbv1 xsaves split\_lock\_detect user\_shstk avx\_vnni dtherm ida arat pln pts hwp hwp\_notify hwp\_act\_window hwp\_epp hwp\_pkg\_req hfi vnmi umip pku ospke waitpkg gfni vaes vpclmulqdq rdpid bus\_lock\_detect movdiri movdir64b fsrm md\_clear serialize arch\_lbr ibt flush\_l1d arch\_capabilities
vmx flags	: vnmi preemption\_timer posted\_intr invvpid ept\_x\_only ept\_ad ept\_1gb flexpriority apicv tsc\_offset vtpr mtf vapic ept vpid unrestricted\_guest vapic\_reg vid ple shadow\_vmcs pml ept\_violation\_ve ept\_mode\_based\_exec tsc\_scaling usr\_wait\_pause notify\_vm\_exiting
bugs		: spectre\_v1 spectre\_v2 spec\_store\_bypass swapgs bhi
bogomips	: 5990.40
clflush size	: 64
cache\_alignment	: 64
address sizes	: 46 bits physical, 48 bits virtual
power management:
processor	: 15
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 170
model name	: Intel(R) Core(TM) Ultra 7 155H
stepping	: 4
microcode	: 0x17
cpu MHz		: 2272.653
cache size	: 24576 KB
physical id	: 0
siblings	: 22
core id		: 3
cpu cores	: 16
apicid		: 6
initial apicid	: 6
fpu		: yes
fpu\_exception	: yes
cpuid level	: 35
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc art arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc cpuid aperfmperf tsc\_known\_freq pni pclmulqdq dtes64 monitor ds\_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm 3dnowprefetch cpuid\_fault epb intel\_ppin ssbd ibrs ibpb stibp ibrs\_enhanced tpr\_shadow flexpriority ept vpid ept\_ad fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid rdseed adx smap clflushopt clwb intel\_pt sha\_ni xsaveopt xsavec xgetbv1 xsaves split\_lock\_detect user\_shstk avx\_vnni dtherm ida arat pln pts hwp hwp\_notify hwp\_act\_window hwp\_epp hwp\_pkg\_req hfi vnmi umip pku ospke waitpkg gfni vaes vpclmulqdq rdpid bus\_lock\_detect movdiri movdir64b fsrm md\_clear serialize arch\_lbr ibt flush\_l1d arch\_capabilities
vmx flags	: vnmi preemption\_timer posted\_intr invvpid ept\_x\_only ept\_ad ept\_1gb flexpriority apicv tsc\_offset vtpr mtf vapic ept vpid unrestricted\_guest vapic\_reg vid ple shadow\_vmcs pml ept\_violation\_ve ept\_mode\_based\_exec tsc\_scaling usr\_wait\_pause notify\_vm\_exiting
bugs		: spectre\_v1 spectre\_v2 spec\_store\_bypass swapgs bhi
bogomips	: 5990.40
clflush size	: 64
cache\_alignment	: 64
address sizes	: 46 bits physical, 48 bits virtual
power management:
processor	: 16
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 170
model name	: Intel(R) Core(TM) Ultra 7 155H
stepping	: 4
microcode	: 0x17
cpu MHz		: 2197.520
cache size	: 24576 KB
physical id	: 0
siblings	: 22
core id		: 4
cpu cores	: 16
apicid		: 8
initial apicid	: 8
fpu		: yes
fpu\_exception	: yes
cpuid level	: 35
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc art arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc cpuid aperfmperf tsc\_known\_freq pni pclmulqdq dtes64 monitor ds\_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm 3dnowprefetch cpuid\_fault epb intel\_ppin ssbd ibrs ibpb stibp ibrs\_enhanced tpr\_shadow flexpriority ept vpid ept\_ad fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid rdseed adx smap clflushopt clwb intel\_pt sha\_ni xsaveopt xsavec xgetbv1 xsaves split\_lock\_detect user\_shstk avx\_vnni dtherm ida arat pln pts hwp hwp\_notify hwp\_act\_window hwp\_epp hwp\_pkg\_req hfi vnmi umip pku ospke waitpkg gfni vaes vpclmulqdq rdpid bus\_lock\_detect movdiri movdir64b fsrm md\_clear serialize arch\_lbr ibt flush\_l1d arch\_capabilities
vmx flags	: vnmi preemption\_timer posted\_intr invvpid ept\_x\_only ept\_ad ept\_1gb flexpriority apicv tsc\_offset vtpr mtf vapic ept vpid unrestricted\_guest vapic\_reg vid ple shadow\_vmcs pml ept\_violation\_ve ept\_mode\_based\_exec tsc\_scaling usr\_wait\_pause notify\_vm\_exiting
bugs		: spectre\_v1 spectre\_v2 spec\_store\_bypass swapgs bhi
bogomips	: 5990.40
clflush size	: 64
cache\_alignment	: 64
address sizes	: 46 bits physical, 48 bits virtual
power management:
processor	: 17
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 170
model name	: Intel(R) Core(TM) Ultra 7 155H
stepping	: 4
microcode	: 0x17
cpu MHz		: 2190.533
cache size	: 24576 KB
physical id	: 0
siblings	: 22
core id		: 5
cpu cores	: 16
apicid		: 10
initial apicid	: 10
fpu		: yes
fpu\_exception	: yes
cpuid level	: 35
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc art arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc cpuid aperfmperf tsc\_known\_freq pni pclmulqdq dtes64 monitor ds\_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm 3dnowprefetch cpuid\_fault epb intel\_ppin ssbd ibrs ibpb stibp ibrs\_enhanced tpr\_shadow flexpriority ept vpid ept\_ad fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid rdseed adx smap clflushopt clwb intel\_pt sha\_ni xsaveopt xsavec xgetbv1 xsaves split\_lock\_detect user\_shstk avx\_vnni dtherm ida arat pln pts hwp hwp\_notify hwp\_act\_window hwp\_epp hwp\_pkg\_req hfi vnmi umip pku ospke waitpkg gfni vaes vpclmulqdq rdpid bus\_lock\_detect movdiri movdir64b fsrm md\_clear serialize arch\_lbr ibt flush\_l1d arch\_capabilities
vmx flags	: vnmi preemption\_timer posted\_intr invvpid ept\_x\_only ept\_ad ept\_1gb flexpriority apicv tsc\_offset vtpr mtf vapic ept vpid unrestricted\_guest vapic\_reg vid ple shadow\_vmcs pml ept\_violation\_ve ept\_mode\_based\_exec tsc\_scaling usr\_wait\_pause notify\_vm\_exiting
bugs		: spectre\_v1 spectre\_v2 spec\_store\_bypass swapgs bhi
bogomips	: 5990.40
clflush size	: 64
cache\_alignment	: 64
address sizes	: 46 bits physical, 48 bits virtual
power management:
processor	: 18
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 170
model name	: Intel(R) Core(TM) Ultra 7 155H
stepping	: 4
microcode	: 0x17
cpu MHz		: 2200.736
cache size	: 24576 KB
physical id	: 0
siblings	: 22
core id		: 6
cpu cores	: 16
apicid		: 12
initial apicid	: 12
fpu		: yes
fpu\_exception	: yes
cpuid level	: 35
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc art arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc cpuid aperfmperf tsc\_known\_freq pni pclmulqdq dtes64 monitor ds\_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm 3dnowprefetch cpuid\_fault epb intel\_ppin ssbd ibrs ibpb stibp ibrs\_enhanced tpr\_shadow flexpriority ept vpid ept\_ad fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid rdseed adx smap clflushopt clwb intel\_pt sha\_ni xsaveopt xsavec xgetbv1 xsaves split\_lock\_detect user\_shstk avx\_vnni dtherm ida arat pln pts hwp hwp\_notify hwp\_act\_window hwp\_epp hwp\_pkg\_req hfi vnmi umip pku ospke waitpkg gfni vaes vpclmulqdq rdpid bus\_lock\_detect movdiri movdir64b fsrm md\_clear serialize arch\_lbr ibt flush\_l1d arch\_capabilities
vmx flags	: vnmi preemption\_timer posted\_intr invvpid ept\_x\_only ept\_ad ept\_1gb flexpriority apicv tsc\_offset vtpr mtf vapic ept vpid unrestricted\_guest vapic\_reg vid ple shadow\_vmcs pml ept\_violation\_ve ept\_mode\_based\_exec tsc\_scaling usr\_wait\_pause notify\_vm\_exiting
bugs		: spectre\_v1 spectre\_v2 spec\_store\_bypass swapgs bhi
bogomips	: 5990.40
clflush size	: 64
cache\_alignment	: 64
address sizes	: 46 bits physical, 48 bits virtual
power management:
processor	: 19
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 170
model name	: Intel(R) Core(TM) Ultra 7 155H
stepping	: 4
microcode	: 0x17
cpu MHz		: 2200.508
cache size	: 24576 KB
physical id	: 0
siblings	: 22
core id		: 7
cpu cores	: 16
apicid		: 14
initial apicid	: 14
fpu		: yes
fpu\_exception	: yes
cpuid level	: 35
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc art arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc cpuid aperfmperf tsc\_known\_freq pni pclmulqdq dtes64 monitor ds\_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm 3dnowprefetch cpuid\_fault epb intel\_ppin ssbd ibrs ibpb stibp ibrs\_enhanced tpr\_shadow flexpriority ept vpid ept\_ad fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid rdseed adx smap clflushopt clwb intel\_pt sha\_ni xsaveopt xsavec xgetbv1 xsaves split\_lock\_detect user\_shstk avx\_vnni dtherm ida arat pln pts hwp hwp\_notify hwp\_act\_window hwp\_epp hwp\_pkg\_req hfi vnmi umip pku ospke waitpkg gfni vaes vpclmulqdq rdpid bus\_lock\_detect movdiri movdir64b fsrm md\_clear serialize arch\_lbr ibt flush\_l1d arch\_capabilities
vmx flags	: vnmi preemption\_timer posted\_intr invvpid ept\_x\_only ept\_ad ept\_1gb flexpriority apicv tsc\_offset vtpr mtf vapic ept vpid unrestricted\_guest vapic\_reg vid ple shadow\_vmcs pml ept\_violation\_ve ept\_mode\_based\_exec tsc\_scaling usr\_wait\_pause notify\_vm\_exiting
bugs		: spectre\_v1 spectre\_v2 spec\_store\_bypass swapgs bhi
bogomips	: 5990.40
clflush size	: 64
cache\_alignment	: 64
address sizes	: 46 bits physical, 48 bits virtual
power management:
processor	: 20
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 170
model name	: Intel(R) Core(TM) Ultra 7 155H
stepping	: 4
microcode	: 0x17
cpu MHz		: 400.000
cache size	: 2048 KB
physical id	: 0
siblings	: 22
core id		: 32
cpu cores	: 16
apicid		: 64
initial apicid	: 64
fpu		: yes
fpu\_exception	: yes
cpuid level	: 35
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc art arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc cpuid aperfmperf tsc\_known\_freq pni pclmulqdq dtes64 monitor ds\_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm 3dnowprefetch cpuid\_fault epb intel\_ppin ssbd ibrs ibpb stibp ibrs\_enhanced tpr\_shadow flexpriority ept vpid ept\_ad fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid rdseed adx smap clflushopt clwb intel\_pt sha\_ni xsaveopt xsavec xgetbv1 xsaves split\_lock\_detect user\_shstk avx\_vnni dtherm ida arat pln pts hwp hwp\_notify hwp\_act\_window hwp\_epp hwp\_pkg\_req hfi vnmi umip pku ospke waitpkg gfni vaes vpclmulqdq rdpid bus\_lock\_detect movdiri movdir64b fsrm md\_clear serialize arch\_lbr ibt flush\_l1d arch\_capabilities
vmx flags	: vnmi preemption\_timer posted\_intr invvpid ept\_x\_only ept\_ad ept\_1gb flexpriority apicv tsc\_offset vtpr mtf vapic ept vpid unrestricted\_guest vapic\_reg vid ple shadow\_vmcs pml ept\_violation\_ve ept\_mode\_based\_exec tsc\_scaling usr\_wait\_pause notify\_vm\_exiting
bugs		: spectre\_v1 spectre\_v2 spec\_store\_bypass swapgs bhi
bogomips	: 5990.40
clflush size	: 64
cache\_alignment	: 64
address sizes	: 46 bits physical, 48 bits virtual
power management:
processor	: 21
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 170
model name	: Intel(R) Core(TM) Ultra 7 155H
stepping	: 4
microcode	: 0x17
cpu MHz		: 400.000
cache size	: 2048 KB
physical id	: 0
siblings	: 22
core id		: 33
cpu cores	: 16
apicid		: 66
initial apicid	: 66
fpu		: yes
fpu\_exception	: yes
cpuid level	: 35
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc art arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc cpuid aperfmperf tsc\_known\_freq pni pclmulqdq dtes64 monitor ds\_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm 3dnowprefetch cpuid\_fault epb intel\_ppin ssbd ibrs ibpb stibp ibrs\_enhanced tpr\_shadow flexpriority ept vpid ept\_ad fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid rdseed adx smap clflushopt clwb intel\_pt sha\_ni xsaveopt xsavec xgetbv1 xsaves split\_lock\_detect user\_shstk avx\_vnni dtherm ida arat pln pts hwp hwp\_notify hwp\_act\_window hwp\_epp hwp\_pkg\_req hfi vnmi umip pku ospke waitpkg gfni vaes vpclmulqdq rdpid bus\_lock\_detect movdiri movdir64b fsrm md\_clear serialize arch\_lbr ibt flush\_l1d arch\_capabilities
vmx flags	: vnmi preemption\_timer posted\_intr invvpid ept\_x\_only ept\_ad ept\_1gb flexpriority apicv tsc\_offset vtpr mtf vapic ept vpid unrestricted\_guest vapic\_reg vid ple shadow\_vmcs pml ept\_violation\_ve ept\_mode\_based\_exec tsc\_scaling usr\_wait\_pause notify\_vm\_exiting
bugs		: spectre\_v1 spectre\_v2 spec\_store\_bypass swapgs bhi
bogomips	: 5990.40
clflush size	: 64
cache\_alignment	: 64
address sizes	: 46 bits physical, 48 bits virtual
power management:

## Installation

### Kernel

#### Input devices

#### USB 3.0 support

**USB controller:*USB controller: Intel Corporation Meteor Lake-P USB 3.2 Gen 2x1 xHCI Host Controller (rev 20)*, 6.10.7-gentoo**

Kernel version 3.3 at least is required for the USB 3 support.

#### Drives and storage

**Non-Volatile memory controller: Yangtze Memory Technologies Co.,Ltd PC300 NVMe SSD (DRAM-less) (rev 03), 6.10.7-gentoo**

If you want to install the user space tools run this:

`root #``emerge --ask sys-apps/nvme-cli`
You can also read the full [NVMe](https://wiki.gentoo.org/wiki/NVMe) article for more advanced NVMe support.

#### Graphics

Compile the Xe driver for Intel Arc as a module

**VGA compatible controller: Intel Corporation Meteor Lake-P \[Intel Arc Graphics\] (rev 08), 6.10.7-gentoo**

You also need the EFI frame buffer driver

**VGA compatible controller: Intel Corporation Meteor Lake-P \[Intel Arc Graphics\] (rev 08), 6.10.7-gentoo**

##### Force use of Xe probe

You can add `xe.force_probe='7d55'` as module parameters in GRUB for example, or you can directly build that into the kernel, like in the example below.

**VGA compatible controller: Intel Corporation Meteor Lake-P \[Intel Arc Graphics\] (rev 08), 6.10.7-gentoo**

#### Wi-Fi

**Network controller: Intel Corporation Meteor Lake PCH CNVi WiFi (rev 20), 6.10.7-gentoo**

This driver needs some firmware to run

`root #``emerge --ask sys-kernel/linux-firmware`
#### Sound

**Sound device**

You need to get the firmware from the SOF project

`root #``emerge --ask sys-firmware/sof-firmware`
#### CPU frequency scaling

**CPU frequency scaling, 6.10.7-gentoo**

#### NPU

**CPU frequency scaling, 6.10.7-gentoo**

#### Webcam

**Bus 003 Device 004: ID 2b7e:c817 SunplusIT Inc XiaoMi WebCam**

#### Bluetooth

### Misc

#### Fingerprint reader

The laptop is equipied with a fingerprint reader, it is supported officially by fprint, but not before v1.94.9. This version is marked as testing on Gentoo. You also can patch an older version if you want to use the stable package. Thanks to portage we can automatically apply patches to packages, you need to create a directory for patches.

`root #``mkdir -p /etc/portage/patches/sys-auth/libfprint`
Then create a file with .patch or .diff extension under the directory you just created and fill it with this patch :

**`/etc/portage/patches/sys-auth/libfprint/fixfprintreader.patch`**

```
diff --git a/libfprint/drivers/goodixmoc/goodix.c b/libfprint/drivers/goodixmoc/goodix.c
index 5a3ffac..5b9af95 100644
--- a/libfprint/drivers/goodixmoc/goodix.c
+++ b/libfprint/drivers/goodixmoc/goodix.c
@@ -1632,6 +1632,7 @@ static const FpIdEntry id_table[] = {
   { .vid = 0x27c6,  .pid = 0x659A,  },
   { .vid = 0x27c6,  .pid = 0x659C,  },
   { .vid = 0x27c6,  .pid = 0x6A94,  },
+  { .vid = 0x27c6,  .pid = 0x689A,  },
   { .vid = 0,  .pid = 0,  .driver_data = 0 },   /* terminating entry */
 };
```
You can now install fprint and it will support the laptop's fingerprint reader

`root #``emerge --ask sys-auth/fprintd`
