<!-- source: https://wiki.gentoo.org/wiki/LLVM/Polly | group: Gentoo Wiki (Main) | wiki-title: LLVM/Polly -->
---
title: LLVM/Polly
url: https://wiki.gentoo.org/wiki/LLVM/Polly
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-28"
fingerprint: d600735d5aabfbca
license: CC BY-SA 4.0
---

# LLVM/Polly

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Polly is a high-level loop and data-locality optimizer and optimization infrastructure for LLVM. It uses an abstract mathematical representation based on integer polyhedra to analyze and optimize the memory access pattern of a program. We currently perform classical loop transformations, especially tiling and loop fusion to improve data-locality. Polly can also exploit OpenMP level parallelism, expose SIMDization opportunities.

## Installation

### USE flags


| [+debug](https://packages.gentoo.org/useflags/+debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

### Emerge

First, emerge Polly:

`root #``emerge --ask llvm-core/polly`
Next, emerge [llvm-core/clang-runtime\[polly\]](https://packages.gentoo.org/packages/llvm-core/clang-runtime)

This installs a file similar to the following:

**`/etc/clang/20/gentoo-plugins.cfg`**

## Configuration

### package.env

**`/etc/portage/env/polly`**

```
COMMON_FLAGS="${COMMON_FLAGS} -mllvm -polly -mllvm -polly-vectorizer=stripmine -mllvm -polly-omp-backend=LLVM -mllvm -polly-parallel -mllvm -polly-num-threads=9 -mllvm -polly-scheduling=dynamic"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
FCFLAGS="${COMMON_FLAGS}"
FFLAGS="${COMMON_FLAGS}"
```
**`/etc/portage/package.env`**

**Example**
