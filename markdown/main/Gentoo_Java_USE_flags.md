<!-- source: https://wiki.gentoo.org/wiki/Gentoo_Java_USE_flags | group: Gentoo Wiki (Main) | wiki-title: Gentoo Java USE flags -->
---
title: Gentoo Java USE flags
url: https://wiki.gentoo.org/wiki/Gentoo_Java_USE_flags
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-07-18"
fingerprint: c7c21bf966fa4529
license: CC BY-SA 4.0
---

# Gentoo Java USE flags

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

#### About USE flags

For more information regarding USE flags, refer to the [USE flags](https://devmanual.gentoo.org/general-concepts/use-flags) chapter from the Gentoo Development Guide.

#### Java specific USE flags

There are a few specific common USE flags for Java ebuilds as follows. These use flags do not go in the normal `USE` variable but go in `JAVA_PKG_IUSE` instead. Any use flag other than the following would go in the normal `USE` variable. The `JAVA_PKG_IUSE` must precede the [`inherit`](https://devmanual.gentoo.org/ebuild-writing/using-eclasses/) line in an ebuild.

The USE flags that go in `JAVA_PKG_IUSE`

- If USE FLAG [binary](https://packages.gentoo.org/useflags/binary)
- The `doc` flag will build API documentation using javadoc.
- The `source` flag installs a zip of the source code of a package. This is traditionally used for IDEs to 'attach' source to the libraries that are being use;
- The `test` Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently)

#### Masked gentoo-vm

The USE flag [gentoo-vm](https://packages.gentoo.org/useflags/gentoo-vm)[bug #805008](https://bugs.gentoo.org/show_bug.cgi?id=805008) for details on [how to unmask](https://wiki.gentoo.org/wiki/User:Sam/Portage_help/Java_unmasking) this USE flag.
