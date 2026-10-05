<!-- source: https://wiki.gentoo.org/wiki/Embedded_Handbook/Tuples | group: Gentoo Wiki (Main) | wiki-title: Embedded Handbook/Tuples -->
---
title: Embedded Handbook/Tuples
url: https://wiki.gentoo.org/wiki/Embedded_Handbook/Tuples
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-03-09"
fingerprint: bc92d9bf07a23772
license: CC BY-SA 4.0
---

# Embedded Handbook/Tuples

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The GNU config project exists to take all known systems out there and generate a short string which describes all relevant aspects (from a toolchain/build perspective) of a system in question.

The canonicalized form is: **machine-vendor-kernel-operating system**. Many tuples may actually omit the vendor field (since it is completely arbitrary). The **operating system** field has also expanded its meaning over time to indicate the userland and/or userland ABI.

## Examples

Here is a (by no means complete) table of many common configurations. Note that not all permutations will work as some fields (like **kernel** or **operating system**) imply someone has actually ported the relevant **machine** to it which may not be the case.

| Machine | Vendor | Kernel | Operating system | 
|---|---|---|---|
| aarch64 alpha  arm / armeb  avr / avr32  bfin  cris  hppa / hppa1.1 / hppa2.0 / hppa64  ia64  i386 / i486 / i586 / i686  m68k  mips / mipsel / mips64 / mips64el  nios / nios2  powerpc / powerpc64  sparc / sparcv8 / sparcv9 / sparc64  s390 / s390x  sh / sh3 / sh4 / sheb / sh3eb / sh4eb / sh64  vax  x86\_64 | android gentoo  pc  softfloat \[1\] unknown | elf \[2\]  freebsd6.2  linux  mingw32 / mingw64  uclinux \[3\] | gnu \[4\]  gnueabi \[5\]  musl \[6\]  musleabihf \[7\]  uclibc \[8\]  uclibceabi | 

1. Remember that part where we said the **vendor** field was completely arbitrary? That's almost completely true. In the Gentoo world, we've allocated the field `softfloat` to indicate softfloat toolchains. If you need a softfloat toolchain, make sure to set the vendor field in your tuple with this in it.
2. When you use `elf` as the operating system, you're actually saying you do not want to run an operating system at all, but just some code directly on the hardware. This is almost exclusively accomplished with newlib (with help from libgloss).
3. The `uclinux` field produces FLAT binaries which run on Linux with the MMU disabled (so called no-mmu).
4. The `gnu` field indicates glibc as the system libc.
5. The `eabi` suffix will work on only a few embedded architectures (**ARM** and **PPC** at the moment). This tells the toolchain to generate code for the newer Embedded ABI.
6. The `musl` field indicates musl as the system libc.
7. The `eabihf` suffix will work on only a few embedded architectures (**ARM** at the moment). This tells the toolchain to generate code for the Embedded ABI and utilize `hardfloat` floating-point computation.
8. The `uclibc` field indicates uClibc as the system libc.

Some quick examples of bringing it all together. Say you want to run a glibc environment under Linux on a SuperH4 machine. The tuple there would simply be: `sh4-unknown-linux-gnu`. Or perhaps you'd like to run some code directly on the hardware (no operating system) with an **ARM** processor. The tuple there would be: `arm-elf`.

## Kernel arches

While in many cases, the value Portage uses for `ARCH` and `KEYWORDS` matches the value used by the kernel, this is not always the case. It also does not match exactly the machine list in the table above. Over time, the kernel may even rename/unify some architectures, so an exact list is not possible without tracking every version!

To find the current list of architectures, simply peek in the arch subdirectory of your kernel sources. Keep in mind that the kernel will have many processors unified under a single architecture. So while you may have an omap or strong ARM or xscale processor, these cpus are all part of the same ARM architecture. The same goes for i386, i486, i586, and i686 processors. These all fall under the **i386** architecture.
