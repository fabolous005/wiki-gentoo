<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2015/Ideas/Embedded_Gentoo_using_musl | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2015/Ideas/Embedded Gentoo using musl -->
---
title: Google Summer of Code/2015/Ideas/Embedded Gentoo using musl
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2015/Ideas/Embedded_Gentoo_using_musl
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-11-09"
fingerprint: "14aaa7c80dbf33bb"
license: CC BY-SA 4.0
---

# Google Summer of Code/2015/Ideas/Embedded Gentoo using musl

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This project involves integrating an alternative standard C library called musl into Gentoo, both for cross compiling and native compiling. [Musl](http://www.musl-libc.org) aims to be "lightweight, fast, simple, free, and strives to be correct in the sense of standards-conformance and safety. So far, stage4 tarballs have been built for [amd64](http://distfiles.gentoo.org/experimental/amd64/musl), [i686](http://distfiles.gentoo.org/experimental/x86/musl) and [armv7a-hardfloat-eabi](http://distfiles.gentoo.org/experimental/arm/musl). These were initially built using cross compiling toolchians which themselves were built using [crossdev](http://www.gentoo.org/proj/en/base/embedded/handbook/cross-compiler.xml), but then were rebuilt on native hardware using home grown scripts, and not [catalyst](https://www.gentoo.org/proj/en/releng/catalyst). Picking up from here, the next steps in the project are:

- To build cross compiling toolschains for mips32r2-o32, mipsel3-o32 and mips64r2-n32 architectures/abis. This will involve properly intergrating musl with crossdev which currently has some bugs.
- To build stage 4 tarballs for the those architectures. This involves, among other things, patching various packages that don't conform to strict POSIX which musl assumes.
- These stages need to be converted to stage3's, properly built using catalyst rather than using the technique which the homegrown scripts use. ROOT=rootfs emerge -e @system
- If all goes well, the project can continue to porting over the [hardened tool chain](https://wiki.gentoo.org/wiki/Project:Hardened) to those architectures.



| Contacts | Required Skills | 
|---|---|
|  |  |
