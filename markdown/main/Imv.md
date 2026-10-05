<!-- source: https://wiki.gentoo.org/wiki/Imv | group: Gentoo Wiki (Main) | wiki-title: Imv -->
---
title: imv
url: https://wiki.gentoo.org/wiki/Imv
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-02-14"
fingerprint: "7603497451d639c6"
license: CC BY-SA 4.0
---

# imv

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**imv** is a free and open-source simple image viewer for [X11](https://wiki.gentoo.org/wiki/X11) and [Wayland](https://wiki.gentoo.org/wiki/Wayland). It can also be used for providing a desktop background for tiling [window managers](https://wiki.gentoo.org/wiki/Window_managers) like [i3](https://wiki.gentoo.org/wiki/I3).

## Installation

### USE flags


| [+X](https://packages.gentoo.org/useflags/+X) | Add support for X11 | 
| [+jpeg](https://packages.gentoo.org/useflags/+jpeg) | Add JPEG image support | 
| [+png](https://packages.gentoo.org/useflags/+png) | Add support for libpng (PNG images) | 
| [bmp](https://packages.gentoo.org/useflags/bmp) | Add bitmap (.bmp) image support using media-libs/libnsbmp | 
| [gif](https://packages.gentoo.org/useflags/gif) | Add GIF image support | 
| [heif](https://packages.gentoo.org/useflags/heif) | Enable support for ISO/IEC 23008-12:2017 HEIF/HEIC image format | 
| [icu](https://packages.gentoo.org/useflags/icu) | Enable ICU (Internationalization Components for Unicode) support, using dev-libs/icu | 
| [jpegxl](https://packages.gentoo.org/useflags/jpegxl) | Add JPEG XL image support | 
| [svg](https://packages.gentoo.org/useflags/svg) | Add support for SVG (Scalable Vector Graphics) | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [tiff](https://packages.gentoo.org/useflags/tiff) | Add support for the TIFF image format | 
| [wayland](https://packages.gentoo.org/useflags/wayland) | Enable dev-libs/wayland backend | 
| [webp](https://packages.gentoo.org/useflags/webp) | Add support for the WebP image format | 

### Emerge

Install [media-gfx/imv](https://packages.gentoo.org/packages/media-gfx/imv):

`root #``emerge --ask media-gfx/imv`
### Usage

Refer to the [imv(1)](https://man.archlinux.org/man/imv.1.en) [man page for information such as command-line options and default bindings. Refer to the](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [imv-msg(1)](https://man.archlinux.org/man/imv-msg.1.en) [man page for information about sending commands to imv via a socket.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

## See also

- [Feh](https://wiki.gentoo.org/wiki/Feh) — an open-source image viewer that is mainly aimed at command-line users.
