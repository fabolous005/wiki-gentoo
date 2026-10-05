<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2014/Ideas/Embedded_Gentoo_using_musl | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2014/Ideas/Embedded Gentoo using musl -->
---
title: Google Summer of Code/2014/Ideas/Embedded Gentoo using musl
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2014/Ideas/Embedded_Gentoo_using_musl
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-03-29"
fingerprint: "14aaa7c80dbf33bb"
license: CC BY-SA 4.0
---

# Google Summer of Code/2014/Ideas/Embedded Gentoo using musl

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This project involves integrating an alternative standard C library called musl into Gentoo, both for cross compiling and native compiling.  Musl aims to be "lightweight, fast, simple, free, and strives to be correct in the sense of standards-conformance and safety." [\[1\]](http://www.musl-libc.org)  So far, stage4 tarballs have been built for amd64 [\[2\]](http://distfiles.gentoo.org/experimental/amd64/musl), i686 [\[3\]](http://distfiles.gentoo.org/experimental/x86/musl) and armv7a-hardfloat-eabi [\[4\]](http://distfiles.gentoo.org/experimental/arm/musl). These were initially built using cross compiling toolchians which themselves were built using crossdev [\[5\]](http://www.gentoo.org/proj/en/base/embedded/handbook/cross-compiler.xml?style=printable), but then were rebuilt on native hardware using home grown scripts [\[6\]](http://git.overlays.gentoo.org/gitweb/?p=proj/releng.git;a=tree;f=tools-musl), and not catalyst [\[7\]](https://www.gentoo.org/proj/en/releng/catalyst). Picking up from here, the next steps in the project are:

1. To build cross compiling toolschains for mips32r2-o32, mipsel3-o32 and mips64r2-n32 architectures/abis. This will involve properly intergrating musl with crossdev which currently has some bugs [\[8\]](https://bugs.gentoo.org/show_bug.cgi?id=498114).
2. To build stage 4 tarballs for the those architectures.  This involves, among other things, patching various packages that don't conform to strict POSIX which musl assumes [\[9\]](http://git.overlays.gentoo.org/gitweb/?p=proj/hardened-dev.git;a=shortlog;h=refs/heads/musl).
3. These stages need to be converted to stage3's, properly built using catalyst rather than using the ROOT=rootfs emerge -e @system technique which the homegrown scripts use.
4. If all goes well, the project can continue to porting over the hardened tool chain to those architectures [Project:Hardened](https://wiki.gentoo.org/wiki/Project:Hardened).



| Contacts | Required Skills | 
|---|---|
|  |  |
