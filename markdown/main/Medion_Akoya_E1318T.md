<!-- source: https://wiki.gentoo.org/wiki/Medion_Akoya_E1318T | group: Gentoo Wiki (Main) | wiki-title: Medion Akoya E1318T -->
---
title: Medion Akoya E1318T
url: https://wiki.gentoo.org/wiki/Medion_Akoya_E1318T
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: f4cacd5e332956d
license: CC BY-SA 4.0
---

# Medion Akoya E1318T

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


The Medion Akoya E1318T is a touchscreen netbook based on the AMD Kabini APU. It is very similar to the E1317T, so most information here should apply equally to both models.

## Hardware Status

This status list is for kernel 3.15, older kernels do not support the Wi-Fi chip or certain functions of the GPU.

### Components

| Device | Works | Notes | 
|---|---|---|
| CPU: AMD A4-1200 |  |  | 
| SATA: AMD FCH SATA Controller |  |  | 
| Video: AMD Kabini Radeon HD 8180 |  |  | 
| Audio: AMD FCH Azalia HD Audio |  |  | 
| Ethernet: Realtek RTL8101E/RTL8102E PCI Express Fast Ethernet |  |  | 
| Wireless LAN: Realtek RTL8723BE PCIe Wireless Network Adapter |  | Driven by rtl8723be | 
| Touchscreen: Shenzhen Huiding Technology Goodix GT91x USB |  | Driven by hid-multitouch | 
| Bluetooth: Realtek (IMC Networks) RTL8723AU USB Bluetooth |  | This currently needs an out of tree [driver](https://github.com/lwfinger/rtl8723au_bt/tree/new) | 
| Camera: Realtek USB Webcam |  |  | 
| SD card reader: AMD FCH SD Flash Controller |  | Driven by sdhci-pci | 
| Hardware monitoring |  | Driven by k10temp and fam15h\_power | 
| Hotkeys |  |  | 

### ACPI / Power Management

| Function | Works | Notes | 
|---|---|---|
| CPU frequency scaling |  | Driven by acpi\_cpufreq | 
| GPU Powersaving (DPM) |  |  | 
| SATA Power Management (ALPM) |  |  | 
| Suspend to RAM |  |  | 
| Suspend to disk (hibernate) |  |  | 
| Display backlight control |  | Driven by acpi\_video | 

### Extra hardware information

`root #``lspci -nnk````
00:00.0 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Family 16h Processor Root Complex [1022:1536]
        Subsystem: Pegatron Device [1b0a:220f]
00:01.0 VGA compatible controller [0300]: Advanced Micro Devices, Inc. [AMD/ATI] Kabini [Radeon HD 8180] [1002:9839]
        Subsystem: Pegatron Device [1b0a:2208]
        Kernel driver in use: radeon
00:01.1 Audio device [0403]: Advanced Micro Devices, Inc. [AMD/ATI] Device [1002:9840]
        Subsystem: Pegatron Device [1b0a:220f]
        Kernel driver in use: snd_hda_intel
00:02.0 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Family 16h Processor Function 0 [1022:1538]
00:02.3 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Family 16h Processor Functions 5:1 [1022:1439]
        Kernel driver in use: pcieport
00:02.5 PCI bridge [0604]: Advanced Micro Devices, Inc. [AMD] Family 16h Processor Functions 5:1 [1022:1439]
        Kernel driver in use: pcieport
00:10.0 USB controller [0c03]: Advanced Micro Devices, Inc. [AMD] FCH USB XHCI Controller [1022:7814] (rev 01)
        Subsystem: Pegatron Device [1b0a:220c]
        Kernel driver in use: xhci_hcd
00:11.0 SATA controller [0106]: Advanced Micro Devices, Inc. [AMD] FCH SATA Controller [AHCI mode] [1022:7801] (rev 40)
        Subsystem: Pegatron Device [1b0a:220f]
        Kernel driver in use: ahci
00:12.0 USB controller [0c03]: Advanced Micro Devices, Inc. [AMD] FCH USB OHCI Controller [1022:7807] (rev 39)
        Subsystem: Pegatron Device [1b0a:220f]
        Kernel driver in use: ohci-pci
00:12.2 USB controller [0c03]: Advanced Micro Devices, Inc. [AMD] FCH USB EHCI Controller [1022:7808] (rev 39)
        Subsystem: Pegatron Device [1b0a:220f]
        Kernel driver in use: ehci-pci
00:13.0 USB controller [0c03]: Advanced Micro Devices, Inc. [AMD] FCH USB OHCI Controller [1022:7807] (rev 39)
        Subsystem: Pegatron Device [1b0a:220f]
        Kernel driver in use: ohci-pci
00:13.2 USB controller [0c03]: Advanced Micro Devices, Inc. [AMD] FCH USB EHCI Controller [1022:7808] (rev 39)
        Subsystem: Pegatron Device [1b0a:220f]
        Kernel driver in use: ehci-pci
00:14.0 SMBus [0c05]: Advanced Micro Devices, Inc. [AMD] FCH SMBus Controller [1022:780b] (rev 3a)
        Subsystem: Pegatron Device [1b0a:220f]
        Kernel driver in use: piix4_smbus
00:14.2 Audio device [0403]: Advanced Micro Devices, Inc. [AMD] FCH Azalia Controller [1022:780d] (rev 02)
        Subsystem: Pegatron Device [1b0a:2209]
        Kernel driver in use: snd_hda_intel
00:14.3 ISA bridge [0601]: Advanced Micro Devices, Inc. [AMD] FCH LPC Bridge [1022:780e] (rev 11)
        Subsystem: Pegatron Device [1b0a:220f]
00:14.7 SD Host controller [0805]: Advanced Micro Devices, Inc. [AMD] FCH SD Flash Controller [1022:7813] (rev 01)
        Subsystem: Pegatron Device [1b0a:220f]
        Kernel driver in use: sdhci-pci
00:18.0 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Family 16h Processor Function 0 [1022:1530]
00:18.1 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Family 16h Processor Function 1 [1022:1531]
00:18.2 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Family 16h Processor Function 2 [1022:1532]
00:18.3 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Family 16h Processor Function 3 [1022:1533]
        Kernel driver in use: k10temp
00:18.4 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Family 16h Processor Function 4 [1022:1534]
        Kernel driver in use: fam15h_power
00:18.5 Host bridge [0600]: Advanced Micro Devices, Inc. [AMD] Family 16h Processor Function 5 [1022:1535]
01:00.0 Network controller [0280]: Realtek Semiconductor Co., Ltd. RTL8723BE PCIe Wireless Network Adapter [10ec:b723]
        Subsystem: AzureWave Device [1a3b:2159]
        Kernel driver in use: rtl8723be
02:00.0 Ethernet controller [0200]: Realtek Semiconductor Co., Ltd. RTL8101E/RTL8102E PCI Express Fast Ethernet controller [10ec:8136] (rev 07)
        Subsystem: Pegatron Device [1b0a:220b]
        Kernel driver in use: r8169
```
`root #``lsusb`
Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 004 Device 002: ID 27c6:0002  
Bus 004 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
Bus 001 Device 003: ID 0bda:570b Realtek Semiconductor Corp. 
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 003 Device 002: ID 13d3:3410 IMC Networks 
Bus 003 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
Bus 006 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 005 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

## Configuration details

### Compiler flags

*Main article: [Safe CFLAGS](https://wiki.gentoo.org/wiki/Safe_CFLAGS)*

**`make.conf`**

### Kernel

### Required packages

- [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) for the Wi-Fi firmware and the GPU microcode

## Enabling AMD-V (SVM) hardware virtualization

This netbook's CPU/Chipset supports AMD-v hardware virtualization. However, the UEFI firmware does not expose a setting for it. Luckily, one user has found a way to enable the setting for the very similar Acer Aspire V5-122p notebook: [\[1\]](http://forums.mydigitallife.info/threads/45118-Acer-Aspire-V5-122p-Enable-SVM-How-To).

First, make sure that efivarfs is mounted to /sys/firmware/efi/efivars. Then, enter /sys/firmware/efi/efivars and examine the contents of the file containing the UEFI setup configuration.

`root #````
hexdump -C Setup-a04a27f4-df00-4d42-b552-39511302113d
```
`000000f0 01 00**00** 00 00 00 00 00  00 00 00 00 00 01 00 00  |................|`

The interesting line is shown above, with the setting for AMD-v marked in bold at offset 0xf2. Now, change this setting to 1 and verify that the change was applied:

`root #````
printf "\x1" | dd bs=1 seek=242 conv=notrunc of=Setup-a04a27f4-df00-4d42-b552-39511302113d
```
`root #````
hexdump -C Setup-a04a27f4-df00-4d42-b552-39511302113d
```
`000000f0 01 00**01** 00 00 00 00 00  00 00 00 00 00 01 00 00  |................|`

Now power off(!) your system and verify that KVM is now enabled on next boot in dmesg:

`user $``dmesg|grep kvm`
kvm: Nested virtualization enabled
kvm: Nested paging enabled
