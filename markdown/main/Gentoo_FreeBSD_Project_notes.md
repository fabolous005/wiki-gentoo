<!-- source: https://wiki.gentoo.org/wiki/Gentoo_FreeBSD/Project_notes | group: Gentoo Wiki (Main) | wiki-title: Gentoo FreeBSD/Project notes -->
---
title: Gentoo FreeBSD/Project notes
url: https://wiki.gentoo.org/wiki/Gentoo_FreeBSD/Project_notes
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2017-07-16"
fingerprint: "56acbcd95ec60fe1"
license: CC BY-SA 4.0
---

# Gentoo FreeBSD/Project notes

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Deprecated article**

This article is **deprecated (obsolete)**. Contents are <u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

Gentoo FreeBSD project (or Gentoo/FBSD, or G/FBSD) was an effort to create a complete FreeBSD-based Gentoo system, sharing the complete administration facilities of Gentoo with the reliability of the FreeBSD kernel and userland.

An experimental, yet incomplete releases were created in the past, and it may be still possible to install Gentoo FreeBSD following the [install guide](https://wiki.gentoo.org/wiki/Gentoo_FreeBSD/Install_Guide).

## Mini FAQ

- Is Gentoo FreeBSD just a Portage on top of FreeBSD system?
- No, Gentoo FreeBSD aims to provide a complete FreeBSD-based system using Gentoo design principles. This means that it's going to use the Gentoo init system, administration utilities and toolchain support.

- Which toolchain is being used to build the system?
- Gentoo toolchain is being used. This means that GCC can be chosen between the currently available versions in portage (3.3, 3.4, 4.0 and 4.1), and the same goes with binutils, for which the needed patch form FreeBSD has already been forwardported (and applied upstream for 2.17 series).

- Is Stack Smashing Protection supported?
- Yes, as we use Gentoo toolchain we also have GCC patched with Propolice, and since 2 May 2006 the base system is patched to support SSP by default. The implementation is not yet perfect as there are things that needs to be cleaned up, but it works.

## Building a Gentoo FreeBSD cross-compiler

### Introduction

Complementing the DistCC Cross-compiling Guide, this document will explain how to build a Gentoo FreeBSD cross-compiler, as for now it's not yet entirely automated.

The instruction here given works for building cross-compilers to Gentoo FreeBSD, as they use the ebuilds currently in portage. They might not work entirely for generic vanilla FreeBSD as there are a few things that changes (like SSP support) but you should be okay if you don't enable them.

Please if you find a problem with these when building a generic FreeBSD cross compiler don't open a bug report unless you also have a patch, as they are not enough people working on Gentoo FreeBSD to support vanilla FreeBSD building.

### Prerequisites and crossdev

Before using crossdev you need to install some prerequisites that are needed to complete the building of a stage4 compiler (C++ compiler). These are needed because crossdev does not merge them before the rest.

The first package you need is [virtual/pmake](https://packages.gentoo.org/packages/virtual/pmake), or directly [sys-devel/pmake](https://packages.gentoo.org/packages/sys-devel/pmake), that provides a BSD-compatible make command, needed to build headers and the base library for Gentoo FreeBSD.

The second package is a complement to the above pmake. Although it ships with some .mk files (that contain BSD makefile templates used to build applications using BSD style), they are too bare bone to actually build FreeBSD software, so you need to install the file set from FreeBSD. The ebuild is [sys-freebsd/freebsd-mk-defs](https://packages.gentoo.org/packages/sys-freebsd/freebsd-mk-defs) and it is cross-compile aware: when building on a non-FreeBSD system it installs in /usr/share/mk/freebsd and uses GNU syntax.

The third package is sys-apps/mtree, that provides the BSD mtree command, needed to build Gentoo FreeBSD headers.

Then you just need to use [sys-devel/crossdev](https://packages.gentoo.org/packages/sys-devel/crossdev) to build the desired cross compiler. Please note that the version specified in `CHOST` has to be the same as the one used for freebsd-lib:

`root #``crossdev -s4 --gcc 3.4.6-r1 --libc '6.1*' --target i686-gentoo-freebsd6.1 --with-headers`
The version of GCC should always be the last for the 3.4.x series, GCC 4 is untested, the version of libc has to be accorded to the version of FreeBSD you want to build for, while the `--with-headers` option to crossdev forces it to build the headers before CGG itself.
