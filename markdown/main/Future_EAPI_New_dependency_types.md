<!-- source: https://wiki.gentoo.org/wiki/Future_EAPI/New_dependency_types | group: Gentoo Wiki (Main) | wiki-title: Future EAPI/New dependency types -->
---
title: Future EAPI/New dependency types
url: https://wiki.gentoo.org/wiki/Future_EAPI/New_dependency_types
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-21"
fingerprint: "3761aa251d679fd9"
license: CC BY-SA 4.0
---

# Future EAPI/New dependency types

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page provides a summary of new dependency types requested for various features.

## The types

- `BADEPEND` (aka `CDEPEND`, `TDEPEND`)
- *Build-against dependencies*; [bug #392239](https://bugs.gentoo.org/show_bug.cgi?id=392239). It would be used to inject additional `DEPEND` into a package being built against the package in question.
- **(Only?) use case**: xorg packages, where e.g. building against *libX11* requires header files in *xorg-proto*. However, *libX11* can still be used without *xorg-proto* installed, so `RDEPEND` is not really correct. An alternative is to specify *xorg-proto* in every single ebuild linking against *libX11* directly or indirectly.
- *Note:* This has also been suggested under the names [CDEPEND](https://public-inbox.gentoo.org/gentoo-dev/200510250903.28250.electronerd@monolith3d.com/) (for "compile-against depend") and [TDEPEND](https://bugs.gentoo.org/392239#c12) (for "transitive depend").

- `DYNRDEPEND`
- *Dynamic runtime dependencies*; [portage-dev](https://public-inbox.gentoo.org/gentoo-portage-dev/200402131401.50261.pauldv@gentoo.org/). A list of packages that will depended on when they are available at runtime. This would need to be implemented with logic to actually record the existence of those packages in the metadata for binary or installed packages.

- `FDEPEND`
- *Fetch dependencies*; [bug #481434](https://bugs.gentoo.org/show_bug.cgi?id=481434). If implemented, it will be used to specify the dependencies necessary for the `src_fetch()` phase (e.g. a particular VCS). Thus, the package manager will be required to delay the execution of that phase until all fetch dependencies have been satisfied.
- **Alternative solution:** put fetch dependencies in `BDEPEND`, and request `src_fetch()` implementation to check whether the tool requirements are satisfied already. Allow the package manager to execute `src_fetch()` without satisfying the dependencies and if it fails, re-try when the dependencies are satisfied.

- `LDEPEND`
- *Library dependencies*. Packages providing a library that is linked to, the first dependency type listed for BSD ports, cf: [LIB\_DEPENDS](https://docs.freebsd.org/en/books/porters-handbook/makefiles/#makefile-depend), which portage descends from. These would automatically be considered both `DEPEND` and `RDEPEND`, but soname changes would be tracked, and this would provide better data for both embedded and normal builds, eg: for `--preserved-rebuild`.

- `SDEPEND`
- *Suggested dependencies*; [bug #327701](https://bugs.gentoo.org/show_bug.cgi?id=327701). A way of providing suggestions to install additional packages which can provide additional features to the package in question.

- `TDEPEND`
- *Test dependencies*; [bug #541938](https://bugs.gentoo.org/show_bug.cgi?id=541938). Additional dependencies needed for the `src_test()` phase. This type would probably need to be split between CBUILD & CHOST dependencies, e.g. `TBDEPEND` and `THDEPEND`.
- *Note:* The name `TDEPEND` was also suggested in [bug #317337](https://bugs.gentoo.org/show_bug.cgi?id=317337) for "target build-time dependencies" (i.e. what has become `DEPEND` in EAPI 8).
