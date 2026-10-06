<!-- source: https://wiki.gentoo.org/wiki/Fortran | group: Gentoo Wiki (Main) | wiki-title: Fortran -->
---
title: Fortran
url: https://wiki.gentoo.org/wiki/Fortran
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-29"
fingerprint: "44005aef5dcef404"
license: CC BY-SA 4.0
---

# Fortran

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Fortran** is a general-purpose, compiled imperative programming language that is especially suited to numeric computation and scientific computing.

## Installation

### GCC

**gfortran** is [GCC](https://wiki.gentoo.org/wiki/GCC)'s Fortran <sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>. Enable `fortran` to obtain **gfortran**.

If your package.use is a file:

FILE **`/etc/portage/package.use`**

```
sys-devel/gcc fortran
```
If your package.use is a folder:

FILE **`/etc/portage/package.use/gcc-fortran`**

```
sys-devel/gcc fortran
```
For more information, see this [wiki](https://gcc.gnu.org/wiki/GFortran).

### Flang

[Flang](https://github.com/llvm/llvm-project/tree/main/flang) is [LLVM](https://wiki.gentoo.org/wiki/LLVM)'s fortran compiler.


### USE flags for
            [llvm-runtimes/flang-rt](https://packages.gentoo.org/packages/llvm-runtimes/flang-rt)
            
            LLVM's Fortran runtime

| [+debug](https://packages.gentoo.org/useflags/+debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

Change the `FC` variable in /etc/portage/make.conf to select the Flang compiler. Changing `F77` might also be necessary:

FILE **`/etc/portage/make.conf`**

```
FCFLAGS="${FCFLAGS}"
FFLAGS="${FFLAGS}"
F77FLAGS="${F77FLAGS}"
FC="flang"
F77="flang"
```
## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) [https://gcc.gnu.org/fortran](https://gcc.gnu.org/fortran) Retrieved on Feb 2, 2023
