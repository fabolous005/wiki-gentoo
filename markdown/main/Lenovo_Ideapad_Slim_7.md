<!-- source: https://wiki.gentoo.org/wiki/Lenovo_Ideapad_Slim_7 | group: Gentoo Wiki (Main) | wiki-title: Lenovo Ideapad Slim 7 -->
---
title: Lenovo Ideapad Slim 7
url: https://wiki.gentoo.org/wiki/Lenovo_Ideapad_Slim_7
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: bd46ac5ddb36906d
license: CC BY-SA 4.0
---

# Lenovo Ideapad Slim 7

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


The IdeaPad Slim 7 is a Laptop released in 2020 with a Ryzen 4700U (Renoir) APU
and 8GB of dual-channel memory. The hardware is generally well-supported by
open-source drivers.

## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | AMD Ryzen 7 4700U with Radeon Graphics |  | N/A | N/A | 5.7-rc7 | 8 cores, 8 threads, Integrated GPU | 
| GPU | Vega |  | 1002:1636 | amdgpu | 5.7-rc7 | Tested with Mesa 20.2.0-devel | 
| Sound | Advanced Micro Devices, Inc. \[AMD\] Family 17h (Models 10h-1fh) HD Audio Controller |  | 1022:15e3 | snd\_hda\_intel | 5.7-rc7 | See the note in the "Troubleshooting" section when dual-booting Windows. | 
| Wifi and Bluetooth | Intel Corporation Wi-Fi 6 AX200 |  | 8086:2723 | iwlwifi | 5.7-rc7 |  | 
| SD Card Reader | Realtek Semiconductor Corp. |  | 0bda:0177 | ums-realtek | 5.7-rc7 |  | 
| SMBus | Advanced Micro Devices, Inc. \[AMD\] FCH SMBus Controller |  | 1022:790b | i2c\_piix4, sp5100\_tco | 5.7-rc7 |  | 
| Touchpad | Unknown |  | 1022:790b | i2c\_hid, hid\_multitouch | 5.7-rc7 |  | 
| I2C bus | Synopsys DesignWare I2C adapter |  |  | i2c-designware-pci | 5.7-rc7 | Required for the touchpad to work | 
| Webcam | Chicony Electronics Co., Ltd |  | 04f2:b6cb | uvcvideo | 5.7-rc7 |  | 
| Fingerprint Sensor | Elan Microelectronics Corp. |  | 04f3:0c4d |  |  |  | 

### lspci

`user $``lspci -nn -k`
00:00.0 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Renoir Root Complex \[1022:1630\]
	Subsystem: Lenovo Renoir Root Complex \[17aa:380d\]
00:00.2 IOMMU \[0806\]: Advanced Micro Devices, Inc. \[AMD\] Renoir IOMMU \[1022:1631\]
	Subsystem: Lenovo Renoir IOMMU \[17aa:380d\]
00:01.0 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Renoir PCIe Dummy Host Bridge \[1022:1632\]
00:02.0 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Renoir PCIe Dummy Host Bridge \[1022:1632\]
00:02.2 PCI bridge \[0604\]: Advanced Micro Devices, Inc. \[AMD\] Renoir PCIe GPP Bridge \[1022:1634\]
	Kernel driver in use: pcieport
00:02.3 PCI bridge \[0604\]: Advanced Micro Devices, Inc. \[AMD\] Renoir PCIe GPP Bridge \[1022:1634\]
	Kernel driver in use: pcieport
00:08.0 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Renoir PCIe Dummy Host Bridge \[1022:1632\]
00:08.1 PCI bridge \[0604\]: Advanced Micro Devices, Inc. \[AMD\] Renoir Internal PCIe GPP Bridge to Bus \[1022:1635\]
	Kernel driver in use: pcieport
00:08.2 PCI bridge \[0604\]: Advanced Micro Devices, Inc. \[AMD\] Renoir Internal PCIe GPP Bridge to Bus \[1022:1635\]
	Kernel driver in use: pcieport
00:14.0 SMBus \[0c05\]: Advanced Micro Devices, Inc. \[AMD\] FCH SMBus Controller \[1022:790b\] (rev 51)
	Subsystem: Lenovo FCH SMBus Controller \[17aa:380d\]
	Kernel driver in use: piix4\_smbus
	Kernel modules: sp5100\_tco
00:14.3 ISA bridge \[0601\]: Advanced Micro Devices, Inc. \[AMD\] FCH LPC Bridge \[1022:790e\] (rev 51)
	Subsystem: Lenovo FCH LPC Bridge \[17aa:380d\]
00:18.0 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Renoir Device 24: Function 0 \[1022:1448\]
00:18.1 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Renoir Device 24: Function 1 \[1022:1449\]
00:18.2 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Renoir Device 24: Function 2 \[1022:144a\]
00:18.3 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Renoir Device 24: Function 3 \[1022:144b\]
00:18.4 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Renoir Device 24: Function 4 \[1022:144c\]
00:18.5 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Renoir Device 24: Function 5 \[1022:144d\]
00:18.6 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Renoir Device 24: Function 6 \[1022:144e\]
00:18.7 Host bridge \[0600\]: Advanced Micro Devices, Inc. \[AMD\] Renoir Device 24: Function 7 \[1022:144f\]
01:00.0 Network controller \[0280\]: Intel Corporation Wi-Fi 6 AX200 \[8086:2723\] (rev 1a)
	Subsystem: Intel Corporation Wi-Fi 6 AX200 \[8086:0084\]
	Kernel driver in use: iwlwifi
	Kernel modules: iwlwifi
02:00.0 Non-Volatile memory controller \[0108\]: Sandisk Corp Device \[15b7:5006\]
	Subsystem: Sandisk Corp Device \[15b7:5006\]
	Kernel driver in use: nvme
03:00.0 VGA compatible controller \[0300\]: Advanced Micro Devices, Inc. \[AMD/ATI\] Renoir \[1002:1636\] (rev c2)
	Subsystem: Lenovo Renoir \[17aa:380d\]
	Kernel driver in use: amdgpu
03:00.1 Audio device \[0403\]: Advanced Micro Devices, Inc. \[AMD/ATI\] Device \[1002:1637\]
	Subsystem: Lenovo Device \[17aa:380d\]
	Kernel driver in use: snd\_hda\_intel
03:00.2 Encryption controller \[1080\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h (Models 10h-1fh) Platform Security Processor \[1022:15df\]
	Subsystem: Lenovo Family 17h (Models 10h-1fh) Platform Security Processor \[17aa:380d\]
03:00.3 USB controller \[0c03\]: Advanced Micro Devices, Inc. \[AMD\] Renoir USB 3.1 \[1022:1639\]
	Subsystem: Lenovo Renoir USB 3.1 \[17aa:380d\]
	Kernel driver in use: xhci\_hcd
03:00.4 USB controller \[0c03\]: Advanced Micro Devices, Inc. \[AMD\] Renoir USB 3.1 \[1022:1639\]
	Subsystem: Lenovo Renoir USB 3.1 \[17aa:380d\]
	Kernel driver in use: xhci\_hcd
03:00.5 Multimedia controller \[0480\]: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2/FireFlight/Renoir Audio Processor \[1022:15e2\] (rev 01)
	Subsystem: Lenovo Raven/Raven2/FireFlight/Renoir Audio Processor \[17aa:380d\]
03:00.6 Audio device \[0403\]: Advanced Micro Devices, Inc. \[AMD\] Family 17h (Models 10h-1fh) HD Audio Controller \[1022:15e3\]
	Subsystem: Lenovo Family 17h (Models 10h-1fh) HD Audio Controller \[17aa:380d\]
	Kernel driver in use: snd\_hda\_intel
03:00.7 Signal processing controller \[1180\]: Advanced Micro Devices, Inc. \[AMD\] Raven/Raven2/Renoir Sensor Fusion Hub \[1022:15e4\]
	Subsystem: Lenovo Raven/Raven2/Renoir Sensor Fusion Hub \[17aa:380d\]
04:00.0 SATA controller \[0106\]: Advanced Micro Devices, Inc. \[AMD\] FCH SATA Controller \[AHCI mode\] \[1022:7901\] (rev 81)
	Subsystem: Lenovo FCH SATA Controller \[AHCI mode\] \[17aa:380d\]
	Kernel driver in use: ahci
04:00.1 SATA controller \[0106\]: Advanced Micro Devices, Inc. \[AMD\] FCH SATA Controller \[AHCI mode\] \[1022:7901\] (rev 81)
	Subsystem: Lenovo FCH SATA Controller \[AHCI mode\] \[17aa:380d\]
	Kernel driver in use: ahci

### lsusb

`user $``lsusb`
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 003 Device 004: ID 8087:0029 Intel Corp.
Bus 003 Device 003: ID 04f3:0c4d Elan Microelectronics Corp.
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 001 Device 003: ID 04f2:b6cb Chicony Electronics Co., Ltd
Bus 001 Device 007: ID 0bda:0177 Realtek Semiconductor Corp.
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

### i2cdetect

`root #``i2cdetect -l`
i2c-3	i2c       	AMDGPU DM i2c hw bus 1          	I2C adapter
i2c-1	i2c       	Synopsys DesignWare I2C adapter 	I2C adapter
i2c-8	smbus     	SMBus PIIX4 adapter port 2 at 0b00	SMBus adapter
i2c-6	i2c       	AMDGPU DM aux hw bus 2          	I2C adapter
i2c-4	i2c       	AMDGPU DM i2c hw bus 2          	I2C adapter
i2c-2	i2c       	AMDGPU DM i2c hw bus 0          	I2C adapter
i2c-0	i2c       	Synopsys DesignWare I2C adapter 	I2C adapter
i2c-7	smbus     	SMBus PIIX4 adapter port 0 at 0b00	SMBus adapter
i2c-5	i2c       	AMDGPU DM aux hw bus 0          	I2C adapter

## Installation

To access the UEFI BIOS menu, hold the F2 key while powering the system on. It may be necessary to disable secure boot to boot from the installation USB drive. To access the UEFI boot menu, hold the F12 key while the system powers on. This can also be useful when dual-booting with Windows.

There was nothing unusual about the installation process for this laptop. Be sure that the correct kernel options are enabled and that the GPU and wireless firmware are included.

### Firmware

All of the needed firmware is available in [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware).

amdgpu/renoir\_asd.bin
amdgpu/renoir\_me.bin
amdgpu/renoir\_rlc.bin
amdgpu/renoir\_ce.bin
amdgpu/renoir\_mec.bin
amdgpu/renoir\_sdma.bin
amdgpu/renoir\_dmcub.bin
amdgpu/renoir\_mec2.bin
amdgpu/renoir\_vcn.bin
amdgpu/renoir\_gpu\_info.bin
amdgpu/renoir\_pfp.bin
amd-ucode/microcode\_amd\_fam17h.bin
iwlwifi-cc-a0-46.ucode

### Kernel

See the above list when filling in the "Build named firmware blobs into the kernel binary" section.

**Enable support for these hardware drivers**

## Troubleshooting

### Corrupted or Silent Audio

The laptop can get into a state where the speakers are silent and the headphone jack outputs a very unpleasant squealing sound. This is caused by booting into Windows and then rebooting the laptop into Linux. To fix this issue, power down the laptop and wait for roughly 30 seconds before turning it on again and then booting directly to Linux.

### System fails to correctly resume from suspen-to-RAM

This is still being investigated.
