<!-- source: https://wiki.gentoo.org/wiki/Multilib/Tips_%26_tricks | group: Gentoo Wiki (Main) | wiki-title: Multilib/Tips & tricks -->
---
title: Multilib/Tips & tricks
url: https://wiki.gentoo.org/wiki/Multilib/Tips_%26_tricks
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-06-02"
fingerprint: a7659a7fae7dc58f
license: CC BY-SA 4.0
---

# Multilib/Tips & tricks

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page aims to collect advices on handling different build systems when doing multilib

## Binary Package Dependencies

When writing an ebuild for a binary package of the alternative ABI, the gx86-multilib dependencies should be specified in RDEPEND the same way that a regular dependency atom would, except for the addition of the USE dep for the particular ABI that is required. Since the binary package requires a particular lib SONAME, the version or slot needs to be as specific as possible so that the installed lib matches the requirements of the binary; specifying the entire SLOT/SUBSLOT is recommended, however a lower-bound and upper-bound $PV will also suffice.

Because of how the gx86-multilib project uses USE flags, and the way defaults have been set in all profiles, gx86-multilib dependencies can be specified directly in RDEPEND and do not need to be wrapped by an "arch" use conditional. For instance, there is no need to have separate "x86? ( ... )" and "amd64? ( ... )" dependency lists in the general case.

## Documentation

### Pre-built gtk-doc documentation

Some packages (especially GNOME) include pre-built gtk-doc HTML docs in the source tarball. The gtk-doc Makefile doesn't handle out-of-source builds well, and enabling those cause the pre-built docs not to be installed, or to be generated again.

This issue can be solved via placing a symlink to pre-generated docs inside the build-dir, like:
