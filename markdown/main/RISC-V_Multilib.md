<!-- source: https://wiki.gentoo.org/wiki/RISC-V_Multilib | group: Gentoo Wiki (Main) | wiki-title: RISC-V Multilib -->
---
title: RISC-V Multilib
url: https://wiki.gentoo.org/wiki/RISC-V_Multilib
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-06"
fingerprint: "5f9b7eeb3e61cf73"
license: CC BY-SA 4.0
---

# RISC-V Multilib

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

As riscv has a few [ABIs](https://wiki.gentoo.org/wiki/RISC-V_ABIs) and the multilib story is a bit complicated as well.

Gentoo will use a multilib compatible LIBDIR layout for both multilib and non-multilib profiles.

TODO: add details on what other distros do

## rv64gc

This is the 64bit architecture (rv64) with extensions imadfc (i.e., g is a shorthand for imadf). It supports 2 ABIs, lp64 (softfloat) and lp64d (hardfloat double).

The gcc default is

-march=rv64gc -mabi=lp64d

Our multilib structure follows the guidelines as implemented in GCC:

\# from profiles/arch/riscv/rv64gc/make.defaults
# Library directories
LIBDIR\_lp64d="lib64/lp64d"
LIBDIR\_lp64="lib64/lp64"
SYMLINK\_LIB="no"
# Flags for lp64d
CFLAGS\_lp64d="-mabi=lp64d"
# Flags for lp64
CFLAGS\_lp64="-mabi=lp64"

## rv32i

This is the 32-bit architecture, which is not supported by glibc yet (but will be soon), but [rv32ia](https://github.com/riscv-collab/riscv-gnu-toolchain/issues/798) should be already.
