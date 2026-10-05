<!-- source: https://wiki.gentoo.org/wiki/Lumina | group: Gentoo Wiki (Main) | wiki-title: Lumina -->
---
title: Lumina
url: https://wiki.gentoo.org/wiki/Lumina
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-21"
fingerprint: bbd3764ccbb5f99b
license: CC BY-SA 4.0
---

# Lumina

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is

**archived (obsolete)**. Contents are surely incorrect for current usage, and are intended for historical reference only.

TLDR:

**Do not use this article!**

**Lumina** is a lightweight [desktop environment](https://wiki.gentoo.org/wiki/Desktop_environment), free of [D-Bus](https://wiki.gentoo.org/wiki/D-Bus) and \*kit, designed to have as few system dependencies and requirements as possible.

## Installation

The [x11-wm/lumina](https://packages.gentoo.org/packages/x11-wm/lumina) package is configurable by one [USE flag](https://wiki.gentoo.org/wiki/USE_flag):


To install Lumina desktop, run:

`root #``emerge --ask x11-wm/lumina`
## Configuration

A configuration file is installed in /etc/luminaDesktop.conf. Lumina also has a bunch of own configuration tools.

## Invocation

Lumina provides its own replacement for [startx](https://wiki.gentoo.org/wiki/Xorg/Guide#Using_startx) to be started from console.

`user $``start-lumina-desktop`
Alternatively it can be added to the \~./xinitrc file for being started via [startx](https://wiki.gentoo.org/wiki/Xorg/Guide#Using_startx) or a [display manager](https://wiki.gentoo.org/wiki/Display_manager)

**`~/.xinitrc`**

```
[[ -f ~/.Xresources ]] && xrdb -merge -I$HOME ~/.Xresources
exec start-lumina-desktop
```
