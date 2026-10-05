<!-- source: https://wiki.gentoo.org/wiki/Ryzen | group: Gentoo Wiki (Main) | wiki-title: Ryzen -->
---
title: Ryzen
url: https://wiki.gentoo.org/wiki/Ryzen
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-26"
fingerprint: "5e07d957a13edf86"
license: CC BY-SA 4.0
---

# Ryzen

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Ryzen is a multithreaded, high performance processor manufactured by AMD. The first generation based on the [Zen microarchitecture](<https://en.wikipedia.org/wiki/Zen_(microarchitecture)>) (µarch) was released in Q1, 2017 as Ryzen 1000 series. A refresh called Zen+ was released in Q2, 2018, as the Ryzen 2000 series. The second generation is the Ryzen 3000/4000 series, based on the [Zen 2 microarchitecture](https://en.wikipedia.org/wiki/Zen_2), and was released in Q3, 2019. The third generation *aka* [Zen 3 microarchitecture](https://en.wikipedia.org/wiki/Zen_3) was released in Q4, 2020. The Ryzen 5000 series features processors of the Zen 2 and the Zen 3 microarchitectures. Released in Q4, 2022, the Ryzen 7000 series of CPUs feature the [Zen 4 microarchitecture](https://en.wikipedia.org/wiki/Zen_4). The Ryzen 9000 series, first released in Q3, 2024 feature the [Zen 5 microarchitecture](https://en.wikipedia.org/wiki/Zen_5).

## Hardware

To find which Zen generation a running Ryzen system really is (especially on Ryzen 7000 systems, which can be either Zen 2, Zen 3, or Zen 4)<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>, running the following command(s) will help determine the actual Zen generation:

| 1 <sup>st</sup> command | → | to look for in output: | → | 2 <sup>nd</sup> command | → | look for: | → | CONCLUSION | 
|---|---|---|---|---|---|---|---|---|
| `user $``uname -p` | → | Ryzen 1000 or 2000 |  | → |  | → | → | Zen 1 | 
|  |  | Ryzen 3000 or 4000 |  | → |  | → | → | Zen 2 | 
|  |  | Ryzen 5000 | → | `user $``grep -m1 "cpu family" /proc/cpuinfo` | → | `cpu family: 23` (17h) | → | Zen 2 | 
|  |  |  |  |  | → | `cpu family: 25` (19h) | → | Zen 3 | 
|  |  | Ryzen 6000 |  | → |  | → | → | Zen 3 | 
|  |  | Ryzen 7000 | → | `user $``grep -m2 -P "(cpu family)\|model[^ ]" /proc/cpuinfo` | → | `cpu family: 23` (17h) | → | Zen 2 | 
|  |  |  |  |  | → | `cpu family: 25` (19h) and either `model: 0-15` or `model: 32-95` | → | Zen 3 | 
|  |  |  |  |  | → | `cpu family: 25` (19h) and either `model: 16-31` or `model: 96+` | → | Zen 4 | 

Alternatively, on the actual system, running gcc can show the native compiler optimization, provided the version of gcc in use has support for it (see [#GCC](https://wiki.gentoo.org#GCC)):

`user $``LC_ALL=C gcc -### -E - -march=native`
In the output, one line should show `"-march=znver`, where ***n***"*n* (`1` through `6`) is the Zen generation of the currently running system.

The [sys-apps/cpuid](https://packages.gentoo.org/packages/sys-apps/cpuid) utility can show the µarch as well:

`root #``emerge --ask sys-apps/cpuid``user $``cpuid -1 | grep uarch` (uarch synth) = AMD Zen 3, 7nm

### Ryzen Threadripper

| Device | µarch | Status | Bus ID | Kernel driver(s) | Kernel version | Latest microcode patch level | Notes | 
|---|---|---|---|---|---|---|---|
| Ryzen TR 1900X | Zen |  | N/A | N/A | ? | ? |  | 
| Ryzen TR 1920X | Zen |  | N/A | N/A | 4.19.44+ | 0x08001137 | For stability use kernel parameters: `processor.max_cstate=1 rcu_nocbs=0-11 idle=nomwait` | 
| Ryzen TR 1950X | Zen |  | N/A | N/A | 4.19.52+ | 0x8001137 | Firmware blob: amd-ucode/microcode\_amd\_fam17h.bin | 
| Ryzen TR 2920X | Zen+ |  | N/A | N/A | ? | ? |  | 
| Ryzen TR 2950X | Zen+ |  | N/A | N/A | ? | 0x0800820d | Firmware blob: amd-ucode/microcode\_amd\_fam17h.bin | 
| Ryzen TR 2970WX | Zen+ |  | N/A | N/A | ? | ? |  | 
| Ryzen TR 2990WX | Zen+ |  | N/A | N/A | ? | ? |  | 
| Ryzen TR 3990X | Zen2 |  | N/A | N/A | 5.4+ | 0x0830104d | Firmware blob: amd-ucode/microcode\_amd\_fam17h.bin | 
| Ryzen TR PRO 7965WX | Zen4 |  | N/A | N/A | 6.12.31+ | 0x0a10810c | Firmware blob: amd-ucode/microcode\_amd\_fam19h.bin has outdated 0x0a108109 microcode. | 

### Ryzen 9

| Device | µarch | Status | Bus ID | Kernel driver(s) | Kernel version | Latest microcode patch level | Notes | 
|---|---|---|---|---|---|---|---|
| Ryzen 9 7900X and 7950X | Zen 4 |  | N/A | [amdgpu](https://wiki.gentoo.org/wiki/AMDGPU) | 6.0.5 | 0xa601203 | Firmware blob: amd-ucode/microcode\_amd\_fam19h.bin from linux-firmware-20221214. | 
| Ryzen 9 5950X | Zen 3 |  | N/A | N/A | 5.15.41 | 0xa20120a | Firmware blob: amd-ucode/microcode\_amd\_fam19h.bin from linux-firmware-20220509. | 
| Ryzen 9 5900X | Zen 3 |  | N/A | N/A | 5.10+ | ? | System booted with 5.10.15 with minor issues, but would recommend 5.13+. Tested with latest 5.15.2 also without issues. Firmware Blob: amd-ucode/microcode\_amd\_fam17h.bin & linux-firmware-20211027 | 
| Ryzen 9 3950X | Zen 2 |  | N/A | N/A | 5.4.0-rc5 | ? | Earlier version of kernel were not tried(experimental kernel due to AMDGPU drivers are needed for newest AMD video card). Booting with keyboard that requires 2 USB connectors fails at GRUB time. Workaround - disconnect keyboard until kernel is loading. Once kernel started, keyboard can be connected as usual. At this time it's not clear if problem is related to CPU or motherboard of keyboard or combination. On same motherboard/keyboard but with Ryzen 5 - it worked fine. | 
| Ryzen 9 3900X | Zen 2 |  | N/A | N/A | 5.4.38 | ? | Earlier kernel versions not tested | 

### Ryzen 7

| Device | µarch | Status | Bus ID | Kernel driver(s) | Kernel version | Latest microcode patch level | Notes | 
|---|---|---|---|---|---|---|---|
| Ryzen 7 9800X3D | Zen 5 |  | N/A | [amdgpu](https://wiki.gentoo.org/wiki/AMDGPU) | 6.10.14+ | 0x0b404023 | Firmware blob: amd-ucode/microcode\_amd\_fam19h.bin from linux-firmware-20241110. | 
| Ryzen 7 PRO 5850U | Zen 3 |  | N/A | [amdgpu](https://wiki.gentoo.org/wiki/AMDGPU) | 5.10+ | 0x0a50000c | "Cezanne" APU; kernel 5.11 recommended, full support since 5.13 | 
| Ryzen 7 5800H |  |  |  |  |  |  |  | 
| Ryzen 7 5700U | Zen 2 |  | N/A | [amdgpu](https://wiki.gentoo.org/wiki/AMDGPU) | 5.15.32+ | 0x8608103 | "Lucienne" APU | 
| Ryzen 7 5700G | Zen 3 |  | 1002-1638 | [amdgpu](https://wiki.gentoo.org/wiki/AMDGPU) | 5.10.150+ | 0x0a50000c | "Cezanne" APU | 
| Ryzen 7 4800H | Zen 2 |  | N/A |  | 5.16.15+ | 0x08600106 |  | 
| Ryzen 7 3700X | Zen 2 |  | N/A | N/A | ? | ? |  | 
| Ryzen 7 2700X | Zen+ |  | N/A | N/A | 4.4.10+ | 0x08008206 | AGESA 1002c | 
| Ryzen 7 1800X | Zen |  | N/A | N/A | 4.4.10+ | 0x08001138 | AGESA 0072 | 
| Ryzen 7 1700X | Zen |  | N/A | N/A | 4.4.10+ | 0x08001129 | ? | 
| Ryzen 7 1700 | Zen |  | N/A | N/A | 4.4.10+ | 0x08001138 | ? | 

### Ryzen 5

| Device | µarch | Status | Bus ID | Kernel driver(s) | Kernel version | Latest microcode patch level | Notes | 
|---|---|---|---|---|---|---|---|
| Ryzen 5 5600G | Zen 3 |  | N/A | N/A | ? | ? | ? | 
| Ryzen 5 3600X | Zen 2 |  | N/A | N/A | 4.19.66+ | 0x08701013 | 1.0.0.4B | 
| Ryzen 5 1600X | Zen |  | N/A | N/A | ? | ? | ? | 
| Ryzen 5 1600 | Zen |  | N/A | N/A | 4.4.10+ | 0x08001138 | 1.0.0.4C | 
| Ryzen 5 1500X | Zen |  | N/A | N/A | ? | ? | ? | 
| Ryzen 5 1400 | Zen |  | N/A | N/A | ? | ? | ? | 

## Installation

### Firmware

To install the Zen microcode, emerge [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware):

`root #``emerge --ask sys-kernel/linux-firmware`
The firmware blobs will need to be added to the kernel in order to be loaded.

### Kernel

Enable support for Ryzen hardware in kernel 4.11.0 and newer:

**Kernel 4.11.0 or newer**

Processor type and features  --->
  \[\*\] Symmetric multi-processing support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SMP\</code> to find this item.
  \[\*\] Support x2apic [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_X86\_X2APIC\</code> to find this item.
  \[\*\] AMD ACPI2Platform devices support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_X86\_AMD\_PLATFORM\_DEVICE\</code> to find this item.
  \[\*\] Supported processor vendors [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_PROCESSOR\_SELECT\</code> to find this item.  --->
    \[\*\]   Support AMD processors [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CPU\_SUP\_AMD\</code> to find this item.
  \[\*\] SMT (Hyperthreading) scheduler support
  \[\*\] Multi-core scheduler support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SCHED\_MC\</code> to find this item.
  \[\*\] Machine Check / overheating reporting [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_X86\_MCE\</code> to find this item.
  \[\*\]   AMD MCE features [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_X86\_MCE\_AMD\</code> to find this item.
  Performance monitoring  --->
    \<\*> Intel/AMD rapl performance events [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_PERF\_EVENTS\_INTEL\_RAPL\</code> to find this item.
  \[\*\]   AMD microcode loading support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_MICROCODE\_AMD\</code> to find this item.
Power management and ACPI options  --->
  CPU Frequency scaling  --->
      Default CPUFreq governor (ondemand) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CPU\_FREQ\_DEFAULT\_GOV\_ONDEMAND\</code> to find this item.  --->
    \<\*>   ACPI Processor P-States driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_X86\_ACPI\_CPUFREQ\</code> to find this item.
    \[ /\*\]     Legacy cpb sysfs knob support for AMD CPUs [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_X86\_ACPI\_CPUFREQ\_CPB\</code> to find this item.
    \< >   AMD Opteron/Athlon64 PowerNow! [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_X86\_POWERNOW\_K8\</code> to find this item.
    \<\*>   AMD frequency sensitivity feedback powersave bias [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_X86\_AMD\_FREQ\_SENSITIVITY\</code> to find this item.
Device Drivers  --->
  Generic Driver Options --->
    Firmware loader  --->
      \[\*\] Firmware loading facility [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_FW\_LOADER\</code> to find this item.
      (amd-ucode/microcode\_amd\_fam17h.bin) External firmware blobs to build into the kernel binary [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_EXTRA\_FIRMWARE\</code> to find this item.
      (/lib/firmware) Firware blobs root directory [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_EXTRA\_FIRMWARE\_DIR\</code> to find this item.
  \[\*\] Hardware Monitoring support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_HWMON\</code> to find this item. --->
    \<\*>   AMD Family 10h+ temperature sensor [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SENSORS\_K10TEMP\</code> to find this item.
  \[\*\] IOMMU Hardware Support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_IOMMU\_SUPPORT\</code> to find this item.  --->
    \[\*\]   AMD IOMMU support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_AMD\_IOMMU\</code> to find this item.
    \<\*>     AMD IOMMU Version 2 driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_AMD\_IOMMU\_V2\</code> to find this item.

For Zen 2 (or newer) CPUs, an alternative [AMD P-State driver](https://wiki.gentoo.org/wiki/Power_management/Guide#AMD_P-State) can be used instead of the traditional ACPI driver. AMD P-State supports the schedutil and ondemand governors for dynamic frequency control. See [upstream's documentation](https://www.kernel.org/doc/html/latest/admin-guide/pm/amd-pstate.html) for more information.

**Kernel 5.17 or newer provide the alternative AMD P-State driver**

```
Power management and ACPI options  --->
  CPU Frequency scaling  --->
      Default CPUFreq governor (schedutil) 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_CPU_FREQ_DEFAULT_GOV_SCHEDUTIL</code> to find this item.  --->
    <*>   AMD Processor P-State driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_X86_AMD_PSTATE</code> to find this item.
Additionally, the following options enable various chipset drivers common to all Ryzen platforms:

**Additional chipset drivers**

```
Device Drivers  --->
  I2C support --->
    I2C Hardware Bus Support --->
      <*> Intel PIIX4 and compatible (ATI/AMD/Serverworks/Broadcom/SMSC) 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_I2C_PIIX4</code> to find this item.
  [*] Pin Controllers [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_PINCTRL</code> to find this item. --->
    [*]   AMD GPIO pin control [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_PINCTRL_AMD</code> to find this item.
[*] Cryptographic API [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_CRYPTO</code> to find this item. --->
  [*]   Hardware crypto devices [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_CRYPTO_HW</code> to find this item.  --->
    [*]   Support for AMD Secure Processor [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_CRYPTO_DEV_CCP</code> to find this item.
    [ /*]     Cryptographic Coprocessor device [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_CRYPTO_DEV_SP_CCP</code> to find this item.
    < /M/*>     Encryption and hashing offload support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_CRYPTO_DEV_CCP_CRYPTO</code> to find this item.
    <*>   Secure Processor device driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_CRYPTO_DEV_CCP_DD</code> to find this item.
    <*>   Platform Security Processor (PSP) device [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_CRYPTO_DEV_SP_PSP</code> to find this item.
For Ryzen desktop chipsets, enable AMD Promontory GPIO:

**Support for AMD Promontory GPIO**

Device Drivers  --->
  \[\*\]   GPIO Support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_GPIOLIB\</code> to find this item.  --->
    Memory mapped GPIO drivers  --->
      \<\*> AMD Promontory GPIO support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_GPIO\_AMDPT\</code> to find this item.

For Zen 3 (or newer) APUs (e.g. in notebooks or Chromebooks), the AMD Power Management Controller<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup> can be enabled:

**Kernel 5.11 or newer (`CONFIG_AMD_PMC`)**

Device Drivers  --->
  \[\*\] X86 Platform Specific Device Drivers [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_X86\_PLATFORM\_DEVICES\</code> to find this item.  --->
    \<\*>   AMD SoC PMC driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_AMD\_PMC\</code> to find this item.

Those using [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources) with the `experimental` USE flag will have additional Processor family options made available:

**Kernel (gentoo-sources) – choose one of:**

```
Processor type and features  --->
  Processor family  --->
    (X) AMD Zen 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_MZEN</code> to find this item.
    ( ) AMD Zen 2 [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_MZEN2</code> to find this item.
    ( ) AMD Zen 3 [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_MZEN3</code> to find this item.
    ( ) AMD Zen 4 [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_MZEN4</code> to find this item.
    ( ) AMD Zen 5 [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_MZEN5</code> to find this item.
    ( ) AMD Zen 6 [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_MZEN6</code> to find this item.
This enables `-march=znver1` (MZEN) and `-march=znver` (MZEN*nn*) to be set for the kernel's make process, which will optimize the kernel's binary for the target CPU architecture. The specific options are only available with the appropriate GCC version.

## Configuration

### GCC

[CFLAGS](https://wiki.gentoo.org/wiki/CFLAGS) in /etc/portage/make.conf may be used for GCC compiler optimization as follows, where `znver1` acts as a placeholder for the actual Zen µarch:

**`/etc/portage/make.conf`**

**Zen compiler optimization**

```
CFLAGS="-O2 -march=znver1"
```
Alternatively, `-march=native` will produce optimized code for system on which GCC is running on, which is not suitable for every configuration (e. g. for distcc see [Distcc#CFLAGS and CXXFLAGS](https://wiki.gentoo.org/wiki/Distcc#CFLAGS_and_CXXFLAGS)).

For historical purposes, the following table shows minimum GCC versions for use with Zen compiler optimizations.

| µarch | `-march` parameter | GCC version | Remarks | 
|---|---|---|---|
| any Zen | n/a | up to 5.x and 6.2 | No support for `znver1` or newer. | 
| Zen, Zen+ | `znver1` | 6.3 and newer | Neither GCC 6 <sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup> nor 7<sup>[\[4\]](https://wiki.gentoo.org#cite_note-4)</sup> were optimized for Zen. Optimizations were introduced with GCC 8<sup>[\[5\]](https://wiki.gentoo.org#cite_note-5)</sup><sup>[\[6\]](https://wiki.gentoo.org#cite_note-6)</sup> and 9.<sup>[\[7\]](https://wiki.gentoo.org#cite_note-7)</sup> | 
| Zen 2 | `znver2` | 9.2 and newer | Support for Zen 2 was backported from GCC 10. | 
| Zen 3 | `znver3` | 10.3 <sup>[\[8\]](https://wiki.gentoo.org#cite_note-8)</sup> and newer |  | 
| Zen 4 | `znver4` | 12.3 <sup>[\[9\]](https://wiki.gentoo.org#cite_note-9)</sup> and newer | Support for Zen 4 was backported from GCC 13.0. | 
| Zen 5 | `znver5` | 14.1 <sup>[\[10\]](https://wiki.gentoo.org#cite_note-10)</sup> and newer |  | 
| Zen 6 | `znver6` | 16.1 <sup>[\[11\]](https://wiki.gentoo.org#cite_note-11)</sup> and newer |  | 

When there is no specific compiler optimization option (`-march=`) available, the previous version should be used, i. e. for Zen 2 `-march=znver1` should be used on GCC from versions 6.3 to 9.1.

For very old GCC versions, a generic option like `-march=skylake` may be used. Bulldozer optimizations (`-march=bdver`) will *n***not** work, because Bulldozer has extensions (namely FMA4, XOP and TBM) that were removed with the Zen microarchitecture.

## Drivers for lm-sensors

The HWMON (lm-sensors) driver for ASUS motherboards of this class are included in kernel versions 5.17 and higher. For those in need to use older kernel versions, consider using the asus-wmi-sensors driver available under [https://github.com/electrified/asus-wmi-sensors](https://github.com/electrified/asus-wmi-sensors). Please also check if the motherboard is supported or not.

An ebuild is available using an overlay. Add it with either:

`root #``eselect repository add nightdragon_layman git https://github.com/NightDragon1/nightdragon_layman.git`
Alternatively there is a community ebuild repository available:

`root #``mkdir -p /etc/portage/repos.conf``root #``wget  -O /etc/portage/repos.conf/gyakovlev.conf https://raw.githubusercontent.com/gyakovlev/gentoo-overlay/master/gyakovlev.conf`
or:

`root #``curl -Lo /etc/portage/repos.conf/gyakovlev.conf --create-dirs https://raw.githubusercontent.com/gyakovlev/gentoo-overlay/master/gyakovlev.conf`
## Troubleshooting

### 1st gen Ryzen series (1000 series)

#### Segmentation faults during compilation

If segmentation faults (segfaults, short SEGVs) are encountered frequently on Zen it might be anything from a software bug to a hardware bug. Since the CPU is under heavy load during a compilation process, this is most commonly the very time to discover such recurring SEGVs. With certain adjustments it may be possible to mitigate these segfaults—there have been reports of success and failure.

When encountering frequent SEGVs, please first ensure the most recently compiled binutils is selected via

`user $``eselect binutils list`
\[1\] x86\_64-pc-linux-gnu-2.27
 \[2\] x86\_64-pc-linux-gnu-2.28 \*

If an early CPU batch is affected (2017), it should have been replaced through RMA (Return Merchandise Authorization) which AMD provided on its website. The recommendation to disable all overclocking and set proper timings for RAM is only for systems that were overclocked—at the designated speed CPU and RAM will not produce recurring SEGVs!

#### Faulty hardware

A problem residing inside the Ryzen processor itself should only affect the very few early Ryzen batches that were produced, available and sold mid-2017. As of 2017-08-08 AMD had confirmed the issue[\[13\]](https://wiki.gentoo.org#cite_note-13)<sup>[\[14\]](https://wiki.gentoo.org#cite_note-14)</sup> and RMA was possible within the warranty period.

- Consider downgrading or upgrading the BIOS/UEFI to the most stable.
- Some motherboards' BIOS/UEFI setups have an option to disable OPCache. This has been observed to limit or stop segfaults albeit with a 5-7% performance cost.
- Some users have reported that disabling ASLR resolves the segfault issues. This can be done at runtime by issuing echo 0 > /proc/sys/kernel/randomize\_va\_space and to make it permanent:

**`/etc/sysctl.conf`**

**Disabling ASLR**

```
 = 0
```
#### Soft freezes on 1st gen Ryzen 7

**Problem:** First generation Ryzen 7 systems will mysteriously soft freeze after a period of time.<sup>[\[15\]](https://wiki.gentoo.org#cite_note-15)</sup> Keyboard and mouse do not respond to input, output on the display freezes. System requires a hard reset (pressing the reset button on the case, pressing and holding the power button for 5 seconds, or pulling the power cord) in order to unfreeze. This is specifically an issue with freezing kernel, and not segfaulting.[\[16\]](https://wiki.gentoo.org#cite_note-16)

**Solution:** This issue may be correctable by disabling c6 power states in the motherboard's firmware or by adjust adding the following kernel symbol, and passing the following kernel cmdline parameter at boot time:

```
General setup  --->
  RCU Subsystem  --->
    [*] Make expert-level adjustments to RCU configuration 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_RCU_EXPERT</code> to find this item.
    [*] Offload RCU callback processing from boot-selected CPUs [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_RCU_NOCB_CPU</code> to find this item.
Then pass the following kernel commandline parameter at boot time `rcu_nocbs=0-15`. This is generally performed with secondary bootloaders such as [GRUB](https://wiki.gentoo.org/wiki/GRUB) or [systemd/systemd-boot](https://wiki.gentoo.org/wiki/Systemd/systemd-boot). Alternatively, when booting via [EFI stub](https://wiki.gentoo.org/wiki/EFI_stub), parameters set within the kernel's .config file and built into the kernel binary. Refer to the appropriate article for details on updating the system's bootloader configuration.

Another possible fix is to add the following kernel cmdline parameters: `clearcpuid=514 rcu_nocbs=0-15 pci=noaer idle=nomwait`

#### Broken VME (Virtual-8086 Mode Enhancements) on 1st gen Ryzen

1st Ryzen processors generation had a broken VME implementation:<sup>[\[17\]](https://wiki.gentoo.org#cite_note-17)</sup> 16-bits VM86 tasks within a 32-bits protected mode OS crash. This has been fixed by a later microcode revision.[\[18\]](https://wiki.gentoo.org#cite_note-18)

### Overclocking or wrong settings

When experiencing segfaults on an otherwise healthy system, the following could help to solve the problem:

- Ensure using the newest binutils; an older instance of binutils could be built against older opcode facilitating crashes due to poor linkage.
- Ensure RAM voltage and timing are correct for the RAM; BIOS/UEFI implementations are conservative while performing autosetting.
- Consider downgrading the BIOS/UEFI to the most stable version. ASUS and ASRock have been known to push very beta BIOS/UEFI versions that have shown to be quite unstable.

### Random reboots with mce events

Likely due to [errata 1109](https://web.archive.org/web/20221024235010/https://www.amd.com/system/files/TechDocs/55449_Fam_17h_M_00h-0Fh_Rev_Guide.pdf) a Ryzen system may encounter random, spontaneous reboots with MCE hardware errors being logged on startup. An example MCE event looks like this:

Oct 31 11:46:23 fire kernel: \[    0.677235\] \[Hardware Error\]: System Fatal error.
Oct 31 11:46:23 fire kernel: \[    0.677439\] \[Hardware Error\]: CPU:10 (17:1:1) MC5\_STATUS\[-|UE|MiscV|PCC|AddrV|-|-|SyndV|TCC\]: 0xbea0000000000108
Oct 31 11:46:23 fire kernel: \[    0.677798\] \[Hardware Error\]: Error Addr: 0x0001ffff810796c0
Oct 31 11:46:23 fire kernel: \[    0.678003\] \[Hardware Error\]: IPID: 0x000500b000000000, Syndrome: 0x000000004d000000
Oct 31 11:46:23 fire kernel: \[    0.678356\] \[Hardware Error\]: Execution Unit Extended Error Code: 0
Oct 31 11:46:23 fire kernel: \[    0.678562\] \[Hardware Error\]: Execution Unit Error: Watchdog timeout error.
Oct 31 11:46:23 fire kernel: \[    0.678562\] \[Hardware Error\]: cache level: RESV, tx: GEN, mem-tx: GEN

Suggested workarounds include:

- Adding the kernel boot parameter `idle=nomwait`. Note that any solution that prevents the kernel from executing the `MWAIT` instruction will not prevent the issue from occurring 100% of the time, as other code could execute the instruction.
- Modifying the ["Power Supply Idle Control" setting](https://bugzilla.kernel.org/show_bug.cgi?id=196683#c194) in the BIOS.
- Consider disabling C-States. This can be done the BIOS/UEFI or with the boot parameter `processor.max_cstate=5`.

See also [this kernel Bugzilla entry](https://bugzilla.kernel.org/show_bug.cgi?id=196683), [this AMD forum discussion](https://community.amd.com/thread/216084?start=75&tstart=0), and many other discussions.

## See also

- [AMDGPU](https://wiki.gentoo.org/wiki/AMDGPU) — the open source graphics drivers for AMD Radeon and other GPUs.
- [AMDGPU-PRO](https://wiki.gentoo.org/wiki/AMDGPU-PRO) — the next generation *closed source* graphics component that operates on top of the open source [AMDGPU](https://wiki.gentoo.org/wiki/AMDGPU) drivers for newer AMD/ATI Radeon graphics cards.
- [AMD microcode](https://wiki.gentoo.org/wiki/AMD_microcode) — describes updating the [microcode](https://wiki.gentoo.org/wiki/Microcode) for [AMD](https://wiki.gentoo.org/wiki/AMD) processors.

## External resources

- [http://www.phoronix.com/scan.php?page=article&item=amd-ryzen-znver1&num=1](http://www.phoronix.com/scan.php?page=article&item=amd-ryzen-znver1&num=1) - Phoronix compiler optimization benchmarks for Ryzen 7.

## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) [Announcing New Model Numbers for 2023+ Mobile Proc... - AMD Community](https://community.amd.com/t5/corporate/announcing-new-model-numbers-for-2023-mobile-processors/ba-p/543985), AMD.com. Retrieved 30 June 2023.
2. [↑](https://wiki.gentoo.org#cite_ref-2) [Linux Kernel Driver DataBase: CONFIG\_AMD\_PMC: AMD SoC PMC driver](https://cateee.net/lkddb/web-lkddb/AMD_PMC.html), LKDDB. Retrieved 30 June 2023.
3. [↑](https://wiki.gentoo.org#cite_ref-3) [GCC bug report](https://gcc.gnu.org/bugzilla/show_bug.cgi?id=80313)
4. [↑](https://wiki.gentoo.org#cite_ref-4) [GCC & LLVM Clang Compiler Benchmarks On AMD's EPYC 7601](https://www.phoronix.com/scan.php?page=article&item=amd-epyc-compilers) (Phoronix, 26 Sept 2017, by Michael Larabel); [page 6](https://www.phoronix.com/scan.php?page=article&item=amd-epyc-compilers&num=6): *…and then GCC 7.2.0 was in last with only coming out ahead in five of the benchmarks.*
5. [↑](https://wiki.gentoo.org#cite_ref-5) [SUSE Developer Working On AMD Zen Tuning For GCC](https://www.phoronix.com/scan.php?page=news_item&px=Jan-Zen-GCC-Tuning) (Phoronix, 5 Oct 2017, by Michael Larabel)
6. [↑](https://wiki.gentoo.org#cite_ref-6) [Running Some Fresh GCC 8.0 Compiler Benchmarks On AMD EPYC With "znver1"](https://www.phoronix.com/scan.php?page=news_item&px=GCC-8.0-znver1-20171030) (Phoronix, 31 Oct 2017, by Michael Larabel)
7. [↑](https://wiki.gentoo.org#cite_ref-7) [Extensive Benchmarks Looking At AMD Znver1 GCC 9 Performance, EPYC Compiler Tuning](https://www.phoronix.com/scan.php?page=article&item=amd-epyc-gcc9&num=1) (Phoronix, 20 Feb 2019, by Michael Larabel)
8. [↑](https://wiki.gentoo.org#cite_ref-8) [GCC 10 Release Series — Changes, New Features, and Fixes - GNU Project - Free Software Foundation (FSF)](https://gcc.gnu.org/gcc-10/changes.html) (GNU.org)
9. [↑](https://wiki.gentoo.org#cite_ref-9) [GCC 12 Release Series — Changes, New Features, and Fixes - GNU Project](https://gcc.gnu.org/gcc-12/changes.html) (GNU.org)
10. [↑](https://wiki.gentoo.org#cite_ref-10) [AMD Zen 5 "Znver5" CPU Enablement Merged For GCC 14](https://www.phoronix.com/news/AMD-Zen-5-Znver5-Merged-GCC14) (phoronix.com)
11. [↑](https://wiki.gentoo.org#cite_ref-11) [GCC 16.1 Released With AMD Zen 6 Support, Algol 68 & Many C++ Improvements](https://www.phoronix.com/news/GCC-16.1-Released) (phoronix.com)
12. [↑](https://wiki.gentoo.org#cite_ref-12) [https://gcc.gnu.org/bugzilla/show\_bug.cgi?id=80313](https://gcc.gnu.org/bugzilla/show_bug.cgi?id=80313)
13. [↑](https://wiki.gentoo.org#cite_ref-13) Phoronix - [AMD **Confirms** Linux Performance Marginality Problem Affecting Some, Doesn't Affect Epyc / Theadripper](https://www.phoronix.com/scan.php?page=news_item&px=Ryzen-Segv-Response)
14. [↑](https://wiki.gentoo.org#cite_ref-14) AMD Forums - [gcc segmentation faults on Ryzen / Linux](https://community.amd.com/thread/215773)
15. [↑](https://wiki.gentoo.org#cite_ref-15) [https://bugzilla.kernel.org/show\_bug.cgi?id=196683](https://bugzilla.kernel.org/show_bug.cgi?id=196683)
16. [↑](https://wiki.gentoo.org#cite_ref-16) [https://bugzilla.kernel.org/show\_bug.cgi?id=196683#c9](https://bugzilla.kernel.org/show_bug.cgi?id=196683#c9)
17. [↑](https://wiki.gentoo.org#cite_ref-17) [https://www.os2museum.com/wp/vme-broken-on-amd-ryzen](https://www.os2museum.com/wp/vme-broken-on-amd-ryzen)
18. [↑](https://wiki.gentoo.org#cite_ref-18) [https://www.os2museum.com/wp/vme-fixed-on-amd-ryzen](https://www.os2museum.com/wp/vme-fixed-on-amd-ryzen)
