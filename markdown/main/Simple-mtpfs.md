<!-- source: https://wiki.gentoo.org/wiki/Simple-mtpfs | group: Gentoo Wiki (Main) | wiki-title: Simple-mtpfs -->
---
title: Simple-mtpfs
url: https://wiki.gentoo.org/wiki/Simple-mtpfs
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-03-14"
fingerprint: "87511bbef9d6e187"
license: CC BY-SA 4.0
---

# Simple-mtpfs

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Simple MTP FUSE filesystem driver written in C++.

## Installation

### Kernel

See the [MTP](https://wiki.gentoo.org/wiki/MTP) meta article or the [FUSE](https://wiki.gentoo.org/wiki/FUSE) article for instructions on enabling FUSE support in the Linux kernel.

### Emerge

Install [sys-fs/simple-mtpfs](https://packages.gentoo.org/packages/sys-fs/simple-mtpfs):

`root #``emerge --ask sys-fs/simple-mtpfs`
### Usage

`user $````
mkdir ~/AndroidDevice
```
`user $````
simple-mtpfs ~/AndroidDevice
```
Unmount:

`user $``fusermount -u ~/AndroidDevice`
