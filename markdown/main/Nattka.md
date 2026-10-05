<!-- source: https://wiki.gentoo.org/wiki/Nattka | group: Gentoo Wiki (Main) | wiki-title: Nattka -->
---
title: NATTkA
url: https://wiki.gentoo.org/wiki/Nattka
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-12-23"
fingerprint: "96d30aed6ee57b6d"
license: CC BY-SA 4.0
---

# NATTkA

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**NATTkA** is a toolkit for dealing with keywording and stabilization bugs in Gentoo.

## Installation

### USE flags


| [depgraph-order](https://packages.gentoo.org/useflags/depgraph-order) | Process packages in depgraph order whenever possible. | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

`root #``emerge --ask app-portage/nattka`
## Usage

Package list for keywording

`user $``nattka make-package-list -a '~arm ~arm64 ~ppc64 ~x86' dev-java/assertj-core`
Package list for stablereq

`user $``nattka make-package-list -s 'dev-java/log4j-12-api-2.18.0'`
## See also

- [Pkgcheck](https://wiki.gentoo.org/wiki/Pkgcheck) — a [pkgcore](https://wiki.gentoo.org/wiki/Pkgcore)-based QA utility for ebuild repos.
- [Pkgdev](https://wiki.gentoo.org/wiki/Pkgdev) — a collection of tools for Gentoo development.
