<!-- source: https://wiki.gentoo.org/wiki/CHOST | group: Gentoo Wiki (Main) | wiki-title: CHOST -->
---
title: CHOST
url: https://wiki.gentoo.org/wiki/CHOST
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-04"
fingerprint: "3dafc8ba5d801bea"
license: CC BY-SA 4.0
---

# CHOST

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The `CHOST` variable tells the compiler which platform code should be built for. Unlike the `[CFLAGS](https://wiki.gentoo.org/wiki/GCC_optimization)` variable, which is used for the optimizations, the `CHOST` variable is fixed and cannot be changed easily.

The [profile](https://wiki.gentoo.org/wiki/Portage/Profiles) provides the default `CHOST` values, but it can be changed in the /etc/portage/make.conf file.

The variable is a dash-separated tuple in the form of *ARCH*-*VENDOR*-*OS*-*LIBC*:

| Values | Descriptions | 
|---|---|
| *ARCH* | Specifies the CPU architecture. | 
| *VENDOR* | Specifies the hardware platform or vendor. | 
| *OS* | Specifies the operating system. | 
| *LIBC* | Specifies the C library to use. | 

Only *ARCH* is strictly required in all cases, but it is good practice to specify all four fields (for Linux machines at least). For operating systems suffixed by a '\*', a kernel version should go in its place. Additionally, the *LIBC* field should usually be ommitted for said operating systems.

The following non-comprehensive table lists some field values known to work:

| Architecture | Vendor | Operating System | C Library | 
|---|---|---|---|
| required | optional | optional | optional | 
| aarch64 / aarch64\_be arm64 (only use this for arm64-apple-{darwin, ios}) alpha arm / armv4 / armv4l / armv4t / armv4tl / armv5te / armv5tel / armv6j / armv7a hppa / hppa1.1 / hppa2.0 / hppa64 i386 / i486 / i586 / i686 ia64 loongarch64 m68k mips / mipsel mips64 / mips64el powerpc / powerpcle powerpc64 / powerpc64le riscv32 / riscv64 s390 / s390x sh / sh4 / sh64 sparc / sparc64 x86\_64 | apple gentoo ibm (s390{x} only) pc pc\_t64 softfloat softfp hardfloat unknown | darwin\* elf freebsd\* gentoo-freebsd\* ios\* linux solaris\* uclinux | eabi (arm) gnu gnuabi64 (mips) gnuabin32 (mips) gnueabi (arm) gnueabihf (arm) gnux32 (x86\_64) musl musleabi (arm) musleabihf (arm) uclibc | 

## Examples

- aarch64-unknown-linux-gnu
- arm64-apple-darwin21
- armv5tel-softfloat-linux-gnueabi
- powerpc-apple-darwin9
- powerpc64le-unknown-linux-musl
- x86\_64-pc-linux-gnu
- x86\_64-pc-solaris2.11

## See also

- [Changing the CHOST variable](https://wiki.gentoo.org/wiki/Changing_the_CHOST_variable) — explains how to change the `[CHOST]` variable of an existing system.
