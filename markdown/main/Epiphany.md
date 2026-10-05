<!-- source: https://wiki.gentoo.org/wiki/Epiphany | group: Gentoo Wiki (Main) | wiki-title: Epiphany -->
---
title: Epiphany
url: https://wiki.gentoo.org/wiki/Epiphany
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-08-16"
fingerprint: e5f0ad36b109b977
license: CC BY-SA 4.0
---

# Epiphany

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Epiphany**, sometimes called simply **web**, is a simple, fast Webkit-based web browser built for [GNOME](https://wiki.gentoo.org/wiki/GNOME) and [Pantheon](https://wiki.gentoo.org/wiki/Pantheon). It's main goal is to create a web browser that is [free](https://en.wikipedia.org/wiki/Free_software), open source, aesthetically pleasing and clean. It comes with a built-in adblocker and [Intelligent Tracking Prevention](https://webkit.org/tracking-prevention/) by default.

## Installation

### USE flags


### Emerge

`root #``emerge --ask www-client/epiphany`
### Flatpak

Epiphany also offers a flatpak version. It is the recommended way of installing epiphany by the developers.

Install Flatpak:

`root #``emerge --ask sys-apps/flatpak`
Install epiphany from flathub.

`root #``flatpak install flathub org.gnome.Epiphany`
Run through the command line:

`user $``flatpak run org.gnome.Epiphany`
See [Flatpak](https://wiki.gentoo.org/wiki/Flatpak) for more information.
