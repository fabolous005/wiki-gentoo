<!-- source: https://wiki.gentoo.org/wiki/OneAPI | group: Gentoo Wiki (Main) | wiki-title: OneAPI -->
---
title: OneAPI
url: https://wiki.gentoo.org/wiki/OneAPI
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-03"
fingerprint: aa4c65ee09af50de
license: CC BY-SA 4.0
---

# OneAPI

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**OneAPI** is an open standard API for using coprocessors like GPU, AI accelerators and FPGA

# OneAPI support

Install the driver of your [intel](https://wiki.gentoo.org/wiki/Intel) GPU

To enable graphics card rendering with Intel graphics cards with OneAPI, we needs some libraries dependencies:

See \[[documentation](https://docs.blender.org/manual/fr/dev/render/cycles/gpu_rendering.html#oneapi-intel)\] of Blender

## Installation

### dev-libs/intel-compute-runtime

`root #``emerge --ask --verbose dev-libs/intel-compute-runtime`
### dev-libs/level-zero

`root #``emerge --ask --verbose dev-libs/level-zero`
### dev-util/intel-graphics-compiler

Add **vc** use flag for **intel-graphics-compiler**:

`root #```echo `dev-util/intel-graphics-compiler vc` > /etc/portage/package.use/intel```root #``emerge --ask --verbose dev-util/intel-graphics-compiler`
#### Compilation with llvm\_slot\_17

Recently, **dev-util/intel-graphics-compiler** and **dev-libs/intel-vc-intrinsics** uses the use flag llvm\_slot\_17.

#### Compilation with llvm\_slot\_22

**dev-util/intel-graphics-compiler** unmasked version (2.41.9-r1) use llvm\_slot\_22

unmask some packages:

**`/etc/portage/package.accept_keywords/intel`**

```
 ~amd64 
dev-util/intel-graphics-compiler ~amd64
media-libs/gmmlib ~amd64
dev-util/spirv-tools ~amd64
dev-util/spirv-headers ~amd64
dev-util/glslang ~amd64
dev-util/intel-graphics-system-controller ~amd64
```
**dev-libs/intel-vc-intrinsics** work with with llvm\_slot\_22 use flag

`root #```echo `dev-libs/intel-vc-intrinsics llvm_slot_22` >> /etc/portage/package.use/intel``
## Use OneAPI for blender

If you have some issues, perhaps you need this package **intel-level-zero-gpu-raytracing**

Install this ebuild at the version 1.3.0  of **dev-libs/level-zero-gpu-raytracing** from on this Gentoo overlay \[[repos](https://codeberg.org/lastrodamo/gentoo-overlay/src/branch/main/dev-libs/level-zero-gpu-raytracing)\].

See how to install a [custom repository](https://wiki.gentoo.org/wiki/Handbook:AMD64/Portage/CustomTree#Creating_a_custom_ebuild_repository)

`root #``emerge --ask --verbose dev-libs/level-zero-gpu-raytracing`
