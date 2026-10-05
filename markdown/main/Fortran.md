<!-- source: https://wiki.gentoo.org/wiki/Fortran | group: Gentoo Wiki (Main) | wiki-title: Fortran -->
---
title: Fortran
url: https://wiki.gentoo.org/wiki/Fortran
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-29"
fingerprint: "44004a7f59cef40c"
license: CC BY-SA 4.0
---

# Fortran

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Fortran** is a general-purpose, compiled imperative programming language that is especially suited to numeric computation and scientific computing.

## Installation

### GCC

**gfortran** is [GCC](https://wiki.gentoo.org/wiki/GCC)'s Fortran <sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>. Enable `fortran` to obtain **gfortran**.

If your package.use is a file:

**`/etc/portage/package.use`**

If your package.use is a folder:

**`/etc/portage/package.use/gcc-fortran`**

For more information, see this [wiki](https://gcc.gnu.org/wiki/GFortran).

### Flang

[Flang](https://github.com/llvm/llvm-project/tree/main/flang) is [LLVM](https://wiki.gentoo.org/wiki/LLVM)'s fortran compiler.


| [+debug](https://packages.gentoo.org/useflags/+debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

Change the `FC` variable in /etc/portage/make.conf to select the Flang compiler. Changing `F77` might also be necessary:

**`/etc/portage/make.conf`**

```
FCFLAGS="${FCFLAGS}"
FFLAGS="${FFLAGS}"
F77FLAGS="${F77FLAGS}"
FC="flang"
F77="flang"
```
