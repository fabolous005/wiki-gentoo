<!-- source: https://wiki.gentoo.org/wiki/Mold | group: Gentoo Wiki (Main) | wiki-title: Mold -->
---
title: mold
url: https://wiki.gentoo.org/wiki/Mold
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-09-15"
fingerprint: "9d337a9805022f0e"
license: CC BY-SA 4.0
---

# mold

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**mold** is a linker that aims to provide drop-in compatibility with existing Unix linkers. It is many times faster than the BFD linker from GNU's [binutils](https://wiki.gentoo.org/wiki/Binutils) and slightly faster than the [LLD linker](https://wiki.gentoo.org/wiki/LLVM/LLD) from [LLVM](https://wiki.gentoo.org/wiki/LLVM) in some use cases. Its speed is achieved through the usage of optimized data structures and parallelization.<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> mold is still in the early stages of development and as features are added to be on par with the other linkers<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>, this could affect the speedy nature of mold.

The mold linker is in the Gentoo package repository and can be installed using the following command:

`root #``emerge --ask sys-devel/mold`
[GCC 12+](https://wiki.gentoo.org/wiki/C) and [Clang](https://wiki.gentoo.org/wiki/LLVM/Clang) support adding `-fuse-ld=mold` to `LDFLAGS` in the [make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf) file so that mold is used to link all packages:

**`/etc/portage/make.conf`**

```
LDFLAGS="${LDFLAGS} -fuse-ld=mold"
```
A [patch](https://gist.github.com/00-matt/dda791a36318bafb68576b8576b1d283/raw/fuse-ld-mold.patch) that allows GCC 11 to invoke mold as a linker is also available. The patch can be placed in [/etc/portage/patches](https://wiki.gentoo.org/wiki//etc/portage/patches):

`root #````
mkdir -p /etc/portage/patches/sys-devel/gcc-11.2.1_p20211127
```
`root #````
curl -Lo /etc/portage/patches/sys-devel/gcc-11.2.1_p20211127/fuse-ld-mold.patch \
    https://gist.github.com/00-matt/dda791a36318bafb68576b8576b1d283/raw/fuse-ld-mold.patch
```
Some packages do not build with mold (see [bug #830404](https://bugs.gentoo.org/show_bug.cgi?id=830404) for a list). An [environment](https://wiki.gentoo.org/wiki//etc/portage/package.env) can be created to selectively disable it for some packages:

**`/etc/portage/env/no-mold`**

```
LDFLAGS="-Wl,-O1 -Wl,--as-needed"
```
**`/etc/portage/package.env`**

```
# mold does not support linker scripts; it cannot be used to link the kernel
sys-kernel/vanilla-kernel no-mold
```
If some packages fail to build in report-LLVMgold.so cannot open shared subject, when you declare -flto and -fuse-ld=mold. Then you probably lack LLVMgold (linker plugin) which can make you secure.

`root #``emerge --ask llvm-core/llvm[binutils-plugin]`
- [Gold](https://wiki.gentoo.org/wiki/Gold) — a linker intended as a replacement for the ld.bfd linker.
