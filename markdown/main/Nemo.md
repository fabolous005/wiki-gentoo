<!-- source: https://wiki.gentoo.org/wiki/Nemo | group: Gentoo Wiki (Main) | wiki-title: Nemo -->
---
title: Nemo
url: https://wiki.gentoo.org/wiki/Nemo
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-05-01"
fingerprint: de41d93cd9e73cc4
license: CC BY-SA 4.0
---

# Nemo

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Nemo** is a fork of [GNOME](https://wiki.gentoo.org/wiki/GNOME)'s [Nautilus](https://wiki.gentoo.org/index.php?title=Nautilus&action=edit&redlink=1) file manager for [Cinnamon](https://wiki.gentoo.org/wiki/Cinnamon). Like the rest of Cinnamon's forks of GNOME software it preserved the more traditional GUI style while being actively developed to keep up with modern features.

## Installation

### USE flags


| [+nls](https://packages.gentoo.org/useflags/+nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [exif](https://packages.gentoo.org/useflags/exif) | Add support for reading EXIF headers from JPEG and TIFF images | 
| [gtk-doc](https://packages.gentoo.org/useflags/gtk-doc) | Build and install gtk-doc based developer documentation for dev-util/devhelp, IDE and offline use | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [tracker](https://packages.gentoo.org/useflags/tracker) | Add support for app-misc/tinysparql search | 
| [wayland](https://packages.gentoo.org/useflags/wayland) | Enable dev-libs/wayland backend | 
| [xmp](https://packages.gentoo.org/useflags/xmp) | Enable support for Extensible Metadata Platform (Adobe XMP) | 

### Emerge

**Nemo** can be easily installed via emerge:

`root #``emerge --ask gnome-extra/nemo`
## See also

- [File managers](https://wiki.gentoo.org/wiki/File_managers) — a computer program that allows for the manipulation of files and directories on a computer's [filesystem](https://wiki.gentoo.org/wiki/Filesystem).
