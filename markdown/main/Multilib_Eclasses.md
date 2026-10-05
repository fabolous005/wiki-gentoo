<!-- source: https://wiki.gentoo.org/wiki/Multilib/Eclasses | group: Gentoo Wiki (Main) | wiki-title: Multilib/Eclasses -->
---
title: Multilib/Eclasses
url: https://wiki.gentoo.org/wiki/Multilib/Eclasses
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-08-24"
fingerprint: "5bf32369ee41cd23"
license: CC BY-SA 4.0
---

# Multilib/Eclasses

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The multilib projects maintains a few eclasses to help developers integrating multilib support in their projects. While the use of any eclass is not technically necessary, it is convenient and provides flexibility in adding new ABIs.

## multilib.eclass

The multilib.eclass is **not** a part of the gx86-multilib eclass suite. It predates the initiative, and provides two kinds of functions:

1. Common functions to improve support for multilib systems, e.g. the `get_libdir` function.
2. Functions to help building packages for multiple ABIs implicitly.

The first category is used commonly by multilib and non-multilib ebuilds. If a multilib ebuild uses one of those functions, it needs to explicitly *inherit multilib*.

The second category is deprecated and must not be used in new multilib packages.

## gx86-multilib eclass suite

### Eclass listing

The suite consists of the following eclasses:

| Eclass | Goal/use | 
|---|---|
| [multilib-build](https://wiki.gentoo.org/wiki/Multilib/multilib-build) | Common helpers and low-level routines for multilib packages. | 
| [multilib-minimal](https://wiki.gentoo.org/wiki/Multilib/multilib-minimal) | Convenient phase function framework for building multilib packages. | 
| [cmake-multilib](https://wiki.gentoo.org/index.php?title=Multilib/cmake-multilib&action=edit&redlink=1) | Multilib wrapper on top of cmake phases. | 
| [meson-multilib](https://wiki.gentoo.org/index.php?title=Multilib/meson-multilib&action=edit&redlink=1) | Multilib wrapper on top of meson phases. | 

### Eclass choice algorithm

When choosing the correct eclass to use, the following algorithm can be used:

1. If the ebuild uses *cmake*, *cmake-multilib* should be used.
2. If building for multilib ABIs is done and none of the aforementioned conditions apply, *multilib-minimal* should be used.
3. *multilib-build* should be used only in special cases. For example, virtuals since they do not need any phase functions.
