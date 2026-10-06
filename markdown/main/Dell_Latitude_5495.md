<!-- source: https://wiki.gentoo.org/wiki/Dell_Latitude_5495 | group: Gentoo Wiki (Main) | wiki-title: Dell Latitude 5495 -->
---
title: Dell Latitude 5495
url: https://wiki.gentoo.org/wiki/Dell_Latitude_5495
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-20"
fingerprint: "1e46acc5a73aa56c"
license: CC BY-SA 4.0
---

# Dell Latitude 5495

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The Dell Latitude 5495 is a 14" laptop with Ryzen Pro 2500U or 2700U processor.

## Hardware

### Components

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | AMD Ryzen PRO 2500U / 2700U | Works | N/A | N/A | 4.19.1 |  | 
| GPIO | AMD GPIO Pin Controller | Works |  | pinctrl\_amd | 4.19.2 |  | 
| I2C/SMBus Controller | AMD MP2 I2C Controller | Works | PCI 1022:15e6 | i2c\_amd\_mp2 |  | Driver is currently not mainlined. Get it from [\[1\]](https://lore.kernel.org/patchwork/project/lkml/list/?submitter=24972) or [\[2\]](https://github.com/Syniurge/i2c-amd-mp2/) | 
| I2C/SMBus Controller | AMD FCH SMBus Controller | Works | PCI 1022:790b | piix4\_smbus | 4.19.1 |  | 
| TPM | TPM 2.0 | Works |  | tcg\_crb, tpm\_tis | 4.19.7 |  | 
| SATA | AMD FCH SATA Controller | Works | PCI 1022:7901 | ahci | 4.19.1 |  | 
| Video | AMD Radeon Vega graphics | Works | PCI 1002:15dd | [amdgpu](https://wiki.gentoo.org/wiki/Amdgpu) | 4.19.1 |  | 
| Audio | AMD HD Audio | Works | PCI 1002:15de | snd\_hda\_intel, snd\_hda\_codec\_realtek | 4.19.1 |  | 
| Ethernet | Broadcom NetXtreme BCM5762 Gigabit Ethernet | Works | PCI 14e4:1687 | tg3 | 4.19.1 |  | 
| Wireless LAN | Qualcomm Atheros QCA6174 802.11ac | Works | [PCI 168c:003e](https://wiki.gentoo.org/wiki/Qualcomm_Atheros_QCA6174) | ath10k\_pci | 4.19.1 |  | 
| Wireless WAN | Sierra Wireless EM7455 | Not tested | USB 413c:81b6 | cdc\_mbim, qcserial |  |  | 
| Touchpad | Alps U1 Dual Button | Works | I2C 044e:120a | i2c\_hid | 4.19.1 | Touchpad gestures require i2c\_amd\_mp2 to work. See [#Touchpad](https://wiki.gentoo.org#Touchpad) | 
| SD Card reader | Realtek RTS525A PCI Express Card Reader | Works | PCI 10ec:525a | rtsx\_pci, mmc\_realtek\_pci | 4.19.1 |  | 
| Bluetooth | Qualcomm Atheros Bluetooth controller | Works | USB 0cf3:e010 | btusb | 4.19.1 |  | 
| Webcam | Sunplus Innovation Technology Webcam HD | Works | USB 1bcf:2b96 | uvcvideo | 4.19.1 |  | 
| Smartcard Reader | Broadcom 5880 | Not tested | USB 0a5c:5833 | N/A |  |  | 
| Fingerprint Reader | Broadcom 5880 | Unsupported | USB 0a5c:5833 | N/A |  | [sys-auth/libfprint](https://packages.gentoo.org/packages/sys-auth/libfprint) does not support this, see [\[3\]](https://gitlab.freedesktop.org/libfprint/libfprint/issues/38) [\[4\]](https://gitlab.freedesktop.org/libfprint/libfprint/issues/71) [\[5\]](https://gitlab.freedesktop.org/libfprint/libfprint/issues/88) and others | 
| Hardware Monitoring |  | Works |  | amdgpu, ath10k\_pci, k10temp | 4.19.1 |  | 
| Hotkeys |  | Works |  | dell\_wmi, wmi\_bmof, dell\_smbios, dell\_smo8800, dell\_rbtn | 4.19.1 |  | 

### ACPI / Power management

| Function | Status | Kernel driver(s) | Kernel version | BIOS version | Notes | 
|---|---|---|---|---|---|
| CPU frequency scaling | Yes | intel\_pstate | 4.19.1 | 1.2.3 |  | 
| GPU Powersaving (PowerPlay) | Yes | amdgpu | 4.19.1 | 1.2.3 |  | 
| SATA Power Management (ALPM) | Yes |  | 4.19.2 | 1.2.3 |  | 
| PCIe Power Management (ASPM) | Partial |  | 4.19.1 | 1.2.3 | PCIe errors may spam the kernel log unless you disable ASPM through kernel config or with pcie\_aspm=off | 
| USB Type C Power Delivery | Not tested |  |  |  |  | 
| Suspend to RAM | Yes |  | 4.19.7 | 1.2.3 | See [#Suspend](https://wiki.gentoo.org#Suspend) | 
| Suspend to disk (hibernate) | Yes |  | 4.19.1 | 1.2.3 | See [#Suspend](https://wiki.gentoo.org#Suspend) | 
| Display backlight control | Yes | acpi\_video | 4.19.1 | 1.2.3 |  | 
| Keyboard backlight control | Not tested |  |  |  |  | 

### Extra hardware information

`root #``lspci -nnk````
00:00.0 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Device [1022:15d0]
        Subsystem: Dell Device [1028:0814]
00:00.2 IOMMU [0806]: Advanced Micro Devices, Inc. [AMD] Device [1022:15d1]
        Subsystem: Dell Device [1028:0814]
00:01.0 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Family 17h (Models 00h-0fh) PCIe Dummy Host Bridge [1022:1452]
00:01.2 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Device [1022:15d3]
        Kernel driver in use: pcieport
00:01.3 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Device [1022:15d3]
        Kernel driver in use: pcieport
00:01.4 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Device [1022:15d3]
        Kernel driver in use: pcieport
00:08.0 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Family 17h (Models 00h-0fh) PCIe Dummy Host Bridge [1022:1452]
00:08.1 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Device [1022:15db]
        Kernel driver in use: pcieport
00:08.2 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Device [1022:15dc]
        Kernel driver in use: pcieport
00:14.0 SMBus [0c05]: Advanced Micro Devices, Inc. [AMD] FCH SMBus Controller [1022:790b] (rev 61)
        Subsystem: Dell FCH SMBus Controller [1028:0814]
        Kernel driver in use: piix4_smbus
        Kernel modules: i2c_piix4
00:14.3 ISA bridge [0601]: Advanced Micro Devices, Inc. [AMD] FCH LPC Bridge [1022:790e] (rev 51)
        Subsystem: Dell FCH LPC Bridge [1028:0814]
00:18.0 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Device [1022:15e8]
00:18.1 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Device [1022:15e9]
00:18.2 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Device [1022:15ea]
00:18.3 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Device [1022:15eb]
        Kernel driver in use: k10temp
        Kernel modules: k10temp
00:18.4 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Device [1022:15ec]
00:18.5 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Device [1022:15ed]
00:18.6 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Device [1022:15ee]
00:18.7 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Device [1022:15ef]
01:00.0 Network controller [0280]: Qualcomm Atheros QCA6174 802.11ac Wireless Network Adapter [168c:003e] (rev 32)
        Subsystem: Dell QCA6174 802.11ac Wireless Network Adapter [1028:364a]
        Kernel driver in use: ath10k_pci
        Kernel modules: ath10k_pci
02:00.0 Ethernet controller [0200]: Broadcom Limited NetXtreme BCM5762 Gigabit Ethernet PCIe [14e4:1687] (rev 10)
        Subsystem: Dell NetXtreme BCM5762 Gigabit Ethernet PCIe [1028:0814]
        Kernel driver in use: tg3
03:00.0 Unassigned class [ff00]: Realtek Semiconductor Co., Ltd. RTS525A PCI Express Card Reader [10ec:525a] (rev 01)
        Subsystem: Realtek Semiconductor Co., Ltd. RTS525A PCI Express Card Reader [10ec:525a]
        Kernel driver in use: rtsx_pci
        Kernel modules: rtsx_pci
04:00.0 VGA compatible controller [0300]: Advanced Micro Devices, Inc. [AMD/ATI] Device [1002:15dd] (rev d0)
        Subsystem: Dell Device [1028:0814]
        Kernel driver in use: amdgpu
        Kernel modules: amdgpu
04:00.1 Audio device [0403]: Advanced Micro Devices, Inc. [AMD/ATI] Device [1002:15de]
        Subsystem: Dell Device [1028:0814]
        Kernel driver in use: snd_hda_intel
        Kernel modules: snd_hda_intel
04:00.2 Encryption controller [1080]: Advanced Micro Devices, Inc. [AMD] Device [1022:15df]
        Subsystem: Dell Device [1028:0814]
04:00.3 USB controller [0c03]: Advanced Micro Devices, Inc. [AMD] Device [1022:15e0]
        Subsystem: Dell Device [1028:0814]
        Kernel driver in use: xhci_hcd
04:00.4 USB controller [0c03]: Advanced Micro Devices, Inc. [AMD] Device [1022:15e1]
        Subsystem: Dell Device [1028:0814]
        Kernel driver in use: xhci_hcd
04:00.6 Audio device [0403]: Advanced Micro Devices, Inc. [AMD] Device [1022:15e3]
        Subsystem: Dell Device [1028:0814]
        Kernel driver in use: snd_hda_intel
        Kernel modules: snd_hda_intel
04:00.7 Non-VGA unclassified device [0000]: Advanced Micro Devices, Inc. [AMD] Device [1022:15e6]
        Subsystem: Advanced Micro Devices, Inc. [AMD] Device [1022:15e4]
05:00.0 SATA controller [0106]: Advanced Micro Devices, Inc. [AMD] FCH SATA Controller [AHCI mode] [1022:7901] (rev 61)
        Subsystem: Dell FCH SATA Controller [AHCI mode] [1028:0814]
        Kernel driver in use: ahci
```
## Installation

[EFI stub](https://wiki.gentoo.org/wiki/EFI_stub) will boot fine, so you do not need any separate boot loader like [GRUB](https://wiki.gentoo.org/wiki/GRUB) or [Syslinux](https://wiki.gentoo.org/wiki/Syslinux).

After making hardware changes to the internal storage, such as replacing a SATA with M.2 [NVMe](https://wiki.gentoo.org/wiki/NVMe) drive or vice versa, the BIOS/UEFI will get confused on the first boot and mess up the boot order. Subsequent boots will be stable.

This is a common problem with Dell Laptops, cf. [\[6\]](https://www.instructables.com/id/Add-a-Second-SSD-to-a-Dell-Latitude-E5470-Laptop/), step 11.

## Configuration details

## Compiler flags

See [Safe CFLAGS](https://wiki.gentoo.org/wiki/Safe_CFLAGS)

**`/etc/portage/make.conf`**

```
COMMON_FLAGS="-O2 -pipe -march=znver1"
CFLAGS="${COMMON_FLAGS}"
```
**`/etc/portage/package.use/00cpu-flags`**

```
*/* CPU_FLAGS_X86: aes avx avx2 f16c fma3 mmx mmxext pclmul popcnt sse sse2 sse3 sse4_1 sse4_2 sse4a ssse3
```
## Required packages

- [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) for the Wi-fi and graphics firmware
- [sys-apps/pcsc-tools](https://packages.gentoo.org/packages/sys-apps/pcsc-tools), [app-crypt/ccid](https://packages.gentoo.org/packages/app-crypt/ccid) for the smartcard reader

## Troubleshooting

### Suspend

- After suspend to disk (hibernate), the computer will reboot instead of powering off. (Tested with BIOS version 1.2.3)

Solution: Set hibernation mode to "shutdown" instead of "platform".

`root #``echo shutdown > /sys/power/disk`
For automating this during boot, see [/etc/local.d](https://wiki.gentoo.org/wiki//etc/local.d).

- After suspend to RAM, computer will hang on resume. Next boot, the BIOS will complain about TPM not responding.

Solution: Enable tcg\_tis driver, or disable TPM function in the BIOS.

### Touchpad

The Alps touchpad is recognized as PS/2 mouse and will provide only some functions. This is because the AMD MP2 I2C controller which it is connected to has no driver in the mainline kernel as of 4.19.

For installing the i2c\_amd\_mp2 out-of-tree driver, see the link in the [table of hardware](https://wiki.gentoo.org#Components).

Otherwise, as a workaround, you can use button scrolling, pressing both buttons on the touchpad will cause finger movement to scroll:

**`/etc/X11/xorg.conf.d/40-libinput.conf`**

```
# Match PS/2 Generic Mouse which is actually the touchpad
Section "InputClass"
        Identifier "PS/2 touchpad"
        MatchIsPointer "on"
        MatchProduct "PS/2 Generic Mouse"
        Driver "libinput"
        Option "MiddleEmulation" "true"
        Option "ScrollMethod" "button"
EndSection
```
