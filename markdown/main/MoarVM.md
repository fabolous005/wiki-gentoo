<!-- source: https://wiki.gentoo.org/wiki/MoarVM | group: Gentoo Wiki (Main) | wiki-title: MoarVM -->
---
title: MoarVM
url: https://wiki.gentoo.org/wiki/MoarVM
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-01-31"
fingerprint: c643780d1a63b94e
license: CC BY-SA 4.0
---

# MoarVM

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**MoarVM** is the [Rakudo](https://wiki.gentoo.org/wiki/Rakudo) compiler's virtual machine for the [Raku](https://wiki.gentoo.org/wiki/Raku) Programming Language.

## Installation

### USE flags


| [+jit](https://packages.gentoo.org/useflags/+jit) | Enable Just-In-Time-Compiler. Has no effect except on AMD64 and Darwin. | 
| [asan](https://packages.gentoo.org/useflags/asan) | Enable clang's Address Sanitizer functionality. Expect longer compile time. | 
| [clang](https://packages.gentoo.org/useflags/clang) | Use clang compiler instead of GCC | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [optimize](https://packages.gentoo.org/useflags/optimize) | Enable optimization via CFLAGS | 
| [static-libs](https://packages.gentoo.org/useflags/static-libs) | Build static versions of dynamic libraries as well | 
| [ubsan](https://packages.gentoo.org/useflags/ubsan) | Enable clang's Undefined Behavior Sanitizer functionality. Expect longer compile time. | 

### Emerge

Emerge the package base

`root #``emerge --ask dev-lang/moarvm`
## Configuration

### Environment variables

- `MVM_JIT_DISABLE` (bool) Disables the just-in-time compiler. JIT compilation is enabled by default.
- `MVM_SPESH_DISABLE` (bool) Disables the runtime bytecode optimizer. This optimization is enabled by default.
- `MVM_SPESH_INLINE_DISABLE` (bool) Disables inlining of call frames by the bytecode optimizer. This optimization is enabled by default.
- `MVM_SPESH_OSR_DISABLE` (bool) Disables the on-stack replacement of bytecode by the optimizer. This optimization is enabled by default.
- `MVM_CROSS_THREAD_WRITE_LOG` (bool) Produce warnings when a thread does a write to an object it didn't allocate and doesn't have a lock for.
- `MVM_CROSS_THREAD_WRITE_LOG_INCLUDE_LOCKED` (bool) Extend the above to include objects that are locked as well.

## Removal

MoarVM is a dependency of Rakudo, the Raku compiler. As such it's not typically installed or removed on its own.

### Unmerge

`root #``emerge --ask --depclean --verbose dev-lang/moarvm`
## See Also

- [Rakudo](https://wiki.gentoo.org/wiki/Rakudo) — a compiler that implements the [Raku](https://wiki.gentoo.org/wiki/Raku) programming language.
- [NQP](https://wiki.gentoo.org/wiki/NQP) — a lightweight [Raku](https://wiki.gentoo.org/wiki/Raku)-like environment for MoarVM, JVM, and other virtual machines.
- [Zef](https://wiki.gentoo.org/index.php?title=Zef&action=edit&redlink=1)
- [Perl](https://wiki.gentoo.org/wiki/Perl) — a general purpose interpreted programming language with a powerful regular expression engine.
