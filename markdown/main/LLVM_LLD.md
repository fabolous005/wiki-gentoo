<!-- source: https://wiki.gentoo.org/wiki/LLVM/LLD | group: Gentoo Wiki (Main) | wiki-title: LLVM/LLD -->
---
title: LLVM/LLD
url: https://wiki.gentoo.org/wiki/LLVM/LLD
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-04-14"
fingerprint: de407a1e55c65cce
license: CC BY-SA 4.0
---

# LLVM/LLD

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**LLD** is a linker provided by the [LLVM](https://wiki.gentoo.org/wiki/LLVM) project and can be used as an alternative to the standard linker, BFD. It supports more modern features than BFD and tightly integrates with [Clang](https://wiki.gentoo.org/wiki/LLVM/Clang) and the LLVM toolchain, especially when [link-time optimizing](https://wiki.gentoo.org/wiki/LTO) a system.

## Installation

### USE flags


| [+debug](https://packages.gentoo.org/useflags/+debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 
| [zstd](https://packages.gentoo.org/useflags/zstd) | Enable support for ZSTD compression | 

### Emerge

`root #``emerge --ask llvm-core/lld`
