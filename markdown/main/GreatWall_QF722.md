<!-- source: https://wiki.gentoo.org/wiki/GreatWall_QF722 | group: Gentoo Wiki (Main) | wiki-title: GreatWall QF722 -->
---
title: GreatWall QF722
url: https://wiki.gentoo.org/wiki/GreatWall_QF722
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-20"
fingerprint: "3f103e5fc3b23379"
license: CC BY-SA 4.0
---

# GreatWall QF722

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The GreatWall QF722 is an AArch64 based laptop featuring an ARMv8 based Phytium FT-2000/4-M processor.

This product is not meant for consumer market but government usage, which makes it niche and expensive to get (as brand new).

**Pros:**

- More standardized overall hardware architecture compared to other SoC-based ARM Laptops

**Cons:**

- Niche and expensive to get on the consumer market
- Some hardware have no mainline support
- Barely benefits anything from using an ARM processor (less noise and heat, longer battery life, etc. compared to x86-based laptops)

## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | Phytium FT-2000/4-M |  | N/A | N/A | 6.12.21-gentoo-dist | SCMI based governor having issues, unable to scale CPU frequency; workloads are not properly distributed among cores | 
| GPU | AMD RX550X |  | 1002:6987:17aa:380e / 03-00 | amdgpu | 6.12.21-gentoo-dist |  | 
| NVMe ssd | WD Blue SN550 NVMe SSD 256GB |  | - | nvme | 6.12.21-gentoo-dist |  | 
| WLAN Adapter | RTL8821CE 802.11ac PCIe Wireless Network Adapter |  | 10ec:c821:1a3b:3043 / 02-80-00 | rtw88\_8821ce | 6.12.21-gentoo-dist |  | 
| Bluetooth Adapter | IMC Networks Bluetooth Radio |  | 13d3:3533 / e0-01-01 | btusb | 6.12.21-gentoo-dist | Doesn't work on NeoKylin 5.10 kernel due to linux-firmware issues | 
| USB Controller | ASMedia Technology Inc. ASM2142/ASM3142 USB 3.1 Host Controller |  | 1b21:2142:1b21:2142 / 0c-03-30 | xhci\_hcd | 6.12.21-gentoo-dist |  | 
| USB Controller | VIA Technologies, Inc. VL805/806 xHCI USB 3.0 Controller |  | 1106:3483:1106:3483 / 0c-03-30 | xhci\_hcd | 6.12.21-gentoo-dist |  | 
| Fingerprint Sensor | PXAT MOH |  | 2f0a:0f0d / ff-00-00 | - | 6.12.21-gentoo-dist | No drivers available. | 
| Audio | PhyHDA (wrapped Realtek ALC269VC) |  | ? | - | 6.12.21-gentoo-dist | No drivers available. | 
| Webcam | Shenzhen Kingcome Optoelectronic KNC-172F |  | 2b7e:0172 / 0e-01-00 | uvcvideo | 6.12.21-gentoo-dist |  | 
| Embedded Controller (EC) | - |  | - | gwnb\_power | 6.12.21-gentoo-dist | Requires out-of-tree kernel module; Sluggish, power status change takes \~30s to be reflected | 
| Keyboard | AT Translated Set 2 keyboard |  | ps/2:0001-0001-at-translated-set-2-keyboard | ft8042 | 6.12.21-gentoo-dist | Requires out-of-tree kernel module; multimedia keys non-functional. | 
| Trackpad | ImPS/2 Wheel Mouse |  | ps/2:logitech-0005-imps-2-wheel-mouse | ft8042 | 6.12.21-gentoo-dist | Requires out-of-tree kernel module. | 

### Detailed information

`root #``uname -r`
6.12.21-gentoo

`root #``lscpu````
Architecture:             aarch64
  CPU op-mode(s):         32-bit, 64-bit
  Byte Order:             Little Endian
CPU(s):                   4
  On-line CPU(s) list:    0-3
Vendor ID:                Phytium
  BIOS Vendor ID:         PHYTIUM LTD
  Model name:             FTC663
    BIOS Model name:      FT-2000/4-M Greatwall CPU @ 2.2GHz
    BIOS CPU family:      257
    Model:                3
    Thread(s) per core:   1
    Core(s) per socket:   4
    Socket(s):            1
    Stepping:             0x1
    BogoMIPS:             96.00
    Flags:                fp asimd evtstrm aes pmull sha1 sha2 crc32 cpuid
Caches (sum of all):      
  L1d:                    64 KiB (3 instances)
  L1i:                    96 KiB (3 instances)
  L2:                     2 MiB (2 instances)
  L3:                     4 MiB (1 instance)
NUMA:                     
  NUMA node(s):           1
  NUMA node0 CPU(s):      0-3
Vulnerabilities:          
  Gather data sampling:   Not affected
  Itlb multihit:          Not affected
  L1tf:                   Not affected
  Mds:                    Not affected
  Meltdown:               Mitigation; PTI
  Mmio stale data:        Not affected
  Reg file data sampling: Not affected
  Retbleed:               Not affected
  Spec rstack overflow:   Not affected
  Spec store bypass:      Not affected
  Spectre v1:             Mitigation; __user pointer sanitization
  Spectre v2:             Not affected
  Srbds:                  Not affected
  Tsx async abort:        Not affected
```
`root #``lspci -nnk`
00:00.0 PCI bridge \[0604\]: Cadence Design Systems, Inc. Device \[17cd:dc16\]
	Kernel driver in use: pcieport
00:01.0 PCI bridge \[0604\]: Cadence Design Systems, Inc. Device \[17cd:dc08\]
	Kernel driver in use: pcieport
00:02.0 PCI bridge \[0604\]: Cadence Design Systems, Inc. Device \[17cd:dc01\]
	Kernel driver in use: pcieport
00:03.0 PCI bridge \[0604\]: Cadence Design Systems, Inc. Device \[17cd:dc16\]
	Kernel driver in use: pcieport
00:04.0 PCI bridge \[0604\]: Cadence Design Systems, Inc. Device \[17cd:dc08\]
	Kernel driver in use: pcieport
00:05.0 PCI bridge \[0604\]: Cadence Design Systems, Inc. Device \[17cd:dc01\]
	Kernel driver in use: pcieport
01:00.0 VGA compatible controller \[0300\]: Advanced Micro Devices, Inc. \[AMD/ATI\] Lexa \[Radeon 540X/550X/630 / RX 640 / E9171 MCM\] \[1002:6987\] (rev c0)
	Subsystem: Lenovo Device \[17aa:380e\]
	Kernel driver in use: amdgpu
	Kernel modules: amdgpu
01:00.1 Audio device \[0403\]: Advanced Micro Devices, Inc. \[AMD/ATI\] Baffin HDMI/DP Audio \[Radeon RX 550 640SP / RX 560/560X\] \[1002:aae0\]
	Subsystem: Lenovo Device \[17aa:aae0\]
	Kernel driver in use: snd\_hda\_intel
	Kernel modules: snd\_hda\_intel
02:00.0 USB controller \[0c03\]: ASMedia Technology Inc. ASM2142/ASM3142 USB 3.1 Host Controller \[1b21:2142\]
	Subsystem: ASMedia Technology Inc. ASM2142/ASM3142 USB 3.1 Host Controller \[1b21:2142\]
	Kernel driver in use: xhci\_hcd
04:00.0 USB controller \[0c03\]: VIA Technologies, Inc. VL805/806 xHCI USB 3.0 Controller \[1106:3483\] (rev 01)
	Subsystem: VIA Technologies, Inc. VL805/806 xHCI USB 3.0 Controller \[1106:3483\]
	Kernel driver in use: xhci\_hcd
05:00.0 Non-Volatile memory controller \[0108\]: Sandisk Corp SanDisk Ultra 3D / WD PC SN530, IX SN530, Blue SN550 NVMe SSD (DRAM-less) \[15b7:5009\] (rev 01)
	Subsystem: Sandisk Corp WD Blue SN550 NVMe SSD \[15b7:5009\]
	Kernel driver in use: nvme
06:00.0 Network controller \[0280\]: Realtek Semiconductor Co., Ltd. RTL8821CE 802.11ac PCIe Wireless Network Adapter \[10ec:c821\]
	Subsystem: AzureWave Device \[1a3b:3043\]
	Kernel driver in use: rtw\_8821ce
	Kernel modules: rtw88\_8821ce

`root #``lsusb`
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 003 Device 002: ID 2109:3431 VIA Labs, Inc. Hub
Bus 003 Device 003: ID 2f0a:0f0d PXAT MOH
Bus 003 Device 004: ID 13d3:3533 IMC Networks Bluetooth Radio 
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub

`root #``lsusb -vt````
/:  Bus 001.Port 001: Dev 001, Class=root_hub, Driver=xhci_hcd/2p, 480M
    ID 1d6b:0002 Linux Foundation 2.0 root hub
/:  Bus 002.Port 001: Dev 001, Class=root_hub, Driver=xhci_hcd/2p, 10000M
    ID 1d6b:0003 Linux Foundation 3.0 root hub
/:  Bus 003.Port 001: Dev 001, Class=root_hub, Driver=xhci_hcd/1p, 480M
    ID 1d6b:0002 Linux Foundation 2.0 root hub
    |__ Port 001: Dev 002, If 0, Class=Hub, Driver=hub/4p, 480M
        ID 2109:3431 VIA Labs, Inc. Hub
        |__ Port 001: Dev 003, If 0, Class=Vendor Specific Class, Driver=[none], 12M
            ID 2f0a:0f0d  
        |__ Port 003: Dev 004, If 0, Class=Wireless, Driver=btusb, 12M
            ID 13d3:3533 IMC Networks 
        |__ Port 003: Dev 004, If 1, Class=Wireless, Driver=btusb, 12M
            ID 13d3:3533 IMC Networks 
/:  Bus 004.Port 001: Dev 001, Class=root_hub, Driver=xhci_hcd/4p, 5000M
    ID 1d6b:0003 Linux Foundation 3.0 root hub
```
## Installation

Due to missing specifically tailored PS/2 controller drivers, the built-in keyboard and trackpad won't be functional in Gentoo Minimal Installation Disk and many other LiveCD environments. Either prepare a set of USB keyboard (& mouse for GUI), or use few distros that have this driver built in: [openKylin](https://www.openkylin.top/index-en.html), [NeoKylin](https://kylinos.cn/), or [Uniontech UOS](https://uos.uniontech.com/).

### Firmware

The firmware follows UEFI 2.7 standard. No DeviceTree is used.

[Handbook:AMD64/Installation/Bootloader#UEFI systems](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Bootloader#UEFI_systems) could be used as a cross reference to set up GRUB.

### Kernel

#### Very Basics

**Basic Configurations**

#### Power Management

**Power Management Options**

#### USB Support

**USB Support**

#### Keyboard & Trackpad

**Keyboard & Trackpad**

#### WLAN

**WLAN**

#### Webcam

**Enable USB Video Class for Webcam**

#### GPU

Reference to [AMDGPU](https://wiki.gentoo.org/wiki/AMDGPU).

#### Sound

Reference to [ALSA](https://wiki.gentoo.org/wiki/ALSA).

#### Bluetooth

Reference to [Bluetooth](https://wiki.gentoo.org/wiki/Bluetooth). The Bluetooth adapter firmware requires [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware).

### Alternative: Using Phytium-Linux-Kernel

Phytium Corporation provided a forked 6.6.133 kernel for their processors. However, this kernel still needs out-of-tree modules to function on this machine.

#### Very Basics

This machine has a buggy ACPI PPTT table in the firmware so `CONFIG_SCHED_MC` option must be disabled. Otherwise, the `topology_span_sane` in the kernel would fail and eventually cause scheduler failure. The symptom would be :

- A dmesg error like this:

- All the processes are being assigned to CPU core 0, while the others remain idle, unless `taskset` is used to set the core that they run on.

### Alternative: Using NeoKylin Kernel

**Todo:**

- Deprecated. This kernel is being outdated and with a lot of oddball issues

Using the kernel and kernel modules from NeoKylin will easily solve CPU governor, keyboard, trackpad, EC, and audio issues, as this device has been certified to be compatible with NeoKylin.

Acquire NeoKylin kernel from NeoKylin rootfs, either of a NeoKylin LiveCD or full installation, and copy it into places:

`root #``cp ${KYLIN_ROOTFS}/vmlinuz-${KERNEL_VERSION}-generic /boot/vmlinuz-${KERNEL_VERSION}-generic``root #``cp -r ${KYLIN_ROOTFS}/lib/modules/${KERNEL_VERSION}-generic /lib/modules/${KERNEL_VERSION}-generic`
Then rengenrate initramfs and grub configuration:

`root #``dracut --kver ${KERNEL_VERSION}-generic && grub-mkconfig -o /boot/grub/grub.cfg`
#### Optional: reinstalling linux-firmware

[sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) has to be reinstalled without USE flag `compress-zstd` if formerly enabled, as NeoKylin kernel didn't enable ZSTD-compressed firmware support (till the last edit of this article).

**`/etc/portage/package.use/linux-firmware`**

**Disabling compress-zstd USE flag**

`root #``emerge -av linux-firmware`
#### Optional: Suppressing Oddball Kernel Messages

NeoKylin kernel may generate oddball kernel messages such as `Authorization binary is corrupted, please call xxx-xxx-xxxx for help` or `denied {execute} for pid 1 comm='systemd' name='/usr/lib/systemd/systemd'...` . These are due to missing userland executables for Kysec, a security measure in NeoKylin kernel. These executables aren't actually being blocked from executing, but could be annoying while reading the output of dmesg for debugging purposes.

The kernel parameter `security=` could be passed to the kernel on boot to disable Kysec to suppress these kernel messages.

### Emerge

#### Keyboard & Mouse

The built-in keyboard and trackpad use the PS/2 protocol and an Intel 8042-compatible interface, which is not supported by the mainline Linux kernel on ARM. A modded `i8042` kernel module called `ft8042` is required. This module could be acquired from [ATZLinux Kernel project](https://gitee.com/atzlinux/atzlinux-kernel/tree/master/drivers/input/serio/ft8042), but required patching to compile on Linux kernel 6.12 .

A patched version made by the author of this article could be found at [here](https://github.com/Randname666/ft8042).

#### Embedded Controller (EC)

The EC requires a platform-specific driver `gwnb_power` to function. This could be acquired from [ATZLinux Kernel project](https://gitee.com/atzlinux/atzlinux-kernel/tree/master/drivers/power/supply/gw-nb-ec).

#### Sound

Built-in audio is provided by PhyHDA, which is probably wrapped/rebranded/modified Realtek Audio.

The driver for this sound card is out-of-tree, way too broken, and may require extensive effort to adapt to Linux kernel 6.12+.

#### Linux-firmware

Bluetooth and GPU require [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware).

`root #``emerge --ask sys-kernel/linux-firmware`
## Configuration

### Optional: Capping CPU Frequency

As the CPU frequency scaling is not functional, the CPU frequency may be capped in UEFI setup to reduce heat and battery consumption, if no demanding tasks will be performed.

Press `Delete` on boot to enter UEFI setup.

### Optional: Change the CPU governor

NeoKylin kernel is compiled with Performance set as default governor. It could be changed to other governors in order to reduce heat and battery consumption.

Reference to [Power management/Processor#Manual governor/driver change](https://wiki.gentoo.org/wiki/Power_management/Processor#Manual_governor.2Fdriver_change) and [Power management/Guide#Using Laptop Mode Tools](https://wiki.gentoo.org/wiki/Power_management/Guide#Using_Laptop_Mode_Tools).
