<!-- source: https://wiki.gentoo.org/wiki/TUXEDO_Aura_15_(Gen2) | group: Gentoo Wiki (Main) | wiki-title: TUXEDO Aura 15 (Gen2) -->
---
title: TUXEDO Aura 15 (Gen2)
url: https://wiki.gentoo.org/wiki/TUXEDO_Aura_15_(Gen2)
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-20"
fingerprint: "1f338b57889e7b92"
license: CC BY-SA 4.0
---

# TUXEDO Aura 15 (Gen2)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

![](https://wiki.gentoo.org/images/thumb/9/93/20220423_190142_nometadata.jpg/300px-20220423_190142_nometadata.jpg)

The **TUXEDO Aura 15 (Gen2)** is a configurable Linux notebook from 2022. It can be run using only open source drivers without sacrificing any functionality. When buying a configuration with 32G memory and a R7 5700U, the notebook can easily run Gentoo.

## Hardware

| Device | Make/model | Status | Kernel version | Note | 
|---|---|---|---|---|
| APU | AMD Ryzen 7 5700U |  | 6.1.31 |  | 
| APU | AMD Ryzen 5 5500U | <sup>[\[1\]](https://wiki.gentoo.org#cite_note-tuxedo_linux-1)</sup> | N/A |  | 
| APU | AMD Ryzen 3 5300U | <sup>[\[1\]](https://wiki.gentoo.org#cite_note-tuxedo_linux-1)</sup> | N/A |  | 
| Video | AMD ATI 05:00.0 Lucienne |  | 6.1.31 |  | 
| Webcam | BisonCam,NB Pro (1MP 720p) |  | 6.1.31 |  | 
| External speaker | Stereo High Definition Audio |  | 6.1.31 |  | 
| Microphone | N/A |  | 6.1.31 |  | 
| Keyboard | QWERTZ |  | 6.1.31 | should work with every other layout; to control backlighting, go to [TUXEDO's/Clevo's driver modules](<https://wiki.gentoo.org/wiki/TUXEDO_Aura_15_(Gen2)#TUXEDO.27s_.26_Clevo.27s_driver_modules>) | 
| LTE/HSDPA+ 4G/GPS Standalone, A-GPS, GPS XTRA, Glonass | Huawei ME936 | <sup>[\[1\]](https://wiki.gentoo.org#cite_note-tuxedo_linux-1)</sup> | N/A |  | 

## Installation

### make.conf

**`/etc/portage/make.conf`**

**For every Notebook configuration**

```
CHOST="x86_64-pc-linux-gnu"
COMMON_FLAGS="-march=znver2"
```
**`/etc/portage/package.use/00video`**

```
 VIDEO_CARDS: -* amdgpu radeonsi radeon
```
**`/etc/portage/package.use/00grub`**

```
 GRUB_PLATFORMS: efi-64
```
**`/etc/portage/package.use/00cpu-flags`**

**Only Ryzen 7 5700U**

```
 CPU_FLAGS_X86: aes avx avx2 f16c fma3 mmx mmxext pclmul popcnt rdrand sha sse sse2 sse3 sse4_1 sse4_2 sse4a ssse3
```
### Kernel

- Read [this](https://wiki.gentoo.org/wiki/Ryzen#Kernel) article for the processor.
- Read [this](https://wiki.gentoo.org/wiki/Webcam#Kernel) article for the webcam.
- Enable these options for the remaining hardware:

**Other options with 6.1.31-gentoo**

#### TUXEDO's & Clevo's driver modules

Read the [TUXEDO Software](https://wiki.gentoo.org/wiki/TUXEDO_Software) article for further instructions.

## See also

- [Ryzen](https://wiki.gentoo.org/wiki/Ryzen) — a multithreaded, high performance processor manufactured by AMD.
- [AMDGPU](https://wiki.gentoo.org/wiki/AMDGPU) — the open source graphics drivers for AMD Radeon and other GPUs.
