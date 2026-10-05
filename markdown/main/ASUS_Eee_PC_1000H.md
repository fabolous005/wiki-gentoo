<!-- source: https://wiki.gentoo.org/wiki/ASUS_Eee_PC_1000H | group: Gentoo Wiki (Main) | wiki-title: ASUS Eee PC 1000H -->
---
title: ASUS Eee PC 1000H
url: https://wiki.gentoo.org/wiki/ASUS_Eee_PC_1000H
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-20"
fingerprint: "3f304f676ae2b9e9"
license: CC BY-SA 4.0
---

# ASUS Eee PC 1000H

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



## Hardware Specs

- [Intel Atom N270](https://ark.intel.com/products/36331/Intel-Atom-Processor-N270-512K-Cache-1-60-GHz-533-MHz-FSB-) (with turbo up to 1.8GHz using the eeepc\_laptop driver)
- 10.1" LCD screen (1024×600)
- i945 graphics chipset (“gen3”, OpenGL 1.4)
- 1 GB DDR2-533 RAM (upgradable to 2GB)
- 160GB hard disk (2.5" SATA, replaceable)
- 3× USB2 ports, SD card reader (SDHC compatible), VGA port
- 10/100 Ethernet
- 2.4GHz Wi-Fi B/G/N support
- Bluetooth 2.1
- HDA audio
- 1.3 megapixel webcam

Although the N270 CPU was considered weak even at launch, its lack of speculative execution hardware makes it immune to [Spectre and related security issues](https://wiki.gentoo.org/wiki/Project:Security/Vulnerabilities/Meltdown_and_Spectre):

`user $` `lscpu | grep Vuln`
Vulnerability Itlb multihit:     Not affected
Vulnerability L1tf:              Not affected
Vulnerability Mds:               Not affected
Vulnerability Meltdown:          Not affected
Vulnerability Spec store bypass: Not affected
Vulnerability Spectre v1:        Not affected
Vulnerability Spectre v2:        Not affected
Vulnerability Tsx async abort:   Not affected

## Installation

As with most netbooks the Eee PC 1000H lacks an optical drive; an external one may be used, but a more common choice is [creating LiveUSB boot media](https://wiki.gentoo.org/wiki/LiveUSB). To boot from external media you have to press `Esc` during the BIOS boot splash screen and select the right installation medium. After booting a live system follow the [Gentoo Linux x86 Handbook](https://wiki.gentoo.org/wiki/Handbook:X86).

### make.conf

**`/etc/portage/make.conf`**

```
CFLAGS="-O2 -pipe -march=bonnell -msahf -mmovbe -mfxsr"
CXXFLAGS="${CFLAGS}"
```
**`/etc/portage/package.use/00video`**

```
 VIDEO_CARDS: -* intel i915
```
**`/etc/portage/package.use/00input`**

```
 INPUT_DEVICES: libinput
```
**`/etc/portage/package.use/00cpu-flags`**

```
 CPU_FLAGS_X86: mmx mmxext sse sse2 sse3 ssse3
```
### Compilation speed

To reduce compiling time and decrease the likelihood of builds failing due to low memory, consider using one or more of [ccache](https://wiki.gentoo.org/wiki/Ccache), [distcc](https://wiki.gentoo.org/wiki/Distcc), or [zram](https://wiki.gentoo.org/wiki/Zram). Upgrading the RAM to 2GB will help too, especially if building modern web browsers.

## Kernel Configuration

### CPU

**Intel Atom N270**

### Hard disk

lspci and other tools will show the drive controller operating in IDE emulation mode, thus the appropriate driver is the PATA one:

**00:1f.2 IDE interface: Intel Corporation 82801GBM/GHM (ICH7-M Family) SATA Controller \[IDE mode\]**

### Graphics

**00:02.0 VGA compatible controller: Intel Corporation Mobile 945GSE Express Integrated Graphics Controller**

### Sound

**00:1b.0 Audio device: Intel Corporation NM10/ICH7 Family High Definition Audio Controller**

### Ethernet

**03:00.0 Ethernet controller: Qualcomm Atheros AR8121/AR8113/AR8114 Gigabit or Fast Ethernet**

### Wireless

**01:00.0 Network controller: Ralink corp. RT2790 Wireless 802.11n 1T/2R PCIe**

The required firmware files are available in [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) and [net-wireless/wireless-regdb](https://packages.gentoo.org/packages/net-wireless/wireless-regdb). PCIe hotplug is required for the `Fn+F2` keyboard toggle to actually switch the card on and off.

### Touchpad

**ETPS/2 Elantech Touchpad**

### ACPI, LEDs and Hotkeys

**Asus EeePC extra buttons, hotplug toggles, hardware sensors, turbo mode support**

If you have updated the BIOS to a recent version, it assumes Windows 7 is running by default and disables the interfaces needed by the eeepc\_laptop driver. To fix this, append acpi\_osi=Linux to the kernel command line.

Newer BIOS revisions have more `Fn` combinations (mostly on the unlabelled F-keys). Stable versions of the kernel have already received updates to recognize most of these, but `Fn`+`space` is missing; see below for a fix.

### USB

**00:1d.7 USB controller: Intel Corporation NM10/ICH7 Family USB2 EHCI Controller**

Several internal devices are on the USB bus:

#### Bluetooth

**0b05:b700 ASUSTek Computer, Inc. Broadcom Bluetooth 2.1**

#### SD card reader

**058f:6335 Alcor Micro Corp. SD/MMC Card Reader**

#### Webcam

**04f2:b071 Chicony Electronics Co., Ltd 2.0M UVC Webcam / CNF7129**
