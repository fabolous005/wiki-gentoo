<!-- source: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_T14_Gen3_(Intel) | group: Gentoo Wiki (Main) | wiki-title: Lenovo ThinkPad T14 Gen3 (Intel) -->
---
title: Lenovo ThinkPad T14 Gen3 (Intel)
url: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_T14_Gen3_(Intel)
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-06-27"
fingerprint: "3f5b9d176baa1369"
license: CC BY-SA 4.0
---

# Lenovo ThinkPad T14 Gen3 (Intel)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Following article aims to make the Lenovo ThinkPad T14 Gen3 (21AH) usable on Gentoo.

## Hardware information

### lscpu

`root #``lscpu````
Architecture:            x86_64
  CPU op-mode(s):        32-bit, 64-bit
  Address sizes:         46 bits physical, 48 bits virtual
  Byte Order:            Little Endian
CPU(s):                  16
  On-line CPU(s) list:   0-15
Vendor ID:               GenuineIntel
  BIOS Vendor ID:        Intel(R) Corporation
  Model name:            12th Gen Intel(R) Core(TM) i7-1270P
    BIOS Model name:     12th Gen Intel(R) Core(TM) i7-1270P None CPU @ 2.2GHz
    BIOS CPU family:     198
    CPU family:          6
    Model:               154
    Thread(s) per core:  2
    Core(s) per socket:  12
    Socket(s):           1
    Stepping:            3
    CPU(s) scaling MHz:  63%
    CPU max MHz:         4800.0000
    CPU min MHz:         400.0000
    BogoMIPS:            4993.00
    Flags:               fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc art arch_perfmon pebs bts rep_good 
                         nopl xtopology nonstop_tsc cpuid aperfmperf tsc_known_freq pni pclmulqdq dtes64 monitor ds_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes
                          xsave avx f16c rdrand lahf_lm abm 3dnowprefetch cpuid_fault epb ssbd ibrs ibpb stibp ibrs_enhanced tpr_shadow flexpriority ept vpid ept_ad fsgsbase tsc_adjust bmi1 avx2 smep bmi2 erms invpcid rdse
                         ed adx smap clflushopt clwb intel_pt sha_ni xsaveopt xsavec xgetbv1 xsaves split_lock_detect avx_vnni dtherm ida arat pln pts hwp hwp_notify hwp_act_window hwp_epp hwp_pkg_req hfi vnmi umip pku osp
                         ke waitpkg gfni vaes vpclmulqdq tme rdpid movdiri movdir64b fsrm md_clear serialize pconfig arch_lbr ibt flush_l1d arch_capabilities
```
### lspci

`root #``lspci -nnk`
00:00.0 Host bridge \[0600\]: Intel Corporation Device \[8086:4621\] (rev 02)
	Subsystem: Lenovo Device \[17aa:22e8\]
	Kernel driver in use: igen6\_edac
	Kernel modules: igen6\_edac
00:02.0 VGA compatible controller \[0300\]: Intel Corporation Alder Lake-P Integrated Graphics Controller \[8086:46a6\] (rev 0c)
	Subsystem: Lenovo Alder Lake-P Integrated Graphics Controller \[17aa:22e8\]
	Kernel driver in use: i915
	Kernel modules: i915
00:04.0 Signal processing controller \[1180\]: Intel Corporation Alder Lake Innovation Platform Framework Processor Participant \[8086:461d\] (rev 02)
	Subsystem: Lenovo Alder Lake Innovation Platform Framework Processor Participant \[17aa:22e8\]
	Kernel driver in use: proc\_thermal\_pci
	Kernel modules: processor\_thermal\_device\_pci
00:06.0 PCI bridge \[0604\]: Intel Corporation 12th Gen Core Processor PCI Express x4 Controller #0 \[8086:464d\] (rev 02)
	Subsystem: Lenovo 12th Gen Core Processor PCI Express x4 Controller \[17aa:22e8\]
	Kernel driver in use: pcieport
00:07.0 PCI bridge \[0604\]: Intel Corporation Alder Lake-P Thunderbolt 4 PCI Express Root Port #0 \[8086:466e\] (rev 02)
	Subsystem: Lenovo Alder Lake-P Thunderbolt 4 PCI Express Root Port \[17aa:22e8\]
	Kernel driver in use: pcieport
00:07.2 PCI bridge \[0604\]: Intel Corporation Alder Lake-P Thunderbolt 4 PCI Express Root Port #2 \[8086:462f\] (rev 02)
	Subsystem: Lenovo Alder Lake-P Thunderbolt 4 PCI Express Root Port \[17aa:22e8\]
	Kernel driver in use: pcieport
00:0a.0 Signal processing controller \[1180\]: Intel Corporation Platform Monitoring Technology \[8086:467d\] (rev 01)
	Subsystem: Lenovo Platform Monitoring Technology \[17aa:22e8\]
	Kernel driver in use: intel\_vsec
	Kernel modules: intel\_vsec
00:0d.0 USB controller \[0c03\]: Intel Corporation Alder Lake-P Thunderbolt 4 USB Controller \[8086:461e\] (rev 02)
	Subsystem: Lenovo Alder Lake-P Thunderbolt 4 USB Controller \[17aa:22e8\]
	Kernel driver in use: xhci\_hcd
00:0d.2 USB controller \[0c03\]: Intel Corporation Alder Lake-P Thunderbolt 4 NHI #0 \[8086:463e\] (rev 02)
	Subsystem: Lenovo Alder Lake-P Thunderbolt 4 NHI \[17aa:22e8\]
	Kernel driver in use: thunderbolt
	Kernel modules: thunderbolt
00:0d.3 USB controller \[0c03\]: Intel Corporation Alder Lake-P Thunderbolt 4 NHI #1 \[8086:466d\] (rev 02)
	Subsystem: Lenovo Alder Lake-P Thunderbolt 4 NHI \[17aa:22e8\]
	Kernel driver in use: thunderbolt
	Kernel modules: thunderbolt
00:14.0 USB controller \[0c03\]: Intel Corporation Alder Lake PCH USB 3.2 xHCI Host Controller \[8086:51ed\] (rev 01)
	Subsystem: Lenovo Alder Lake PCH USB 3.2 xHCI Host Controller \[17aa:22e8\]
	Kernel driver in use: xhci\_hcd
00:14.2 RAM memory \[0500\]: Intel Corporation Alder Lake PCH Shared SRAM \[8086:51ef\] (rev 01)
	Subsystem: Lenovo Alder Lake PCH Shared SRAM \[17aa:22e8\]
00:14.3 Network controller \[0280\]: Intel Corporation Alder Lake-P PCH CNVi WiFi \[8086:51f0\] (rev 01)
	Subsystem: Intel Corporation Alder Lake-P PCH CNVi WiFi \[8086:0090\]
	Kernel driver in use: iwlwifi
	Kernel modules: iwlwifi
00:15.0 Serial bus controller \[0c80\]: Intel Corporation Alder Lake PCH Serial IO I2C Controller #0 \[8086:51e8\] (rev 01)
	Subsystem: Lenovo Alder Lake PCH Serial IO I2C Controller \[17aa:22e8\]
	Kernel driver in use: intel-lpss
00:16.0 Communication controller \[0780\]: Intel Corporation Alder Lake PCH HECI Controller \[8086:51e0\] (rev 01)
	Subsystem: Lenovo Alder Lake PCH HECI Controller \[17aa:22e8\]
	Kernel driver in use: mei\_me
	Kernel modules: mei\_me
00:1f.0 ISA bridge \[0601\]: Intel Corporation Alder Lake PCH eSPI Controller \[8086:5182\] (rev 01)
	Subsystem: Lenovo Alder Lake PCH eSPI Controller \[17aa:22e8\]
00:1f.3 Audio device \[0403\]: Intel Corporation Alder Lake PCH-P High Definition Audio Controller \[8086:51c8\] (rev 01)
	Subsystem: Lenovo Alder Lake PCH-P High Definition Audio Controller \[17aa:22e8\]
	Kernel driver in use: snd\_hda\_intel
	Kernel modules: snd\_hda\_intel, snd\_sof\_pci\_intel\_tgl
00:1f.4 SMBus \[0c05\]: Intel Corporation Alder Lake PCH-P SMBus Host Controller \[8086:51a3\] (rev 01)
	Subsystem: Lenovo Alder Lake PCH-P SMBus Host Controller \[17aa:22e8\]
	Kernel driver in use: i801\_smbus
	Kernel modules: i2c\_i801
00:1f.5 Serial bus controller \[0c80\]: Intel Corporation Alder Lake-P PCH SPI Controller \[8086:51a4\] (rev 01)
	Subsystem: Lenovo Alder Lake-P PCH SPI Controller \[17aa:22e8\]
00:1f.6 Ethernet controller \[0200\]: Intel Corporation Ethernet Connection (16) I219-LM \[8086:1a1e\] (rev 01)
	Subsystem: Lenovo Ethernet Connection (16) I219-LM \[17aa:22e8\]
	Kernel driver in use: e1000e
	Kernel modules: e1000e
02:00.0 Non-Volatile memory controller \[0108\]: Samsung Electronics Co Ltd NVMe SSD Controller PM9B1 \[144d:a80b\] (rev 02)
	Subsystem: Samsung Electronics Co Ltd NVMe SSD Controller PM9B1 \[144d:a80b\]
	Kernel driver in use: nvme
	Kernel modules: nvme

### lsusb

`root #``lsusb`
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 003 Device 003: ID 174f:1812 Syntek Integrated Camera
Bus 003 Device 002: ID 27c6:6594 Shenzhen Goodix Technology Co.,Ltd. Goodix USB2.0 MISC
Bus 003 Device 005: ID 8087:0033 Intel Corp. 
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

## Installation

#### UEFI updates

EFI updates can be done through Linux according to the [driver list](https://pcsupport.lenovo.com/us/en/products/laptops-and-netbooks/thinkpad-t-series-laptops/thinkpad-t14-gen-3-type-21ah-21aj/downloads/driver-list)

#### Legacy boot

I haven't really looked into it, but I didn't much find a way to boot anything using Legacy boot (if you did, update this part)

### Firmware

The [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) package is required as it provides the firmware for the Intel iRIS driver.

### Kernel

As of kernel version 6.4.x, everything is functional, except for the sound and [NVMe](https://wiki.gentoo.org/wiki/NVMe) which they need a [GRUB](https://wiki.gentoo.org/wiki/GRUB) modification.

**`/etc/default/grub`**

```
# Add this line to reduce NVMe failures, since latency is not matched by default between hardware and software!
GRUB_CMDLINE_LINUX="nvme_core.default_ps_max_latency_us=0"
# Add this line to enable audio...
GRUB_CMDLINE_LINUX_DEFAULT="snd_hda_intel.dmic_detect=0"
```
Then update GRUB's configuration:

`root #``grub-mkconfig -o /boot/grub/grub.cfg`
## Configuration

#### Compiler settings

More info is available at:

**`/etc/portage/make.conf`**

```
# O2 for super-safe output
CHOST="x86_64-pc-linux-gnu"
CFLAGS="-march=alderlake -O2 -pipe"
CXXFLAGS="${CFLAGS}"
FCFLAGS="${CFLAGS}"
FFLAGS="${CFLAGS}"
MAKEOPTS="-j8"
```
#### Packages settings

For more info: [CPU\_FLAGS\_X86](https://wiki.gentoo.org/wiki/CPU_FLAGS_X86)

**`/etc/portage/make.conf`**

```
# Obtained via cpuid2cpuflags
CPU_FLAGS_X86="aes avx avx2 f16c fma3 mmx mmxext pclmul popcnt rdrand sha sse sse2 sse3 sse4_1 sse4_2 ssse3"
```
#### USE flags

**`/etc/portage/make.conf`**

```
VIDEO_CARDS="intel"
# Input devices
INPUT_DEVICES="libinput"
# Useflags
USE="libinput -xvmc -vdpau -cuda -radeonsi -amdgpu"
```
