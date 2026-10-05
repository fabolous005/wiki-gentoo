<!-- source: https://wiki.gentoo.org/wiki/Ideas/Clang_system_compiler_support | group: Gentoo Wiki (Main) | wiki-title: Ideas/Clang system compiler support -->
---
title: Ideas/Clang system compiler support
url: https://wiki.gentoo.org/wiki/Ideas/Clang_system_compiler_support
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2012-09-21"
fingerprint: cdb5fb68f10bb34c
license: CC BY-SA 4.0
---

# Ideas/Clang system compiler support

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

LLVM/Clang is a new compiler that was written in C++ with a strong emphasis on code correctness and extensibility. Its static analyzer is excellent at detecting incorrect code and this enables us to improve the quality of Gentoo as a whole.

Supporting LLVM/Clang as a System Compiler in Gentoo requires fixing all packages in @system that fail to build with it. It also requires that bootloaders and kernel sources are available that can be built with it. Once these hurdles are resolved, it will be possible to provide a switch so that users can select it as the system compiler in place of GCC.

## Development team

Owner: ryao \<ryao at gentoo.org>

Team:

- ?

## Current Status

Last updated: 06082012

Target date (approximate): 24122012

Percentage of completion: ?%

Bug: [bug #408963](https://bugs.gentoo.org/show_bug.cgi?id=408963)

## Description

See [summary](https://wiki.gentoo.org#top)

## Dependencies

None

## Contingency plan

None needed. Packages continue to be tested mostly with GCC

## Documentation

Unknown
