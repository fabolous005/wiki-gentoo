<!-- source: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_L14_(Gen_5) | group: Gentoo Wiki (Main) | wiki-title: Lenovo ThinkPad L14 (Gen 5) -->
---
title: Lenovo ThinkPad L14 (Gen 5)
url: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_L14_(Gen_5)
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-13"
fingerprint: "8f4a2ed5e332346d"
license: CC BY-SA 4.0
---

# Lenovo ThinkPad L14 (Gen 5)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

The **Lenovo ThinkPad L14 (Gen 5)** is a 14-inch laptop manufactured by Lenovo for business needs. As such, it is thicker than many modern laptops and has plenty of ports. The laptop is also quite easy to maintain.

## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | AMD Ryzen 7 PRO 77535U with Radeon Graphics |  | N/A | N/A | 6.17.1 |  | 
| Video card | Advanced Micro Devices, Inc. \[AMD/ATI\] Rembrandt \[Radeon 680M\] (rev d5) |  | 1002:1681 | amdgpu | 6.17.1 |  | 
| Wi-Fi | Qualcomm Technologies, Inc QCNFA765 Wireless Network Adapter (rev 01) |  | 17cb:1103 | ath11k\_pci | 6.17.1 |  | 
| Ethernet | Realtek Semiconductor Co., Ltd. RTL8111/8168/8211/8411 PCI Express Gigabit Ethernet Controller (rev 0e) |  | 10ec:8168 | r8169 | 6.17.1 |  | 
| Bluetooth | USI Co., Ltd |  | 10ab:9309 | btusb | 6.17.1 |  | 
| NVMe | SK hynix BC901 NVMe Solid State Drive (DRAM-less) (rev 03) |  | 1c5c:1d59 | nvme | 6.17.1 |  | 
| Audio | Advanced Micro Devices, Inc. \[AMD\] Family 17h/19h/1ah HD Audio Controller |  | 1022:15e3 | snd\_hda\_intel | 6.17.1 |  | 
| Smart card reader | Generic EMV Smartcard Reader |  | 2ce3:9563 | usbfs | 6.17.1 |  | 
| Finger print reader | Shenzhen Goodix Technology Co.,Ltd. Goodix USB2.0 MISC |  | 27c6:6594 | usbfs | 6.17.1 |  | 
| Camera | IMC Networks Integrated Camera |  | 13d3:54aa | uvcvideo | 6.17.1 |  | 
| Keyboard | AT Translated Set 2 keyboard |  | 0001:0001 | atkbd | 6.17.1 |  | 
| Mouse | ELAN06D8:00 04F3:3195 Mouse |  | 04f3:3195 | i2c\_designware | 6.17.1 |  | 
| Touchpad | ELAN06D8:00 04F3:3195 Touchpad |  | 04f3:3195 | i2c\_designware | 6.17.1 |  | 
| TrackPoint | TPPS/2 Elan TrackPoint |  | 0002:000a | psmouse | 6.17.1 |  | 

### Accessories

| Device | Make/model | Status | Bus ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| Dock | ThinkPad Universal USB-C Dock |  | N/A | N/A | N/A | To be tested | 
| Dock | ThinkPad Universal USB-C Smart Dock |  | N/A | N/A | N/A | To be tested | 
| Dock | ThinkPad Universal Thunderbolt 4 Smart Dock |  | N/A | N/A | N/A | To be tested | 
| Dock | ThinkPad Thunderbolt 4 Workstation Dock |  | N/A | N/A | N/A | To be tested | 
| Dock | ThinkPad Universal Thunderbolt 4 Dock |  | N/A | N/A | N/A | To be tested | 
| Dock | ThinkPad Hybrid USB-C with USB-A Dock |  | N/A | N/A | N/A | To be tested | 
| Dock | Lenovo USB-C Slim Travel Dock |  | N/A | N/A | N/A | To be tested | 
| Dock | Lenovo USB-C Dual Display Travel Dock |  | N/A | N/A | N/A | To be tested | 
| Dock | Lenovo USB-C Universal Business Dock |  | N/A | N/A | N/A | To be tested | 
| Dock | Lenovo USB-C Mini Dock |  | N/A | N/A | N/A | To be tested | 

### Detailed information

`root #``uname -r`
6.17.1-gentoo-dist

`root #``lscpu`
Architecture:                            x86\_64
CPU op-mode(s):                          32-bit, 64-bit
Address sizes:                           48 bits physical, 48 bits virtual
Byte Order:                              Little Endian
CPU(s):                                  12
On-line CPU(s) list:                     0-11
Vendor ID:                               AuthenticAMD
Model name:                              AMD Ryzen 5 Pro 7535U with Radeon Graphics
CPU family:                              25
Model:                                   68
Thread(s) per core:                      2
Core(s) per socket:                      6
Socket(s):                               1
Stepping:                                1
Frequency boost:                         enabled
CPU(s) scaling MHz:                      30%
CPU max MHz:                             4630.4429
CPU min MHz:                             418.4140
BogoMIPS:                                5790.74
Flags:                                   fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx mmxext fxsr\_opt pdpe1gb rdtscp lm constant\_tsc rep\_good nopl xtopology nonstop\_tsc cpuid extd\_apicid aperfmperf rapl pni pclmulqdq monitor ssse3 fma cx16 sse4\_1 sse4\_2 x2apic movbe popcnt aes xsave avx f16c rdrand lahf\_lm cmp\_legacy svm extapic cr8\_legacy abm sse4a misalignsse 3dnowprefetch osvw ibs skinit wdt tce topoext perfctr\_core perfctr\_nb bpext perfctr\_llc mwaitx cpb cat\_l3 cdp\_l3 hw\_pstate ssbd mba ibrs ibpb stibp vmmcall fsgsbase bmi1 avx2 smep bmi2 erms invpcid cqm rdt\_a rdseed adx smap clflushopt clwb sha\_ni xsaveopt xsavec xgetbv1 xsaves cqm\_llc cqm\_occup\_llc cqm\_mbm\_total cqm\_mbm\_local user\_shstk clzero irperf xsaveerptr rdpru wbnoinvd cppc arat npt lbrv svm\_lock nrip\_save tsc\_scale vmcb\_clean flushbyasid decodeassists pausefilter pfthreshold avic v\_vmsave\_vmload vgif v\_spec\_ctrl umip pku ospke vaes vpclmulqdq rdpid overflow\_recov succor smca fsrm debug\_swap
Virtualization:                          AMD-V
L1d cache:                               192 KiB (6 instances)
L1i cache:                               192 KiB (6 instances)
L2 cache:                                3 MiB (6 instances)
L3 cache:                                16 MiB (1 instance)
NUMA node(s):                            1
NUMA node0 CPU(s):                       0-11
Vulnerability Gather data sampling:      Not affected
Vulnerability Ghostwrite:                Not affected
Vulnerability Indirect target selection: Not affected
Vulnerability Itlb multihit:             Not affected
Vulnerability L1tf:                      Not affected
Vulnerability Mds:                       Not affected
Vulnerability Meltdown:                  Not affected
Vulnerability Mmio stale data:           Not affected
Vulnerability Old microcode:             Not affected
Vulnerability Reg file data sampling:    Not affected
Vulnerability Retbleed:                  Not affected
Vulnerability Spec rstack overflow:      Mitigation; Safe RET
Vulnerability Spec store bypass:         Mitigation; Speculative Store Bypass disabled via prctl
Vulnerability Spectre v1:                Mitigation; usercopy/swapgs barriers and \_\_user pointer sanitization
Vulnerability Spectre v2:                Mitigation; Retpolines; IBPB conditional; IBRS\_FW; STIBP always-on; RSB filling; PBRSB-eIBRS Not affected; BHI Not affected
Vulnerability Srbds:                     Not affected
Vulnerability Tsa:                       Mitigation; Clear CPU buffers
Vulnerability Tsx async abort:           Not affected
Vulnerability Vmscape:                   Mitigation; IBPB before exit to userspace

`root #``lspci -nnk`
00:00.0 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h-19h PCIe Root Complex \[1022:14b5\] (rev 01)
	Subsystem: Lenovo Device \[17aa:50e7\]
00:00.2 IOMMU \[0806\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h-19h IOMMU \[1022:14b6\]
	Subsystem: Lenovo Device \[17aa:50e7\]
00:01.0 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h-19h PCIe Dummy Host Bridge \[1022:14b7\] (rev 01)
00:01.2 PCI bridge \[0604\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h-19h PCIe GPP Bridge \[1022:14ba\]
	Subsystem: Lenovo Device \[17aa:50e7\]
	Kernel driver in use: pcieport
00:02.0 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h-19h PCIe Dummy Host Bridge \[1022:14b7\] (rev 01)
00:02.4 PCI bridge \[0604\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h-19h PCIe GPP Bridge \[1022:14ba\]
	Subsystem: Lenovo Device \[17aa:50e7\]
	Kernel driver in use: pcieport
00:02.5 PCI bridge \[0604\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h-19h PCIe GPP Bridge \[1022:14ba\]
	Subsystem: Lenovo Device \[17aa:50e7\]
	Kernel driver in use: pcieport
00:03.0 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h-19h PCIe Dummy Host Bridge \[1022:14b7\] (rev 01)
00:03.1 PCI bridge \[0604\]: Advanced Micro Devices, Inc. \[AMD\] Family 19h USB4/Thunderbolt PCIe tunnel \[1022:14cd\]
	Subsystem: Lenovo Device \[17aa:50e7\]
	Kernel driver in use: pcieport
00:04.0 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h-19h PCIe Dummy Host Bridge \[1022:14b7\] (rev 01)
00:08.0 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h-19h PCIe Dummy Host Bridge \[1022:14b7\] (rev 01)
00:08.1 PCI bridge \[0604\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h-19h Internal PCIe GPP Bridge \[1022:14b9\] (rev 10)
	Subsystem: Device \[50e7:17aa\]
	Kernel driver in use: pcieport
00:08.3 PCI bridge \[0604\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h-19h Internal PCIe GPP Bridge \[1022:14b9\] (rev 10)
	Subsystem: Device \[50e7:17aa\]
	Kernel driver in use: pcieport
00:14.0 SMBus \[0c05\]: Advanced Micro Devices, Inc. \[AMD\] FCH SMBus Controller \[1022:790b\] (rev 71)
	Subsystem: Lenovo Device \[17aa:50e7\]
	Kernel driver in use: piix4\_smbus
	Kernel modules: i2c\_piix4, sp5100\_tco
00:14.3 ISA bridge \[0601\]: Advanced Micro Devices, Inc. \[AMD\] FCH LPC Bridge \[1022:790e\] (rev 51)
	Subsystem: Lenovo Device \[17aa:50e7\]
00:18.0 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Rembrandt Data Fabric: Device 18h; Function 0 \[1022:1679\]
00:18.1 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Rembrandt Data Fabric: Device 18h; Function 1 \[1022:167a\]
00:18.2 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Rembrandt Data Fabric: Device 18h; Function 2 \[1022:167b\]
00:18.3 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Rembrandt Data Fabric: Device 18h; Function 3 \[1022:167c\]
	Kernel driver in use: k10temp
	Kernel modules: k10temp
00:18.4 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Rembrandt Data Fabric: Device 18h; Function 4 \[1022:167d\]
00:18.5 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Rembrandt Data Fabric: Device 18h; Function 5 \[1022:167e\]
00:18.6 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Rembrandt Data Fabric: Device 18h; Function 6 \[1022:167f\]
00:18.7 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Rembrandt Data Fabric: Device 18h; Function 7 \[1022:1680\]
01:00.0 Network controller \[0280\]: Qualcomm Technologies, Inc QCNFA765 Wireless Network Adapter \[17cb:1103\] (rev 01)
	Subsystem: Lenovo Device \[17aa:9309\]
	Kernel driver in use: ath11k\_pci
	Kernel modules: ath11k\_pci
02:00.0 Non-Volatile memory controller \[0108\]: SK hynix BC901 NVMe Solid State Drive (DRAM-less) \[1c5c:1d59\] (rev 03)
	Subsystem: SK hynix BC901 NVMe Solid State Drive (DRAM-less) \[1c5c:1d59\]
	Kernel driver in use: nvme
	Kernel modules: nvme
03:00.0 Ethernet controller \[0200\]: Realtek Semiconductor Co., Ltd. RTL8111/8168/8211/8411 PCI Express Gigabit Ethernet Controller \[10ec:8168\] (rev 0e)
	Subsystem: Lenovo Device \[17aa:50e7\]
	Kernel driver in use: r8169
	Kernel modules: r8169
74:00.0 VGA compatible controller \[0300\]: Advanced Micro Devices, Inc. \[AMD/ATI\] Rembrandt \[Radeon 680M\] \[1002:1681\] (rev d5)
	Subsystem: Lenovo Device \[17aa:50e7\]
	Kernel driver in use: amdgpu
	Kernel modules: amdgpu
74:00.1 Audio device \[0403\]: Advanced Micro Devices, Inc. \[AMD/ATI\] Radeon High Definition Audio Controller \[Rembrandt/Strix\] \[1002:1640\]
	Subsystem: Lenovo Device \[17aa:50e7\]
	Kernel driver in use: snd\_hda\_intel
	Kernel modules: snd\_hda\_intel
74:00.2 Encryption controller \[1080\]: Advanced Micro Devices, Inc. \[AMD\] Family 19h PSP/CCP \[1022:1649\]
	Subsystem: Lenovo Device \[17aa:50e7\]
	Kernel driver in use: ccp
74:00.3 USB controller \[0c03\]: Advanced Micro Devices, Inc. \[AMD\] Rembrandt USB4 XHCI controller #3 \[1022:161d\]
	Subsystem: Lenovo Device \[17aa:50e7\]
	Kernel driver in use: xhci\_hcd
74:00.4 USB controller \[0c03\]: Advanced Micro Devices, Inc. \[AMD\] Rembrandt USB4 XHCI controller #4 \[1022:161e\]
	Subsystem: Lenovo Device \[17aa:50e7\]
	Kernel driver in use: xhci\_hcd
74:00.5 Multimedia controller \[0480\]: Advanced Micro Devices, Inc. \[AMD\] Audio Coprocessor \[1022:15e2\] (rev 60)
	Subsystem: Lenovo Device \[17aa:50e7\]
	Kernel driver in use: snd\_pci\_acp6x
	Kernel modules: snd\_pci\_acp3x, snd\_rn\_pci\_acp3x, snd\_pci\_acp5x, snd\_pci\_acp6x, snd\_acp\_pci, snd\_rpl\_pci\_acp6x, snd\_pci\_ps, snd\_sof\_amd\_renoir, snd\_sof\_amd\_rembrandt, snd\_sof\_amd\_vangogh, snd\_sof\_amd\_acp63, snd\_sof\_amd\_acp70
74:00.6 Audio device \[0403\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h/19h/1ah HD Audio Controller \[1022:15e3\]
	Subsystem: Lenovo Device \[17aa:50e7\]
	Kernel driver in use: snd\_hda\_intel
	Kernel modules: snd\_hda\_intel
75:00.0 USB controller \[0c03\]: Advanced Micro Devices, Inc. \[AMD\] Rembrandt USB4 XHCI controller #8 \[1022:161f\]
	Subsystem: Lenovo Device \[17aa:50e7\]
	Kernel driver in use: xhci\_hcd
75:00.3 USB controller \[0c03\]: Advanced Micro Devices, Inc. \[AMD\] Rembrandt USB4 XHCI controller #5 \[1022:15d6\]
	Subsystem: Lenovo Device \[17aa:50e7\]
	Kernel driver in use: xhci\_hcd
75:00.4 USB controller \[0c03\]: Advanced Micro Devices, Inc. \[AMD\] Rembrandt USB4 XHCI controller #6 \[1022:15d7\]
	Subsystem: Lenovo Device \[17aa:50e7\]
	Kernel driver in use: xhci\_hcd
75:00.5 USB controller \[0c03\]: Advanced Micro Devices, Inc. \[AMD\] Rembrandt USB4/Thunderbolt NHI controller #1 \[1022:162e\]
	Subsystem: Lenovo Device \[17aa:50e7\]
	Kernel driver in use: thunderbolt
	Kernel modules: thunderbolt

`root #``lsusb`
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 002: ID 27c6:6594 Shenzhen Goodix Technology Co.,Ltd. Goodix USB2.0 MISC
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 003 Device 002: ID 05e3:0610 Genesys Logic, Inc. Hub
Bus 003 Device 003: ID 2ce3:9563 Generic EMV Smartcard Reader
Bus 003 Device 004: ID 10ab:9309 USI Co., Ltd 
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 005 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 005 Device 002: ID 13d3:54aa IMC Networks Integrated Camera
Bus 006 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 007 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 008 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 009 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 010 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub

`root #``lsusb -vt````
/:  Bus 001.Port 001: Dev 001, Class=root_hub, Driver=xhci_hcd/4p, 480M
    ID 1d6b:0002 Linux Foundation 2.0 root hub
    |__ Port 003: Dev 002, If 0, Class=Vendor Specific Class, Driver=[none], 12M
        ID 27c6:6594 Shenzhen Goodix Technology Co.,Ltd. 
/:  Bus 002.Port 001: Dev 001, Class=root_hub, Driver=xhci_hcd/2p, 10000M
    ID 1d6b:0003 Linux Foundation 3.0 root hub
/:  Bus 003.Port 001: Dev 001, Class=root_hub, Driver=xhci_hcd/3p, 480M
    ID 1d6b:0002 Linux Foundation 2.0 root hub
    |__ Port 003: Dev 002, If 0, Class=Hub, Driver=hub/3p, 480M
        ID 05e3:0610 Genesys Logic, Inc. Hub
        |__ Port 001: Dev 003, If 0, Class=Chip/SmartCard, Driver=[none], 12M
            ID 2ce3:9563  
        |__ Port 002: Dev 004, If 0, Class=Wireless, Driver=btusb, 12M
            ID 10ab:9309 USI Co., Ltd 
        |__ Port 002: Dev 004, If 1, Class=Wireless, Driver=btusb, 12M
            ID 10ab:9309 USI Co., Ltd 
/:  Bus 004.Port 001: Dev 001, Class=root_hub, Driver=xhci_hcd/2p, 10000M
    ID 1d6b:0003 Linux Foundation 3.0 root hub
/:  Bus 005.Port 001: Dev 001, Class=root_hub, Driver=xhci_hcd/1p, 480M
    ID 1d6b:0002 Linux Foundation 2.0 root hub
    |__ Port 001: Dev 002, If 0, Class=Video, Driver=uvcvideo, 480M
        ID 13d3:54aa IMC Networks 
    |__ Port 001: Dev 002, If 1, Class=Video, Driver=uvcvideo, 480M
        ID 13d3:54aa IMC Networks 
/:  Bus 006.Port 001: Dev 001, Class=root_hub, Driver=xhci_hcd/0p, 5000M
    ID 1d6b:0003 Linux Foundation 3.0 root hub
/:  Bus 007.Port 001: Dev 001, Class=root_hub, Driver=xhci_hcd/1p, 480M
    ID 1d6b:0002 Linux Foundation 2.0 root hub
/:  Bus 008.Port 001: Dev 001, Class=root_hub, Driver=xhci_hcd/1p, 10000M
    ID 1d6b:0003 Linux Foundation 3.0 root hub
/:  Bus 009.Port 001: Dev 001, Class=root_hub, Driver=xhci_hcd/1p, 480M
    ID 1d6b:0002 Linux Foundation 2.0 root hub
/:  Bus 010.Port 001: Dev 001, Class=root_hub, Driver=xhci_hcd/1p, 10000M
    ID 1d6b:0003 Linux Foundation 3.0 root hub
```
## Installation

### Firmware

#### AMDGPU

Firmware blobs required for GPU are:

`user $``echo amdgpu/*`
amdgpu/yellow\_carp\_ce.bin amdgpu/yellow\_carp\_dmcub.bin amdgpu/yellow\_carp\_me.bin amdgpu/yellow\_carp\_mec.bin amdgpu/yellow\_carp\_mec2.bin amdgpu/yellow\_carp\_pfp.bin amdgpu/yellow\_carp\_rlc.bin amdgpu/yellow\_carp\_sdma.bin amdgpu/yellow\_carp\_ta.bin amdgpu/yellow\_carp\_toc.bin amdgpu/yellow\_carp\_vcn.bin

#### ATH11K

Firmware blobs required for Wi-Fi connectivity are:

`user $``echo ath11k/*`
ath11k/WCN6855/hw2.1/amss.bin ath11k/WCN6855/hw2.1/board-2.bin ath11k/WCN6855/hw2.1/firmware-2.bin ath11k/WCN6855/hw2.1/m3.bin

#### BTUSB

Firmware blobs required for Bluetooth connectivity are:

`user $``echo qca/*`
qca/nvm\_usb\_00130201\_gf.bin qca/rampatch\_usb\_00130201.bin

### Kernel

TODO.

### Emerge

TODO.

## Configuration

### Portage

**`/etc/portage/make.conf`**
