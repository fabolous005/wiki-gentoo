<!-- source: https://wiki.gentoo.org/wiki/Compiz | group: Gentoo Wiki (Main) | wiki-title: Compiz -->
---
title: Compiz
url: https://wiki.gentoo.org/wiki/Compiz
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-11-14"
fingerprint: e295d066198cb34e
license: CC BY-SA 4.0
---

# Compiz

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Deprecated article**

This article is **deprecated (obsolete)**. Contents are <u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

Information on this page is outdated and at least needs to be updated with information for release 2.4.1 and newer.

**Compiz** is an open-source compositing [window manager](https://wiki.gentoo.org/wiki/Window_manager) for [X11](https://wiki.gentoo.org/wiki/X11).

## Install

### Preinstall

First compiz must be keyworded in, and USE flags set to pull in emerald:

**`/etc/portage/package.accept_keywords`**

**Emerald package.accept\_keywords example**

```
x11-plugins/compiz-plugins-main
x11-libs/compiz-bcop
x11-wm/compiz-fusion
x11-plugins/compiz-plugins-extra
x11-libs/libcompizconfig
x11-wm/compiz
dev-python/compizconfig-python
x11-libs/compizconfig-backend-gconf
x11-wm/emerald
x11-themes/emerald-themes
x11-apps/fusion-icon
x11-misc/ccsm
```
**`/etc/portage/package.use`**

**Emerald package.use example**

```
x11-wm/compiz-fusion emerald
```
### Backend

Install a backend that is appropriate for the system. Failure to do so might result in unstable compiz behavior. Possible backends include:

`root #``emerge --ask x11-libs/compizconfig-backend-gconf`
or

`root #``emerge --ask x11-libs/compizconfig-backend-kconfig4`
### Emerge

`root #``emerge --ask compiz-fusion fusion-icon ccsm`
## Setup

Press `Alt` + `F2`. Enter "fusion-icon". Then run.

Right click the fusion-icon and select settings manager.

Some options must be selected since ccsm's default configuration is empty. These should help set the system to be configured correctly.

Turn on

"Effects"

Window Decoration

"window management"

Move Window, Resize Window, & Application Switcher
