<!-- source: https://wiki.gentoo.org/wiki/Ebuild | group: Gentoo Wiki (Main) | wiki-title: Ebuild -->
---
title: ebuild
url: https://wiki.gentoo.org/wiki/Ebuild
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-12"
fingerprint: cb2b384eb6fb2338
license: CC BY-SA 4.0
---

# ebuild

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


An **ebuild** file is a text file, usually stored in a [repository](https://wiki.gentoo.org/wiki/Ebuild_repository), which identifies a specific software package and tells the Gentoo package manager how to handle it. Ebuilds adhere to a specific [EAPI](https://wiki.gentoo.org/wiki/EAPI) version, and are standardized through the [Package Manager Specification](https://wiki.gentoo.org/wiki/Package_Manager_Specification):

The ebuild file format is in its basic form a subset of the format of a bash script. The interpreter is assumed to be GNU bash


Ebuilds contain metadata about each version of a piece of available software (name, version number, license, home page address...), dependency information (both build-time and run-time), and instructions on how to build and install the software (configure, compile, build, install, test...).

The default location for ebuilds in Gentoo is the [Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository) ([/var/db/repos/gentoo](https://wiki.gentoo.org/wiki//var/db/repos/gentoo)/).

## Live ebuilds

An ebuild is a **[live ebuild](https://wiki.gentoo.org/wiki/Live_ebuilds)** if the source is fetched from a revision control system (VCS). They tend to, but not necessarily, have the version number 9999 so that they can be easily distinguished from normal ebuilds based on upstream releases.

In a formal sense, an ebuild is *live* if it has a variable `PROPERTIES` with a value "live" inside it. If an ebuild inherits a VCS eclass (e.g. git-r3, mercurial, darcs), it will be live, because these eclasses have a line `PROPERTIES+=" live"`.

## See also

- [Basic guide to write Gentoo Ebuilds](https://wiki.gentoo.org/wiki/Basic_guide_to_write_Gentoo_Ebuilds) — getting started writing **[ebuilds]**, to harness the power of [Portage](https://wiki.gentoo.org/wiki/Portage), to install and manage even more software.
- [Submitting ebuilds](https://wiki.gentoo.org/wiki/Submitting_ebuilds) — explains how to submit ebuilds for inclusion in the [Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository)
- [Package Manager Specification](https://wiki.gentoo.org/wiki/Package_Manager_Specification) — a standardization effort to ensure that the [ebuild] file format, the ebuild repository format (of which the Gentoo ebuild repository is the main incarnation), as well as behavior of the package managers interacting with these ebuilds is properly agreed upon and documented.
- [Portage](https://wiki.gentoo.org/wiki/Portage) — the official [package manager](https://en.wikipedia.org/wiki/Package_manager) and [distribution system](https://www.gentoo.org/get-started/about/) for Gentoo.

## External resources

- [ebuild eclass reference](https://devmanual.gentoo.org/eclass-reference/ebuild/index.html) in the developer manual
- [ebuild-maintainer-quiz.txt](https://gitweb.gentoo.org/sites/projects/comrel.git/tree/recruiters/quizzes/ebuild-maintainer-quiz.txt) - Gentoo developer ebuild quiz
- [ebuild command's man page](https://dev.gentoo.org/~zmedico/portage/doc/man/ebuild.1.html)
- [ebuild file format man page](https://dev.gentoo.org/~zmedico/portage/doc/man/ebuild.5.html)
- [Gentoo devmanual](https://devmanual.gentoo.org/index.html)
- [Quickstart ebuild guide](https://devmanual.gentoo.org/quickstart/index.html) from the devmanual
