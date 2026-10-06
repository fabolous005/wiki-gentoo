<!-- source: https://wiki.gentoo.org/wiki/CurlFtpFS | group: Gentoo Wiki (Main) | wiki-title: CurlFtpFS -->
---
title: CurlFtpFS
url: https://wiki.gentoo.org/wiki/CurlFtpFS
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-10-03"
fingerprint: "1a96209adbc459dd"
license: CC BY-SA 4.0
---

# CurlFtpFS

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Article status**

This article has some todo items:

- Add instructions for using from fstab.
- Expand article.


**CurlFtpFS** allows for [mounting](https://wiki.gentoo.org/wiki/Mount) an FTP folder as a regular directory to the local directory tree.

## Installation

### Kernel

CurlFtpFS needs [FUSE](https://wiki.gentoo.org/wiki/FUSE) activated in the kernel:

KERNEL **Activating FUSE**

```
File systems --->
   <*> FUSE (Filesystem in Userspace) support
```
### Emerge

Install [net-fs/curlftpfs](https://packages.gentoo.org/packages/net-fs/curlftpfs):

`root #``emerge --ask net-fs/curlftpfs`
## Usage

### As a regular user

First, create a mount point:

`user $``mkdir ./ftp`
#### Mounting

Then mount the necessary *catalog* from the *server* to this mount point:

`user $``curlftpfs ftp://server/catalog/ ./ftp/ -o user=username:password,utf8`
#### Unmounting

`user $``fusermount -u ./ftp`
## See also

- [SSHFS](https://wiki.gentoo.org/wiki/SSHFS) — a secure shell client used to mount remote filesystems to local machines.
