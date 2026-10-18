<!-- source: https://wiki.gentoo.org/wiki/Fractal | group: Gentoo Wiki (Main) | wiki-title: Fractal -->
---
title: Fractal
url: https://wiki.gentoo.org/wiki/Fractal
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-17"
fingerprint: "8a60d86c89a76914"
license: CC BY-SA 4.0
---

# Fractal

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Fractal** is a [Matrix](https://wiki.gentoo.org/wiki/Matrix) messaging app for [GNOME](https://wiki.gentoo.org/wiki/GNOME), written in [Rust](https://wiki.gentoo.org/wiki/Rust). Its interface is optimized for collaboration in large groups, such as free software projects, and will fit all screens, big or small.

## Instalation

### USE flags


### USE flags for
            [net-im/fractal](https://packages.gentoo.org/packages/net-im/fractal)
            
            Matrix messaging app for GNOME written in Rust

| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 

### Emerge

Fractal has a package in the official Gentoo repository.

To emerge it, run:

`root #``emerge --ask net-im/fractal`


### Flatpak

Fractal is also available as a [Flatpak](https://wiki.gentoo.org/wiki/Flatpak) package. Install it and later run it.

`user $``flatpak install flathub org.gnome.Fractal``user $``flatpak run org.gnome.Fractal`
