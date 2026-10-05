<!-- source: https://wiki.gentoo.org/wiki/MuPDF | group: Gentoo Wiki (Main) | wiki-title: MuPDF -->
---
title: MuPDF
url: https://wiki.gentoo.org/wiki/MuPDF
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-11-21"
fingerprint: "4f43535c94c639cc"
license: CC BY-SA 4.0
---

# MuPDF

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

MuPDF is a free and open-source software framework written in C that implements a PDF, XPS, and EPUB parsing and rendering engine, that can work as a standalone pdf reader.  In Gentoo, several packages like [app-text/llpp](https://packages.gentoo.org/packages/app-text/llpp) or [app-text/zathura-pdf-mupdf](https://packages.gentoo.org/packages/app-text/zathura-pdf-mupdf) use it internally for PDF rendering.

## Installation

### USE flags


| [+javascript](https://packages.gentoo.org/useflags/+javascript) | Enable javascript support | 
| [+jpeg2k](https://packages.gentoo.org/useflags/+jpeg2k) | Support for JPEG 2000, a wavelet-based image compression format | 
| [X](https://packages.gentoo.org/useflags/X) | Add support for X11 | 
| [archive](https://packages.gentoo.org/useflags/archive) | Enable support for CBR and other archive formats using libarchive | 
| [barcode](https://packages.gentoo.org/useflags/barcode) | Enable support for barcode detection/generation for mutool using zxingcpp | 
| [brotli](https://packages.gentoo.org/useflags/brotli) | Enable Brotli compression support | 
| [opengl](https://packages.gentoo.org/useflags/opengl) | Add support for OpenGL (3D graphics) | 
| [ssl](https://packages.gentoo.org/useflags/ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 

The package provides the following binaries:

- mupdf
- mupdf-gl
- mupdf-x11
- mupdf-x11-curl
- mutool: all purpose tool for dealing with PDF files (draw, clean, extract, info, create, pages, poster, show, run JavaScript, convert, merge)

### Emerge

Install [app-text/mupdf](https://packages.gentoo.org/packages/app-text/mupdf):

`root #``emerge --ask app-text/mupdf`
## External resources

- [http://www.linuxfromscratch.org/blfs/view/cvs/pst/mupdf.html](http://www.linuxfromscratch.org/blfs/view/cvs/pst/mupdf.html)
- [https://packages.debian.org/buster/mupdf](https://packages.debian.org/buster/mupdf)
- [https://wiki.archlinux.org/index.php/MuPDF](https://wiki.archlinux.org/index.php/MuPDF)
- [https://www.mupdf.com/product/ecosystem](https://www.mupdf.com/product/ecosystem) — Overview of projects, viewers, tools, and libraries based on MuPDF
