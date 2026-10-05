<!-- source: https://wiki.gentoo.org/wiki/Make | group: Gentoo Wiki (Main) | wiki-title: Make -->
---
title: make
url: https://wiki.gentoo.org/wiki/Make
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-02-04"
fingerprint: eb0abb5b56d73530
license: CC BY-SA 4.0
---

# make

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Make** is a [tool to *build*](https://wiki.gentoo.org/wiki/Build_automation#Available_software) software from source code (which usually includes compiling). [makefiles](<https://en.wikipedia.org/wiki/Make_(software)#Makefiles>) contain the instructions to build packages, and these are sometimes generated with [Autotools](https://wiki.gentoo.org/wiki/Autotools).

The make utility is [specified by](https://pubs.opengroup.org/onlinepubs/9799919799/utilities/make.html#tag_20_76) [POSIX](https://wiki.gentoo.org/wiki/POSIX). Gentoo's default make implementation is [GNU Make](https://www.gnu.org/software/make/), [dev-build/make](https://packages.gentoo.org/packages/dev-build/make) (often called *gmake*), installed as part of the @system set. However, the main Gentoo repository also provides [bmake](http://www.crufty.net/help/sjg/bmake.html), [dev-build/bmake](https://packages.gentoo.org/packages/dev-build/bmake), derived from NetBSD's make.

## Invocation

## See also

- [Autotools](https://wiki.gentoo.org/wiki/Autotools) — a build system often used for open source projects.
- [Build automation](https://wiki.gentoo.org/wiki/Build_automation) — software that automates the compilation, clean up, and installation stages of the software creation process.

## External resources

- [man 5 ebuild](https://dev.gentoo.org/~zmedico/portage/doc/man/ebuild.5.html) - See the section **emake \[make options\]**
