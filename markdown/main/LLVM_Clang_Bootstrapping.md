<!-- source: https://wiki.gentoo.org/wiki/LLVM/Clang/Bootstrapping | group: Gentoo Wiki (Main) | wiki-title: LLVM/Clang/Bootstrapping -->
---
title: LLVM/Clang/Bootstrapping
url: https://wiki.gentoo.org/wiki/LLVM/Clang/Bootstrapping
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-27"
fingerprint: fff1f30a65623ea0
license: CC BY-SA 4.0
---

# LLVM/Clang/Bootstrapping

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Advanced users may optionally choose to bootstrap [Clang](https://wiki.gentoo.org/wiki/LLVM/Clang) by building it using the full LLVM toolchain.

This is *required* only if looking to move a system to be entirely [GCC](https://wiki.gentoo.org/wiki/GCC)-free (which is currently not possible on [glibc](https://wiki.gentoo.org/wiki/Glibc) systems).
**This is not yet supported and is dangerous**.

## Bootstrapping the Clang toolchain

Mixing Clang and GCC should be fine, unless using [default-libcxx](https://packages.gentoo.org/useflags/default-libcxx) [or](https://wiki.gentoo.org/wiki/USE_flag) [default-compiler-rt](https://packages.gentoo.org/useflags/default-compiler-rt)[. Clang and GCC should otherwise always 
produce output with the same ABI.](https://wiki.gentoo.org/wiki/USE_flag)

For purity, however, some may wish to build Clang and the rest of the LLVM toolchain with itself, to prove it works and to fully dogfood using Clang.

Be warned that because of a combination of how LLVM is packaged and LLVM's internal structure, this is liable to break across upgrades of major versions.

### Preparing the environment

Prepare the environment for the Clang toolchain:

**`/etc/portage/env/compiler-clang`**

```
COMMON_FLAGS="-march=native -O2 -pipe"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
CC="clang"
CXX="clang++"
LDFLAGS="-fuse-ld=lld -rtlib=compiler-rt -unwindlib=libunwind -Wl,--as-needed"
```
This example replaces not only the compiler but also the GNU linker ld.bfd with the LLVM linker lld. It is a drop-in replacement, but significantly faster than the bfd linker.

Set USE flags `default-compiler-rt default-lld llvm-libunwind` for Clang via /etc/portage/package.use:

**`/etc/portage/package.use/clang`**

```
 default-compiler-rt default-lld llvm-libunwind
llvm-runtimes/clang-runtime default-compiler-rt default-lld llvm-libunwind
```
Then install Clang, LLVM, compiler-rt, llvm-runtimes/libunwind, and lld with the default GCC environment:

`root #``emerge llvm-core/clang llvm-core/llvm llvm-runtimes/compiler-rt llvm-runtimes/libunwind llvm-core/lld`
It is also possible to add the `default-libcxx` USE flag to use LLVM's C++ STL with clang, however this is **heavily** discouraged because libstdc++ and libc++ are not ABI compatible. i.e. A program built against libstdc++ will likely break when using a library built against libc++, and vice versa.

Note that [llvm-runtimes/libunwind](https://packages.gentoo.org/packages/llvm-runtimes/libunwind) deals with linking issues that [sys-libs/libunwind](https://packages.gentoo.org/packages/sys-libs/libunwind) has, so it is preferred to use and replace the non-llvm libunwind package if installed (it builds with `-lgcc_s` to resolve issues with `__register_frame` / `__deregister_frame` undefined symbols).

## Finalizing

Enable the Clang environment for these packages now:

**`/etc/portage/package.env`**

```
 compiler-clang
llvm-core/llvm compiler-clang
llvm-runtimes/libcxx compiler-clang
llvm-runtimes/libcxxabi compiler-clang
llvm-runtimes/compiler-rt compiler-clang
llvm-runtimes/compiler-rt-sanitizers compiler-clang
llvm-runtimes/libunwind compiler-clang static-libs
llvm-core/lld compiler-clang
```
Repeat the emerge step with the new environment - the toolchain will now be rebuilt with itself instead of GCC:

`root #``emerge llvm-core/clang llvm-core/llvm llvm-runtimes/libcxx llvm-runtimes/libcxxabi llvm-runtimes/compiler-rt llvm-runtimes/compiler-rt-sanitizers llvm-runtimes/libunwind llvm-core/lld`
Clang may now be used with other packages!
