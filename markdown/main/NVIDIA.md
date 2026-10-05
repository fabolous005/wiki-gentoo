<!-- source: https://wiki.gentoo.org/wiki/NVIDIA | group: Gentoo Wiki (Main) | wiki-title: NVIDIA -->
---
title: NVIDIA
url: https://wiki.gentoo.org/wiki/NVIDIA
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-16"
fingerprint: afa79a7c028c99ac
license: CC BY-SA 4.0
---

# NVIDIA

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**NVIDIA** is a popular graphical chipset manufacturer. NVIDIA GPUs can use either the open source ([nouveau](https://wiki.gentoo.org/wiki/Nouveau), [x11-drivers/xf86-video-nouveau](https://packages.gentoo.org/packages/x11-drivers/xf86-video-nouveau)) or proprietary (closed source) drivers ([nvidia-drivers](https://wiki.gentoo.org/wiki/NVIDIA/nvidia-drivers), [x11-drivers/nvidia-drivers](https://packages.gentoo.org/packages/x11-drivers/nvidia-drivers)).

## Which driver to choose?

For best performance in heavy 3D workloads and games, the proprietary driver is usually preferred, especially on newer hardware. It is available only on **amd64** and **arm64**. For older/legacy GPUs, the open-source Nouveau driver may be a viable option as NVIDIA is actively phasing out support for these older cards in the proprietary driver.

| Chipset | Code name | Series | OpenGL | OpenCL | [VAAPI](https://wiki.gentoo.org/wiki/VAAPI)<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> / [VDPAU](https://wiki.gentoo.org/wiki/VDPAU) | [Vulkan](https://wiki.gentoo.org/wiki/Vulkan) | [CUDA compute capability](https://wiki.gentoo.org/index.php?title=CUDA_compute_capability&action=edit&redlink=1)<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup> | Latest [nvidia-drivers](https://wiki.gentoo.org/wiki/NVIDIA/nvidia-drivers) support<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup> | Open source kernel module support <sup>[\[4\]](https://wiki.gentoo.org#cite_note-4)</sup> | [nouveau](https://wiki.gentoo.org/wiki/Nouveau) support<sup>[\[5\]](https://wiki.gentoo.org#cite_note-5)</sup> | 
|---|---|---|---|---|---|---|---|---|---|---|
| NV04 | [Fahrenheit](<https://en.wikipedia.org/wiki/Fahrenheit_(microarchitecture)>) | Riva TNT, Riva TNT2 |  |  |  |  |  |  |  |  | 
| NV10 | [Celsius](<https://en.wikipedia.org/wiki/Celsius_(microarchitecture)>) | GeForce 256, GeForce 2, GeForce 4 MX |  |  |  |  |  |  |  |  | 
| NV20 | [Kelvin](<https://en.wikipedia.org/wiki/Kelvin_(microarchitecture)>) | GeForce 3, GeForce 4 Ti |  |  |  |  |  |  |  |  | 
| NV30 | [Rankine](<https://en.wikipedia.org/wiki/Rankine_(microarchitecture)>) | GeForce 5 / GeForce FX |  |  |  |  |  |  |  |  | 
| NV40 | [Curie](<https://en.wikipedia.org/wiki/Curie_(microarchitecture)>) | GeForce 6, GeForce 7 |  |  |  |  |  |  |  |  | 
| NV50 | [Tesla](<https://en.wikipedia.org/wiki/Tesla_(microarchitecture)>) | GeForce 8, GeForce 9, GeForce 100, GeForce 200, GeForce 300 |  |  |  |  |  |  |  |  | 
| NVC0 | [Fermi](<https://en.wikipedia.org/wiki/Fermi_(microarchitecture)>) | GeForce 400, GeForce 500 |  |  |  |  |  |  |  |  | 
| NVE0 | [Kepler](<https://en.wikipedia.org/wiki/Kepler_(microarchitecture)>) | GeForce 600, GeForce 700, GeForce GTX Titan |  |  |  |  |  |  |  |  | 
| NV110 | [Maxwell](<https://en.wikipedia.org/wiki/Maxwell_(microarchitecture)>) | GeForce 700, GeForce 900 |  |  |  |  |  | <sup>[\[6\]](https://wiki.gentoo.org#cite_note-maxwell-support-6)</sup> |  |  | 
| NV130 | [Pascal](<https://en.wikipedia.org/wiki/Pascal_(microarchitecture)>) | GeForce 10 series |  |  |  |  |  | <sup>[\[6\]](https://wiki.gentoo.org#cite_note-maxwell-support-6)</sup> |  |  | 
| NV140 | [Volta](<https://en.wikipedia.org/wiki/Volta_(microarchitecture)>) | Titan V |  |  |  |  |  | <sup>[\[6\]](https://wiki.gentoo.org#cite_note-maxwell-support-6)</sup> |  |  | 
| NV160 | [Turing](<https://en.wikipedia.org/wiki/Turing_(microarchitecture)>) | GeForce 16 series, GeForce 20 series |  |  |  |  |  |  |  |  | 
| NV170 | [Ampere](<https://en.wikipedia.org/wiki/Ampere_(microarchitecture)>) | GeForce 30 series |  |  |  |  |  |  |  |  | 
| AD10x | [Ada Lovelace](<https://en.wikipedia.org/wiki/Ada_Lovelace_(microarchitecture)>) | GeForce 40 series |  |  |  |  |  |  |  |  | 
| GB20x | [Blackwell](<https://en.wikipedia.org/wiki/Blackwell_(microarchitecture)>) | GeForce 50 series |  |  |  |  |  |  |  |  | 

A full list of NVIDIA GPU capabilities can be found [here](https://en.wikipedia.org/wiki/List_of_Nvidia_graphics_processing_units).

## Hybrid GPUs systems

Some systems allows seamlessly switches between two GPUs. It is typically used on systems that have an integrated [Intel](https://wiki.gentoo.org/wiki/Intel) GPU and a discrete NVIDIA GPU. The main benefit of using that technology is to extend battery life by providing maximum GPU performance only when needed.

See the [Hybrid graphics article](https://wiki.gentoo.org/wiki/Hybrid_graphics) for steps to enable this function.

## Available software and articles

- [NVIDIA/nvidia-drivers](https://wiki.gentoo.org/wiki/NVIDIA/nvidia-drivers) — The [x11-drivers/nvidia-drivers](https://packages.gentoo.org/packages/x11-drivers/nvidia-drivers) package contains the *proprietary* graphics driver for [NVIDIA] graphic cards.
- [Nouveau](https://wiki.gentoo.org/wiki/Nouveau) — an open source driver for [NVIDIA] graphic cards.
- [Hybrid graphics](https://wiki.gentoo.org/wiki/Hybrid_graphics) — details the system management of NVIDIA or AMD switchable graphics and Intel hybrid graphics.
- [Nouveau & nvidia-drivers switching](https://wiki.gentoo.org/wiki/Nouveau_%26_nvidia-drivers_switching) — describes how to switch between [NVIDIA's binary driver](https://wiki.gentoo.org/wiki/NVIDIA/nvidia-drivers) and the open source [nouveau](https://wiki.gentoo.org/wiki/Nouveau) driver.
