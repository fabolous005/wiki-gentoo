<!-- source: https://wiki.gentoo.org/wiki/Embedded_Handbook/General/Introduction | group: Gentoo Wiki (Main) | wiki-title: Embedded Handbook/General/Introduction -->
---
title: Embedded Handbook/General/Introduction
url: https://wiki.gentoo.org/wiki/Embedded_Handbook/General/Introduction
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-05-26"
fingerprint: "2dc0fc7b56061760"
license: CC BY-SA 4.0
---

# Embedded Handbook/General/Introduction

[Embedded Handbook](https://wiki.gentoo.org/wiki/Special:MyLanguage/Embedded_Handbook) |

[General](https://wiki.gentoo.org/wiki/Special:MyLanguage/Embedded_Handbook/General)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



**outdated**. You can help the Gentoo community by verifying and

[updating this article](https://wiki.gentoo.org/index.php?title=Embedded_Handbook/General/Introduction&action=edit).



Cross development has traditionally been a black art, requiring a lot of research, trial and error, and perseverance. Intrepid developers face a shortage of documentation and the lack of mature, comprehensive open source toolkits for multi-platform cross development. Ongoing work by the [Embedded](https://wiki.gentoo.org/wiki/Project:Embedded) or [Toolchain](https://wiki.gentoo.org/wiki/Project:Toolchain) projects, and other contributors is yielding a Gentoo-based development platform that greatly simplifies cross development.

### The toolchain

The term "toolchain" refers to the collection of packages used to build up a system (the "tools" which are used in the "chain" of events to take some input and produce some output). It is a loose definition in terms of what packages exactly are considered part of the toolchain, but for the sake of keeping things simple, we will consider the components that are needed to compile code into something fun and usable.

Your typical toolchain is therefore composed of the following:

- [sys-devel/binutils](https://packages.gentoo.org/packages/sys-devel/binutils)
- Essential utilities for handling binaries (includes assembler and linker).
- [sys-devel/gcc](https://packages.gentoo.org/packages/sys-devel/gcc)
- The GNU Compiler Collection (the C and C++ compiler).
- [sys-libs/glibc](https://packages.gentoo.org/packages/sys-libs/glibc), [sys-libs/uclibc-ng](https://packages.gentoo.org/packages/sys-libs/uclibc-ng), or [sys-libs/newlib](https://packages.gentoo.org/packages/sys-libs/newlib)
- The system C library.
- [sys-kernel/linux-headers](https://packages.gentoo.org/packages/sys-kernel/linux-headers)
- Kernel headers needed by the system C library.
- [sys-devel/gdb](https://packages.gentoo.org/packages/sys-devel/gdb)
- The GNU debugger.

All proper Gentoo systems have a toolchain installed as part of the base system. This toolchain is configured to build binaries native to its host platform.

In order to build binaries on the host system for a non-native platform you'll need a special toolchain - a so-called cross toolchain - which can target that particular platform. Gentoo provides a simple but powerful tool called crossdev for this purpose. Crossdev can build and install arbitrary GCC-supported cross toolchains on the host system, and because Gentoo installs toolchain files into target-specific directories the toolchains built by crossdev will not interfere with the host's native toolchain.

### Toolchain tuples

All toolchains have a prefix (think `CHOST`). More details on that can be found in the [system tuples article](https://wiki.gentoo.org/wiki/Embedded_Handbook/Tuples).

### Environment variables

Certain environment variables used by the Gentoo toolchain and Portage can thoroughly confuse developers inexperienced with cross development. The following table explains some tricky variables and provides sample values based on the cross development examples presented in this guide. See *[More terminology and variables](https://wiki.gentoo.org#More_terminology_and_variables)* (below) for more unusual variables and related concepts.

| Variable name | Meaning when building cross-toolchain | Meaning when building cross-binaries | 
|---|---|---|
| `CBUILD` | Platform you are building on | Platform you are building on | 
| `CHOST` | Platform the cross-toolchain will run on | Platform the binaries built by cross-toolchain will run on | 
| `CTARGET` | Platform the binaries built by cross-toolchain will run on | Platform the binaries built by cross-toolchain will run on. Redundant, but there's no harm in setting this, and a few binaries do like it. | 
| `ROOT` | Path to the virtual root (/) you are installing into |  | 
| `PORTAGE_CONFIGROOT` | Path to the virtual root (/) Portage can find its config files (like /etc/make.conf) |  | 

Say we have an **AMD64** desktop as our normal Gentoo machine and we have an **ARM** PDA we wanted to develop for, the above table would look like:

| Variable name | Value for building cross-toolchain | Value for building cross-binaries | 
|---|---|---|
| `CBUILD` | `x86_64-pc-linux-gnu` | `x86_64-pc-linux-gnu` | 
| `CHOST` | `x86_64-pc-linux-gnu` | `arm-unknown-linux-gnu` | 
| `CTARGET` | `arm-unknown-linux-gnu` | Not set. | 
| `ROOT` | Not set - defaults to `/` | `/path/where/you/install` | 
| `PORTAGE_CONFIGROOT` | Not set - defaults to `/` | `/path/where/your/portage/env/for/arm/pda/is` | 

### More terminology and variables

- canadian cross
- The process of building a cross-compiler which will run on a different machine from the one it was compiled on (CBUILD != CHOST && CHOST != CTARGET)
- sysroot
- The system root is where all the cross-compiler libraries and headers are installed. In other words, every library and header the cross-compiler needs to generate cross-compiled binaries are put into this directory. In theory, this means the toolchain is good enough. In practice, the cross-compiler often wants some of a packages's library dependencies, such as ncurses, installed into sysroot first.
- hardfloat
- The system has a hardware Floating Point Unit (FPU) to handle floating point math
- softfloat
- The system lacks a hardware FPU so all floating point operations are approximated with fixed point math
- PIE
- Position Independent Executable (`-fPIE -pie`)
- PIC
- Position Independent Code (`-fPIC`)
- CRT
- C run time
