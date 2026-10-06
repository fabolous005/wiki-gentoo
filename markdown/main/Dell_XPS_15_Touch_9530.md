<!-- source: https://wiki.gentoo.org/wiki/Dell_XPS_15_Touch_9530 | group: Gentoo Wiki (Main) | wiki-title: Dell XPS 15 Touch 9530 -->
---
title: Dell XPS 15 Touch 9530
url: https://wiki.gentoo.org/wiki/Dell_XPS_15_Touch_9530
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-29"
fingerprint: "6f5e0e35c33e1b69"
license: CC BY-SA 4.0
---

# Dell XPS 15 Touch 9530

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



## Laptop specifications

There are two models that have different storage and battery options.

- 4th Generation Intel® Core™ i7-4702HQ processor (6M Cache, up to 3.2 GHz)
- 16 GB DDR3 1600MHz (Samsung)
- Intel HD Graphics 4600
- NVIDIA® GeForce® GT 750M 2GB GDDR5
- High Definition Audio
- 15.6 inch LED Backlit Touch Display with Truelife and QHD+ resolution (3200 x 1800) (IZGO)
- 1TB 5400 rpm SATA Hard Drive + 32GB mSATA Solid State Drive or 512Gb mSATA Solid State Drive (Samsung SSD SM841)
- Intel® Centrino® Wireless-AC 7260 + Bluetooth 4.0
- 61 WHr, 6-Cell Battery (integrated) or 91 WHr, 6-Cell Battery (integrated) (for 512Gb SSD)
- 3x USB 3.0 w/PowerShare and 1x USB 2.0 with PowerShare
- 1x mini DisplayPort
- 1x HDMI
- 3-in-1 media card reader supporting SD, SDIO, SDXC
- headset jack

Printout of lspci:

`root #``lspci`
00:00.0 Host bridge: Intel Corporation Xeon E3-1200 v3/4th Gen Core Processor DRAM Controller (rev 06)
00:01.0 PCI bridge: Intel Corporation Xeon E3-1200 v3/4th Gen Core Processor PCI Express x16 Controller (rev 06)
00:02.0 VGA compatible controller: Intel Corporation 4th Gen Core Processor Integrated Graphics Controller (rev 06)
00:03.0 Audio device: Intel Corporation Xeon E3-1200 v3/4th Gen Core Processor HD Audio Controller (rev 06)
00:04.0 Signal processing controller: Intel Corporation Device 0c03 (rev 06)
00:14.0 USB controller: Intel Corporation 8 Series/C220 Series Chipset Family USB xHCI (rev 05)
00:16.0 Communication controller: Intel Corporation 8 Series/C220 Series Chipset Family MEI Controller #1 (rev 04)
00:1a.0 USB controller: Intel Corporation 8 Series/C220 Series Chipset Family USB EHCI #2 (rev 05)
00:1b.0 Audio device: Intel Corporation 8 Series/C220 Series Chipset High Definition Audio Controller (rev 05)
00:1c.0 PCI bridge: Intel Corporation 8 Series/C220 Series Chipset Family PCI Express Root Port #1 (rev d5)
00:1c.2 PCI bridge: Intel Corporation 8 Series/C220 Series Chipset Family PCI Express Root Port #3 (rev d5)
00:1c.3 PCI bridge: Intel Corporation 8 Series/C220 Series Chipset Family PCI Express Root Port #4 (rev d5)
00:1d.0 USB controller: Intel Corporation 8 Series/C220 Series Chipset Family USB EHCI #1 (rev 05)
00:1f.0 ISA bridge: Intel Corporation HM87 Express LPC Controller (rev 05)
00:1f.2 SATA controller: Intel Corporation 8 Series/C220 Series Chipset Family 6-port SATA Controller 1 \[AHCI mode\] (rev 05)
00:1f.3 SMBus: Intel Corporation 8 Series/C220 Series Chipset Family SMBus Controller (rev 05)
00:1f.6 Signal processing controller: Intel Corporation 8 Series Chipset Family Thermal Management Controller (rev 05)
02:00.0 3D controller: NVIDIA Corporation GK107M \[GeForce GT 750M\] (rev ff)
06:00.0 Network controller: Intel Corporation Wireless 7260 (rev 6b)
07:00.0 Unassigned class \[ff00\]: Realtek Semiconductor Co., Ltd. Device 5249 (rev 01)

Printout of lspci -kk:

`root #``lspci````
00:00.0 Host bridge: Intel Corporation Xeon E3-1200 v3/4th Gen Core Processor DRAM Controller (rev 06)                                                                                                                             
        Subsystem: Dell Device 05fe                                                                                                                                                                                                
00:01.0 PCI bridge: Intel Corporation Xeon E3-1200 v3/4th Gen Core Processor PCI Express x16 Controller (rev 06)                                                                                                                   
        Kernel driver in use: pcieport                                                                                                                                                                                             
00:02.0 VGA compatible controller: Intel Corporation 4th Gen Core Processor Integrated Graphics Controller (rev 06)                                                                                                                
        Subsystem: Dell Device 05fe                                                                                                                                                                                                
        Kernel driver in use: i915                                                                                                                                                                                                 
00:03.0 Audio device: Intel Corporation Xeon E3-1200 v3/4th Gen Core Processor HD Audio Controller (rev 06)                                                                                                                        
        Subsystem: Intel Corporation Device 2010                                                                                                                                                                                   
        Kernel driver in use: snd_hda_intel                                                                                                                                                                                        
00:04.0 Signal processing controller: Intel Corporation Device 0c03 (rev 06)                                                                                                                                                       
        Subsystem: Intel Corporation Device 2010                                                                                                                                                                                   
00:14.0 USB controller: Intel Corporation 8 Series/C220 Series Chipset Family USB xHCI (rev 05)                                                                                                                                    
        Subsystem: Dell Device 05fe
        Kernel driver in use: xhci_hcd
00:16.0 Communication controller: Intel Corporation 8 Series/C220 Series Chipset Family MEI Controller #1 (rev 04)
        Subsystem: Dell Device 05fe
00:1a.0 USB controller: Intel Corporation 8 Series/C220 Series Chipset Family USB EHCI #2 (rev 05)
        Subsystem: Dell Device 05fe
        Kernel driver in use: ehci-pci
00:1b.0 Audio device: Intel Corporation 8 Series/C220 Series Chipset High Definition Audio Controller (rev 05)
        Subsystem: Dell Device 05fe
        Kernel driver in use: snd_hda_intel
00:1c.0 PCI bridge: Intel Corporation 8 Series/C220 Series Chipset Family PCI Express Root Port #1 (rev d5)
        Kernel driver in use: pcieport
00:1c.2 PCI bridge: Intel Corporation 8 Series/C220 Series Chipset Family PCI Express Root Port #3 (rev d5)
        Kernel driver in use: pcieport
00:1c.3 PCI bridge: Intel Corporation 8 Series/C220 Series Chipset Family PCI Express Root Port #4 (rev d5)
        Kernel driver in use: pcieport
00:1d.0 USB controller: Intel Corporation 8 Series/C220 Series Chipset Family USB EHCI #1 (rev 05)
        Subsystem: Dell Device 05fe
        Kernel driver in use: ehci-pci
00:1f.0 ISA bridge: Intel Corporation HM87 Express LPC Controller (rev 05)
        Subsystem: Dell Device 05fe
00:1f.2 SATA controller: Intel Corporation 8 Series/C220 Series Chipset Family 6-port SATA Controller 1 [AHCI mode] (rev 05)
        Subsystem: Dell Device 05fe
        Kernel driver in use: ahci
00:1f.3 SMBus: Intel Corporation 8 Series/C220 Series Chipset Family SMBus Controller (rev 05)
        Subsystem: Dell Device 05fe
00:1f.6 Signal processing controller: Intel Corporation 8 Series Chipset Family Thermal Management Controller (rev 05)
        Subsystem: Dell Device 05fe
02:00.0 3D controller: NVIDIA Corporation GK107M [GeForce GT 750M] (rev a1)
        Subsystem: Dell Device 05fe
        Kernel driver in use: nvidia
        Kernel modules: nvidia
06:00.0 Network controller: Intel Corporation Wireless 7260 (rev 6b)
        Subsystem: Intel Corporation Dual Band Wireless-AC 7260
        Kernel driver in use: iwlwifi
07:00.0 Unassigned class [ff00]: Realtek Semiconductor Co., Ltd. Device 5249 (rev 01)
        Subsystem: Dell Device 05fe
        Kernel driver in use: rtsx_pci
```
Printout of lsusb (builtin devices, no external devices connected):

`root #``lsusb`
Bus 001 Device 002: ID 8087:8008 Intel Corp. 
Bus 002 Device 002: ID 8087:8000 Intel Corp. 
Bus 003 Device 002: ID 06cb:0ac3 Synaptics, Inc. 
Bus 003 Device 016: ID 8087:07dc Intel Corp. 
Bus 003 Device 007: ID 0bda:573c Realtek Semiconductor Corp. 
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub

Printout of lsmod (builtin devices, no external devices connected):

`root #``lsmod`
Module                  Size  Used by
msr                     2572  0 
cpufreq\_stats           3866  0 
usbhid                 32696  0
udf                    66691  0 
hidp                   13512  1 
rfcomm                 28729  14 
btusb                  15461  0  
ipv6                  296947  79 
bbswitch                4382  0 
bnep                   10006  2 
bluetooth             187785  27 bnep,hidp,btusb,rfcomm
uvcvideo               62389  0 
videobuf2\_vmalloc       2808  1 uvcvideo
videobuf2\_memops        2282  1 videobuf2\_vmalloc
videobuf2\_core         27398  1 uvcvideo
videodev              111316  2 uvcvideo,videobuf2\_core
hid\_multitouch          9141  0 
snd\_hda\_codec\_hdmi     27310  1 
arc4                    1967  2 
x86\_pkg\_temp\_thermal     4878  0 
coretemp                6027  0 
kvm\_intel             124696  0 
kvm                   369289  1 kvm\_intel
crc32c\_intel           14183  0 
dell\_wmi                1625  0 
sparse\_keymap           3642  1 dell\_wmi
dell\_laptop             9169  0 
dcdbas                  5982  1 dell\_laptop
iwlmvm                 92136  0 
mac80211              418790  1 iwlmvm
joydev                  8732  0 
iwlwifi                78719  1 iwlmvm
i2c\_i801                9053  0 
cfg80211              440971  3 iwlwifi,mac80211,iwlmvm
snd\_hda\_codec\_realtek    41843  1 
fan                     2770  0 
rfkill                 17524  4 cfg80211,bluetooth
thermal                 9279  0 
nouveau               834226  0 
i915                  616006  5 
mxm\_wmi                 1909  1 nouveau
snd\_hda\_intel          36196  20 
ttm                    66379  1 nouveau
intel\_agp              11233  1 i915
intel\_gtt              12443  2 i915,intel\_agp
snd\_hda\_codec         152404  3 snd\_hda\_codec\_realtek,snd\_hda\_codec\_hdmi,snd\_hda\_intel
i2c\_algo\_bit            5253  2 i915,nouveau
drm\_kms\_helper         39776  2 i915,nouveau
snd\_hwdep               5723  1 snd\_hda\_codec
rtc\_cmos                8248  0 
ac                      3607  0 
battery                 7442  0 
drm                   262538  6 ttm,i915,drm\_kms\_helper,nouveau
snd\_pcm                73213  8 snd\_hda\_codec\_hdmi,snd\_hda\_codec,snd\_hda\_intel
video                  12465  2 i915,nouveau
snd\_page\_alloc          7870  2 snd\_pcm,snd\_hda\_intel
snd\_timer              18305  6 snd\_pcm
wmi                     9049  3 dell\_wmi,mxm\_wmi,nouveau
tpm\_tis                 9765  0 
agpgart                31021  4 drm,ttm,intel\_agp,intel\_gtt
tpm                    17823  1 tpm\_tis
i2c\_core               24158  8 drm,i915,i2c\_i801,drm\_kms\_helper,i2c\_algo\_bit,nouveau,videodev
tpm\_bios               10275  1 tpm
snd                    64049  37 snd\_hda\_codec\_realtek,snd\_hwdep,snd\_timer,snd\_hda\_codec\_hdmi,snd\_pcm,snd\_hda\_codec,snd\_hda\_intel
processor              26693  0 
soundcore               6503  1 snd
button                  5436  2 i915,nouveau
thermal\_sys            19141  5 fan,video,thermal,processor,x86\_pkg\_temp\_thermal
efivarfs                5347  0 
dm\_zero                 1257  0 
dm\_persistent\_data     45419  0 
dm\_round\_robin          2211  0 
dm\_multipath           13973  1 dm\_round\_robin
dm\_delay                4238  0 
dm\_bufio               17697  1 dm\_persistent\_data
xts                     3082  0 
aesni\_intel            44752  6 
ablk\_helper             2677  1 aesni\_intel
cryptd                  8808  3 aesni\_intel,ablk\_helper
glue\_helper             5424  1 aesni\_intel
lrw                     3790  1 aesni\_intel
gf128mul                7343  2 lrw,xts
aes\_x86\_64              7731  1 aesni\_intel
sha256\_generic          9969  2 
scsi\_transport\_iscsi    70676  0 
fscache                39529  1
lockd                  59264  1
sunrpc                216017  2 lockd
raid6\_pq               92436  1 
xor                    11267  1 
ext3                  174491  0 
jbd                    63659  1 ext3
ext2                   43604  0 
dm\_snapshot            26149  0 
dm\_mirror              11840  0 
dm\_region\_hash          9753  1 dm\_mirror
dm\_log                  8426  2 dm\_region\_hash,dm\_mirror
led\_class               4141  3 iwlmvm,dell\_laptop
xhci\_hcd               95579  0 
ohci\_hcd               17165  0 
uhci\_hcd               19532  0 
usb\_storage            48129  1 
ehci\_pci                3513  0 
ehci\_hcd               34859  1 ehci\_pci
usbcore               164229  10 btusb,uhci\_hcd,uvcvideo,usb\_storage,ohci\_hcd,ehci\_hcd,ehci\_pci,usbhid,xhci\_hcd
usb\_common              2169  1 usbcore
scsi\_transport\_fc      47451  0 
scsi\_tgt               10625  1 scsi\_transport\_fc
sr\_mod                 12995  0 
cdrom                  31684  1 sr\_mod

Information from /proc/cpuinfo :

`root #``cat /proc/cpuinfo`
processor  : 0
vendor\_id  : GenuineIntel
cpu family : 6
model  : 60
model name : Intel(R) Core(TM) i7-4702HQ CPU @ 2.20GHz
stepping : 3
microcode  : 0x12
cpu MHz  : 3171.867
cache size : 6144 KB
physical id : 0
siblings : 8
core id  : 0
cpu cores  : 4
apicid  : 0
initial apicid  : 0
fpu   : yes
fpu\_exception : yes
cpuid level : 13
wp  : yes
flags  : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc aperfmperf eagerfpu pni pclmulqdq dtes64 monitor ds\_cpl vmx est tm2 ssse3 fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm ida arat epb xsaveopt pln pts dtherm tpr\_shadow vnmi flexpriority ept vpid fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid
bogomips : 4389.55
clflush size  : 64
cache\_alignment : 64
address sizes : 39 bits physical, 48 bits virtual
power management:
processor  : 1
vendor\_id  : GenuineIntel
cpu family : 6
model  : 60
model name : Intel(R) Core(TM) i7-4702HQ CPU @ 2.20GHz
stepping : 3
microcode  : 0x12
cpu MHz  : 3135.171
cache size : 6144 KB
physical id : 0
siblings : 8
core id  : 1
cpu cores  : 4
apicid  : 2
initial apicid  : 2
fpu   : yes
fpu\_exception : yes
cpuid level : 13
wp  : yes
flags  : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc aperfmperf eagerfpu pni pclmulqdq dtes64 monitor ds\_cpl vmx est tm2 ssse3 fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm ida arat epb xsaveopt pln pts dtherm tpr\_shadow vnmi flexpriority ept vpid fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid
bogomips : 4389.55
clflush size  : 64
cache\_alignment : 64
address sizes : 39 bits physical, 48 bits virtual
power management:
processor  : 2
vendor\_id  : GenuineIntel
cpu family : 6
model  : 60
model name : Intel(R) Core(TM) i7-4702HQ CPU @ 2.20GHz
stepping : 3
microcode  : 0x12
cpu MHz  : 3172.039
cache size : 6144 KB
physical id : 0
siblings : 8
core id  : 2
cpu cores  : 4
apicid  : 4
initial apicid  : 4
fpu   : yes
fpu\_exception : yes
cpuid level : 13
wp  : yes
flags  : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc aperfmperf eagerfpu pni pclmulqdq dtes64 monitor ds\_cpl vmx est tm2 ssse3 fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm ida arat epb xsaveopt pln pts dtherm tpr\_shadow vnmi flexpriority ept vpid fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid
bogomips : 4389.55
clflush size  : 64
cache\_alignment : 64
address sizes : 39 bits physical, 48 bits virtual
power management:
processor  : 3
vendor\_id  : GenuineIntel
cpu family : 6
model  : 60
model name : Intel(R) Core(TM) i7-4702HQ CPU @ 2.20GHz
stepping : 3
microcode  : 0x12
cpu MHz  : 3177.195
cache size : 6144 KB
physical id : 0
siblings : 8
core id  : 3
cpu cores  : 4
apicid  : 6
initial apicid  : 6
fpu   : yes
fpu\_exception : yes
cpuid level : 13
wp  : yes
flags  : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc aperfmperf eagerfpu pni pclmulqdq dtes64 monitor ds\_cpl vmx est tm2 ssse3 fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm ida arat epb xsaveopt pln pts dtherm tpr\_shadow vnmi flexpriority ept vpid fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid
bogomips : 4389.55
clflush size  : 64
cache\_alignment : 64
address sizes : 39 bits physical, 48 bits virtual
power management:
processor  : 4
vendor\_id  : GenuineIntel
cpu family : 6
model  : 60
model name : Intel(R) Core(TM) i7-4702HQ CPU @ 2.20GHz
stepping : 3
microcode  : 0x12
cpu MHz  : 3144.882
cache size : 6144 KB
physical id : 0
siblings : 8
core id  : 0
cpu cores  : 4
apicid  : 1
initial apicid  : 1
fpu   : yes
fpu\_exception : yes
cpuid level : 13
wp  : yes
flags  : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc aperfmperf eagerfpu pni pclmulqdq dtes64 monitor ds\_cpl vmx est tm2 ssse3 fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm ida arat epb xsaveopt pln pts dtherm tpr\_shadow vnmi flexpriority ept vpid fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid
bogomips : 4389.55
clflush size  : 64
cache\_alignment : 64
address sizes : 39 bits physical, 48 bits virtual
power management:
processor  : 5
vendor\_id  : GenuineIntel
cpu family : 6
model  : 60
model name : Intel(R) Core(TM) i7-4702HQ CPU @ 2.20GHz
stepping : 3
microcode  : 0x12
cpu MHz  : 3177.023
cache size : 6144 KB
physical id : 0
siblings : 8
core id  : 1
cpu cores  : 4
apicid  : 3
initial apicid  : 3
fpu   : yes
fpu\_exception : yes
cpuid level : 13
wp  : yes
flags  : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc aperfmperf eagerfpu pni pclmulqdq dtes64 monitor ds\_cpl vmx est tm2 ssse3 fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm ida arat epb xsaveopt pln pts dtherm tpr\_shadow vnmi flexpriority ept vpid fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid
bogomips : 4389.55
clflush size  : 64
cache\_alignment : 64
address sizes : 39 bits physical, 48 bits virtual
power management:
processor  : 6
vendor\_id  : GenuineIntel
cpu family : 6
model  : 60
model name : Intel(R) Core(TM) i7-4702HQ CPU @ 2.20GHz
stepping : 3
microcode  : 0x12
cpu MHz  : 3160.781
cache size : 6144 KB
physical id : 0
siblings : 8
core id  : 2
cpu cores  : 4
apicid  : 5
initial apicid  : 5
fpu   : yes
fpu\_exception : yes
cpuid level : 13
wp  : yes
flags  : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc aperfmperf eagerfpu pni pclmulqdq dtes64 monitor ds\_cpl vmx est tm2 ssse3 fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm ida arat epb xsaveopt pln pts dtherm tpr\_shadow vnmi flexpriority ept vpid fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid
bogomips : 4389.55
clflush size  : 64
cache\_alignment : 64
address sizes : 39 bits physical, 48 bits virtual
power management:
processor  : 7
vendor\_id  : GenuineIntel
cpu family : 6
model  : 60
model name : Intel(R) Core(TM) i7-4702HQ CPU @ 2.20GHz
stepping : 3
microcode  : 0x12
cpu MHz  : 3124.171
cache size : 6144 KB
physical id : 0
siblings : 8
core id  : 3
cpu cores  : 4
apicid  : 7
initial apicid  : 7
fpu   : yes
fpu\_exception : yes
cpuid level : 13
wp  : yes
flags  : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc aperfmperf eagerfpu pni pclmulqdq dtes64 monitor ds\_cpl vmx est tm2 ssse3 fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm ida arat epb xsaveopt pln pts dtherm tpr\_shadow vnmi flexpriority ept vpid fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid
bogomips : 4389.55
clflush size  : 64
cache\_alignment : 64
address sizes : 39 bits physical, 48 bits virtual
power management:
