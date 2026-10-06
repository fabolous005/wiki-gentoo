<!-- source: https://wiki.gentoo.org/wiki/Dell_XPS_13_9350 | group: Gentoo Wiki (Main) | wiki-title: Dell XPS 13 9350 -->
---
title: Dell XPS 13 9350
url: https://wiki.gentoo.org/wiki/Dell_XPS_13_9350
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-29"
fingerprint: "3f488d06f9a23169"
license: CC BY-SA 4.0
---

# Dell XPS 13 9350

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is currently a WIP. Please add to it.

As Dell makes several differently configured laptops under the same model name, this article is based upon the P54G version. (you can look up yours under the hatch, after "Reg Model".

## Hardware

| Class | Product | 
|---|---|
| Processor | Intel® Core I5-6200U CPU | 
| Wi-Fi | Intel Wifi 8260 | 
| Bluetooth | Intel Bluetooth | 
| Audio | Intel HD Audio | 
| Graphics | Intel HD Graphics 520 | 

`root #``lspci -nn`
00:00.0 Host bridge \[0600\]: Intel Corporation Sky Lake Host Bridge/DRAM Registers \[8086:1904\] (rev 08)
00:02.0 VGA compatible controller \[0300\]: Intel Corporation Sky Lake Integrated Graphics \[8086:1916\] (rev 07)
00:04.0 Signal processing controller \[1180\]: Intel Corporation Device \[8086:1903\] (rev 08)
00:14.0 USB controller \[0c03\]: Intel Corporation Device \[8086:9d2f\] (rev 21)
00:14.2 Signal processing controller \[1180\]: Intel Corporation Device \[8086:9d31\] (rev 21)
00:15.0 Signal processing controller \[1180\]: Intel Corporation Device \[8086:9d60\] (rev 21)
00:15.1 Signal processing controller \[1180\]: Intel Corporation Device \[8086:9d61\] (rev 21)
00:16.0 Communication controller \[0780\]: Intel Corporation Device \[8086:9d3a\] (rev 21)
00:17.0 SATA controller \[0106\]: Intel Corporation Device \[8086:9d03\] (rev 21)
00:1c.0 PCI bridge \[0604\]: Intel Corporation Device \[8086:9d10\] (rev f1)
00:1c.4 PCI bridge \[0604\]: Intel Corporation Device \[8086:9d14\] (rev f1)
00:1c.5 PCI bridge \[0604\]: Intel Corporation Device \[8086:9d15\] (rev f1)
00:1f.0 ISA bridge \[0601\]: Intel Corporation Device \[8086:9d48\] (rev 21)
00:1f.2 Memory controller \[0580\]: Intel Corporation Device \[8086:9d21\] (rev 21)
00:1f.3 Audio device \[0403\]: Intel Corporation Device \[8086:9d70\] (rev 21)
00:1f.4 SMBus \[0c05\]: Intel Corporation Device \[8086:9d23\] (rev 21)
3a:00.0 Network controller \[0280\]: Intel Corporation Wireless 8260 \[8086:24f3\] (rev 3a)
3b:00.0 Unassigned class \[ff00\]: Realtek Semiconductor Co., Ltd. Device \[10ec:525a\] (rev 01)

`root #``lsusb`
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 001 Device 002: ID 8087:0a2b Intel Corp. 
Bus 001 Device 003: ID 0c45:670c Microdia 
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

# Wi-Fi

## Kernel Options

We're trying to make [iwlwifi](https://wiki.gentoo.org/wiki/Iwlwifi) work, for more information go to the specific [iwlwifi](https://wiki.gentoo.org/wiki/Iwlwifi) article.
After enabling the basic 802.11 support kernel options, let's enable the driver !

```
        Device Drivers  --->
        [*] Network device support  --->
        --- Network device support
        [*]   Wireless LAN  --->
            --- Wireless LAN
            [*]   Intel devices
            < >     Intel PRO/Wireless 2100 Network Connection
            < >     Intel PRO/Wireless 2200BG and 2915ABG Network Connection
            < >     Intel Wireless WiFi 4965AGN (iwl4965)
            < >     Intel PRO/Wireless 3945ABG/BG Network Connection (iwl3945)
            <M>     Intel Wireless WiFi Next Gen AGN - Wireless-N/Advanced-N/Ultimate-N (iwlwifi)
            <M>       Intel Wireless WiFi DVM Firmware support
            <M>       Intel Wireless WiFi MVM Firmware support
```
{Important|In case the driver is built into the kernel (`<*>`) instead as a module (`<M>`), also the firmware needs to be built [into the kernel](https://wiki.gentoo.org/wiki/Kernel_Modules#Compile-in-kernel_modules_vs_Loadable_kernel_modules_.28LKMs.29). See the section [When using built-in configuration](https://wiki.gentoo.org#When_using_built-in_configuration).}

## Firmware

Firmware for the 8260 chip is needed. It is available in [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware):

`root #``emerge --ask sys-kernel/linux-firmware`
### When using built-in configuration

In case the driver is built into the kernel (`<*>`) instead as a module (`<M>`), also the firmware needs to be built [into the kernel](https://wiki.gentoo.org/wiki/Kernel_Modules#Compile-in-kernel_modules_vs_Loadable_kernel_modules_.28LKMs.29).

```
        Device Drivers  --->
            Generic Driver Options  --->
            -*- Userspace firmware loading support
            [ ]   Include in-kernel firmware blobs in kernel binary
            (iwlwifi-8000C-XX.ucode) External firmware blobs to build into the kernel binary
            (/lib/firmware) Firmware blobs root directory
            [ ] Fallback user-helper invocation for firmware loading
```
(Replace the XX with the latest version)

# Kernel Modules & Drivers

## SMBus

`root #``lspci -nn -k`
00:1f.4 SMBus \[0c05\]: Intel Corporation Sunrise Point-LP SMBus \[8086:9d23\] (rev 21)
	Subsystem: Dell Sunrise Point-LP SMBus \[1028:0704\]
	Kernel driver in use: i801\_smbus

## PCI-E Card Reader

`root #``lspci -nn -k`
3b:00.0 Unassigned class \[ff00\]: Realtek Semiconductor Co., Ltd. RTS525A PCI Express Card Reader \[10ec:525a\] (rev 01)
	Subsystem: Dell RTS525A PCI Express Card Reader \[1028:0704\]
	Kernel driver in use: rtsx\_pci
	Kernel modules: rtsx\_pci

## Memory Controller

`root #``lspci -nn -k`
00:1f.2 Memory controller \[0580\]: Intel Corporation Sunrise Point-LP PMC \[8086:9d21\] (rev 21)
	Subsystem: Dell Sunrise Point-LP PMC \[1028:0704\]
	Kernel driver in use: intel\_pmc\_core

## CPU

`root #``lspci -nn -k`
00:04.0 Signal processing controller \[1180\]: Intel Corporation Xeon E3-1200 v5/E3-1500 v5/6th Gen Core Processor Thermal Subsystem \[8086:1903\] (rev 08)
	Subsystem: Dell Xeon E3-1200 v5/E3-1500 v5/6th Gen Core Processor Thermal Subsystem \[1028:0704\]
	Kernel driver in use: proc\_thermal
	Kernel modules: processor\_thermal\_device
00:14.2 Signal processing controller \[1180\]: Intel Corporation Sunrise Point-LP Thermal subsystem \[8086:9d31\] (rev 21)
	Subsystem: Dell Sunrise Point-LP Thermal subsystem \[1028:0704\]
	Kernel driver in use: intel\_pch\_thermal
	Kernel modules: intel\_pch\_thermal
00:16.0 Communication controller \[0780\]: Intel Corporation Sunrise Point-LP CSME HECI #1 \[8086:9d3a\] (rev 21)
	Subsystem: Dell Sunrise Point-LP CSME HECI \[1028:0704\]
	Kernel driver in use: mei\_me

## Touchscreen

`root #``lspci -nn -k`
00:15.0 Signal processing controller \[1180\]: Intel Corporation Sunrise Point-LP Serial IO I2C Controller #0 \[8086:9d60\] (rev 21)
	Subsystem: Dell Sunrise Point-LP Serial IO I2C Controller \[1028:0704\]
	Kernel driver in use: intel-lpss
	Kernel modules: intel\_lpss\_pci
00:15.1 Signal processing controller \[1180\]: Intel Corporation Sunrise Point-LP Serial IO I2C Controller #1 \[8086:9d61\] (rev 21)
	Subsystem: Dell Sunrise Point-LP Serial IO I2C Controller \[1028:0704\]
	Kernel driver in use: intel-lpss
	Kernel modules: intel\_lpss\_pci

## NVMe

`root #``lspci -nn -k`
3c:00.0 Non-Volatile memory controller \[0108\]: Toshiba America Info Systems Device \[1179:0115\] (rev 01)
	Subsystem: Toshiba America Info Systems Device \[1179:0001\]
	Kernel driver in use: nvme

## Sata Controller

`root #``lspci -nn -k`
00:17.0 SATA controller \[0106\]: Intel Corporation Sunrise Point-LP SATA Controller \[AHCI mode\] \[8086:9d03\] (rev 21)
	Subsystem: Dell Sunrise Point-LP SATA Controller \[AHCI mode\] \[1028:0704\]
	Kernel driver in use: ahci

## Webcam

`root #``lsusb -t````
/:  Bus 02.Port 1: Dev 1, Class=root_hub, Driver=xhci_hcd/6p, 5000M
/:  Bus 01.Port 1: Dev 1, Class=root_hub, Driver=xhci_hcd/12p, 480M
    |__ Port 5: Dev 4, If 0, Class=Video, Driver=uvcvideo, 480M
    |__ Port 5: Dev 4, If 1, Class=Video, Driver=uvcvideo, 480M
```
```
.config - Linux/x86 4.14.65-gentoo Kernel Configuration
 → Device Drivers → Multimedia support → 
[*]   Cameras/video grabbers support
[*]   Media USB Adapters  --->
       <M>   USB Video Class (UVC) 
       [*]     UVC input events device support
```
## Bluetooth

`root #``lsusb -t````
/:  Bus 02.Port 1: Dev 1, Class=root_hub, Driver=xhci_hcd/6p, 5000M
/:  Bus 01.Port 1: Dev 1, Class=root_hub, Driver=xhci_hcd/12p, 480M
    |__ Port 3: Dev 2, If 0, Class=Wireless, Driver=btusb, 12M
    |__ Port 3: Dev 2, If 1, Class=Wireless, Driver=btusb, 12M
```
```
.config - Linux/x86 4.9.95-gentoo Kernel Configuration
 → Search (CONFIG_BT_HCIBTUSB) 
 Symbol: BT_HCIBTUSB [=m]
 Type  : tristate
 Prompt: HCI USB driver
 Location:
  │     -> Networking support (NET [=y])  
  │       -> Bluetooth subsystem support (BT [=m])│  
  │ (1)     -> Bluetooth device drivers│  
  │   Defined at drivers/bluetooth/Kconfig:21│  
  │   Depends on: NET [=y] && BT [=m] && USB [=y]│  
  │   Selects: BT_INTEL [=m]
```
## Sound

`root #``lspci -nn -k`
00:1f.3 Audio device \[0403\]: Intel Corporation Sunrise Point-LP HD Audio \[8086:9d70\] (rev 21)
	Subsystem: Dell Sunrise Point-LP HD Audio \[1028:0704\]
	Kernel driver in use: snd\_hda\_intel

# Graphics

Use intel HD graphics: [intel](https://wiki.gentoo.org/wiki/Intel).

## Kernel

```
Processor type and features  --->
    [*] MTRR (Memory Type Range Register) support
        Device Drivers  --->
            Graphics support  --->
                <*> /dev/agpgart (AGP Support)  --->
                    --- /dev/agpgart (AGP Support)
                    < >   AMD Opteron/Athlon64 on-CPU GART support
                    -*-   Intel 440LX/BX/GX, I8xx and E7x05 chipset support
                    < >   SiS chipset support
                    < >   VIA chipset support
                [ ] VGA Arbitration
                [ ] Laptop Hybrid Graphics - GPU switching support
                <*> Direct Rendering Manager (XFree86 4.1.0 and higher DRI support)  --->
                    --- Direct Rendering Manager (XFree86 4.1.0 and higher DRI support)
                    [*]   Enable legacy fbdev support for your modesetting driver
                [ ] Allow to specify an EDID data set instead of probing for it
                    I2C encoder or helper chips  --->
                < > 3dfx Banshee/Voodoo3+
                < > ATI Rage 128
                < > ATI Radeon
                < > AMD GPU
                < > Nouveau (NVIDIA) cards
                < > Intel I810
                <*> Intel 8xx/9xx/G3x/G4x/HD Graphics
                [ ]   Enable preliminary support for prerelease Intel hardware by default
                < > Matrox g200/g400
                < > SiS video cards
                < > Via unichrome video cards
                < > Savage video cards
                < > Virtual GEM provider
                < > DRM driver for VMware Virtual GPU
                < > Intel GMA5/600 KMS Framebuffer
                < > DisplayLink
                < > AST server chips
                < > Kernel modesetting driver for MGA G200 server engines
                < > Cirrus driver for QEMU emulated device
                < > QXL virtual GPU
                < > DRM Support for bochs dispi vga interface (qemu stdvga)
                    Display Panels  ----                 
                    Display Interface Bridges  ----
                    Frame buffer Devices  --->
                -*- Backlight & LCD device support  --->
                    Console display driver support  --->
                [*] Bootup logo  --->
```
## Firmware

We need firmware for this Skylake chip, for runtime power management, if it wasn't installed before, install [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware):

`root #``emerge --ask sys-kernel/linux-firmware`
This time we'll include the firmware in the kernel.

```
Device Drivers  --->
    Generic Driver Options  --->
        -*- Userspace firmware loading support
        [*] Include in-kernel firmware blobs in kernel binary
            (i915/skl_dmc_ver1_27.bin)
            (/lib/firmware) Firmware blobs root directory
```
If you receive the following error

`root #``dmesg`
i915 0000:00:02.0: Direct firmware load for i915/skl\_dmc\_ver1\_26.bin failed with error -2

We need to patch the firmware to load the 1.27 firmware with this file [Firmware Patch](https://patchwork.freedesktop.org/patch/187559/)

I used this cmd to implement the patch

`root #``patch /usr/src/linux/drivers/gpu/drm/i915/intel_csr.c drm-i915-skl-DMC-firmware-for-skylake-v1.27.patch`
# Microcode

If you are seeing this message during boot time

`root #``dmesg`
\[    0.000000\] Using ACPI (MADT) for SMP configuration information
\[    0.000000\] ACPI: HPET id: 0x8086a701 base: 0xfed00000
\[    0.000000\] \[Firmware Bug\]: TSC\_DEADLINE disabled due to Errata; please update microcode to version: 0xb2 (or later)
\[    0.000000\] smpboot: Allowing 4 CPUs, 0 hotplug CPUs
\[    0.000000\] PM: Registered nosave memory: \[mem 0x00000000-0x00000fff\]
\[    0.000000\] PM: Registered nosave memory: \[mem 0x00058000-0x00058fff\]

Installing the Intel Microcode may help.

## Installing with UEFI System

Add initramfs to your use flag to allow emerge to copy the microcode file to your /boot partition.

**`/etc/portage/make.conf`**

```
USE="initramfs"
```
`root #``emerge --ask emerge --ask sys-firmware/intel-microcode`
Now grub should be able to find the /boot/intel\_uc.img file

`root #``grub-mkconfig -o /boot/grub/grub.cfg`
# Driver Summary

`root #``lsmod`
Module                  Size  Used by
rtsx\_pci\_sdmmc         20480  0
mmc\_core              126976  1 rtsx\_pci\_sdmmc
wmi\_bmof               16384  0
dell\_wmi               16384  0
dell\_laptop            24576  0
dell\_smbios            16384  2 dell\_wmi,dell\_laptop
dcdbas                 16384  1 dell\_smbios
iwlmvm                311296  0
x86\_pkg\_temp\_thermal    16384  0
btusb                  49152  0
iwlwifi               270336  1 iwlmvm
btrtl                  16384  1 btusb
btbcm                  16384  1 btusb
rtsx\_pci               45056  1 rtsx\_pci\_sdmmc
btintel                16384  1 btusb
uvcvideo               90112  0
videobuf2\_vmalloc      16384  1 uvcvideo
hid\_multitouch         24576  0
videobuf2\_memops       16384  1 videobuf2\_vmalloc
bluetooth             393216  5 btrtl,btintel,btbcm,btusb
videobuf2\_v4l2         24576  1 uvcvideo
processor\_thermal\_device    16384  0
videobuf2\_core         36864  2 videobuf2\_v4l2,uvcvideo
ecdh\_generic           24576  1 bluetooth
intel\_soc\_dts\_iosf     16384  1 processor\_thermal\_device
intel\_lpss\_pci         20480  0
intel\_pch\_thermal      16384  0
intel\_lpss             16384  1 intel\_lpss\_pci
mfd\_core               16384  2 rtsx\_pci,intel\_lpss
wmi                    24576  2 dell\_wmi,wmi\_bmof
int3403\_thermal        16384  0
int3400\_thermal        16384  0
intel\_hid              16384  0
acpi\_thermal\_rel       16384  1 int3400\_thermal
int340x\_thermal\_zone    16384  2 int3403\_thermal,processor\_thermal\_device
efivarfs               16384  1
ecb                    16384  0
ixgbe                 270336  0
mdio                   16384  1 ixgbe
xfs                  1134592  0
xhci\_plat\_hcd          16384  0

`root #``lsusb -t````
/:  Bus 02.Port 1: Dev 1, Class=root_hub, Driver=xhci_hcd/6p, 5000M
/:  Bus 01.Port 1: Dev 1, Class=root_hub, Driver=xhci_hcd/12p, 480M
    |__ Port 3: Dev 2, If 0, Class=Wireless, Driver=btusb, 12M
    |__ Port 3: Dev 2, If 1, Class=Wireless, Driver=btusb, 12M
    |__ Port 4: Dev 3, If 0, Class=Human Interface Device, Driver=usbhid, 12M
    |__ Port 5: Dev 4, If 0, Class=Video, Driver=uvcvideo, 480M
    |__ Port 5: Dev 4, If 1, Class=Video, Driver=uvcvideo, 480M
```
`root #``lspci -nn -k`
00:00.0 Host bridge \[0600\]: Intel Corporation Xeon E3-1200 v5/E3-1500 v5/6th Gen Core Processor Host Bridge/DRAM Registers \[8086:1904\] (rev 08)
	Subsystem: Dell Xeon E3-1200 v5/E3-1500 v5/6th Gen Core Processor Host Bridge/DRAM Registers \[1028:0704\]
	Kernel driver in use: skl\_uncore
00:02.0 VGA compatible controller \[0300\]: Intel Corporation HD Graphics 520 \[8086:1916\] (rev 07)
	Subsystem: Dell HD Graphics 520 \[1028:0704\]
	Kernel driver in use: i915
00:04.0 Signal processing controller \[1180\]: Intel Corporation Xeon E3-1200 v5/E3-1500 v5/6th Gen Core Processor Thermal Subsystem \[8086:1903\] (rev 08)
	Subsystem: Dell Xeon E3-1200 v5/E3-1500 v5/6th Gen Core Processor Thermal Subsystem \[1028:0704\]
	Kernel driver in use: proc\_thermal
	Kernel modules: processor\_thermal\_device
00:14.0 USB controller \[0c03\]: Intel Corporation Sunrise Point-LP USB 3.0 xHCI Controller \[8086:9d2f\] (rev 21)
	Subsystem: Dell Sunrise Point-LP USB 3.0 xHCI Controller \[1028:0704\]
	Kernel driver in use: xhci\_hcd
00:14.2 Signal processing controller \[1180\]: Intel Corporation Sunrise Point-LP Thermal subsystem \[8086:9d31\] (rev 21)
	Subsystem: Dell Sunrise Point-LP Thermal subsystem \[1028:0704\]
	Kernel driver in use: intel\_pch\_thermal
	Kernel modules: intel\_pch\_thermal
00:15.0 Signal processing controller \[1180\]: Intel Corporation Sunrise Point-LP Serial IO I2C Controller #0 \[8086:9d60\] (rev 21)
	Subsystem: Dell Sunrise Point-LP Serial IO I2C Controller \[1028:0704\]
	Kernel driver in use: intel-lpss
	Kernel modules: intel\_lpss\_pci
00:15.1 Signal processing controller \[1180\]: Intel Corporation Sunrise Point-LP Serial IO I2C Controller #1 \[8086:9d61\] (rev 21)
	Subsystem: Dell Sunrise Point-LP Serial IO I2C Controller \[1028:0704\]
	Kernel driver in use: intel-lpss
	Kernel modules: intel\_lpss\_pci
00:16.0 Communication controller \[0780\]: Intel Corporation Sunrise Point-LP CSME HECI #1 \[8086:9d3a\] (rev 21)
	Subsystem: Dell Sunrise Point-LP CSME HECI \[1028:0704\]
	Kernel driver in use: mei\_me
00:16.3 Serial controller \[0700\]: Intel Corporation Device \[8086:9d3d\] (rev 21)
	Subsystem: Dell Device \[1028:0704\]
	Kernel driver in use: serial
00:17.0 SATA controller \[0106\]: Intel Corporation Sunrise Point-LP SATA Controller \[AHCI mode\] \[8086:9d03\] (rev 21)
	Subsystem: Dell Sunrise Point-LP SATA Controller \[AHCI mode\] \[1028:0704\]
	Kernel driver in use: ahci
00:1c.0 PCI bridge \[0604\]: Intel Corporation Sunrise Point-LP PCI Express Root Port #1 \[8086:9d10\] (rev f1)
	Kernel driver in use: pcieport
00:1c.4 PCI bridge \[0604\]: Intel Corporation Sunrise Point-LP PCI Express Root Port #5 \[8086:9d14\] (rev f1)
	Kernel driver in use: pcieport
00:1c.5 PCI bridge \[0604\]: Intel Corporation Sunrise Point-LP PCI Express Root Port #6 \[8086:9d15\] (rev f1)
	Kernel driver in use: pcieport
00:1d.0 PCI bridge \[0604\]: Intel Corporation Sunrise Point-LP PCI Express Root Port #9 \[8086:9d18\] (rev f1)
	Kernel driver in use: pcieport
00:1f.0 ISA bridge \[0601\]: Intel Corporation Sunrise Point-LP LPC Controller \[8086:9d48\] (rev 21)
	Subsystem: Dell Sunrise Point-LP LPC Controller \[1028:0704\]
00:1f.2 Memory controller \[0580\]: Intel Corporation Sunrise Point-LP PMC \[8086:9d21\] (rev 21)
	Subsystem: Dell Sunrise Point-LP PMC \[1028:0704\]
	Kernel driver in use: intel\_pmc\_core
00:1f.3 Audio device \[0403\]: Intel Corporation Sunrise Point-LP HD Audio \[8086:9d70\] (rev 21)
	Subsystem: Dell Sunrise Point-LP HD Audio \[1028:0704\]
	Kernel driver in use: snd\_hda\_intel
00:1f.4 SMBus \[0c05\]: Intel Corporation Sunrise Point-LP SMBus \[8086:9d23\] (rev 21)
	Subsystem: Dell Sunrise Point-LP SMBus \[1028:0704\]
	Kernel driver in use: i801\_smbus
3a:00.0 Network controller \[0280\]: Intel Corporation Wireless 8260 \[8086:24f3\] (rev 3a)
	Subsystem: Intel Corporation Wireless 8260 \[8086:0050\]
	Kernel driver in use: iwlwifi
	Kernel modules: iwlwifi
3b:00.0 Unassigned class \[ff00\]: Realtek Semiconductor Co., Ltd. RTS525A PCI Express Card Reader \[10ec:525a\] (rev 01)
	Subsystem: Dell RTS525A PCI Express Card Reader \[1028:0704\]
	Kernel driver in use: rtsx\_pci
	Kernel modules: rtsx\_pci
3c:00.0 Non-Volatile memory controller \[0108\]: Toshiba America Info Systems Device \[1179:0115\] (rev 01)
	Subsystem: Toshiba America Info Systems Device \[1179:0001\]
	Kernel driver in use: nvme

# Remaining boot issues

Here are the remaining issues with my current build: Profile: Gentoo systemd 17.0 boot: uefi disk: LVM 9350 ver: P54G BIOS: 1.7.0

`root #``dmesg`
(NULL device \*): hwmon\_device\_register() is deprecated. Please convert the driver to use hwmon\_device\_register\_with\_info().
usb: port power management may be unreliable
...
i8042: Warning: Keylock active
...
NOHZ: local\_softirq\_pending 282 \<---Lots of these with different numbers
...
snd\_hda\_codec\_realtek hdaudioC0D0: Failed to find dell wmi symbol dell\_micmute\_led\_set
...
systemd\[1\]: File /lib/systemd/system/systemd-journald.service:35 configures an IP firewall (IPAddressDeny=any), but the local system does not support BPF/cgroup based firewalling.
systemd\[1\]: Proceeding WITHOUT firewalling in effect! (This warning is only shown for the first loaded unit using IP firewalling.)
...
ACPI Warning: \\\_SB.IETM.\_ART: Return Package type mismatch at index 0 - found Integer, expected Reference (20170728/nspredef-297)
ACPI: Invalid package element \[0\]: got number, expecting \[R\]
\_ART package 0 is invalid, ignored
...
wmi\_bus wmi\_bus-PNP0C14:01: WQBC data block query control method not found
...
thermal thermal\_zone11: failed to read out thermal zone (-61)
