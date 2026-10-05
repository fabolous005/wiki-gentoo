<!-- source: https://wiki.gentoo.org/wiki/ATI_FAQ | group: Gentoo Wiki (Main) | wiki-title: ATI FAQ -->
---
title: ATI FAQ
url: https://wiki.gentoo.org/wiki/ATI_FAQ
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-09-27"
fingerprint: "1947dc26c91e1b7e"
license: CC BY-SA 4.0
---

# ATI FAQ

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


This article contains Frequently Asked Questions (FAQ) to help users avoid some common installation and configuration issues related to DRI and X11 for AMD/ATI boards.

## Hardware support

### Are AMD/ATI boards supported?

Many AMD/ATI boards (but not all) are supported by Xorg, including 2D/3D accelerated features. For newer GPUs since GCN (Graphics Core Next) Generation 1.1 (Southern Islands and newer) drivers are provided as open source [AMDGPU](https://wiki.gentoo.org/wiki/AMDGPU) and closed source [AMDGPU-PRO](https://wiki.gentoo.org/wiki/AMDGPU-PRO). Both have excellent 2D and 3D accelerated performance.

| GPU | Common Name | Mesa driver | Kernel module | Xorg driver | 
|---|---|---|---|---|
| Rage128 | Rage128 | rage128 | rage128 |  | 
| R100 | Radeon 7xxx, Radeon 64 | radeon | [radeon](https://wiki.gentoo.org/wiki/Radeon) | radeon | 
| R200, R250, R280 | Radeon 8500, Radeon 9000, Radeon 9200 | r200 | radeon | radeon | 
| R300, R400 | Radeon 9500-X850 | r300 | radeon | radeon | 
| R500 | Radeon X1300-X1950 | r300 | radeon | radeon | 
| R600 | Radeon HD2000 series | r600 | radeon | radeon | 
| RV670 | Radeon HD3000 series | r600 | radeon | radeon | 
| RV770 (R700) | Radeon HD4000 series | r600 | radeon | radeon | 
| Evergreen | Radeon HD5000 series | r600 | radeon | radeon | 
| Northern Islands | Radeon HD6000 series | r600 | radeon | radeon | 
| Southern Islands | Radeon HD7000 series (except HD7790), early Radeon R7/R9 series | radeonsi | radeon, [AMDGPU](https://wiki.gentoo.org/wiki/AMDGPU)<sup>1</sup> | radeon, amdgpu <sup>1</sup> | 
| Sea Islands | Radeon HD7790, Radeon R7/R9 series | radeonsi | radeon, AMDGPU, [AMDGPU-PRO](https://wiki.gentoo.org/wiki/AMDGPU-PRO) | radeon, amdgpu | 
| Volcanic Islands | late Radeon R9 series, Radeon R9 Fury, R9 Nano, R9 Fury X, Pro Duo | radeonsi | radeon, AMDGPU, AMDGPU-PRO | amdgpu | 
| Arctic Islands | Radeon RX 400/500 series | radeonsi | AMDGPU, AMDGPU-PRO | amdgpu | 
| Vega | Radeon RX Vega | radeonsi | AMDGPU, AMDGPU-PRO | amdgpu | 
| Navi | Radeon RX 5500/5600/5700 (XT) | radeonsi | AMDGPU, AMDGPU-PRO | amdgpu | 
| Raven Ridge | all AMD [Ryzen](https://wiki.gentoo.org/wiki/Ryzen) APUs "with Radeon Graphics" | radeonsi | AMDGPU, AMDGPU-PRO | amdgpu | 

- <sup>1</sup> Experimental, optional support since kernel 4.9-rc1

### I have an All-In-Wonder/Vivo board. Are the multimedia features supported?

Nothing special is necessary for the board's multimedia features; [x11-drivers/xf86-video-ati](https://packages.gentoo.org/packages/x11-drivers/xf86-video-ati) should work.

### I'm not using an x86-based architecture. What are my options?

Xorg support on the **ppc** or **alpha** platforms is quite similar to **amd64** and **x86**. The open source Xorg drivers should work well on all architectures.

### Are ATI Mobility models supported?

They should be, but there may be a configuration issue due to the OEM PCI ID on certain chips. In such cases the configuration file may need to be manually written.

## See also

- [Xorg/Hardware 3D acceleration guide](https://wiki.gentoo.org/wiki/Xorg/Hardware_3D_acceleration_guide) — a guide to getting 3D acceleration working using the DRM with Xorg in Gentoo.
