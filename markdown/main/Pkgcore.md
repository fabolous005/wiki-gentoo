<!-- source: https://wiki.gentoo.org/wiki/Pkgcore | group: Gentoo Wiki (Main) | wiki-title: Pkgcore -->
---
title: pkgcore
url: https://wiki.gentoo.org/wiki/Pkgcore
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-04-11"
fingerprint: "8f099edf8cc7bb02"
license: CC BY-SA 4.0
---

# pkgcore

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**pkgcore** is an alternative package manager for Gentoo that aims for high performance, extensibility, and a clean design. Written in Python, pkgcore provides an alternative to Portage's emerge command while also subsuming most of the functionality of the [eix](https://wiki.gentoo.org/wiki/Eix) and [eselect repository](https://wiki.gentoo.org/wiki/Eselect/Repository) repository management tools.

## Installation

### Emerge

Use the following emerge command to install the [sys-apps/pkgcore](https://packages.gentoo.org/packages/sys-apps/pkgcore) package:

`root #``emerge --ask sys-apps/pkgcore`
## Configuration

If there is no specific pkgcore configuration (that is /etc/pkgcore.conf or \~/.pkgcore.conf), it reads the Portage configuration and converts it transparently for use. For a detailed explanation of how the pkgcore specific configuration works see [Pkgcore/Configuration](https://wiki.gentoo.org/wiki/Pkgcore/Configuration).

When pkgcore is working in Portage compatibility mode, if /etc/portage/repos.conf is defined without mentioning the gentoo repository, which Portage can infer, pkgcore will fail. Refer to [Troubleshooting pkgcheck](https://wiki.gentoo.org/wiki/Pkgcheck#error:_repos.conf:_default_repo_.27gentoo.27_is_undefined_or_invalid) to move the gentoo repository's definition to Portage's repos.conf.

## Usage

### Updating the system

For synchronizing the local copies of the Portage tree and any overlays the pmaint utility is used. Then pmerge is invoked to update all packages and clean superfluous packages.

`root #````
pmaint sync
```
`root #````
pmerge --ask --upgrade --deep --newuse
```
`root #````
pmerge --ask --clean --with-bdeps
```
### Searching for a package

To search for a package use the pquery program:

`user $``pquery -nv <query>`
For example, to find some audio software for encoding ogg files, one may use the following to find [media-sound/vorbis-tools](https://packages.gentoo.org/packages/media-sound/vorbis-tools).

`user $``pquery -nv --description ogg 'media-sound/*'`
### Installing a package

A package can be installed with pmerge:

`root #``pmerge --ask --verbose <atoms>`
In our example this would be the following:

`root #``pmerge --ask --verbose media-sound/vorbis-tools`
pmerge is intended to be compatible with Portage. It should be possible to use both emerge and pmerge on the same system.

## Removal

`root #``emerge --ask --depclean --verbose sys-apps/pkgcore`
## See also

- [pkgcheck](https://wiki.gentoo.org/wiki/Pkgcheck) — a [pkgcore]-based QA utility for ebuild repos.
- [pkgdev](https://wiki.gentoo.org/wiki/Pkgdev) — a collection of tools for Gentoo development.
- [Portage](https://wiki.gentoo.org/wiki/Portage) — the official [package manager](https://en.wikipedia.org/wiki/Package_manager) and [distribution system](https://www.gentoo.org/get-started/about/) for Gentoo.
