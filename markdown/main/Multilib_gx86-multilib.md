<!-- source: https://wiki.gentoo.org/wiki/Multilib/gx86-multilib | group: Gentoo Wiki (Main) | wiki-title: Multilib/gx86-multilib -->
---
title: Multilib/gx86-multilib
url: https://wiki.gentoo.org/wiki/Multilib/gx86-multilib
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-06-04"
fingerprint: "76d1622a2893d502"
license: CC BY-SA 4.0
---

# Multilib/gx86-multilib

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page will use amd64 with the additional 32-bit API for illustration, users of other multilib-enabled arches should adapt the instructions accordingly.

Due to limitations in the older emul-linux-x86-\* multilib solution, it is generally troublesome to toggle eclass based multilib on individual packages. Thus, this page currently only describes how to enable it system wide. These difficulties are expected to lessen as more of the packages provided by the emul-linux-x86-\* packages are migrated to use eclass based multilib, and other packages are updated to accept the migrated packages in their dependencies.

## Enabling eclass based multilib

### Enabling an additional ABI

#### Enabling per-package

The 32-bit ABI can enabled per package by setting a USE flag:

**`/etc/portage/package.use/abi_x86_32`**

If the package manager requires dependencies to also include this flag, it will prompt to add them.

#### System wide implementation

It is not strictly necessary to specify the default (64-bit) ABI since it is force-enabled by 64-bit profiles, but does not hurt to add it:

**`/etc/portage/make.conf`**

### Note on CFLAGS

For those that enable the 32-bit/64-bit ABI `CFLAGS` options must *NOT* include any `-mXX` options.

**`/etc/portage/make.conf`**

This will allow the `ABI_X86="32 64"` to select the compiler options during the configure and make process.
