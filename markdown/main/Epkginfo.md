<!-- source: https://wiki.gentoo.org/wiki/Epkginfo | group: Gentoo Wiki (Main) | wiki-title: Epkginfo -->
---
title: epkginfo
url: https://wiki.gentoo.org/wiki/Epkginfo
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-03-09"
fingerprint: a6cd7e000976a996
license: CC BY-SA 4.0
---

# epkginfo

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**epkginfo** is a tool used to display package metadata information. It is a shortcut to using the equery meta command.

epkginfo is part of [gentoolkit](https://wiki.gentoo.org/wiki/Gentoolkit).

## Installation

### Emerge

epkginfo comes as part of the Gentoolkit suite. It, along with the rest of the tools, can be installed by running:

`root #``emerge --ask app-portage/gentoolkit`
Please see the [Gentoolkit](https://wiki.gentoo.org/wiki/Gentoolkit) article for information on the other tools included in the [app-portage/gentoolkit](https://packages.gentoo.org/packages/app-portage/gentoolkit) package.

## Usage

### Invocation

As mentioned above epkginfo can be invoked two different ways:

- `epkginfo`
- `equery meta`

Using the shorter method:

`user $``epkginfo --help`
Usage: epkginfo \[options\] pkgspec
Display metadata about a given package.
options
 -h, --help              display this help message
 -d, --description       show an extended package description
 -H, --herd              show the herd(s) for the package
 -k, --keywords          show keywords for all matching package versions
 -l, --license           show licenses for the best maching version
 -m, --maintainer        show the maintainer(s) for the package
 -S, --stablreq          show STABLEREQ arches (cc's) for all matching package versions
 -u, --useflags          show per-package USE flag descriptions
 -U, --upstream          show package's upstream information
 -x, --xml               show the plain metadata.xml file
