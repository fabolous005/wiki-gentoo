<!-- source: https://wiki.gentoo.org/wiki/NQP | group: Gentoo Wiki (Main) | wiki-title: NQP -->
---
title: NQP
url: https://wiki.gentoo.org/wiki/NQP
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-01-31"
fingerprint: f6437ccdca72b94e
license: CC BY-SA 4.0
---

# NQP

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**NQP** is also known as "Not Quite Perl" is a lightweight [Raku](https://wiki.gentoo.org/wiki/Raku)-like environment for MoarVM, JVM, and other virtual machines.

## Installation

### USE flags


| [+moar](https://packages.gentoo.org/useflags/+moar) | Build the MoarVM backend (experimental/broken) | 
| [clang](https://packages.gentoo.org/useflags/clang) | Toggle usage of the clang compiler in conjunction with MoarVM | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [java](https://packages.gentoo.org/useflags/java) | Add support for Java | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

Emerge the package base

`root #``emerge --ask dev-lang/nqp`
## Removal

While NQP can be installed or removed on its own it's more typically handled as a dependency of [Rakudo](https://wiki.gentoo.org/wiki/Rakudo).

### Unmerge

`root #``emerge --ask --depclean --verbose dev-lang/nqp`
## See Also

- [Rakudo](https://wiki.gentoo.org/wiki/Rakudo) — a compiler that implements the [Raku](https://wiki.gentoo.org/wiki/Raku) programming language.
- [MoarVM](https://wiki.gentoo.org/wiki/MoarVM) — [Rakudo](https://wiki.gentoo.org/wiki/Rakudo) compiler's virtual machine for the [Raku](https://wiki.gentoo.org/wiki/Raku) Programming Language.
- [Zef](https://wiki.gentoo.org/index.php?title=Zef&action=edit&redlink=1)
- [Perl](https://wiki.gentoo.org/wiki/Perl) — a general purpose interpreted programming language with a powerful regular expression engine.
