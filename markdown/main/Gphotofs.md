<!-- source: https://wiki.gentoo.org/wiki/Gphotofs | group: Gentoo Wiki (Main) | wiki-title: Gphotofs -->
---
title: Gphotofs
url: https://wiki.gentoo.org/wiki/Gphotofs
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-10-03"
fingerprint: ab1491769d9c1999
license: CC BY-SA 4.0
---

# Gphotofs

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

gphotofs is a FUSE file system for interfacing with digital cameras using gphoto2. Most modern mobile phones are cameras at the same time, and gphotofs can be a good alternative to [MTPfs](https://wiki.gentoo.org/wiki/MTPfs) or [go-mtpfs](https://wiki.gentoo.org/wiki/Go-mtpfs).

## Installation

### Kernel

See the [MTP](https://wiki.gentoo.org/wiki/MTP) meta article or the [FUSE](https://wiki.gentoo.org/wiki/FUSE) article for instructions on enabling FUSE support in the Linux kernel.

### Emerge

Install [media-gfx/gphotofs](https://packages.gentoo.org/packages/media-gfx/gphotofs):

`root #``emerge --ask media-gfx/gphotofs`
## Usage

`user $````
mkdir ~/AndroidDevice
```
`user $````
gphotofs ~/AndroidDevice -o allow_other
```
Unmount:

`user $``fusermount -u ~/AndroidDevice`
