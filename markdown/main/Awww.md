<!-- source: https://wiki.gentoo.org/wiki/Awww | group: Gentoo Wiki (Main) | wiki-title: Awww -->
---
title: awww
url: https://wiki.gentoo.org/wiki/Awww
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-13"
fingerprint: "2bd554950395dbf5"
license: CC BY-SA 4.0
---

# awww

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


*Formerly known as swww.*

**awww** is an Answer to your Wayland Wallpaper Woes. It supports setting wallpapers without restarting the daemon, and animated wallpapers.

## Installation

### Emerge

`root #``emerge --ask gui-apps/awww`
### Usage

Start [awww-daemon(1)](https://man.archlinux.org/man/awww-daemon.1.en) [(e.g. via the compositor's configuration file).](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

Set the wallpaper by using the `img` command of [awww(1)](https://man.archlinux.org/man/awww.1.en)[, described in](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [awww-img(1)](https://man.archlinux.org/man/awww-img.1.en)[, e.g.:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`user $``awww img image.gif`
Stop the daemon via the `kill` command:

`user $``awww kill`
