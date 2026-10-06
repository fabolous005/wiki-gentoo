<!-- source: https://wiki.gentoo.org/wiki/GIMP | group: Gentoo Wiki (Main) | wiki-title: GIMP -->
---
title: GIMP
url: https://wiki.gentoo.org/wiki/GIMP
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-21"
fingerprint: "6e018b5cd0c639c4"
license: CC BY-SA 4.0
---

# GIMP

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**GIMP** is the **GNU I**mage **M**anipulation **P**rogram. It can be used as a simple paint tool, and for photo retouching and general image manipulation.

## Installation

`root #``emerge --ask media-gfx/gimp`
If the GIMP help pages (documentation) is desired, then also emerge the [app-doc/gimp-help](https://packages.gentoo.org/packages/app-doc/gimp-help) package:

`root #``emerge --ask app-doc/gimp-help`
### USE flags


### USE flags for
            [media-gfx/gimp](https://packages.gentoo.org/packages/media-gfx/gimp)
            
            GNU Image Manipulation Program

| [X](https://packages.gentoo.org/useflags/X) | Add support for X11 | 
| [aalib](https://packages.gentoo.org/useflags/aalib) | Add support for media-libs/aalib (ASCII-Graphics Library) | 
| [alsa](https://packages.gentoo.org/useflags/alsa) | Enable ALSA support in midi input controller | 
| [aqua](https://packages.gentoo.org/useflags/aqua) | Include support for the Mac OS X Aqua (Carbon/Cocoa) GUI | 
| [bash-completion](https://packages.gentoo.org/useflags/bash-completion) | Enable bash-completion support | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [fits](https://packages.gentoo.org/useflags/fits) | Enable support for loading/exporting FITS images | 
| [gnome](https://packages.gentoo.org/useflags/gnome) | Add GNOME support | 
| [heif](https://packages.gentoo.org/useflags/heif) | Enable support for ISO/IEC 23008-12:2017 HEIF/HEIC image format | 
| [javascript](https://packages.gentoo.org/useflags/javascript) | Enable javascript support | 
| [jpeg2k](https://packages.gentoo.org/useflags/jpeg2k) | Support for JPEG 2000, a wavelet-based image compression format | 
| [jpegxl](https://packages.gentoo.org/useflags/jpegxl) | Add JPEG XL image support | 
| [lua](https://packages.gentoo.org/useflags/lua) | Enable Lua scripting support | 
| [mng](https://packages.gentoo.org/useflags/mng) | Add support for libmng (MNG images) | 
| [openexr](https://packages.gentoo.org/useflags/openexr) | Support for the OpenEXR graphics file format | 
| [openmp](https://packages.gentoo.org/useflags/openmp) | Build support for the OpenMP (support parallel computing), requires >=sys-devel/gcc-4.2 built with USE="openmp" | 
| [postscript](https://packages.gentoo.org/useflags/postscript) | Enable support for the PostScript language (often with ghostscript-gpl or libspectre) | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [udev](https://packages.gentoo.org/useflags/udev) | Enable virtual/udev integration (device discovery, power and storage device support, etc) | 
| [unwind](https://packages.gentoo.org/useflags/unwind) | Add support for call stack unwinding and function name resolution | 
| [vala](https://packages.gentoo.org/useflags/vala) | Enable bindings for dev-lang/vala | 
| [vector-icons](https://packages.gentoo.org/useflags/vector-icons) | Enable support for vector icons (experimental) | 
| [wayland](https://packages.gentoo.org/useflags/wayland) | Enable dev-libs/wayland backend | 
| [webp](https://packages.gentoo.org/useflags/webp) | Add support for the WebP image format | 
| [wmf](https://packages.gentoo.org/useflags/wmf) | Add support for the Windows Metafile vector image format | 
| [xpm](https://packages.gentoo.org/useflags/xpm) | Add support for XPM graphics format | 

A full list of all USE linguas can be seen by using the equery tool (part of [app-portage/gentoolkit](https://packages.gentoo.org/packages/app-portage/gentoolkit)):

`user $``equery uses media-gfx/gimp`
### Plugins

For some GIMP plugins that are included in the Portage tree, please see the [GIMP Plugins sub-article](https://wiki.gentoo.org/wiki/GIMP/Plugins).

## Troubleshooting

### Non white on black fonts look terrible

![](https://wiki.gentoo.org/images/thumb/2/29/Gimp-fonts-anti-aliasing-trouble.png/300px-Gimp-fonts-anti-aliasing-trouble.png)

If non-white fonts look terrible in GIMP, try turning off sub-pixel hinting by creating fonts.conf in the GIMP user configuration directory (e.g. $HOME/.gimp-2.8/).

FILE **`$HOME/.gimp-2.8/fonts.conf`****Turns off sub-pixel hinting for GIMP**

```
<fontconfig>
  <match target="font">
    <edit name="rgba" mode="assign">
      <const>none</const>
    </edit>
  </match>
</fontconfig>
```
## External resources

- [Arch Linux Forum post](https://bbs.archlinux.org/viewtopic.php?pid=1486825#p1486825) - a proposed solution for wrong looking fonts in GIMP
