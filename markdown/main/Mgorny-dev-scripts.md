<!-- source: https://wiki.gentoo.org/wiki/Mgorny-dev-scripts | group: Gentoo Wiki (Main) | wiki-title: Mgorny-dev-scripts -->
---
title: mgorny-dev-scripts
url: https://wiki.gentoo.org/wiki/Mgorny-dev-scripts
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-05"
fingerprint: a3cc3a2196918730
license: CC BY-SA 4.0
---

# mgorny-dev-scripts

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


mgorny-dev-scripts is a collection of useful utilities to improve the ebuild development experience.

## Installation

### Emerge

`root #``emerge --ask app-portage/mgorny-dev-scripts`
## Usage

### Pkgdiff-mg

pkgdiff-mg is a diff'ing tool that shows the differences in the source code between two different package versions.

#### Basic diff

To show the complete diff between `package-1.0.0.ebuild` and `package-2.0.0.ebuild`:

`user $``pkgdiff-mg package-1.0.0.ebuild package-2.0.0.ebuild`
#### Comparing build system changes

To only view the build system changes, use `--build-system` or `-b`:

`user $``pkgdiff-mg -b package-1.0.0.ebuild package-2.0.0.ebuild`
## See also

- [Iwdevtools](https://wiki.gentoo.org/wiki/Iwdevtools) — a group of small tools to aid with Gentoo development, primarily intended for QA.
- [Pkgdev](https://wiki.gentoo.org/wiki/Pkgdev) — a collection of tools for Gentoo development.
