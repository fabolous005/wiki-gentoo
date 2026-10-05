<!-- source: https://wiki.gentoo.org/wiki/Notes_on_TeX_related_ebuilds | group: Gentoo Wiki (Main) | wiki-title: Notes on TeX related ebuilds -->
---
title: Notes on TeX related ebuilds
url: https://wiki.gentoo.org/wiki/Notes_on_TeX_related_ebuilds
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-02-14"
fingerprint: "87b9caafc6716bd0"
license: CC BY-SA 4.0
---

# Notes on TeX related ebuilds

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

### Dependencies

We have two virtual ebuilds in the tree which consumers of tex/latex should depend on (see also [bug #195894](https://bugs.gentoo.org/show_bug.cgi?id=195894)).

1. [virtual/latex-base](https://packages.gentoo.org/packages/virtual/latex-base) (required in most cases)
2. [virtual/tex-base](https://packages.gentoo.org/packages/virtual/tex-base)

#### Run time dependencies for packages like TeX editors and GUIs

If a package requires (La)TeX at runtime, but we do not know what the user wants to do with it exactly, it should depend on
[app-text/texlive](https://packages.gentoo.org/packages/app-text/texlive).

Examples: [app-office/texstudio](https://packages.gentoo.org/packages/app-office/texstudio).

**`examples/a-tex-gui*.ebuild`**

```
RDEPEND="app-text/texlive"
```
#### Run time dependencies for packages with specific TeX dependencies

If a package requires specific TeX packages at runtime, like in a music score editor, which renders a score with LaTeX, 
it should depend on [virtual/latex-base](https://packages.gentoo.org/packages/virtual/latex-base) plus the specific packages.

Examples: [media-sound/lilypond](https://packages.gentoo.org/packages/media-sound/lilypond).
TODO: FILEBOX

#### Build time dependencies for packages like PDF manuals

If a package requires TeX only at build time, it should depend on [virtual/latex-base](https://packages.gentoo.org/packages/virtual/latex-base) and if required also depend on specific tex packages.

Examples: [app-doc/kicad-doc](https://packages.gentoo.org/packages/app-doc/kicad-doc) and [media-gfx/inkscape](https://packages.gentoo.org/packages/media-gfx/inkscape).

**`media-gfx/inkscape*.ebuild`**

```
RDEPEND="
    latex? (
        virtual/latex-base
        media-gfx/pstoedit[plotutils]
        app-text/dvipsk
    )
"
```
### Migrate SRC\_URI from CTAN to TexLive (draft)

It is a common problem for our ebuilds that the CTAN server provides

1. only the latest version of a package
2. this package sometimes has no version information in the filename (the new version overwrites the old version)

The Gentoo mirror was misused as primary source as workaround for many TeX ebuilds.

**`example.ebuild`**

```
SRC_URI="mirror://gentoo/${P}.zip"
..
```
Sometimes the developer webspace was used, which is much cleaner but not ideal too.

We should migrate to the texlive ftp server [ftp://tug.org/historic/systems/texlive/](ftp://tug.org/historic/systems/texlive/)

**`example.ebuild`**

Enables us to create a generic SRC\_URI

**`example.ebuild`**

```
SRC_URI="ftp://tug.org/historic/systems/texlive/${PV}/tlnet-final/archive/${PN}.tar.xz"
```
### VARTEXFONTS

kpathsea sets the path for font cache generation to (???) which is outside of the sandbox.

Setting VARTEXFONTS=${T}/fonts prevents sandbox violations. 
See also the Tracker [bug #223077](https://bugs.gentoo.org/show_bug.cgi?id=223077)

### Notes to be written

- Procedure how to test a latex ebuild
