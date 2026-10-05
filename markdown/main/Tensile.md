<!-- source: https://wiki.gentoo.org/wiki/Tensile | group: Gentoo Wiki (Main) | wiki-title: Tensile -->
---
title: Tensile
url: https://wiki.gentoo.org/wiki/Tensile
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-02-11"
fingerprint: "414e46721e082ef5"
license: CC BY-SA 4.0
---

# Tensile

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Tensile**, as a part of [ROCm](https://wiki.gentoo.org/wiki/ROCm) stack, is a development toolkit for tuning GEMM operation on GPUs via benchmarks, and then create backend libraries for GEMM applications (rocBLAS).

## Installation

[dev-util/Tensile](https://packages.gentoo.org/packages/dev-util/Tensile) installs the python scripts for running benchmarks, analyzing data, and building backend libraries. It also ships various common benchmark configurations and shell scripts, as well as C++ source code for building tensile\_client.

### Emerge

Install [dev-util/Tensile](https://packages.gentoo.org/packages/dev-util/Tensile):

`root #``emerge --ask dev-util/Tensile`
### Usage

#### Running benchmarks to stretch GPU GEMM performance

Please refer to the [official Tensile wiki](https://github.com/ROCmSoftwarePlatform/Tensile/wiki) about how to write benchmark configurations and run benchmarks. Since Gentoo already installs the command `Tensile`, so extra installation is not needed, just execute

`user $``Tensile [-v] --global-parameters=Architecture=<your GPU arch> <benchmark_config.ymal> <benchmark_directory>`
