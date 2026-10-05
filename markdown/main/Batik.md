<!-- source: https://wiki.gentoo.org/wiki/Batik | group: Gentoo Wiki (Main) | wiki-title: Batik -->
---
title: Batik
url: https://wiki.gentoo.org/wiki/Batik
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-03-01"
fingerprint: "86447f393d5a2da6"
license: CC BY-SA 4.0
---

# Batik

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Batik is a Java-based toolkit for applications or applets that want to use images in the Scalable Vector Graphics (SVG) format for various purposes, such as display, generation or manipulation.

The package has more than 30 modules and is packaged using the [java-pkg-simple](https://devmanual.gentoo.org/eclass-reference/java-pkg-simple.eclass/) [eclass](https://wiki.gentoo.org/wiki/Eclass) while the upstream [build system](https://wiki.gentoo.org/wiki/Build_automation) is [maven](https://wiki.gentoo.org/wiki/Maven).

## Installation

### USE flags


| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [source](https://packages.gentoo.org/useflags/source) | Zip the sources and install them | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

### Emerge

`root #``emerge --ask dev-java/batik`
## Usage

`user $``batik-rasterizer`
See [upstream for SVG Rasterizer](https://xmlgraphics.apache.org/batik/tools/rasterizer.html)

`user $``batik-slideshow``user $``batik-squiggle``user $``batik-svgpp`
See [upstream for SVG Pretty Printer](https://xmlgraphics.apache.org/batik/tools/pretty-printer.html)

`user $``batik-ttf2svg`
