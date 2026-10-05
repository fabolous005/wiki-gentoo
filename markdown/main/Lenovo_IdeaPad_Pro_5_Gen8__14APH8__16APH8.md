<!-- source: https://wiki.gentoo.org/wiki/Lenovo_IdeaPad_Pro_5_Gen8_(14APH8,_16APH8) | group: Gentoo Wiki (Main) | wiki-title: Lenovo IdeaPad Pro 5 Gen8 (14APH8, 16APH8) -->
---
title: Lenovo IdeaPad Pro 5 Gen8 (14APH8, 16APH8)
url: https://wiki.gentoo.org/wiki/Lenovo_IdeaPad_Pro_5_Gen8_(14APH8,_16APH8)
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-14"
fingerprint: "1e462cddb33a056d"
license: CC BY-SA 4.0
---

# Lenovo IdeaPad Pro 5 Gen8 (14APH8, 16APH8)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources (14APH8)**

**Resources (16APH8)**

The **Lenovo IdeaPad Pro 5 Gen 8** is a 14" (14APH8) or 16" (16APH8) laptop with AMD Ryzen 5 7640HS or Ryzen 7 7840HS processor. Both models were announced in December 2022, but went on sale at different times - the 16" model in April 2023, and the 14" model a little later, in July 2023. [\[1\]](https://wiki.gentoo.org#cite_note-1)

The 14" and 16" models differ in chassis, keyboard, screen, and the option for NVIDIA GeForce dGPU on the 16" model.

## Hardware

### Components

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | AMD Ryzen 5 7640HS or Ryzen 7 7840HS |  | N/A | N/A | 6.6.13 |  | 
| iGPU | AMD Radeon 760M / 780M integrated graphics |  | PCI 1002:15bf | [amdgpu](https://wiki.gentoo.org/wiki/Amdgpu) | 6.6.13 |  | 
| dGPU | NVidia GeForce RTX 3050 / 4050 |  | ? | [nvidia](https://wiki.gentoo.org/wiki/Nvidia) |  | Optional on 16APH8 | 
| GPIO | AMD GPIO Pin Controller |  | N/A | pinctrl\_amd | 6.6.13 | Required for I2C to work | 
| I2C/SMBus Controller | AMD FCH SMBus Controller |  | PCI 1022:790b | i2c\_piix4 | 6.6.13 |  | 
| I2C/SMBus Controller | Synopsis DesignWare I2C bus adapter |  | I2C AMDI0010 | i2c\_designware\_platform | 6.6.13 | Required for touchpad to work | 
| TPM | TPM 2.0 |  | I2C MSFT0101 | tcg\_crb, tpm\_tis | 6.6.13 |  | 
| NVMe | PCIe 4.0 NVMe SSD 512GB / 1TB |  | PCI 1cc4:6a03, PCI 1cc4:6a14 | nvme | 6.6.13 |  | 
| Audio | AMD High Definition Audio Controller |  | PCI 1002:1640 | snd\_hda\_intel, snd\_hda\_codec\_realtek | 6.6.13 |  | 
| Audio | AMD ACP6x Audio Coprocessor |  | PCI 1022:15e2 | snd\_pci\_acp6x, snd\_acp\_pci, snd\_rpl\_pci\_acp6x, snd\_pci\_ps | 6.6.13 |  | 
| Wireless LAN | MediaTek MT7922 802.11ax PCIe Wireless Network Adapter |  | PCI 14c3:0616 | mt7921e | 6.6.13 |  | 
| USB4 | AMD Pink Sardine USB4/Thunderbolt NHI controller |  | PCI 1022:1668 | thunderbolt | 6.6.13 |  | 
| Touchpad | ? |  | I2C 06cb:cef5 | i2c\_hid, hid\_multitouch | 6.6.13 |  | 
| SD Card reader | Realtek RTS522A PCIe Card Reader |  | PCI 10ec:522a | rtsx\_pci, rtsx\_pci\_sdmmc | 6.6.13 |  | 
| Bluetooth | Foxconn / Hon Hai Bluetooth Adapter |  | USB 0489:e0d8 | btusb | 6.6.13 |  | 
| Webcam | Syntek Integrated Camera |  | USB 174f:181d | uvcvideo | 6.6.13 |  | 
| Hardware Monitoring | various |  |  | amdgpu, hid\_sensor\_hub, k10temp, mt7921e, nvme | 6.6.13 |  | 
| Hotkeys |  |  |  | ideapad\_laptop | 6.6.13 |  | 

### ACPI / Power management

| Function | Status | Kernel driver(s) | Kernel version | BIOS version | Notes | 
|---|---|---|---|---|---|
| CPU frequency scaling |  | amd\_pstate | 6.6.13 | MKCN28WW |  | 
| GPU Powersaving (PowerPlay) |  | amdgpu | 6.6.13 | MKCN28WW |  | 
| PCIe Power Management (ASPM) |  |  |  |  |  | 
| USB Type C Power Delivery |  | typec\_ucsi | 6.6.13 | MKCN28WW |  | 
| Battery |  | acpi\_battery | 6.6.13 | MKCN28WW |  | 
| Suspend to RAM |  |  | 6.6.13 | MKCN28WW |  | 
| Suspend to disk (hibernate) |  |  |  |  |  | 
| Display backlight control |  | acpi\_video | 6.6.13 | MKCN28WW |  | 
| Keyboard backlight control |  | ideapad\_laptop | 6.6.13 | MKCN28WW |  | 

### Battery charge limit

IdeaPads support Battery Conservation Mode which limits the maximum charge of the battery to \~60%. It is controlled by writing to a file in sysfs.

Check the status of battery conservation mode:

`root #``cat /sys/bus/platform/drivers/ideapad_acpi/VPC2004\:00/conservation_mode`
`0`

Enable battery conservation mode:

`root #``echo 1 > /sys/bus/platform/drivers/ideapad_acpi/VPC2004\:00/conservation_mode`
Disable battery conservation mode:

`root #``echo 0 > /sys/bus/platform/drivers/ideapad_acpi/VPC2004\:00/conservation_mode`
### Extra hardware information

`root #``lspci -nnk`
00:00.0 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:14e8\]
	Subsystem: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:14e8\]
00:00.2 IOMMU \[0806\]: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:14e9\]
	Subsystem: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:14e9\]
00:01.0 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:14ea\]
00:02.0 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:14ea\]
00:02.2 PCI bridge \[0604\]: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:14ee\]
	Subsystem: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:1453\]
	Kernel driver in use: pcieport
00:02.3 PCI bridge \[0604\]: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:14ee\]
	Subsystem: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:1453\]
	Kernel driver in use: pcieport
00:02.4 PCI bridge \[0604\]: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:14ee\]
	Subsystem: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:1453\]
	Kernel driver in use: pcieport
00:03.0 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:14ea\]
00:03.1 PCI bridge \[0604\]: Advanced Micro Devices, Inc. \[AMD\] Family 19h USB4/Thunderbolt PCIe tunnel \[1022:14ef\]
	Subsystem: Advanced Micro Devices, Inc. \[AMD\] Family 19h USB4/Thunderbolt PCIe tunnel \[1022:1453\]
	Kernel driver in use: pcieport
00:04.0 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:14ea\]
00:08.0 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:14ea\]
00:08.1 PCI bridge \[0604\]: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:14eb\]
	Subsystem: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:14eb\]
	Kernel driver in use: pcieport
00:08.2 PCI bridge \[0604\]: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:14eb\]
	Subsystem: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:14eb\]
	Kernel driver in use: pcieport
00:08.3 PCI bridge \[0604\]: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:14eb\]
pcilib: Error reading /sys/bus/pci/devices/0000:00:08.3/label: Operation not permitted
	Subsystem: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:14eb\]
	Kernel driver in use: pcieport
00:14.0 SMBus \[0c05\]: Advanced Micro Devices, Inc. \[AMD\] FCH SMBus Controller \[1022:790b\] (rev 71)
	Subsystem: Advanced Micro Devices, Inc. \[AMD\] FCH SMBus Controller \[1022:790b\]
	Kernel driver in use: piix4\_smbus
	Kernel modules: i2c\_piix4, sp5100\_tco
00:14.3 ISA bridge \[0601\]: Advanced Micro Devices, Inc. \[AMD\] FCH LPC Bridge \[1022:790e\] (rev 51)
	Subsystem: Advanced Micro Devices, Inc. \[AMD\] FCH LPC Bridge \[1022:790e\]
00:18.0 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:14f0\]
00:18.1 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:14f1\]
00:18.2 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:14f2\]
00:18.3 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:14f3\]
	Kernel driver in use: k10temp
	Kernel modules: k10temp
00:18.4 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:14f4\]
00:18.5 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:14f5\]
00:18.6 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:14f6\]
00:18.7 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:14f7\]
01:00.0 Network controller \[0280\]: MEDIATEK Corp. MT7922 802.11ax PCI Express Wireless Network Adapter \[14c3:0616\]
	Subsystem: Lenovo MT7922 802.11ax PCI Express Wireless Network Adapter \[17aa:e0c6\]
	Kernel driver in use: mt7921e
	Kernel modules: mt7921e
02:00.0 Unassigned class \[ff00\]: Realtek Semiconductor Co., Ltd. RTS522A PCI Express Card Reader \[10ec:522a\] (rev 01)
	Subsystem: Realtek Semiconductor Co., Ltd. RTS522A PCI Express Card Reader \[10ec:522a\]
	Kernel driver in use: rtsx\_pci
	Kernel modules: rtsx\_pci
03:00.0 Non-Volatile memory controller \[0108\]: Shenzhen Unionmemory Information System Ltd. RPETJ512MKP1QDQ PCIe 4.0 NVMe SSD 512GB (DRAM-less) \[1cc4:6a03\] (rev 03)
	DeviceName: Realtek
	Subsystem: Shenzhen Unionmemory Information System Ltd. RPETJ512MKP1QDQ PCIe 4.0 NVMe SSD 512GB (DRAM-less) \[1cc4:6a03\]
	Kernel driver in use: nvme
63:00.0 VGA compatible controller \[0300\]: Advanced Micro Devices, Inc. \[AMD/ATI\] Phoenix1 \[1002:15bf\] (rev c7)
	Subsystem: Lenovo Phoenix1 \[17aa:3818\]
	Kernel driver in use: amdgpu
	Kernel modules: amdgpu
63:00.1 Audio device \[0403\]: Advanced Micro Devices, Inc. \[AMD/ATI\] Rembrandt Radeon High Definition Audio Controller \[1002:1640\]
	Subsystem: Lenovo Rembrandt Radeon High Definition Audio Controller \[17aa:3823\]
	Kernel driver in use: snd\_hda\_intel
63:00.2 Encryption controller \[1080\]: Advanced Micro Devices, Inc. \[AMD\] Family 19h (Model 74h) CCP/PSP 3.0 Device \[1022:15c7\]
	Subsystem: Advanced Micro Devices, Inc. \[AMD\] Family 19h (Model 74h) CCP/PSP 3.0 Device \[1022:15c7\]
	Kernel driver in use: ccp
	Kernel modules: ccp
63:00.3 USB controller \[0c03\]: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:15b9\]
	Subsystem: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:15b9\]
	Kernel driver in use: xhci\_hcd
63:00.4 USB controller \[0c03\]: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:15ba\]
	Subsystem: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:15ba\]
	Kernel driver in use: xhci\_hcd
63:00.5 Multimedia controller \[0480\]: Advanced Micro Devices, Inc. \[AMD\] ACP/ACP3X/ACP6x Audio Coprocessor \[1022:15e2\] (rev 63)
	Subsystem: Lenovo ACP/ACP3X/ACP6x Audio Coprocessor \[17aa:3887\]
	Kernel driver in use: snd\_pci\_ps
	Kernel modules: snd\_pci\_acp6x, snd\_acp\_pci, snd\_rpl\_pci\_acp6x, snd\_pci\_ps
63:00.6 Audio device \[0403\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h/19h HD Audio Controller \[1022:15e3\]
	Subsystem: Lenovo Family 17h/19h HD Audio Controller \[17aa:3881\]
	Kernel driver in use: snd\_hda\_intel
64:00.0 Non-Essential Instrumentation \[1300\]: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:14ec\]
	Subsystem: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:14ec\]
65:00.0 Non-Essential Instrumentation \[1300\]: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:14ec\]
	Subsystem: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:14ec\]
65:00.3 USB controller \[0c03\]: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:15c0\]
	Subsystem: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:15c0\]
	Kernel driver in use: xhci\_hcd
65:00.4 USB controller \[0c03\]: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:15c1\]
	Subsystem: Advanced Micro Devices, Inc. \[AMD\] Device \[1022:15c1\]
	Kernel driver in use: xhci\_hcd
65:00.5 USB controller \[0c03\]: Advanced Micro Devices, Inc. \[AMD\] Pink Sardine USB4/Thunderbolt NHI controller #1 \[1022:1668\]
	Subsystem: Advanced Micro Devices, Inc. \[AMD\] Pink Sardine USB4/Thunderbolt NHI controller \[1022:1668\]
	Kernel driver in use: thunderbolt
	Kernel modules: thunderbolt

`root #``lscpu````
Architecture:            x86_64
  CPU op-mode(s):        32-bit, 64-bit
  Address sizes:         48 bits physical, 48 bits virtual
  Byte Order:            Little Endian
CPU(s):                  16
  On-line CPU(s) list:   0-15
Vendor ID:               AuthenticAMD
  Model name:            AMD Ryzen 7 7840HS with Radeon 780M Graphics
    CPU family:          25
    Model:               116
    Thread(s) per core:  2
    Core(s) per socket:  8
    Socket(s):           1
    Stepping:            1
    CPU(s) scaling MHz:  20%
    CPU max MHz:         6080.0000
    CPU min MHz:         400.0000
    BogoMIPS:            7585.85
    Flags:               fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mc
                         a cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall n
                         x mmxext fxsr_opt pdpe1gb rdtscp lm constant_tsc rep_go
                         od amd_lbr_v2 nopl nonstop_tsc cpuid extd_apicid aperfm
                         perf rapl pni pclmulqdq monitor ssse3 fma cx16 sse4_1 s
                         se4_2 x2apic movbe popcnt aes xsave avx f16c rdrand lah
                         f_lm cmp_legacy svm extapic cr8_legacy abm sse4a misali
                         gnsse 3dnowprefetch osvw ibs skinit wdt tce topoext per
                         fctr_core perfctr_nb bpext perfctr_llc mwaitx cpb cat_l
                         3 cdp_l3 hw_pstate ssbd mba perfmon_v2 ibrs ibpb stibp 
                         ibrs_enhanced vmmcall fsgsbase bmi1 avx2 smep bmi2 erms
                          invpcid cqm rdt_a avx512f avx512dq rdseed adx smap avx
                         512ifma clflushopt clwb avx512cd sha_ni avx512bw avx512
                         vl xsaveopt xsavec xgetbv1 xsaves cqm_llc cqm_occup_llc
                          cqm_mbm_total cqm_mbm_local avx512_bf16 clzero irperf 
                         xsaveerptr rdpru wbnoinvd cppc arat npt lbrv svm_lock n
                         rip_save tsc_scale vmcb_clean flushbyasid decodeassists
                          pausefilter pfthreshold v_vmsave_vmload vgif x2avic v_
                         spec_ctrl vnmi avx512vbmi umip pku ospke avx512_vbmi2 g
                         fni vaes vpclmulqdq avx512_vnni avx512_bitalg avx512_vp
                         opcntdq rdpid overflow_recov succor smca flush_l1d
Virtualization features: 
  Virtualization:        AMD-V
Caches (sum of all):     
  L1d:                   256 KiB (8 instances)
  L1i:                   256 KiB (8 instances)
  L2:                    8 MiB (8 instances)
  L3:                    16 MiB (1 instance)
NUMA:                    
  NUMA node(s):          1
  NUMA node0 CPU(s):     0-15
Vulnerabilities:         
  Gather data sampling:  Not affected
  Itlb multihit:         Not affected
  L1tf:                  Not affected
  Mds:                   Not affected
  Meltdown:              Not affected
  Mmio stale data:       Not affected
  Retbleed:              Not affected
  Spec rstack overflow:  Mitigation; Safe RET
  Spec store bypass:     Mitigation; Speculative Store Bypass disabled via prctl
  Spectre v1:            Mitigation; usercopy/swapgs barriers and __user pointer
                          sanitization
  Spectre v2:            Mitigation; Enhanced / Automatic IBRS, IBPB conditional
                         , STIBP always-on, RSB filling, PBRSB-eIBRS Not affecte
                         d
  Srbds:                 Not affected
  Tsx async abort:       Not affected
```
`root #``lsusb`
Bus 006 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 005 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 007 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 003 Device 002: ID 174f:181d Syntek Integrated Camera
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 001 Device 002: ID 0489:e0d8 Foxconn / Hon Hai Wireless\_Device
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

## Troubleshooting

### Screen

- The 14" 2800x1800 120 Hz screen operates only in 60 Hz mode. dmesg contains error messages about invalid DisplayID checksum.

Solution:

Verify that the checksum is the incorrect one:

`root #``hexdump -C /sys/class/drm/card1-eDP-1/edid`

000000f0  00 00 00 00 00 00 00 00  00 00 00 00 00 00 **f0 98**  |................|

Create a modified EDID with the correct checksum in /lib/firmware/edid/edid.bin:

`root #``mkdir -p /lib/firmware/edid``root #``cp /sys/class/drm/card1-eDP-1/edid /lib/firmware/edid/edid.bin``root #``printf '\xf8\x90' | dd bs=1 seek=254 conv=notrunc of=/lib/firmware/edid.bin`
Verify that the checksum is now correct:

`root #``hexdump -C /lib/firmware/edid/edid.bin`

000000f0  00 00 00 00 00 00 00 00  00 00 00 00 00 00 **f8 90**  |................|

If DRM is built-in, then add this to the kernel configuration:

**Add custom EDID to kernel**

If DRM is built as a module, you can do the above, or instead create a file in /etc/modprobe.d/

**`/etc/modprobe.d/drm.conf`**
