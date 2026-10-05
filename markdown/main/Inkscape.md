<!-- source: https://wiki.gentoo.org/wiki/Inkscape | group: Gentoo Wiki (Main) | wiki-title: Inkscape -->
---
title: Inkscape
url: https://wiki.gentoo.org/wiki/Inkscape
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-08-31"
fingerprint: "76010a5c51d638cc"
license: CC BY-SA 4.0
---

# Inkscape

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Inkscape** is a free and open source [vector graphics](https://en.wikipedia.org/wiki/vector_graphics) editor for GNU/Linux, Windows and macOS.

## Installation

### USE flags

The [app-text/dblatex](https://packages.gentoo.org/packages/app-text/dblatex) package has a local `inkscape` [USE flag](https://packages.gentoo.org/useflags/inkscape).

| [X](https://packages.gentoo.org/useflags/X) | Add support for X11 | 
| [cdr](https://packages.gentoo.org/useflags/cdr) | Enable support for CorelDRAW files via media-libs/libcdr | 
| [dia](https://packages.gentoo.org/useflags/dia) | Enable DIA flow chart import via app-office/dia | 
| [exif](https://packages.gentoo.org/useflags/exif) | Add support for reading EXIF headers from JPEG and TIFF images | 
| [graphicsmagick](https://packages.gentoo.org/useflags/graphicsmagick) | Build and link against GraphicsMagick instead of ImageMagick (requires USE=imagemagick if optional) | 
| [imagemagick](https://packages.gentoo.org/useflags/imagemagick) | Enable optional support for the ImageMagick or GraphicsMagick image converter | 
| [inkjar](https://packages.gentoo.org/useflags/inkjar) | Enable support for OpenOffice.org SVG jar files | 
| [jpeg](https://packages.gentoo.org/useflags/jpeg) | Add JPEG image support | 
| [openmp](https://packages.gentoo.org/useflags/openmp) | Build support for the OpenMP (support parallel computing), requires >=sys-devel/gcc-4.2 built with USE="openmp" | 
| [postscript](https://packages.gentoo.org/useflags/postscript) | Enable support for the PostScript language (often with ghostscript-gpl or libspectre) | 
| [readline](https://packages.gentoo.org/useflags/readline) | Enable support for libreadline, a GNU line-editing library that almost everyone wants | 
| [sourceview](https://packages.gentoo.org/useflags/sourceview) | Enable syntax highlighting support via x11-libs/gtksourceview | 
| [spell](https://packages.gentoo.org/useflags/spell) | Add dictionary support | 
| [svg2](https://packages.gentoo.org/useflags/svg2) | Enable support for new SVG2 features | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [visio](https://packages.gentoo.org/useflags/visio) | Enable support for Microsoft Visio diagrams via media-libs/libvisio | 
| [wayland](https://packages.gentoo.org/useflags/wayland) | Enable dev-libs/wayland backend | 
| [wpg](https://packages.gentoo.org/useflags/wpg) | Enable support for WordPerfect graphics via app-text/libwpg | 

### Emerge

`root #``emerge --ask media-gfx/inkscape`
## See also

- [GIMP](https://wiki.gentoo.org/wiki/GIMP) - the GNU Image Manipulation Program; can be used as a simple paint tool, photo retouching and general image manipulation.
