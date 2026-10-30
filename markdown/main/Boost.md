<!-- source: https://wiki.gentoo.org/wiki/Boost | group: Gentoo Wiki (Main) | wiki-title: Boost -->
---
title: Boost
url: https://wiki.gentoo.org/wiki/Boost
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-29"
fingerprint: a680145c7dc478c9
license: CC BY-SA 4.0
---

# Boost

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

**Boost** is a set of [C++](https://wiki.gentoo.org/wiki/C%2B%2B) libraries for various tasks.

## Installation

### USE flags


### USE flags for
            [dev-libs/boost](https://packages.gentoo.org/packages/dev-libs/boost)
            
            Boost Libraries for C++

| [+context](https://packages.gentoo.org/useflags/+context) | Build and install the Boost.Context (and Boost.Fiber) library and all other Boost libraries that depend on it | 
| [+nls](https://packages.gentoo.org/useflags/+nls) | Build libboost\_locale. This library requires compatible C library interfaces, which might not be provided by uClibc or other embedded libraries. | 
| [+stacktrace](https://packages.gentoo.org/useflags/+stacktrace) | Build the full Boost.Stacktrace dynamic library instead of relying on the header-only implementation | 
| [bzip2](https://packages.gentoo.org/useflags/bzip2) | Enable bzip2 compression support | 
| [debug](https://packages.gentoo.org/useflags/debug) | Build and install only the debug version of the Boost libraries. Only enable this flag if you're developing against boost. | 
| [doc](https://packages.gentoo.org/useflags/doc) | Install the full API documentation documentation. This takes over 200MB of extra disk space. | 
| [icu](https://packages.gentoo.org/useflags/icu) | Enable ICU (Internationalization Components for Unicode) support, using dev-libs/icu | 
| [lzma](https://packages.gentoo.org/useflags/lzma) | Support for LZMA compression algorithm | 
| [mpi](https://packages.gentoo.org/useflags/mpi) | Add MPI (Message Passing Interface) layer to the apps that support it | 
| [numpy](https://packages.gentoo.org/useflags/numpy) | Optionally build NumPy extensions when Boost.Python is enabled | 
| [python](https://packages.gentoo.org/useflags/python) | Add optional support/bindings for the Python language | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [test-full](https://packages.gentoo.org/useflags/test-full) | Enable tests that take a very long time to build or run | 
| [tools](https://packages.gentoo.org/useflags/tools) | Build and install the boost tools (bcp, quickbook, inspect, wave) | 
| [zlib](https://packages.gentoo.org/useflags/zlib) | Add support for zlib compression | 
| [zstd](https://packages.gentoo.org/useflags/zstd) | Enable support for ZSTD compression | 

### Emerge

`root #``emerge --ask dev-libs/boost`
## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose dev-libs/boost`
