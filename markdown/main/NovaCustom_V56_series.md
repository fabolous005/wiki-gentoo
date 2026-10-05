<!-- source: https://wiki.gentoo.org/wiki/NovaCustom_V56_series | group: Gentoo Wiki (Main) | wiki-title: NovaCustom V56 series -->
---
title: NovaCustom V56 series
url: https://wiki.gentoo.org/wiki/NovaCustom_V56_series
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-03"
fingerprint: "1e46af0bd33a0506"
license: CC BY-SA 4.0
---

# NovaCustom V56 series

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

NovaCustom V56 comes with Coreboot by default and an option for disabling Intel ME. It comes with Intel Meteor Lake CPU and, optionally, an nVidia RTX 40xx.

## Hardware

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes |  | 
|---|---|---|---|---|---|---|---|
| CPU | Intel core 5 125/155 H |  | N/A | N/A | 7.1.5 |  |  | 
| Video card | Intel Meteor Lake-P |  | 10de:0185 | i945 | 7.1.5 | udev also manages to load the experimental Xe driver - not sure about compatibility. Needs linux-firmware. |  | 
| Intel VPU | ELAN0412 |  |  | intel\_vpu | 7.1.5 | Needs linux-firmware. |  | 
| Sound card | Intel Meteor Lake-P HD Audio |  | 8086:7e28 | snd\_hda\_intel | 7.1.5 |  |  | 
| WiFi adapter | Intel AX211 |  | 8086:7e40 | iwlwifi, iwlmvm | 7.1.5 | Needs linux-firmware. |  | 
| Bluetooth adapter | Intel AX211 |  | 8087:0033 | bluetooth | 7.1.5 | Needs linux-firmware. |  | 
| Touchpad | ELAN0412 |  | 04F3:32EE | hid\_multitouch | 7.1.5 |  |  | 
| Keyboard | Some serial keyboard |  |  | atkbd | 7.1.5 |  |  | 
| Webcam | BisonCam NB Pro |  | 5986:2170 | uvc | 7.1.5 |  |  | 

### Detailed information

`root #``uname -r`
7.1.5-gentoo

`root #``lscpu````
Architecture:                x86_64
  CPU op-mode(s):            32-bit, 64-bit
  Address sizes:             46 bits physical, 48 bits virtual
  Byte Order:                Little Endian
CPU(s):                      18
  On-line CPU(s) list:       0-17
Vendor ID:                   GenuineIntel
  Model name:                Intel(R) Core(TM) Ultra 5 125H
    CPU family:              6
    Model:                   170
    Thread(s) per core:      2
    Core(s) per socket:      14
    Socket(s):               1
    Stepping:                4
    Microcode version:       0x28
    CPU(s) scaling MHz:      18%
    CPU max MHz:             4500.0000
    CPU min MHz:             400.0000
    BogoMIPS:                5992.00
    Flags:                   fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc
                             art arch_perfmon pebs bts rep_good nopl xtopology nonstop_tsc cpuid aperfmperf tsc_known_freq pni pclmulqdq dtes64 monitor ds_cpl vmx smx est tm2 ssse3 sdbg fma c
                             x16 xtpr pdcm pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand lahf_lm abm 3dnowprefetch cpuid_fault epb ssbd ibrs ibpb stibp i
                             brs_enhanced tpr_shadow flexpriority ept vpid ept_ad fsgsbase tsc_adjust bmi1 avx2 smep bmi2 erms invpcid rdseed adx smap clflushopt clwb intel_pt sha_ni xsaveopt
                              xsavec xgetbv1 xsaves split_lock_detect avx_vnni dtherm ida arat pln pts hwp hwp_notify hwp_act_window hwp_epp hwp_pkg_req hfi vnmi umip pku ospke waitpkg gfni v
                             aes vpclmulqdq rdpid bus_lock_detect movdiri movdir64b fsrm md_clear serialize arch_lbr ibt flush_l1d arch_capabilities
Virtualization features:
  Virtualization:            VT-x
Caches (sum of all):
  L1d:                       448 KiB (12 instances)
  L1i:                       768 KiB (12 instances)
  L2:                        14 MiB (7 instances)
  L3:                        18 MiB (1 instance)
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
  Spec rstack overflow:      Not affected
  Spec store bypass:         Mitigation; Speculative Store Bypass disabled via prctl
  Spectre v1:                Mitigation; usercopy/swapgs barriers and __user pointer sanitization
  Spectre v2:                Mitigation; Enhanced / Automatic IBRS; IBPB conditional; PBRSB-eIBRS Not affected; BHI Vulnerable
  Srbds:                     Not affected
  Tsa:                       Not affected
  Tsx async abort:           Not affected
  Vmscape:
```
`root #``lspci -nnk`
00:00.0 Host bridge \[0600\]: Intel Corporation Device \[8086:7d14\] (rev 04)
	Subsystem: CLEVO/KAPOK Computer Device \[1558:a743\]
	Kernel driver in use: igen6\_edac
	Kernel modules: igen6\_edac
00:02.0 VGA compatible controller \[0300\]: Intel Corporation Meteor Lake-P \[Intel Graphics\] \[8086:7dd5\] (rev 08)
	DeviceName: VGA compatible controller
	Subsystem: CLEVO/KAPOK Computer Device \[1558:a743\]
	Kernel driver in use: i915
	Kernel modules: i915
00:04.0 Signal processing controller \[1180\]: Intel Corporation Meteor Lake-P Dynamic Tuning Technology \[8086:7d03\] (rev 04)
	Subsystem: CLEVO/KAPOK Computer Device \[1558:a743\]
	Kernel driver in use: proc\_thermal\_pci
	Kernel modules: processor\_thermal\_device\_pci
00:06.0 PCI bridge \[0604\]: Intel Corporation Arrow Lake-H/U PCIe Root Port \[8086:7ecb\] (rev 10)
	Subsystem: CLEVO/KAPOK Computer Device \[1558:a743\]
	Kernel driver in use: pcieport
	Kernel modules: shpchp
00:07.0 PCI bridge \[0604\]: Intel Corporation Meteor Lake-P Thunderbolt 4 PCI Express Root Port #0 \[8086:7ec4\] (rev 10)
	Subsystem: CLEVO/KAPOK Computer Device \[1558:a743\]
	Kernel driver in use: pcieport
	Kernel modules: shpchp
00:08.0 System peripheral \[0880\]: Intel Corporation Meteor Lake-P Gaussian & Neural-Network Accelerator \[8086:7e4c\] (rev 20)
	Subsystem: CLEVO/KAPOK Computer Device \[1558:a743\]
00:0a.0 Signal processing controller \[1180\]: Intel Corporation Meteor Lake-P Platform Monitoring Technology \[8086:7d0d\] (rev 01)
	Subsystem: CLEVO/KAPOK Computer Device \[1558:a743\]
	Kernel driver in use: intel\_vsec
	Kernel modules: intel\_vsec
00:0b.0 Processing accelerators \[1200\]: Intel Corporation Meteor Lake NPU \[8086:7d1d\] (rev 04)
	Subsystem: CLEVO/KAPOK Computer Device \[1558:a743\]
00:0d.0 USB controller \[0c03\]: Intel Corporation Meteor Lake-P Thunderbolt 4 USB Controller \[8086:7ec0\] (rev 10)
	Subsystem: CLEVO/KAPOK Computer Device \[1558:a743\]
	Kernel driver in use: xhci\_hcd
	Kernel modules: xhci\_pci
00:0d.2 USB controller \[0c03\]: Intel Corporation Meteor Lake-P Thunderbolt 4 NHI #0 \[8086:7ec2\] (rev 10)
	Subsystem: CLEVO/KAPOK Computer Device \[1558:a743\]
	Kernel driver in use: thunderbolt
	Kernel modules: thunderbolt
00:14.0 USB controller \[0c03\]: Intel Corporation Meteor Lake-P USB 3.2 Gen 2x1 xHCI Host Controller \[8086:7e7d\] (rev 20)
	Subsystem: CLEVO/KAPOK Computer Device \[1558:a743\]
	Kernel driver in use: xhci\_hcd
	Kernel modules: xhci\_pci
00:14.2 RAM memory \[0500\]: Intel Corporation Meteor Lake-H/U Shared SRAM \[8086:7e7f\] (rev 20)
	Subsystem: CLEVO/KAPOK Computer Device \[1558:a743\]
	Kernel driver in use: intel\_pmc\_ssram\_telemetry
	Kernel modules: intel\_pmc\_ssram\_telemetry
00:14.3 Network controller \[0280\]: Intel Corporation Meteor Lake PCH CNVi WiFi \[8086:7e40\] (rev 20)
	Subsystem: Intel Corporation Wi-Fi 6E AX211 160MHz \[8086:0094\]
	Kernel driver in use: iwlwifi
	Kernel modules: iwlwifi
00:15.0 Serial bus controller \[0c80\]: Intel Corporation Meteor Lake-P Serial IO I2C Controller #0 \[8086:7e78\] (rev 20)
	Subsystem: CLEVO/KAPOK Computer Device \[1558:a743\]
	Kernel driver in use: intel-lpss
	Kernel modules: intel\_lpss\_pci
00:15.1 Serial bus controller \[0c80\]: Intel Corporation Meteor Lake-P Serial IO I2C Controller #1 \[8086:7e79\] (rev 20)
	Subsystem: CLEVO/KAPOK Computer Device \[1558:a743\]
	Kernel driver in use: intel-lpss
	Kernel modules: intel\_lpss\_pci
00:15.3 Serial bus controller \[0c80\]: Intel Corporation Meteor Lake-P Serial IO I2C Controller #3 \[8086:7e7b\] (rev 20)
	Subsystem: CLEVO/KAPOK Computer Device \[1558:a743\]
	Kernel driver in use: intel-lpss
	Kernel modules: intel\_lpss\_pci
00:16.0 Communication controller \[0780\]: Intel Corporation Meteor Lake-P CSME HECI #1 \[8086:7e70\] (rev 20)
	Subsystem: CLEVO/KAPOK Computer Device \[1558:a743\]
00:1c.0 PCI bridge \[0604\]: Intel Corporation Device \[8086:7e3d\] (rev 20)
	Subsystem: CLEVO/KAPOK Computer Device \[1558:a743\]
	Kernel driver in use: pcieport
	Kernel modules: shpchp
00:1e.0 Communication controller \[0780\]: Intel Corporation Meteor Lake-P Serial IO UART Controller #0 \[8086:7e25\] (rev 20)
	Subsystem: CLEVO/KAPOK Computer Device \[1558:a743\]
	Kernel driver in use: intel-lpss
	Kernel modules: intel\_lpss\_pci
00:1f.0 ISA bridge \[0601\]: Intel Corporation Meteor Lake-H eSPI Controller \[8086:7e02\] (rev 20)
	Subsystem: CLEVO/KAPOK Computer Device \[1558:a743\]
00:1f.3 Multimedia audio controller \[0401\]: Intel Corporation Meteor Lake-P HD Audio Controller \[8086:7e28\] (rev 20)
	DeviceName: Multimedia audio controller
	Subsystem: CLEVO/KAPOK Computer Device \[1558:a743\]
	Kernel driver in use: snd\_hda\_intel
	Kernel modules: snd\_hda\_intel
00:1f.4 SMBus \[0c05\]: Intel Corporation Meteor Lake-P SMBus Controller \[8086:7e22\] (rev 20)
	Subsystem: CLEVO/KAPOK Computer Device \[1558:a743\]
	Kernel driver in use: i801\_smbus
	Kernel modules: i2c\_i801
00:1f.5 Serial bus controller \[0c80\]: Intel Corporation Meteor Lake-P SPI Controller \[8086:7e23\] (rev 20)
	Subsystem: CLEVO/KAPOK Computer Device \[1558:a743\]
	Kernel driver in use: intel-spi
	Kernel modules: spi\_intel\_pci
00:1f.6 Ethernet controller \[0200\]: Intel Corporation Ethernet Connection (18) I219-LM \[8086:550a\] (rev 20)
	DeviceName: Ethernet controller
	Subsystem: CLEVO/KAPOK Computer Device \[1558:a743\]
	Kernel driver in use: e1000e
	Kernel modules: e1000e
01:00.0 Non-Volatile memory controller \[0108\]: Phison Electronics Corporation PS5029-E29T PCIe4 NVMe Controller (DRAM-less) \[1987:5029\] (rev 01)
	Subsystem: Phison Electronics Corporation PS5029-E29T PCIe4 NVMe Controller (DRAM-less) \[1987:5029\]
	Kernel driver in use: nvme
	Kernel modules: nvme
2d:00.0 SD Host controller \[0805\]: O2 Micro, Inc. OZ711 SD/MMC Card Reader Controller \[1217:8621\] (rev 01)
	Subsystem: O2 Micro, Inc. Device \[1217:0002\]
	Kernel driver in use: sdhci-pci
	Kernel modules: sdhci\_pci

`root #``lsusb`
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 003 Device 002: ID 5986:2170 Bison Electronics Inc. BisonCam,NB Pro
Bus 003 Device 003: ID 8087:0033 Intel Corp. AX211 Bluetooth
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub

`root #``lsusb -vt````
/:  Bus 001.Port 001: Dev 001, Class=root_hub, Driver=xhci_hcd/1p, 480M
    ID 1d6b:0002 Linux Foundation 2.0 root hub
/:  Bus 002.Port 001: Dev 001, Class=root_hub, Driver=xhci_hcd/4p, 20000M/x2
    ID 1d6b:0003 Linux Foundation 3.0 root hub
/:  Bus 003.Port 001: Dev 001, Class=root_hub, Driver=xhci_hcd/12p, 480M
    ID 1d6b:0002 Linux Foundation 2.0 root hub
    |__ Port 007: Dev 002, If 0, Class=Video, Driver=uvcvideo, 480M
        ID 5986:2170 Bison Electronics Inc.
    |__ Port 007: Dev 002, If 1, Class=Video, Driver=uvcvideo, 480M
        ID 5986:2170 Bison Electronics Inc.
    |__ Port 010: Dev 003, If 0, Class=Wireless, Driver=btusb, 12M
        ID 8087:0033 Intel Corp. AX211 Bluetooth
    |__ Port 010: Dev 003, If 1, Class=Wireless, Driver=btusb, 12M
        ID 8087:0033 Intel Corp. AX211 Bluetooth
/:  Bus 004.Port 001: Dev 001, Class=root_hub, Driver=xhci_hcd/2p, 20000M/x2
    ID 1d6b:0003 Linux Foundation 3.0 root hub
```
### TODO

**V56 is customizable and also comes with the following hardware:**

- Intel BE200 WiFi/BT card (instead of AX211)
- nVidia RTX 4060/4070 (in addition to the iGPU)

## Installation

If Gentoo is being installed on a V56 with Intel ME disabled, the related kernel modules should be avoided. In case of sys-kernel/gentoo-kernel-bin, the modules need to be blacklisted.

**`/etc/modprobe.d/10-blacklist.conf`**

**Blacklisting Intel ME modules**

### Firmware

Firmware from sys-kernel/linux-firmware is needed for GPU, WiFi, BlueTooth, and the VPU. The intel microcode can be installed from sys-firmware/intel-microcode.

The full list of needed firmware is

### Kernel

**Graphics card**

```
Device Drivers  --->
  Graphics support  --->
    [*] Direct Rendering Manager (XFree86 4.1.0 and higher DRI support) 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_DRM</code> to find this item.  --->
        <*> Intel 8xx/9xx/G3x/G4x/HD Graphics [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_DRM_I915</code> to find this item.
  Frame buffer Devices  --->
    <*> Support for frame buffer device drivers [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_FB</code> to find this item.  --->
        [*] VESA VGA graphics support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_FB_VESA</code> to find this item.
**Networking drivers**

Device Drivers  --->
  \[\*\] Network device support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NETDEVICES\</code> to find this item.  --->
      \[\*\] Ethernet device support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_ETHERNET\</code> to find this item.  --->
          \[\*\] Intel devices [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NET\_VENDOR\_INTEL\</code> to find this item.  --->
              \[\*\] Intel(R) PRO/1000+ PCI-Express Gigabit Ethernet Support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_E1000E\</code> to find this item.
      \[\*\] Wireless LAN [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_WLAN\</code> to find this item.  --->
          \[\*\] Intel devices [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_WLAN\_VENDOR\_INTEL\</code> to find this item.  --->
              \[\*\] Intel Wireless WiFi Next Gen AGN - Wireless-N/Advanced-N/Ultimate-N (iwlwifi) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_IWLWIFI\</code> to find this item.  --->
                  \[\*\] Intel Wireless WiFi MVM Firmware support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_IWLMVM\</code> to find this item.

**Storage (NVMe)**

```
Device Drivers  --->
  NVME Support  ---> 
      <*> NVM Express block device 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_BLK_DEV_NVME</code> to find this item.
           [*] NVMe multipath support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_NVME_MULTIPATH</code> to find this item.
**Input devices (keyboard, touchpad, buttons)**

Power management and ACPI options  --->
  \[\*\] ACPI (Advanced Configuration and Power Interface) Support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_ACPI\</code> to find this item.  --->
      \[\*\] Embedded controller [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_ACPI\_EC\</code> to find this item.
      \<\*> Button [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_ACPI\_BUTTON\</code> to find this item.
      \<\*> Video [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_ACPI\_VIDEO\</code> to find this item.
Device Drivers  --->
  Input device support  --->
    -\*- Generic input layer (needed for keyboard, mouse, ...) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_INPUT\</code> to find this item.  --->
         \<\*> Mouse interface [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_INPUT\_MOUSEDEV\</code> to find this item.
         \<\*> Event interface [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_INPUT\_EVDEV\</code> to find this item.
         \[\*\] Keyboards [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_INPUT\_KEYBOARD\</code> to find this item.  --->
             \[\*\] AT keyboard [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_KEYBOARD\_ATKBD\</code> to find this item.
         \[\*\] Miscellaneous devices [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_INPUT\_MISC\</code> to find this item.  --->
             \<\*> PC speaker support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_INPUT\_PCSPKR\</code> to find this item.
  I2C support  --->
    -\*- I2C support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_I2C\</code> to find this item.  --->
          I2C Hardware Bus support  --->
            \<\*> Intel 82801 (ICH/PCH) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_I2C\_I801\</code> to find this item.
            \<\*> Synopsys DesignWare I2C adapter [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_I2C\_DESIGNWARE\_CORE\</code> to find this item.  --->
                \<\*> Synopsys DesignWare Platform driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_I2C\_DESIGNWARE\_PLATFORM\</code> to find this item.
  \[\*\] SPI support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SPI\</code> to find this item.  --->
      \<\*> Intel PCH/PCU SPI flash PCI driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SPI\_INTEL\_PCI\</code> to find this item.
  \[\*\] Pin controllers  --->
      Intel pinctrl drivers  --->
        \<\*> Intel Meteor Lake pinctrl and GPIO driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_PINCTRL\_METEORLAKE\</code> to find this item.
  -\*- X86 Platform Specific Device Drivers [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_X86\_PLATFORM\_DEVICES\</code> to find this item.  --->
      \<\*> Intel HID event [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_{{{2}}}\</code> to find this item.  --->

**Battery**

Device drivers  --->
  \[\*\] ACPI (Advanced Configuration and Power Interface) Support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_ACPI\</code> to find this item.  --->
      \[\*\] Embedded controller [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_ACPI\_EC\</code> to find this item.
      \<\*> Battery [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_ACPI\_BUTTON\</code> to find this item.

**Web camera**

Device drivers  --->
  \[\*\] Multimedia support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_MEDIA\_SUPPORT\</code> to find this item.  --->
      Media drivers  --->
        \[\*\] Media USB Adapters [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_MEDIA\_USB\_SUPPRT\</code> to find this item.  --->
            \<\*> USB Video Class (UVC) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_USB\_VIDEO\_CLASS\</code> to find this item.

**USB and thunderbolt**

Device drivers  --->
  \[\*\] USB support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_USB\_SUPPORT\</code> to find this item.  --->
      \<\*> Support for Host-side USB [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_USB\</code> to find this item.
      \[\*\] PCI based USB host interface [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_USB\_PCI\</code> to find this item.
      \<\*> xHCI HCD (USB 3.0) support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_USB\_XHCI\_HCD\</code> to find this item.
      \<\*> USB Type-C support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_TYPEC\</code> to find this item.
  \[\*\] Unified support for USB4 and Thunderbolt [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_USB4\</code> to find this item.

**SD card reader**

Device drivers  --->
  \<\*> MMC/SD/SDIO card support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_MMC\</code> to find this item.  --->
      \<\*> Secure Digital Host Controller Interface support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_MMC\_SDHCI\</code> to find this item.  --->
          \<\*> SDHCI support on PCI bus [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_MMC\_SDHCI\_PCI\</code> to find this item.

**Sound card**

Device drivers  --->
  \<\*> Sound card support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SOUND\</code> to find this item.  --->
      \<\*> Advanced Linux Sound Architecture [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\</code> to find this item.  --->
          HD Audio  --->
            \<\*> HD Audio PCI [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_HDA\_INTEL\</code> to find this item.
            \<\*> Realtek HD-audio codec support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_HDA\_CODEC\_REALTEK\</code> to find this item.
            \<\*> HD-audio HDMI codec support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_HDA\_CODEC\_HDMI\</code> to find this item.
          \<\*> ALSA for SoC audio support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_SOC\</code> to find this item.  --->
              \[\*\] Sound Open Firmware (SOF) platforms [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_SOC\_SOF\_TOPLEVEL\</code> to find this item.  --->
                  \[\*\] SOF support for Intel audio DSPs [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_SOC\_SOF\_INTEL\_TOPLEVEL\</code> to find this item.  --->
                      \[\*\]  SOF support for Meteorlake [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_SOC\_SOF\_METEORLAKE\</code> to find this item.

**Intel VPU**

Device Drivers  --->
  \[\*\] Compute Acceleration Framework [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_DRM\_ACCEL\</code> to find this item.  --->
      \<\*> Intel NPU (Neural Processing Unit) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_DRM\_ACCEL\_IVPU\</code> to find this item.

### Emerge

Most of the special keyboard keys use ACPI. Backlight control additionally needs acpilight.

`root #``emerge --ask sys-power/acpid``root #``emerge --ask sys-power/acpilight`
## Configuration

Some of the special keyboard keys need additional configuration.

### Brightness and Sound Control

A desktop environment agnostic way to configure these buttons is with ACPI daemon.

**`/etc/acpi/default.sh`**

```
case "$group" in
	button)
		logger "ACPI button"
		case "$action" in
			power) /etc/acpi/actions/powerbtn.sh ;;
			mute) amixer -q set Master toggle ;;
			micmute) amixer -q set Capture toggle ;;
			volumeup) amixer -q set Master 1dB+ ;;
			volumedown) amixer -q set Master 1dB- ;;
			up) ;;
			down) ;;
			left) ;;
			right) ;;
			*) log_unhandled $* ;;
		esac
		;;
	video)
		case "$action" in
			brightnessup) xbacklight -inc 4 ;;
			brightnessdown) xbacklight -dec 4 ;;
			*) log_unhandled $* ;;
		esac
		;;
esac
```
Make sure the ACPI daemon is running.

`root #``rc-service acpid start`
## Troubleshooting

For users who want a custom kernel, the following things can go wrong.

### The touchpad is not working

The kernel configuration is missing some driver. The touchpad is an I2C device. The I2C master needs CONFIG\_I2C\_DESIGNWARE\_PLATFORM [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_I2C\_DESIGNWARE\_PLATFORM\</code> to find this item..
The touchpad itself needs CONFIG\_HID\_MULTITOUCH [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_HID\_MULTITOUCH\</code> to find this item. and CONFIG\_INTEL\_HID [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_INTEL\_HID\</code> to find this item..

### The touchpad toggle key is not working

This key does not send ACPI events, so must be handled differently. To gather information about the key, install [app-misc/evtest](https://packages.gentoo.org/packages/app-misc/evtest), run the following command and press the touchpad toggle button.

`user $``evtest /dev/input/platform-i8042*-kbd````
Input driver version is 1.0.1
Input device ID: bus 0x11 vendor 0x1 product 0x1 version 0xab83
Input device name: "AT Translated Set 2 keyboard"
Supported events:
  Event type 0 (EV_SYN)
  Event type 1 (EV_KEY)
    Event code 1 (KEY_ESC)
    Event code 2 (KEY_1)
    Event code 3 (KEY_2)
    Event code 4 (KEY_3)
    Event code 5 (KEY_4)
    Event code 6 (KEY_5)
    Event code 7 (KEY_6)
    Event code 8 (KEY_7)
    Event code 9 (KEY_8)
    Event code 10 (KEY_9)
    Event code 11 (KEY_0)
    Event code 12 (KEY_MINUS)
    Event code 13 (KEY_EQUAL)
    Event code 14 (KEY_BACKSPACE)
    Event code 15 (KEY_TAB)
    Event code 16 (KEY_Q)
    Event code 17 (KEY_W)
    Event code 18 (KEY_E)
    Event code 19 (KEY_R)
    Event code 20 (KEY_T)
    Event code 21 (KEY_Y)
    Event code 22 (KEY_U)
    Event code 23 (KEY_I)
    Event code 24 (KEY_O)
    Event code 25 (KEY_P)
    Event code 26 (KEY_LEFTBRACE)
    Event code 27 (KEY_RIGHTBRACE)
    Event code 28 (KEY_ENTER)
    Event code 29 (KEY_LEFTCTRL)
    Event code 30 (KEY_A)
    Event code 31 (KEY_S)
    Event code 32 (KEY_D)
    Event code 33 (KEY_F)
    Event code 34 (KEY_G)
    Event code 35 (KEY_H)
    Event code 36 (KEY_J)
    Event code 37 (KEY_K)
    Event code 38 (KEY_L)
    Event code 39 (KEY_SEMICOLON)
    Event code 40 (KEY_APOSTROPHE)
    Event code 41 (KEY_GRAVE)
    Event code 42 (KEY_LEFTSHIFT)
    Event code 43 (KEY_BACKSLASH)
    Event code 44 (KEY_Z)
    Event code 45 (KEY_X)
    Event code 46 (KEY_C)
    Event code 47 (KEY_V)
    Event code 48 (KEY_B)
    Event code 49 (KEY_N)
    Event code 50 (KEY_M)
    Event code 51 (KEY_COMMA)
    Event code 52 (KEY_DOT)
    Event code 53 (KEY_SLASH)
    Event code 54 (KEY_RIGHTSHIFT)
    Event code 55 (KEY_KPASTERISK)
    Event code 56 (KEY_LEFTALT)
    Event code 57 (KEY_SPACE)
    Event code 58 (KEY_CAPSLOCK)
    Event code 59 (KEY_F1)
    Event code 60 (KEY_F2)
    Event code 61 (KEY_F3)
    Event code 62 (KEY_F4)
    Event code 63 (KEY_F5)
    Event code 64 (KEY_F6)
    Event code 65 (KEY_F7)
    Event code 66 (KEY_F8)
    Event code 67 (KEY_F9)
    Event code 68 (KEY_F10)
    Event code 69 (KEY_NUMLOCK)
    Event code 70 (KEY_SCROLLLOCK)
    Event code 71 (KEY_KP7)
    Event code 72 (KEY_KP8)
    Event code 73 (KEY_KP9)
    Event code 74 (KEY_KPMINUS)
    Event code 75 (KEY_KP4)
    Event code 76 (KEY_KP5)
    Event code 77 (KEY_KP6)
    Event code 78 (KEY_KPPLUS)
    Event code 79 (KEY_KP1)
    Event code 80 (KEY_KP2)
    Event code 81 (KEY_KP3)
    Event code 82 (KEY_KP0)
    Event code 83 (KEY_KPDOT)
    Event code 86 (KEY_102ND)
    Event code 87 (KEY_F11)
    Event code 88 (KEY_F12)
    Event code 89 (KEY_RO)
    Event code 90 (KEY_KATAKANA)
    Event code 91 (KEY_HIRAGANA)
    Event code 92 (KEY_HENKAN)
    Event code 93 (KEY_KATAKANAHIRAGANA)
    Event code 94 (KEY_MUHENKAN)
    Event code 95 (KEY_KPJPCOMMA)
    Event code 96 (KEY_KPENTER)
    Event code 97 (KEY_RIGHTCTRL)
    Event code 98 (KEY_KPSLASH)
    Event code 99 (KEY_SYSRQ)
    Event code 100 (KEY_RIGHTALT)
    Event code 102 (KEY_HOME)
    Event code 103 (KEY_UP)
    Event code 104 (KEY_PAGEUP)
    Event code 105 (KEY_LEFT)
    Event code 106 (KEY_RIGHT)
    Event code 107 (KEY_END)
    Event code 108 (KEY_DOWN)
    Event code 109 (KEY_PAGEDOWN)
    Event code 110 (KEY_INSERT)
    Event code 111 (KEY_DELETE)
    Event code 112 (KEY_MACRO)
    Event code 113 (KEY_MUTE)
    Event code 114 (KEY_VOLUMEDOWN)
    Event code 115 (KEY_VOLUMEUP)
    Event code 116 (KEY_POWER)
    Event code 117 (KEY_KPEQUAL)
    Event code 118 (KEY_KPPLUSMINUS)
    Event code 119 (KEY_PAUSE)
    Event code 121 (KEY_KPCOMMA)
    Event code 122 (KEY_HANGUEL)
    Event code 123 (KEY_HANJA)
    Event code 124 (KEY_YEN)
    Event code 125 (KEY_LEFTMETA)
    Event code 126 (KEY_RIGHTMETA)
    Event code 127 (KEY_COMPOSE)
    Event code 128 (KEY_STOP)
    Event code 140 (KEY_CALC)
    Event code 142 (KEY_SLEEP)
    Event code 143 (KEY_WAKEUP)
    Event code 155 (KEY_MAIL)
    Event code 156 (KEY_BOOKMARKS)
    Event code 157 (KEY_COMPUTER)
    Event code 158 (KEY_BACK)
    Event code 159 (KEY_FORWARD)
    Event code 163 (KEY_NEXTSONG)
    Event code 164 (KEY_PLAYPAUSE)
    Event code 165 (KEY_PREVIOUSSONG)
    Event code 166 (KEY_STOPCD)
    Event code 172 (KEY_HOMEPAGE)
    Event code 173 (KEY_REFRESH)
    Event code 183 (KEY_F13)
    Event code 184 (KEY_F14)
    Event code 185 (KEY_F15)
    Event code 186 (KEY_F16)
    Event code 187 (KEY_F17)
    Event code 188 (KEY_F18)
    Event code 189 (KEY_F19)
    Event code 190 (KEY_F20)
    Event code 191 (KEY_F21)
    Event code 192 (KEY_F22)
    Event code 193 (KEY_F23)
    Event code 194 (KEY_F24)
    Event code 217 (KEY_SEARCH)
    Event code 226 (KEY_MEDIA)
    Event code 530 (KEY_TOUCHPAD_TOGGLE)
  Event type 4 (EV_MSC)
    Event code 4 (MSC_SCAN)
  Event type 17 (EV_LED)
    Event code 0 (LED_NUML) state 0
    Event code 1 (LED_CAPSL) state 0
    Event code 2 (LED_SCROLLL) state 0
Key repeat handling:
  Repeat type 20 (EV_REP)
    Repeat code 0 (REP_DELAY)
      Value    250
    Repeat code 1 (REP_PERIOD)
      Value     33
Properties:
Testing ... (interrupt to exit)
Event: time 1785697430.578524, type 4 (EV_MSC), code 4 (MSC_SCAN), value 1c
Event: time 1785697430.578524, type 1 (EV_KEY), code 28 (KEY_ENTER), value 0
Event: time 1785697430.578524, -------------- SYN_REPORT ------------
Event: time 1785697432.769971, type 4 (EV_MSC), code 4 (MSC_SCAN), value f8
Event: time 1785697432.769971, type 1 (EV_KEY), code 530 (KEY_TOUCHPAD_TOGGLE), value 1
Event: time 1785697432.769971, -------------- SYN_REPORT ------------
Event: time 1785697432.922004, type 4 (EV_MSC), code 4 (MSC_SCAN), value f8
Event: time 1785697432.922004, type 1 (EV_KEY), code 530 (KEY_TOUCHPAD_TOGGLE), value 0
Event: time 1785697432.922004, -------------- SYN_REPORT ------------
```
The most important part of the output are the final six lines. The EV\_MSC line says that evdev has detected a key with ID 0xf8. The EV\_KEY line shows how that key was interpreted. If there is no EV\_KEY line, or the the key was interpreted wrongly, the udev hwdb needs to be modified.

**`/etc/udev/hwdb.d/99-touchpad_toggle.conf`**

After saving the key configuration, update the hwdb.

`root #``systemd-hwdb update`
Finally, reload udev rules for the keyboard.

`root #``udevadm trigger --verbose --sysname-match=event5`
### Special keys do not appear in evtest output

Make sure CONFIG\_ACPI\_EC [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_ACPI\_EC\</code> to find this item. is built into your kernel.

### Battery info is missing from /sys

The ACPI battery driver should expose the battery info in /sys/class/power\_supply/BAT0.
If that directory is missing, the kernel is missing either CONFIG\_ACPI\_BATTERY [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_ACPI\_BATTERY\</code> to find this item. or CONFIG\_ACPI\_EC [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_ACPI\_EC\</code> to find this item..

## See also

- [Intel](https://wiki.gentoo.org/wiki/Intel) — the open source graphics driver for Intel GMA on-board graphics cards and Intel iGPU and Intel Arc dedicated graphics cards, starting with the Intel 810.
- [Iwlwifi](https://wiki.gentoo.org/wiki/Iwlwifi) — the wireless driver for [Intel's current wireless chips](https://wireless.wiki.kernel.org/en/users/drivers/iwlwifi#introduction).
- [ACPI](https://wiki.gentoo.org/wiki/ACPI) — a [power management](https://wiki.gentoo.org/wiki/Power_management) system that is part of the [BIOS](https://wiki.gentoo.org/wiki/BIOS).
- [udev](https://wiki.gentoo.org/wiki/Udev) — [systemd's](https://wiki.gentoo.org/wiki/Systemd) device manager for the Linux kernel.
