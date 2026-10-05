<!-- source: https://wiki.gentoo.org/wiki/Gsettings | group: Gentoo Wiki (Main) | wiki-title: Gsettings -->
---
title: Gsettings
url: https://wiki.gentoo.org/wiki/Gsettings
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-01-26"
fingerprint: f3f596e9ec2122d5
license: CC BY-SA 4.0
---

# Gsettings

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

gsettings is a command-line program allowing getting and setting configuration values in a [dconf](https://en.wikipedia.org/wiki/dconf) database. Unlike the [dconf(1)](https://man.archlinux.org/man/dconf.1.en) [tool,](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [gsettings(1)](https://man.archlinux.org/man/gsettings.1.en) [performs type and consistency checks. gsettings is provided by the](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [dev-libs/glib](https://packages.gentoo.org/packages/dev-libs/glib) package.

Collections of configuration data are called *schemas*; the [gsettings-desktop-schemas](https://packages.gentoo.org/packages/gsettings-desktop-schemas) package provides various schemas for the [GNOME](https://wiki.gentoo.org/wiki/GNOME) desktop.

## Usage

List installed schemas:

`user $``gsettings list-schemas`
List the keys associated with a particular schema, including their current values:

`user $``gsettings list-recursively org.gnome.desktop.interface`
Describe the meaning of a particular key, e.g. `gtk-theme`:

`user $``gsettings describe org.gnome.desktop.interface gtk-theme`
Get the current value of a particular key, e.g. `text-scaling-factor`:

`user $``gsettings get org.gnome.desktop.interface text-scaling-factor`
Set the current value of a particular key, e.e. `text-scaling-factor`:

`user $``gsettings set org.gnome.desktop.interface text-scaling-factor 2.0`
## See also

- [GTK](https://wiki.gentoo.org/wiki/GTK) — a toolkit for creating graphical user interfaces.
- [GNOME](https://wiki.gentoo.org/wiki/GNOME) — a feature-rich desktop environment provided by the [GNOME project](https://www.gnome.org).
