<!-- source: https://wiki.gentoo.org/wiki/Eloi | group: Gentoo Wiki (Main) | wiki-title: Eloi -->
---
title: eloi
url: https://wiki.gentoo.org/wiki/Eloi
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-04"
fingerprint: "8b853e0e9bea3b62"
license: CC BY-SA 4.0
---

# eloi

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Eloi** is an ebuild searcher and installer ([eix](https://wiki.gentoo.org/wiki/Eix) with extra steps). Searches through all Gentoo's overlays provided by eselect repository and listed by Zugania's website.

Eloi can:

1. Find and install an ebuild package from any overlay
2. Enable ebuild overlays
3. More in the future...

## Installation

### Emerge

First, add the [ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository) from here: [https://codeberg.org/lordbaraa/gentoo-overlay](https://codeberg.org/lordbaraa/gentoo-overlay).

Install **app-portage/eloi** using emerge:

`root #``emerge --ask app-portage/eloi`
### Go's Installer

`user $``go install codeberg.org/lordbaraa/eloi@latest`
OR

`user $``go install github.com/mbaraa/eloi@latest`
## Usage

### Update local repositories' cache

These cache files are stored in /var/cache/eloi:

`root #``eloi --download`
### Find an ebuild

Using the `-S` or `--search` flag to find an ebuild, this will list all ebuilds that have `pulseaudio-equalizer` in their name, with other details, like version, overlay name, use flags, license:

`root #``eloi -S pulseaudio-equalizer`
Or:

`root #``eloi --search pulseaudio-equalizer`
### Enable an overlay repository

This can be done using the `--enable` flag:

`root #``eloi --enable underworld`
Or by installing a package from a repository that's not enabled on the system, for now just search for a package and install it, and **Eloi** will add its corresponding repository!
