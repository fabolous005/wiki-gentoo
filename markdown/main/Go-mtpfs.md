<!-- source: https://wiki.gentoo.org/wiki/Go-mtpfs | group: Gentoo Wiki (Main) | wiki-title: Go-mtpfs -->
---
title: Go-mtpfs
url: https://wiki.gentoo.org/wiki/Go-mtpfs
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-21"
fingerprint: bf51a83699c459a8
license: CC BY-SA 4.0
---

# Go-mtpfs

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Go-mtpfs is a simple FUSE-based filesystem written in Go language for mounting Android devices as a MTP device.

## Installation

### Prerequisites

Allow live builds for two packages in /etc/portage/package.accept\_keywords:

FILE **`/etc/portage/package.accept_keywords`**

```
dev-libs/go-fuse **
sys-fs/go-mtpfs **
```
### Kernel

See the [MTP](https://wiki.gentoo.org/wiki/MTP) meta article or the [FUSE](https://wiki.gentoo.org/wiki/FUSE) article for instructions on enabling FUSE support in the Linux kernel.

### Emerge

Install [sys-fs/go-mtpfs](https://packages.gentoo.org/packages/sys-fs/go-mtpfs):

`root #``emerge --ask sys-fs/go-mtpfs`
## Configuration

Appropriate users need to be in the `plugdev` group:

`root #``gpasswd -a <USER_NAME> plugdev`
## Usage

`user $````
mkdir ~/AndroidDevice
```
`user $````
go-mtpfs ~/AndroidDevice &
```
Note: If go-mtpfs is not ran in the background (with `&` at the end), another console will be needed to browse the device and unmount the device (when finished).

Unmount:

`user $``fusermount -u ~/AndroidDevice`
When the device is unmount, go-mtpfs will quit.

### Bugs

1. [dev-libs/go-fuse-9999](https://packages.gentoo.org/packages/dev-libs/go-fuse-9999) right now has a bug [638912](https://bugs.gentoo.org/638912) that prevents it from building.
2. MTP is very unreliable on some devices, old files & directories keep showing up, new ones don't get updated. One way to update media storage database is through [SD Scanner](https://f-droid.org/en/packages/com.gmail.jerickson314.sdscanner/).
