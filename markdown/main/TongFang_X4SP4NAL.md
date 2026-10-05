<!-- source: https://wiki.gentoo.org/wiki/TongFang_X4SP4NAL | group: Gentoo Wiki (Main) | wiki-title: TongFang X4SP4NAL -->
---
title: TongFang X4SP4NAL
url: https://wiki.gentoo.org/wiki/TongFang_X4SP4NAL
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-01"
fingerprint: "7e023cd7e3b2356d"
license: CC BY-SA 4.0
---

# TongFang X4SP4NAL

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The X4SP4NAL is a 14 inch barebone laptop manufactured by the Chinese original design manufacturer Tsinghua Tongfang Co. Ltd. The hardware serves as the basis for other laptop models, such as the XMG EVO 14 (E25) or the Tuxedo InfinityBook 14 Gen10.

## Hardware

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | AMD Ryzen AI 9 HX 370 |  | N/A | N/A | 6.18.2 |  | 
| integrated GPU | Radeon 890M |  | 1002:150e | amdgpu | 6.18.2 |  | 
| NPU | Strix Point Neural Processing Unit |  | 1022:17f0 | amdxdna | 6.18.2 |  | 
| Ethernet | Motorcomm YT6801 |  | 1f0a:6801 | N/A | N/A | Driver not included in Linux kernel | 
| Wifi and Bluetooth | AMD Wi-Fi 6E RZ616 |  | 14c3:7922 | mt7921e, btusb | 6.18.2 |  | 
| SD card reader | Genesys Logic GL9767 |  | 17a0:9767 | sdhci-pci | 6.18.2 |  | 
| Webcam | Kingcome FHD WebCam |  | 2b7e:c906 | uvcvideo | 6.18.2 |  | 
| Speakers and Microphone | Realtek ALC245 |  | 1022:15e3 | snd\_hda\_intel | 6.18.2 |  | 
| Keyboard | N/A |  | N/A | atkbd | 6.18.2 | Regular keys work. Fn buttons and backlight control expected to work starting with kernel 6.19 | 
| Touchpad | Uniwill |  | N/A | hid\_multitouch, i2c\_hid\_acpi, i2c\_designware\_platform | 6.18.2 |  | 

### Alternative Components

The TongFang X4SP5NAL is a barebone laptop that is available in different configurations. Alternatives to the above components and their working status are tracked in the following table.



| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| APU | AMD Ryzen AI 9 365 |  | N/A | N/A | N/A |  | 
| Wifi and Bluetooth | Intel Wi-Fi 6E AX210 |  | N/A | N/A | N/A |  | 

### Detailed information

`root #``uname -r`
6.18.2-gentoo-gentoo-dist

`root #``lscpu````
Architecture:                x86_64
  CPU op-mode(s):            32-bit, 64-bit
  Address sizes:             48 bits physical, 48 bits virtual
  Byte Order:                Little Endian
CPU(s):                      24
  On-line CPU(s) list:       0-23
Vendor ID:                   AuthenticAMD
  Model name:                AMD Ryzen AI 9 HX 370 w/ Radeon 890M
    CPU family:              26
    Model:                   36
    Thread(s) per core:      2
    Core(s) per socket:      12
    Socket(s):               1
    Stepping:                0
    Frequency boost:         enabled
    CPU(s) scaling MHz:      34%
    CPU max MHz:             5157.8950
    CPU min MHz:             605.2640
    BogoMIPS:                3993.61
    Flags:                   fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr s
                             se sse2 ht syscall nx mmxext fxsr_opt pdpe1gb rdtscp lm constant_tsc rep_good amd_lbr_v2 n
                             opl xtopology nonstop_tsc cpuid extd_apicid aperfmperf rapl pni pclmulqdq monitor ssse3 fm
                             a cx16 sse4_1 sse4_2 movbe popcnt aes xsave avx f16c rdrand lahf_lm cmp_legacy svm extapic
                              cr8_legacy abm sse4a misalignsse 3dnowprefetch osvw ibs skinit wdt tce topoext perfctr_co
                             re perfctr_nb bpext perfctr_llc mwaitx cpuid_fault cpb cat_l3 cdp_l3 hw_pstate ssbd mba pe
                             rfmon_v2 ibrs ibpb stibp ibrs_enhanced vmmcall fsgsbase tsc_adjust bmi1 avx2 smep bmi2 erm
                             s invpcid cqm rdt_a avx512f avx512dq rdseed adx smap avx512ifma clflushopt clwb avx512cd s
                             ha_ni avx512bw avx512vl xsaveopt xsavec xgetbv1 xsaves cqm_llc cqm_occup_llc cqm_mbm_total
                              cqm_mbm_local user_shstk avx_vnni avx512_bf16 clzero irperf xsaveerptr rdpru wbnoinvd cpp
                             c arat npt lbrv svm_lock nrip_save tsc_scale vmcb_clean flushbyasid decodeassists pausefil
                             ter pfthreshold avic v_vmsave_vmload vgif x2avic v_spec_ctrl vnmi avx512vbmi umip pku ospk
                             e avx512_vbmi2 gfni vaes vpclmulqdq avx512_vnni avx512_bitalg avx512_vpopcntdq rdpid bus_l
                             ock_detect movdiri movdir64b overflow_recov succor smca fsrm avx512_vp2intersect flush_l1d
                              amd_lbr_pmc_freeze
Virtualization features:     
  Virtualization:            AMD-V
Caches (sum of all):         
  L1d:                       576 KiB (12 instances)
  L1i:                       384 KiB (12 instances)
  L2:                        12 MiB (12 instances)
  L3:                        24 MiB (2 instances)
NUMA:                        
  NUMA node(s):              1
  NUMA node0 CPU(s):         0-23
Vulnerabilities:             
  Gather data sampling:      Not affected
  Ghostwrite:                Not affected
  Indirect target selection: Not affected
  Itlb multihit:             Not affected
  L1tf:                      Not affected
  Mds:                       Not affected
  Meltdown:                  Not affected
  Mmio stale data:           Not affected
  Old microcode:             Not affected
  Reg file data sampling:    Not affected
  Retbleed:                  Not affected
  Spec rstack overflow:      Mitigation; IBPB on VMEXIT only
  Spec store bypass:         Mitigation; Speculative Store Bypass disabled via prctl
  Spectre v1:                Mitigation; usercopy/swapgs barriers and __user pointer sanitization
  Spectre v2:                Mitigation; Enhanced / Automatic IBRS; IBPB conditional; STIBP always-on; PBRSB-eIBRS Not 
                             affected; BHI Not affected
  Srbds:                     Not affected
  Tsa:                       Not affected
  Tsx async abort:           Not affected
  Vmscape:                   Mitigation; IBPB on VMEXIT
```
`root #``lspci -nnk````
00:00.0 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Strix/Strix Halo Root Complex [1022:1507]
        Subsystem: AIstone Global Limited Device [1d05:5006]
00:00.2 IOMMU [0806]: Advanced Micro Devices, Inc. [AMD] Strix/Strix Halo IOMMU [1022:1508]
        Subsystem: AIstone Global Limited Device [1d05:5006]
00:01.0 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Strix/Strix Halo Dummy Host Bridge [1022:1509]
00:01.2 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Strix/Strix Halo PCIe USB4 Bridge [1022:150a]
        Subsystem: Advanced Micro Devices, Inc. [AMD] Strix/Strix Halo PCIe USB4 Bridge [1022:150a]
        Kernel driver in use: pcieport
        Kernel modules: shpchp
00:02.0 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Strix/Strix Halo Dummy Host Bridge [1022:1509]
00:02.2 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Strix/Strix Halo GPP Bridge [1022:150b]
        Subsystem: AIstone Global Limited Device [1d05:5006]
        Kernel driver in use: pcieport
        Kernel modules: shpchp
00:02.3 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Strix/Strix Halo GPP Bridge [1022:150b]
        Subsystem: AIstone Global Limited Device [1d05:5006]
        Kernel driver in use: pcieport
        Kernel modules: shpchp
00:02.4 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Strix/Strix Halo GPP Bridge [1022:150b]
        Subsystem: AIstone Global Limited Device [1d05:5006]
        Kernel driver in use: pcieport
        Kernel modules: shpchp
00:03.0 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Strix/Strix Halo Dummy Host Bridge [1022:1509]
00:03.2 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Strix/Strix Halo GPP Bridge [1022:150b]
        Subsystem: AIstone Global Limited Device [1d05:5006]
        Kernel driver in use: pcieport
        Kernel modules: shpchp
00:08.0 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Strix/Strix Halo Dummy Host Bridge [1022:1509]
00:08.1 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Strix/Strix Halo Internal GPP Bridge to Bus [C:A] [1022:150c]
        Subsystem: AIstone Global Limited Device [1d05:5006]
        Kernel driver in use: pcieport
        Kernel modules: shpchp
00:08.2 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Strix/Strix Halo Internal GPP Bridge to Bus [C:A] [1022:150c]
        Subsystem: AIstone Global Limited Device [1d05:5006]
        Kernel driver in use: pcieport
        Kernel modules: shpchp
00:08.3 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Strix/Strix Halo Internal GPP Bridge to Bus [C:A] [1022:150c]
pcilib: Error reading /sys/bus/pci/devices/0000:00:08.3/label: Operation not permitted
        Subsystem: AIstone Global Limited Device [1d05:5006]
        Kernel driver in use: pcieport
        Kernel modules: shpchp
00:14.0 SMBus [0c05]: Advanced Micro Devices, Inc. [AMD] FCH SMBus Controller [1022:790b] (rev 71)
        Subsystem: AIstone Global Limited Device [1d05:5006]
        Kernel driver in use: piix4_smbus
        Kernel modules: i2c_piix4
00:14.3 ISA bridge [0601]: Advanced Micro Devices, Inc. [AMD] FCH LPC Bridge [1022:790e] (rev 51)
        Subsystem: AIstone Global Limited Device [1d05:5006]
00:18.0 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Strix Data Fabric; Function 0 [1022:16f8]
00:18.1 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Strix Data Fabric; Function 1 [1022:16f9]
00:18.2 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Strix Data Fabric; Function 2 [1022:16fa]
00:18.3 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Strix Data Fabric; Function 3 [1022:16fb]
        Kernel driver in use: k10temp
        Kernel modules: k10temp
00:18.4 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Strix Data Fabric; Function 4 [1022:16fc]
00:18.5 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Strix Data Fabric; Function 5 [1022:16fd]
00:18.6 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Strix Data Fabric; Function 6 [1022:16fe]
00:18.7 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Strix Data Fabric; Function 7 [1022:16ff]
61:00.0 SD Host controller [0805]: Genesys Logic, Inc GL9767 SD Host Controller [17a0:9767] (rev 03)
        Subsystem: AIstone Global Limited Device [1d05:5006]
        Kernel driver in use: sdhci-pci
        Kernel modules: sdhci_pci
62:00.0 Network controller [0280]: MEDIATEK Corp. MT7922 802.11ax PCI Express Wireless Network Adapter [14c3:7922]
        Subsystem: AzureWave Device [1a3b:5911]
        Kernel driver in use: mt7921e
        Kernel modules: mt7921e
63:00.0 Ethernet controller [0200]: Motorcomm Microelectronics. YT6801 Gigabit Ethernet Controller [1f0a:6801] (rev 01)
        DeviceName: Realtek Ethernet
        Subsystem: AIstone Global Limited Device [1d05:7011]
64:00.0 Non-Volatile memory controller [0108]: Sandisk Corp WD Black SN850X NVMe SSD [15b7:5030] (rev 01)
        Subsystem: Sandisk Corp WD Black SN850X NVMe SSD [15b7:5030]
        Kernel driver in use: nvme
        Kernel modules: nvme
65:00.0 Display controller [0380]: Advanced Micro Devices, Inc. [AMD/ATI] Strix [Radeon 880M / 890M] [1002:150e] (rev c1)
        Subsystem: AIstone Global Limited Device [1d05:5006]
        Kernel driver in use: amdgpu
        Kernel modules: amdgpu
65:00.1 Audio device [0403]: Advanced Micro Devices, Inc. [AMD/ATI] Radeon High Definition Audio Controller [Rembrandt/Strix] [1002:1640]
        Subsystem: AIstone Global Limited Device [1d05:5006]
        Kernel driver in use: snd_hda_intel
        Kernel modules: snd_hda_intel
65:00.2 Encryption controller [1080]: Advanced Micro Devices, Inc. [AMD] Strix/Krackan/Strix Halo CCP/ASP [1022:17e0]
        Subsystem: AIstone Global Limited Device [1d05:5006]
        Kernel driver in use: ccp
        Kernel modules: ccp
65:00.4 USB controller [0c03]: Advanced Micro Devices, Inc. [AMD] Device [1022:151e]
        Subsystem: AIstone Global Limited Device [1d05:5006]
        Kernel driver in use: xhci_hcd
        Kernel modules: xhci_pci
65:00.5 Multimedia controller [0480]: Advanced Micro Devices, Inc. [AMD] Audio Coprocessor [1022:15e2] (rev 70)
        Subsystem: AIstone Global Limited Device [1d05:5006]
        Kernel driver in use: snd_acp_pci
        Kernel modules: snd_pci_acp3x, snd_rn_pci_acp3x, snd_pci_acp5x, snd_pci_acp6x, snd_acp_pci, snd_rpl_pci_acp6x, snd_pci_ps
65:00.6 Audio device [0403]: Advanced Micro Devices, Inc. [AMD] Family 17h/19h/1ah HD Audio Controller [1022:15e3]
        DeviceName: Realtek ALC245
        Subsystem: AIstone Global Limited Device [1d05:3011]
        Kernel driver in use: snd_hda_intel
        Kernel modules: snd_hda_intel
66:00.0 Non-Essential Instrumentation [1300]: Advanced Micro Devices, Inc. [AMD] Strix/Strix Halo PCIe Dummy Function [1022:150d]
        Subsystem: AIstone Global Limited Device [1d05:5006]
66:00.1 Signal processing controller [1180]: Advanced Micro Devices, Inc. [AMD] Strix/Krackan/Strix Halo Neural Processing Unit [1022:17f0] (rev 10)
        Subsystem: AIstone Global Limited Device [1d05:5006]
        Kernel driver in use: amdxdna
        Kernel modules: amdxdna
67:00.0 USB controller [0c03]: Advanced Micro Devices, Inc. [AMD] Device [1022:151f]
        Subsystem: AIstone Global Limited Device [1d05:5006]
        Kernel driver in use: xhci_hcd
        Kernel modules: xhci_pci
67:00.3 USB controller [0c03]: Advanced Micro Devices, Inc. [AMD] Device [1022:151a]
        Subsystem: AIstone Global Limited Device [1d05:5006]
        Kernel driver in use: xhci_hcd
        Kernel modules: xhci_pci
67:00.4 USB controller [0c03]: Advanced Micro Devices, Inc. [AMD] Device [1022:151b]
        Subsystem: AIstone Global Limited Device [1d05:5006]
        Kernel driver in use: xhci_hcd
        Kernel modules: xhci_pci
67:00.6 USB controller [0c03]: Advanced Micro Devices, Inc. [AMD] USB4 Router 1 [1022:151d]
        Subsystem: AIstone Global Limited Device [1d05:5006]
        Kernel driver in use: thunderbolt
        Kernel modules: thunderbolt
```
`root #``lsusb`
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 002: ID 2b7e:c906 Kingcome FHD WebCam
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 003 Device 002: ID 05e3:0610 Genesys Logic, Inc. Hub
Bus 003 Device 003: ID 13d3:3585 IMC Networks Wireless\_Device
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 004 Device 002: ID 05e3:0620 Genesys Logic, Inc. GL3523 Hub
Bus 005 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 006 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 007 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 008 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub

`root #``lsusb -vt````
/:  Bus 001.Port 001: Dev 001, Class=root_hub, Driver=xhci_hcd/1p, 480M
    ID 1d6b:0002 Linux Foundation 2.0 root hub
    |__ Port 001: Dev 002, If 0, Class=Video, Driver=uvcvideo, 480M
        ID 2b7e:c906  
    |__ Port 001: Dev 002, If 1, Class=Video, Driver=uvcvideo, 480M
        ID 2b7e:c906  
    |__ Port 001: Dev 002, If 2, Class=Video, Driver=uvcvideo, 480M
        ID 2b7e:c906  
    |__ Port 001: Dev 002, If 3, Class=Video, Driver=uvcvideo, 480M
        ID 2b7e:c906  
    |__ Port 001: Dev 002, If 4, Class=Application Specific Interface, Driver=[none], 480M
        ID 2b7e:c906  
/:  Bus 002.Port 001: Dev 001, Class=root_hub, Driver=xhci_hcd/1p, 10000M
    ID 1d6b:0003 Linux Foundation 3.0 root hub
/:  Bus 003.Port 001: Dev 001, Class=root_hub, Driver=xhci_hcd/5p, 480M
    ID 1d6b:0002 Linux Foundation 2.0 root hub
    |__ Port 001: Dev 002, If 0, Class=Hub, Driver=hub/2p, 480M
        ID 05e3:0610 Genesys Logic, Inc. Hub
    |__ Port 005: Dev 003, If 0, Class=Wireless, Driver=btusb, 480M
        ID 13d3:3585 IMC Networks 
    |__ Port 005: Dev 003, If 1, Class=Wireless, Driver=btusb, 480M
        ID 13d3:3585 IMC Networks 
    |__ Port 005: Dev 003, If 2, Class=Wireless, Driver=btusb, 480M
        ID 13d3:3585 IMC Networks 
/:  Bus 004.Port 001: Dev 001, Class=root_hub, Driver=xhci_hcd/2p, 10000M
    ID 1d6b:0003 Linux Foundation 3.0 root hub
    |__ Port 001: Dev 002, If 0, Class=Hub, Driver=hub/2p, 5000M
        ID 05e3:0620 Genesys Logic, Inc. GL3523 Hub
/:  Bus 005.Port 001: Dev 001, Class=root_hub, Driver=xhci_hcd/1p, 480M
    ID 1d6b:0002 Linux Foundation 2.0 root hub
/:  Bus 006.Port 001: Dev 001, Class=root_hub, Driver=xhci_hcd/1p, 10000M
    ID 1d6b:0003 Linux Foundation 3.0 root hub
/:  Bus 007.Port 001: Dev 001, Class=root_hub, Driver=xhci_hcd/1p, 480M
    ID 1d6b:0002 Linux Foundation 2.0 root hub
/:  Bus 008.Port 001: Dev 001, Class=root_hub, Driver=xhci_hcd/1p, 10000M
    ID 1d6b:0003 Linux Foundation 3.0 root hub
```
## Installation

**`/etc/portage/package.use/00cpu-flags`**

**`/etc/portage/package.use/00video-cards`**

### Firmware

All necessary firmware is available via the [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) package.

`root #``emerge sys-kernel/linux-firmware`
#### iGPU

amdgpu/psp\_14\_0\_1\_toc.bin
amdgpu/psp\_14\_0\_1\_ta.bin
amdgpu/dcn\_3\_5\_1\_dmcub.bin
amdgpu/gc\_11\_5\_1pfp.bin
amdgpu/gc\_11\_5\_1me.bin
amdgpu/gc\_11\_5\_1rlc.bin
amdgpu/gc\_11\_5\_1mec.bin
amdgpu/gc\_11\_5\_1imu.bin
amdgpu/sdma\_6\_1\_1.bin
amdgpu/vcn\_4\_0\_6.bin
amdgpu/vcn\_4\_0\_6\_1.bin
amdgpu/gc\_11\_5\_1\_mes\_2.bin
amdgpu/gc\_11\_5\_1\_mes1.bin

#### NPU (optional)

If you intend to compile NPU support into the kernel, instead of compiling it as a module, the following firmware blob needs to be included.

amdnpu/17f0\_10/npu.sbin



#### AMD Wi-Fi 6E RZ616 (optional)

If you intend to compile Wi-Fi and Bluetooth into the kernel, instead of compiling them as modules, the following firmware blobs need to be included.

mediatek/BT\_RAM\_CODE\_MT7922\_1\_1\_hdr.bin
mediatek/WIFI\_RAM\_CODE\_MT7922\_1.bin
mediatek/WIFI\_MT7922\_patch\_mcu\_1\_1\_hdr.bin
regulatory.db
regulatory.db.p7s

### Kernel

**Enable support for these hardware drivers**

-\*- Networking support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NET\</code> to find this item. --->
  \<M> Bluetooth subsystem support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_BT\</code> to find this item. --->
    \[\*\] Bluetooth Classic (BR/EDR) features [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_BT\_BREDR\</code> to find this item.
      \<M> RFCOMM protocol support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_BT\_RFCOMM\</code> to find this item.
      \<M> HIDP protocol support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_BT\_HIDP\</code> to find this item.
    \[\*\] Bluetooth Low Energy (LE) features [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_BT\_LE\</code> to find this item.
    Bluetooth device drivers --->
      \[\*\] HCI USB driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_BT\_HCIUSB\</code> to find this item.
        \[\*\] Enable USB autosuspend for Bluetooth USB devices by default [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_BT\_HCIBTUSB\_AUTOSUSPEND\</code> to find this item.
        \[\*\] MediaTek protocol support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_BT\_HCIBTUSB\_MTK\</code> to find this item.
  -\*- Wireless [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_WIRELESS\</code> to find this item. --->
    \[M\] cfg80211 - wireless configuration API [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CFG80211\</code> to find this item.
      \[\*\] enable powersave by default [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CFG80211\_DEFAULT\_PS\</code> to find this item.
    \[M\] Generic IEEE 802.11 Networking Stack (mac80211) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_MAC80211\</code> to find this item.
  \[\*\] RF switch subsystem support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_RFKILL\</code> to find this item. --->
Device Drivers --->
  \[\*\] Network device support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NETDEVICES\</code> to find this item. --->
    \[\*\] Wireless LAN [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_WLAN\</code> to find this item. --->   
      \[\*\] MediaTek devices [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_WLAN\_VENDOR\_MEDIATEK\</code> to find this item.
        \<M> MediaTek MT7921E (PCIe) support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_MT7921E\</code> to find this item.
  Input device support --->
    -\*- Generic input layer (needed for keyboard, mouse, ...) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_INPUT\</code> to find this item.
      \[\*\] Keyboards [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_INPUT\_KEYBOARD\</code> to find this item. --->
        \<\*> AT keyboard [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_KEYBOARD\_ATKBD\</code> to find this item.
  \[\*\] Watchdog Timer Support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_WATCHDOG\</code> to find this item. --->
      \<\*> AMD/ATI SP5100 TCO Timer/Watchdog [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SP5100\_TCO\</code> to find this item.
  \[\*\] I2C support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_I2C\</code> to find this item. --->
    I2C Hardware Bus support --->
      \<\*> Intel PIIX4 and compatible (ATI/AMD/Serverworks/Broadcom/SMSC) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_I2C\_PIIX4\</code> to find this item.
      \<\*> Synopsys DesignWare I2C adapter [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_I2C\_DESIGNWARE\_CORE\</code> to find this item.
      \<\*> Synopsys DesignWare Platform driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_I2C\_DESIGNWARE\_PLATFORM\</code> to find this item.
  \<\*> Multimedia support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_MEDIA\_SUPPORT\</code> to find this item. --->
    \[\*\] Filter media drivers [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_MEDIA\_SUPPORT\_FILTER\</code> to find this item.
    Media device types --->
      \[\*\] Cameras and video grabbers [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CAMERA\_SUPPORT\</code> to find this item.
    Video4Linux options --->
      \<\*> V4L2 flash API for LED flash class devices [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_V4L2\_FLASH\_LED\_CLASS\</code> to find this item.
    Media drivers  ---> 
      \[\*\] Media USB Adapters [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_MEDIA\_USB\_SUPPORT\</code> to find this item. ---> 
        \<\*> USB Video Class (UVC) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_USB\_VIDEO\_CLASS\</code> to find this item.
        \[\*\] UVC input events device support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_USB\_VIDEO\_CLASS\_INPUT\_EVDEV\</code> to find this item.
  \[\*\] Compute Acceleration Framework [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_DRM\_ACCEL\</code> to find this item. --->
    \<M> AMD AI Engine [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_DRM\_ACCEL\_AMDXDNA\</code> to find this item. --->
  \<\*> HID bus support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_HID\_SUPPORT\</code> to find this item. --->
    \<\*> HID bus core support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_HID\</code> to find this item.
      Special HID drivers --->
        \<\*> HID Multitouch panels [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_HID\_MULTITOUCH\</code> to find this item. --->
      \<\*> I2C HID support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_I2C\_HID\</code> to find this item. --->
        \<\*> HID over I2C transport layer ACPI driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_I2C\_HID\_ACPI\</code> to find this item. --->
  \<\*> MMC/SD/SDIO card support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_MMC\</code> to find this item. --->
    \<\*> MMC block device driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_MMC\_BLOCK\</code> to find this item.
    \<\*> Secure Digital Host Controller Interface support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_MMC\_SDHCI\</code> to find this item.
      \<\*> SDHCI support on PCI bus [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_MMC\_SDHCI\_PCI\</code> to find this item.
  LED Support --->
    {\*} LED Class Support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_LEDS\_CLASS\</code> to find this item.
    \<\*> LED Flash Class Support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_LEDS\_CLASS\_FLASH\</code> to find this item.
Cryptographic API --->
  \[\*\] Hardware crypto devices [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CRYPTO\_HW\</code> to find this item. --->
    \[\*\] Support for AMD Secure Processor [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CRYPTO\_DEV\_CCP\</code> to find this item.
      \<\*> Secure Processor device driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CRYPTO\_DEV\_CCP\_DD\</code> to find this item.
        \[\*\] Cryptographic Coprocessor device [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CRYPTO\_DEV\_SP\_CCP\</code> to find this item.
        \[\*\] Platform Security Processor (PSP) device [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CRYPTO\_DEV\_SP\_PSP\</code> to find this item.

## See also

- [AMDGPU](https://wiki.gentoo.org/wiki/AMDGPU) — the open source graphics drivers for AMD Radeon and other GPUs.
- [Bluetooth](https://wiki.gentoo.org/wiki/Bluetooth) — describes the configuration and usage of Bluetooth controllers and devices.
- [Linux firmware](https://wiki.gentoo.org/wiki/Linux_firmware) — contains [binary blobs](https://en.wikipedia.org/wiki/binary_blobs) of firmware necessary for partial or full functionality of certain hardware devices on Linux systems.
- [Webcam](https://wiki.gentoo.org/wiki/Webcam) — information on setting up and using a **webcam** on Gentoo using [v4l-utils](https://wiki.gentoo.org/wiki/V4l-utils).
- [Wi-Fi](https://wiki.gentoo.org/wiki/Wi-Fi) — describes the setup of a [Wi-Fi](https://en.wikipedia.org/wiki/Wi-Fi) (wireless) network device.
