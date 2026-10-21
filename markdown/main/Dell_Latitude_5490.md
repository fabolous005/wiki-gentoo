<!-- source: https://wiki.gentoo.org/wiki/Dell_Latitude_5490 | group: Gentoo Wiki (Main) | wiki-title: Dell Latitude 5490 -->
---
title: Dell Latitude 5490
url: https://wiki.gentoo.org/wiki/Dell_Latitude_5490
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-20"
fingerprint: "7f5aae6b7f22b769"
license: CC BY-SA 4.0
---

# Dell Latitude 5490

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The Dell Latitude 5490 is a 14" laptop with 8th generation Intel Core i5/i7 CPU (including an integrated GPU) and optionally a dedicated NVIDIA GPU. This laptop was Linux-friendly since day 1, including BIOS and firmware updates from GNU/Linux.

Have a USB-C PD port for battery charging and Dell specific 19V round plug. Original charger is 65W, a charger conformance detection is performed by BIOS at boot.

## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | Intel(R) Core(TM) i5-8250U CPU @ 1.60GHz | Works | N/A | N/A | 6.18.57 | Multiple CPU models can be selected on Dell Configurator | 
| Graphics | Intel Corporation UHD Graphics 620 \[8086:5917\] (rev 07) | Works | 8086:5917 | i915 | 6.18.57 | integrated GPU, vkcube shows (KBL GT2) detail on GPU selection | 
| WiFi card | Intel Corporation Wireless 8265 / 8275 | Works | 8086:24fd | iwlwifi | 6.18.57 | iwlwifi 0000:02:00.0: loaded firmware version 36.ca7b901d.0 8265-36.ucode op\_mode iwlmvm | 
| Sound | Intel Corporation Sunrise Point-LP HD Audio | Works | 8086:9d71 | snd\_hda\_intel | 6.18.57 |  | 
| Ethernet adapter | Intel Corporation Ethernet Connection (4) I219-LM | Works | 8086:15d7 | e1000e | 6.18.57 | 100/1000 Mbit/s | 
| Integrated Webcam | Microdia Integrated\_Webcam\_HD (USB) | Works | 0c45:6717 | (USB UVC) | 6.18.57 |  | 

### Detailed information

`root #``uname -r`
Linux version 6.18.54-gentoo-dist-bin (root@devbox.amd64.dev.gentoo.org) (x86\_64-pc-linux-gnu-gcc (Gentoo 15.3.0 p8) 15.3.0, GNU ld (Gentoo 2.46.1 p1) 2.46.1) #1 SMP PREEMPT\_DYNAMIC Fri Sep 25 16:20:25 -00 2026

`root #``lscpu````
Architecture:                x86_64
  CPU op-mode(s):            32-bit, 64-bit
  Address sizes:             39 bits physical, 48 bits virtual
  Byte Order:                Little Endian
CPU(s):                      8
  On-line CPU(s) list:       0-3
  Off-line CPU(s) list:      4-7
Vendor ID:                   GenuineIntel
  Model name:                Intel(R) Core(TM) i5-8250U CPU @ 1.60GHz
    CPU family:              6
    Model:                   142
    Thread(s) per core:      1
    Core(s) per socket:      4
    Socket(s):               1
    Stepping:                10
    Microcode version:       0xf6
    CPU(s) scaling MHz:      42%
    CPU max MHz:             3400.0000
    CPU min MHz:             400.0000
    BogoMIPS:                3600.00
    Flags:                   fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb
                              rdtscp lm constant_tsc art arch_perfmon pebs bts rep_good nopl xtopology nonstop_tsc cpuid aperfmperf pni pclmulqdq dtes64 monitor ds_cpl
                              vmx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand lahf_lm abm
                              3dnowprefetch cpuid_fault epb pti ssbd ibrs ibpb stibp tpr_shadow flexpriority ept vpid ept_ad fsgsbase tsc_adjust bmi1 avx2 smep bmi2 er
                             ms invpcid mpx rdseed adx smap clflushopt intel_pt xsaveopt xsavec xgetbv1 xsaves dtherm ida arat pln pts hwp hwp_notify hwp_act_window hw
                             p_epp vnmi md_clear flush_l1d arch_capabilities
Virtualization features:     
  Virtualization:            VT-x
Caches (sum of all):         
  L1d:                       128 KiB (4 instances)
  L1i:                       128 KiB (4 instances)
  L2:                        1 MiB (4 instances)
  L3:                        6 MiB (1 instance)
NUMA:                        
  NUMA node(s):              1
  NUMA node0 CPU(s):         0-3
Vulnerabilities:             
  Gather data sampling:      Mitigation; Microcode
  Indirect target selection: Not affected
  Itlb multihit:             KVM: Mitigation: Split huge pages
  L1tf:                      Mitigation; PTE Inversion; VMX conditional cache flushes, SMT disabled
  Mds:                       Mitigation; Clear CPU buffers; SMT disabled
  Meltdown:                  Mitigation; PTI
  Mmio stale data:           Mitigation; Clear CPU buffers; SMT disabled
  Reg file data sampling:    Not affected
  Retbleed:                  Mitigation; IBRS
  Spec rstack overflow:      Not affected
  Spec store bypass:         Mitigation; Speculative Store Bypass disabled via prctl
  Spectre v1:                Mitigation; usercopy/swapgs barriers and __user pointer sanitization
  Spectre v2:                Mitigation; IBRS; IBPB conditional; RSB filling; PBRSB-eIBRS Not affected; BHI Not affected
  Srbds:                     Mitigation; Microcode
  Tsa:                       Not affected
  Tsx async abort:           Not affected
  Vmscape:                   Mitigation; IBPB before exit to userspace
```
`root #``lspci -nnk````
00:00.0 Host bridge [0600]: Intel Corporation Xeon E3-1200 v6/7th Gen Core Processor Host Bridge/DRAM Registers [8086:5914] (rev 08)
        Subsystem: Dell Device [1028:0816]
        Kernel driver in use: skl_uncore
lspci: Unable to load libkmod resources: error -2
00:02.0 VGA compatible controller [0300]: Intel Corporation UHD Graphics 620 [8086:5917] (rev 07)
        DeviceName:  Onboard IGD
        Subsystem: Dell Device [1028:0816]
        Kernel driver in use: i915
00:04.0 Signal processing controller [1180]: Intel Corporation Xeon E3-1200 v5/E3-1500 v5/6th Gen Core Processor Thermal Subsystem [8086:1903] (rev 08)
        Subsystem: Dell Device [1028:0816]
        Kernel driver in use: proc_thermal
00:14.0 USB controller [0c03]: Intel Corporation Sunrise Point-LP USB 3.0 xHCI Controller [8086:9d2f] (rev 21)
        Subsystem: Dell Device [1028:0816]
        Kernel driver in use: xhci_hcd
00:14.2 Signal processing controller [1180]: Intel Corporation Sunrise Point-LP Thermal subsystem [8086:9d31] (rev 21)
        Subsystem: Dell Device [1028:0816]
        Kernel driver in use: intel_pch_thermal
00:15.0 Signal processing controller [1180]: Intel Corporation Sunrise Point-LP Serial IO I2C Controller #0 [8086:9d60] (rev 21)
        Subsystem: Dell Device [1028:0816]
        Kernel driver in use: intel-lpss
00:15.1 Signal processing controller [1180]: Intel Corporation Sunrise Point-LP Serial IO I2C Controller #1 [8086:9d61] (rev 21)
        Subsystem: Dell Device [1028:0816]
        Kernel driver in use: intel-lpss
00:16.0 Communication controller [0780]: Intel Corporation Sunrise Point-LP CSME HECI #1 [8086:9d3a] (rev 21)
        Subsystem: Dell Device [1028:0816]
        Kernel driver in use: mei_me
00:17.0 RAID bus controller [0104]: Intel Corporation 82801 Mobile SATA Controller [RAID mode] [8086:282a] (rev 21)
        Subsystem: Dell Device [1028:0816]
        Kernel driver in use: ahci
00:1c.0 PCI bridge [0604]: Intel Corporation Sunrise Point-LP PCI Express Root Port #1 [8086:9d10] (rev f1)
        Subsystem: Dell Device [1028:0816]
        Kernel driver in use: pcieport
00:1c.2 PCI bridge [0604]: Intel Corporation Sunrise Point-LP PCI Express Root Port #3 [8086:9d12] (rev f1)
        Subsystem: Dell Device [1028:0816]
        Kernel driver in use: pcieport
00:1f.0 ISA bridge [0601]: Intel Corporation Sunrise Point LPC/eSPI Controller [8086:9d4e] (rev 21)
        Subsystem: Dell Device [1028:0816]
00:1f.2 Memory controller [0580]: Intel Corporation Sunrise Point-LP PMC [8086:9d21] (rev 21)
        Subsystem: Dell Device [1028:0816]
00:1f.3 Audio device [0403]: Intel Corporation Sunrise Point-LP HD Audio [8086:9d71] (rev 21)
        Subsystem: Dell Device [1028:0816]
        Kernel driver in use: snd_hda_intel
00:1f.4 SMBus [0c05]: Intel Corporation Sunrise Point-LP SMBus [8086:9d23] (rev 21)
        Subsystem: Dell Device [1028:0816]
        Kernel driver in use: i801_smbus
00:1f.6 Ethernet controller [0200]: Intel Corporation Ethernet Connection (4) I219-LM [8086:15d7] (rev 21)
        Subsystem: Dell Device [1028:0816]
        Kernel driver in use: e1000e
01:00.0 Unassigned class [ff00]: Realtek Semiconductor Co., Ltd. RTS525A PCI Express Card Reader [10ec:525a] (rev 01)
        Subsystem: Dell Device [1028:0816]
        Kernel driver in use: rtsx_pci
02:00.0 Network controller [0280]: Intel Corporation Wireless 8265 / 8275 [8086:24fd] (rev 78)
        Subsystem: Intel Corporation Device [8086:0050]
        Kernel driver in use: iwlwifi
```
`root #``lsusb`
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 002: ID 0c45:6717 Microdia Integrated\_Webcam\_HD
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub

`root #``lsusb -vt````
/:  Bus 001.Port 001: Dev 001, Class=root_hub, Driver=xhci_hcd/12p, 480M
    ID 1d6b:0002 Linux Foundation 2.0 root hub
    |__ Port 005: Dev 002, 480M
        ID 0c45:6717 Microdia 
/:  Bus 002.Port 001: Dev 001, Class=root_hub, Driver=xhci_hcd/6p, 5000M
    ID 1d6b:0003 Linux Foundation 3.0 root hub
```
