<!-- source: https://wiki.gentoo.org/wiki/Vulkan | group: Gentoo Wiki (Main) | wiki-title: Vulkan -->
---
title: Vulkan
url: https://wiki.gentoo.org/wiki/Vulkan
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-29"
fingerprint: "13f4b7e5f9b352d"
license: CC BY-SA 4.0
---

# Vulkan

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Vulkan** is a next-generation graphics API created by The Khronos Group. It's designed to be used across a variety of platforms, from desktop to mobile computers.

Compared to [OpenGL](https://wiki.gentoo.org/wiki/OpenGL), Vulkan is a much lower-level API and enables developers to squeeze more performance out of a video card.

## Installation

### Prerequisites

#### ICDs

To use Vulkan in any useful capacity, at least one ICD (Installable Client Driver) is required. ICDs may already be installed on the system, depending on the value(s) assigned to the [VIDEO\_CARDS](https://wiki.gentoo.org/wiki/Make.conf#VIDEO_CARDS) variable. The current Vulkan support table is provided below:

| Driver name | `VIDEO_CARDS` value | Package | Supported | 
|---|---|---|---|
| RADV | `amdgpu` | [media-libs/mesa](https://packages.gentoo.org/packages/media-libs/mesa) | <sup>1</sup> | 
| [AMDVLK](https://wiki.gentoo.org/wiki/AMDVLK) | `amdgpu` | [media-libs/amdvlk](https://packages.gentoo.org/packages/media-libs/amdvlk) or [media-libs/amdvlk-bin](https://packages.gentoo.org/packages/media-libs/amdvlk-bin) from [GURU repository](https://wiki.gentoo.org/wiki/Project:GURU) | <sup>5</sup> | 
| radeon/r600 | N/A | N/A |  | 
| RADV | `radeonsi` | [media-libs/mesa](https://packages.gentoo.org/packages/media-libs/mesa) | <sup>1</sup> | 
| ANV | `intel` | [media-libs/mesa](https://packages.gentoo.org/packages/media-libs/mesa) | <sup>2</sup> | 
| NVK | `nouveau nvk` | [x11-drivers/xf86-video-nouveau](https://packages.gentoo.org/packages/x11-drivers/xf86-video-nouveau) [media-libs/mesa](https://packages.gentoo.org/packages/media-libs/mesa) | <sup>4</sup> | 
| NVIDIA | `nvidia` | [x11-drivers/nvidia-drivers](https://packages.gentoo.org/packages/x11-drivers/nvidia-drivers) | <sup>3</sup> | 

- <sup>1</sup> Uses the *RADV* Vulkan driver included in Mesa.
- <sup>2</sup> Uses the *ANV* Vulkan driver included in Mesa. Partial support begins on Ivy Bridge and up, more information can be found on [intel](https://wiki.gentoo.org/wiki/Intel#Feature_support).
- <sup>3</sup> Uses the *NVIDIA* Vulkan driver included in [NVIDIA/nvidia-drivers](https://wiki.gentoo.org/wiki/NVIDIA/nvidia-drivers).
- <sup>4</sup> Currently supports Maxwell (some GTX 700 and 800 series, most 900 series) and later GPUs up to and including Ada, requires at least a Linux 6.6 kernel.<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>
- <sup>5</sup> Uses the [AMDVLK](https://wiki.gentoo.org/wiki/AMDVLK) aka *AMD Open Source Driver for Vulkan* and maintained in GURU. Therefore it has not been placed in `::gentoo` yet.

It may be useful to check the [Vulkan Hardware Database](https://vulkan.gpuinfo.org/) which has detailed GPU hardware capabilities for Vulkan-capable GPUs.

#### Mesa

To enable the Vulkan drivers in [media-libs/mesa](https://packages.gentoo.org/packages/media-libs/mesa), set the [vulkan](https://packages.gentoo.org/useflags/vulkan) [USE](https://wiki.gentoo.org/wiki/USE) flag.

### Loader

Applications don't interact with these ICDs directly, but use the [Vulkan Loader](https://github.com/KhronosGroup/Vulkan-Loader#introduction) provided by the package [media-libs/vulkan-loader](https://packages.gentoo.org/packages/media-libs/vulkan-loader). This loader picks the correct ICD for the application and handles inserting Vulkan layers.

Any packages that are built with Vulkan support will pull in [media-libs/vulkan-loader](https://packages.gentoo.org/packages/media-libs/vulkan-loader), but it can also be installed manually:

`root #``emerge --ask media-libs/vulkan-loader`

### Development

To use Vulkan Validation layers, set the [layers](https://packages.gentoo.org/useflags/layers) [USE flag on](https://wiki.gentoo.org/wiki/USE_flag) [media-libs/vulkan-loader](https://packages.gentoo.org/packages/media-libs/vulkan-loader). This will pull in [media-libs/vulkan-layers](https://packages.gentoo.org/packages/media-libs/vulkan-layers) automatically.

To use vulkaninfo or vkcube for verifying if Vulkan works, install [dev-util/vulkan-tools](https://packages.gentoo.org/packages/dev-util/vulkan-tools):

`root #``emerge --ask dev-util/vulkan-tools`
This is not related with [VulkanTools](https://github.com/LunarG/VulkanTools), part of the LunarG Vulkan SDK and currently not packaged on Gentoo.

## Usage

Support for Vulkan in Gentoo packages can be controlled by setting the [vulkan](https://packages.gentoo.org/useflags/vulkan) [USE flag. For example,](https://wiki.gentoo.org/wiki/USE_flag) [media-libs/libsdl2](https://packages.gentoo.org/packages/media-libs/libsdl2) can optionally enable Vulkan support if set.

## Troubleshooting

### Wrong ELF class

This error that may appear when running vulkaninfo diagnostic tool from [dev-util/vulkan-tools](https://packages.gentoo.org/packages/dev-util/vulkan-tools) and used for Vulkan debugging.

ERROR: \[Loader Message\] Code 0 : /usr/lib32/libvulkan\_intel.so: wrong ELF class: ELFCLASS32
 ERROR: \[Loader Message\] Code 0 : /usr/lib32/libvulkan\_radeon.so: wrong ELF class: ELFCLASS32

This error can be ignored as both 32-bit and 64-bit drivers are attempted to be loaded on a multilib system.

For more information please see [Vulkan-Loader's issue #108](https://github.com/KhronosGroup/Vulkan-Loader/issues/108).

## See also

- [Xorg/Hardware 3D acceleration guide](https://wiki.gentoo.org/wiki/Xorg/Hardware_3D_acceleration_guide) — a guide to getting 3D acceleration working using the DRM with Xorg in Gentoo.
- [OpenGL](https://wiki.gentoo.org/wiki/OpenGL) — a graphics API created by The Khronos Group.
- [OpenCL](https://wiki.gentoo.org/wiki/OpenCL) — a framework for writing programs that execute across heterogeneous computing platforms (CPUs, GPUs, DSPs, FPGAs, ASICs, etc.).
- [NVIDIA/nvidia-drivers](https://wiki.gentoo.org/wiki/NVIDIA/nvidia-drivers) — The [x11-drivers/nvidia-drivers](https://packages.gentoo.org/packages/x11-drivers/nvidia-drivers) package contains the *proprietary* graphics driver for [NVIDIA](https://wiki.gentoo.org/wiki/NVIDIA) graphic cards.
- [AMDVLK](https://wiki.gentoo.org/wiki/AMDVLK) — an open-source Vulkan driver for AMD Radeon™ graphics adapters on Linux
