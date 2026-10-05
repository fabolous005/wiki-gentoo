<!-- source: https://wiki.gentoo.org/wiki/AOCC | group: Gentoo Wiki (Main) | wiki-title: AOCC -->
---
title: AOCC
url: https://wiki.gentoo.org/wiki/AOCC
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-04-13"
fingerprint: "3cc5f05a2c9f9d88"
license: CC BY-SA 4.0
---

# AOCC

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**AOCC** ([AMD](https://wiki.gentoo.org/wiki/AMD) Optimizing C/C++ Compiler) compiler system is a high performance, production quality code generation tool.

The AOCC environment provides various options to developers when building and optimizing C, C++, and Fortran applications targeting 32-bit and 64-bit Linux® platforms. The AOCC compiler system offers a high level of advanced optimizations, multi-threading and processor support that includes global optimization, vectorization, inter-procedural analyses, loop transformations, and code generation. AMD also provides highly optimized libraries, which extract the optimal performance from each x86 processor core when utilized. The AOCC Compiler Suite simplifies and accelerates development and tuning for x86 applications.

## Installation

First [download the latest version from AOCC's homepage](https://developer.amd.com/amd-aocc/). Read the EULA carefully before accepting it. Choose a location to **run it from**,

`root #````
cd /opt
```
`root #``tar -xvf /tmp/aocc-compiler-2.3.0.tar`
Optional: Make a symlink for future upgrades:

`root #``ln -s aocc-compiler-2.3.0/ aocc`
Unarchived media has a script to check for all prerequisities:

`root #````
cd aocc/
```
`root #``sh AOCC-prerequisites-check.sh`
## Usage

You should try to match the symlinks of AOCC to [Clang](https://wiki.gentoo.org/wiki/LLVM/Clang)'s.

`root #````
cd /opt/aocc/bin
```
`root #````
ln -s clang x86_64-pc-linux-gnu-clang
```
`root #````
ln -s clang++ x86_64-pc-linux-gnu-clang++
```
`root #````
ln -s clang++-11 x86_64-pc-linux-gnu-clang++-11
```
`root #````
ln -s clang-11 x86_64-pc-linux-gnu-clang-11
```
`root #````
ln -s clang-cl-11 x86_64-pc-linux-gnu-clang-cl
```
`root #````
ln -s clang-cl-11 x86_64-pc-linux-gnu-clang-cl-11
```
`root #````
ln -s clang-cpp-11 x86_64-pc-linux-gnu-clang-cpp
```
`root #````
ln -s clang-cpp-11 x86_64-pc-linux-gnu-clang-cpp-11
```
### With "AOCC\_PATH" in make.conf / package.env

Please see [/etc/portage/package.env](https://wiki.gentoo.org/wiki//etc/portage/package.env) to see how package.env works.

**`/etc/portage/env/aocc.conf`**

```
AOCC_PATH="/opt/aocc/bin/"
CC="${AOCC_PATH}clang"
CXX="${AOCC_PATH}clang++"
LDFLAGS="-fuse-ld=lld"
```
Add LLVM AR, NM and RANLIB if needed:

**`/etc/portage/env/aocc.conf`**

```
AOCC_PATH="/opt/aocc/bin/"
CC="${AOCC_PATH}clang"
CXX="${AOCC_PATH}clang++"
AR="${AOCC_PATH}llvm-ar"
NM="${AOCC_PATH}llvm-nm"
RANLIB="${AOCC_PATH}llvm-ranlib"
LDFLAGS="-fuse-ld=lld -rtlib=compiler-rt -unwindlib=libunwind"
```
### Via /etc/env.d

`root #``cd /etc/env.d`
**`/etc/env.d/50aocc`**

```
PATH="/opt/aocc/bin"
# we need to duplicate it in ROOTPATH for Portage to respect...
ROOTPATH="/opt/aocc/bin"
MANPATH="/opt/aocc/bin/share/man"
LDPATH="/opt/aocc/lib/:/opt/aocc/lib/lib32:/opt/aocc/lib/lib64"
```
`root #``env-update`
Test that it works:

`user $``clang -v`
### package.env

Please see [/etc/portage/package.env](https://wiki.gentoo.org/wiki//etc/portage/package.env) to see how package.env works.

`root #``cd /etc/portage/env`
**`/etc/portage/env/aocc.conf`**

```
CC="clang"
CXX="clang++"
LDFLAGS="-fuse-ld=lld"
```
Add LLVM AR, NM and RANLIB if needed:

**`/etc/portage/env/aocc.conf`**

```
CC="clang"
CXX="clang++"
AR="llvm-ar"
NM="llvm-nm"
RANLIB="llvm-ranlib"
LDFLAGS="-fuse-ld=lld"
```
If you don't wish to replace vanilla-clang via /etc/env.d, AOCC can be used per-package with:

**`/etc/portage/env/aocc.conf`**

```
AOCC_PATH="/opt/aocc/bin/"
CC="${AOCC_PATH}clang"
CXX="${AOCC_PATH}clang++"
```
### make.conf

AOCC can be used globally by defining it in make.conf:

**`/etc/portage/make.conf`**

```
CC="clang"                                                                                          
CXX="clang++"
LDFLAGS="-fuse-ld=lld"
```
Please see [creating a GCC fallback environment](https://wiki.gentoo.org/wiki/LLVM/Clang#GCC_fallback_environment).

### llvm.eclass

## Switching to AOCC

Please read the [#Installation](https://wiki.gentoo.org#Installation) part above. Continue from there, without choosing method of usage first.

Add following USE flags to your /etc/portage/make.conf

**`/etc/portage/make.conf`**

```
USE="clang compiler-rt default-compiler-rt default-libcxx libcxx libcxxabi libunwind"
```
Compile LLVM/clang toolchain with your (working) system default compiler first:

`root #``emerge -1avt clang llvm libcxx libcxxabi compiler-rt libunwind lld`
After it is done, switch to AOCC:

**`/etc/portage/make.conf`**

```
AOCC_PATH="/opt/aocc-compiler-2.3.0/bin/"
CC="${AOCC_PATH}clang"
CXX="${AOCC_PATH}clang++"
BUILD_CC="${CC}"
BUILD_CXX="${CXX}"
CFLAGS="-march=native -O2 -pipe"
CXXFLAGS="-stdlib=libc++ ${CFLAGS}"
LDFLAGS="-fuse-ld=lld -rtlib=compiler-rt -unwindlib=libunwind"
```
**`/etc/portage/make.conf`**

```
AR="${AOCC_PATH}llvm-ar"
NM="${AOCC_PATH}llvm-nm"
RANLIB="${AOCC_PATH}llvm-ranlib"
```
**`/etc/portage/env/compiler-gcc.conf`**

```
CC="gcc"
CXX="g++"
CXXFLAGS="${CFLAGS}"
LDFLAGS=""
```
**`/etc/portage/env/ldflags-lgcc_s.conf`**

```
LDFLAGS="${LDFLAGS} -lgcc_s"
```
**`/etc/portage/env/ldflags-lm.conf`**

```
LDFLAGS="${LDFLAGS} -lm"
```
And add known packages to workaround-list:

**`/etc/portage/package.env`**

```
### system-clang
###
# Doesn't compile with clang and needs gcc
dev-libs/elfutils compiler-gcc.conf
dev-libs/libgcrypt compiler-gcc.conf
dev-libs/popt compiler-gcc.conf
sys-apps/busybox compiler-gcc.conf
sys-apps/sandbox compiler-gcc.conf
sys-apps/sysvinit compiler-gcc.conf
sys-boot/efibootmgr compiler-gcc.conf
sys-devel/gcc compiler-gcc.conf
sys-libs/binutils-libs compiler-gcc.conf
sys-libs/glibc compiler-gcc.conf
# Needs -lgcc_s in LDFLAGS,
# resolve __register_frame / __deregister_frame undefined symbols
sys-devel/llvm ldflags-lgcc_s.conf
# Needs -lm in LDFLAGS,
# "undefined symbol: sqrt" and "undefined symbol: log10" link errors
sys-devel/gettext ldflags-lm.conf
```
Rebuild clang.

`root #``emerge -1avt clang llvm libcxx libcxxabi compiler-rt libunwind lld`
Check that your config works:

`root #``cat /var/db/pkg/sys-devel/llvm-11.0.0/CC`
Rebuild your system.

`root #``emerge -vt -e @world`
Please see the [original forum post](https://forums.gentoo.org/viewtopic-t-1102590-start-0.html) for where these steps came from.

## Troubleshooting

### "/opt/aocc/bin/clang: file not found!"

For some reason the symlink was wiped few times during `-e @world`. It seems to be related to packages calling `eselect compiler-shadow` during their **emerge**. There are a couple core packages doing so. Beware of this, and avoid using *--keep-going*.

If you get an error saying *clang: error while loading shared libraries: libLLVM-11.so: cannot open shared object file: No such file or directory* you most likely attempted to symlink or override all system LLVM settings. Don't do that.

### multilib system, "multilib-strict check failed!"

It seems like AOCC doesn't work very well on a [multilib](https://wiki.gentoo.org/wiki/Multilib) system, might be due to missing i686 compatible compiler. Workarounds for using system-clang might be needed.

## See also

- [AMD](https://wiki.gentoo.org/wiki/AMD) — a semiconductor company. AMD is best known for producing CPUs based on [x86 intruction set](https://en.wikipedia.org/wiki/x86), motherboard chipsets and their own line of GPUs.
- [Clang](https://wiki.gentoo.org/wiki/LLVM/Clang) — a C/C++/Objective-C/C++, CUDA, and RenderScript language front-end for the LLVM project

## External resources

- [Phoronix web](https://www.phoronix.com/scan.php?page=search&q=AOCC) presenting benchmarks between AOCC, clang and GCC
- [AOCC 2.3 vs clang 11 vs GCC10](https://www.phoronix.com/scan.php?page=article&item=amd-aocc-23)
