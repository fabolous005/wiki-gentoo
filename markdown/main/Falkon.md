<!-- source: https://wiki.gentoo.org/wiki/Falkon | group: Gentoo Wiki (Main) | wiki-title: Falkon -->
---
title: Falkon
url: https://wiki.gentoo.org/wiki/Falkon
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-05-18"
fingerprint: ce40571151dfb16c
license: CC BY-SA 4.0
---

# Falkon

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Falkon** (formerly QupZilla) is a lightweight web browser based on QtWebEngine. Being quite lean, Falkon is visually attractive and functional. It excellently integrates into [desktop environments](https://wiki.gentoo.org/wiki/Desktop_environment) and looks good on [KDE](https://wiki.gentoo.org/wiki/KDE) and [LXQt](https://wiki.gentoo.org/wiki/LXQt).

## Installation

### USE flags


| [+X](https://packages.gentoo.org/useflags/+X) | Add support for X11 | 
| [dbus](https://packages.gentoo.org/useflags/dbus) | Enable dbus support for anything that needs it (gpsd, gnomemeeting, etc) | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [kde](https://packages.gentoo.org/useflags/kde) | Add support for software made by KDE, a free software community | 
| [python](https://packages.gentoo.org/useflags/python) | Add optional support/bindings for the Python language | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

Install [www-client/falkon](https://packages.gentoo.org/packages/www-client/falkon):

`root #``emerge --ask www-client/falkon`
## See also

- [Chromium](https://wiki.gentoo.org/wiki/Chromium) — the open source browser that [Google Chrome](https://wiki.gentoo.org/wiki/Google_Chrome) and many other browsers are based on.
- [Vivaldi](https://wiki.gentoo.org/wiki/Vivaldi) — a browser for our friends.
