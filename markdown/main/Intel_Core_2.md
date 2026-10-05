<!-- source: https://wiki.gentoo.org/wiki/Intel_Core_2 | group: Gentoo Wiki (Main) | wiki-title: Intel Core 2 -->
---
title: Intel Core 2
url: https://wiki.gentoo.org/wiki/Intel_Core_2
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-20"
fingerprint: "264dc331dd6a71a3"
license: CC BY-SA 4.0
---

# Intel Core 2

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes the setup of an Intel Core 2 Solo/Duo/Quad processor.

The Intel Core 2 line of processors supports both the 32-bit and 64-bit mode. The general pros and cons can be read about in the [Wikipedia article](https://en.wikipedia.org/wiki/64-bit_computing#32-bit_vs_64-bit). By now only very few applications do not support 64-bit mode or work in a 32-bit compatibility mode.

## Installation

### BIOS

If you have a dual-core or a quad-core Intel Core 2 CPU, you should first check if all cores are enabled:

`user $``cat /proc/cpuinfo`
If not, enable options like *APIC*, *MultiCore*, etc. in the [BIOS](https://wiki.gentoo.org/wiki/BIOS).

### Kernel

Activate the following kernel options.

For a single-core processor:

For a dual-core or a quad-core processor:

### Software

**`/etc/portage/package.use/00cpu-flags`**

```
CPU_FLAGS_X86="mmx sse sse2 sse3 ssse3"
```
**`/etc/portage/package.use/00cpu-flags`**

**Penryn and newer**

```
CPU_FLAGS_X86="mmx sse sse2 sse3 ssse3 sse4 sse4a sse4_1"
```
```
# Note: -fomit-frame-pointer only makes a difference on x86 since it is included in -O2 on amd64
CFLAGS="-march=native -O2 -pipe -fomit-frame-pointer"
CXXFLAGS="${CFLAGS}"
```
```
MAKEOPTS="-j1"
```
```
MAKEOPTS="-j2"
```
```
MAKEOPTS="-j3"
```
### Temperature sensor

See the [lm\_sensors](https://wiki.gentoo.org/wiki/Lm_sensors) article and activate the kernel driver **coretemp**.

### Virtualization

Most of these processors support Intel VT. Exceptions include:

- Merom \<T5600
- Allendale E4xxx

See the [Virtualization category](https://wiki.gentoo.org/wiki/Category:Virtualization).

## See also

- [Intel microcode](https://wiki.gentoo.org/wiki/Intel_microcode) — describes the process of updating the [microcode](https://wiki.gentoo.org/wiki/Microcode) on Intel processors.
- [Power management/Processor](https://wiki.gentoo.org/wiki/Power_management/Processor) — describes the setup of [power management](https://wiki.gentoo.org/wiki/Power_management) for processors.
- [CPU\_FLAGS\_\*](https://wiki.gentoo.org/wiki/CPU_FLAGS_*) — a `[USE_EXPAND](https://wiki.gentoo.org/wiki//etc/portage/make.conf#USE_EXPAND)` variable containing instruction set and other CPU-specific features.
