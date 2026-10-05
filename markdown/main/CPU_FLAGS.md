<!-- source: https://wiki.gentoo.org/wiki/CPU_FLAGS_* | group: Gentoo Wiki (Main) | wiki-title: CPU FLAGS * -->
---
title: CPU_FLAGS_*
url: https://wiki.gentoo.org/wiki/CPU_FLAGS_*
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-10-24"
fingerprint: a691d7536ab399d4
license: CC BY-SA 4.0
---

# CPU\_FLAGS\_\*

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



`**CPU_FLAGS_***` is a `[USE_EXPAND](https://wiki.gentoo.org/wiki//etc/portage/make.conf#USE_EXPAND)` variable containing instruction set and other CPU-specific features. Currently Gentoo supports `CPU_FLAGS_X86` (for **amd64** and **x86** architectures), `CPU_FLAGS_ARM` (for **arm** and **arm64** architectures), and `CPU_FLAGS_PPC` (for **ppc** and **ppc64** architectures).

A common question is "what's the difference between `CFLAGS` and e.g. `CPU_FLAGS_X86`?"

`CPU_FLAGS_*` is an example of a [USE\_EXPAND](https://wiki.gentoo.org/wiki/USE_EXPAND). It enables specific options in ebuilds which are passed onto the build system. For example, `CPU_FLAGS_X86_SSE2`, if defined for a package, will enable handwritten ASM. These options **enable specific code** which already exists within the package.

`CFLAGS`, on the other hand, are simply used to tell the compiler it is *allowed* to try to *generate* code using such instructions if it is able. It does not mean it will be successful in doing so. e.g. `-msse2` in `CFLAGS` does not mean the compiler will be clever enough to generate SSE2 for a certain function. These options **just permit the compiler to generate certain code with certain instructions**.

It is therefore important to configure `CPU_FLAGS_*` appropriately to get the best performance out of packages.

These variables need to be set as `CPU_FLAGS_X86` (`CPU_FLAGS_ARM`, `CPU_FLAGS_PPC`) variable in a file in [/etc/portage/package.use/](https://wiki.gentoo.org/wiki//etc/portage/package.use):

**`/etc/portage/package.use/my-cpu-flags`**

**Setting CPU\_FLAGS\_X86**

```
# This is just an example, please run the 'cpuid2cpuflags' tool to get an appropriate value for each system!
*/* cpu_flags_x86: aes avx avx2 f16c fma3 mmx mmxext pclmul popcnt rdrand sse sse2 sse3 sse4_1 sse4_2 ssse3
```
When in doubt, consult the flag descriptions using one of the commonly available tools, e.g. equery uses from [app-portage/gentoolkit](https://packages.gentoo.org/packages/app-portage/gentoolkit):

`user $``equery uses media-video/ffmpeg`
Most of the flag names match /proc/cpuinfo names, with the notable exception of `sse3` which is called `pni` in /proc/cpuinfo (please also do not confuse it with distinct `ssse3`).

[app-portage/cpuid2cpuflags](https://packages.gentoo.org/packages/app-portage/cpuid2cpuflags) helps users determine the correct [CPU\_FLAGS\_](https://packages.gentoo.org/useflags/search?q=cpu_flags_)`USE_EXPAND` variables for their CPU architecture.

`root #``emerge --ask app-portage/cpuid2cpuflags``user $``cpuid2cpuflags`
CPU\_FLAGS\_X86: mmx mmxext sse sse2 sse3

Example to apply globally:

`root #``echo "*/* $(cpuid2cpuflags)" > /etc/portage/package.use/00cpu-flags`
- News item: [new CPU\_FLAGS\_PPC USE\_EXPAND](https://gentoo.org/support/news-items/2019-09-11-cpu_flags_ppc-introduction.html)
