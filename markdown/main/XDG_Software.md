<!-- source: https://wiki.gentoo.org/wiki/XDG/Software | group: Gentoo Wiki (Main) | wiki-title: XDG/Software -->
---
title: XDG/Software
url: https://wiki.gentoo.org/wiki/XDG/Software
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-14"
fingerprint: ff19b0da28a634ab
license: CC BY-SA 4.0
---

# XDG/Software

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page describes various command-line programs for managing and using default applications for particular [MIME](https://en.wikipedia.org/wiki/MIME) types, in the context of standards/specifications by [freedesktop.org](https://freedesktop.org), previously known as the [X Desktop Group](https://wiki.gentoo.org/wiki/XDG).

Most of the programs described in this page are provided by the [x11-misc/xdg-utils](https://packages.gentoo.org/packages/x11-misc/xdg-utils) package; exceptions are noted.

## Programs

### xdg-open(1)

The [xdg-open(1)](https://man.archlinux.org/man/xdg-open.1.en) [program can be used to open a file or URL in the default application for that resource:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`user $``xdg-open file.txt`
This will open file.txt in the preferred application for `text/plain` files, e.g. Kate or GEdit.

This will open [https://www.gentoo.org/](https://www.gentoo.org/)

### xdg-mime(1)

The [xdg-mime(1)](https://man.archlinux.org/man/xdg-mime.1.en) [program is the primary way to manage default desktop applications from the command line.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

It can be used to query the MIME type of file:

`user $``xdg-mime query filetype image.ext`
image/tiff

xdg-mime can also be used to query the default application for a particular MIME type:

`user $``xdg-mime query default image/tiff`
org.pwmt.zathura.desktop

To set the default application for a MIME type, specify the relevant .desktop file:

`user $``xdg-mime default nsxiv.desktop image/tiff`
Available .desktop files can be found in /usr/share/applications/ and (by default) \~/.local/share/applications/. However, as the [xdg-mime(1)](https://man.archlinux.org/man/xdg-mime.1.en) [man page notes,](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

\[t\]he application's desktop file must list support for all the MIME types that it wishes to be the default handler for.


### xdg-settings(1)

The [xdg-settings(1)](https://man.archlinux.org/man/xdg-settings.1.en) [program can be used to set the default browser:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`user $``xdg-settings set default-web-browser firefox-bin.desktop`
It can also be used to set the default handler for a particular URL scheme, e.g. `mailto`:

`user $``xdg-settings set default-url-scheme-handler mailto balsa.desktop`
## See also

- [XDG](https://wiki.gentoo.org/wiki/XDG) — the X Desktop Group, now known as [freedesktop.org](https://freedesktop.org)
- [XDG/Base Directories](https://wiki.gentoo.org/wiki/XDG/Base_Directories) — standard directories specified by [freedesktop.org](https://freedesktop.org) (formerly the [X Desktop Group](https://wiki.gentoo.org/wiki/XDG))
- [XDG/xdg-desktop-portal](https://wiki.gentoo.org/wiki/XDG/xdg-desktop-portal) — a frontend to implementations of the xdg-desktop-portal interface.
