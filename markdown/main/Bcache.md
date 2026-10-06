<!-- source: https://wiki.gentoo.org/wiki/Bcache | group: Gentoo Wiki (Main) | wiki-title: Bcache -->
---
title: bcache
url: https://wiki.gentoo.org/wiki/Bcache
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-11-30"
fingerprint: "174249549aa37909"
license: CC BY-SA 4.0
---

# bcache

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**bcache** is a Linux [kernel](https://wiki.gentoo.org/wiki/Kernel) block layer cache. It allows one or more fast disk drives such as flash-based solid state drives ([SSDs](https://wiki.gentoo.org/wiki/SSD)) to act as a cache for one or more slower disk drives.

## Installation

### Kernel

Activate the following kernel options:

KERNEL **Enable block device support in the Kernel (`CONFIG_BCACHE`)**

```
Device Drivers --->
   Multiple devices driver support (RAID and LVM) --->
      <*>   Block device as cache
```
### Emerge

Install [sys-fs/bcache-tools](https://packages.gentoo.org/packages/sys-fs/bcache-tools):

`root #``emerge --ask sys-fs/bcache-tools`
## See also

- [Bcachefs](https://wiki.gentoo.org/wiki/Bcachefs) — a fully-featured [B-tree](https://en.wikipedia.org/wiki/B-tree) [filesystem](https://wiki.gentoo.org/wiki/Filesystem) based on [bcache].

## External resources

- [Howto bcache](https://forums.gentoo.org/viewtopic-t-959542.html) - A Gentoo Forums thread on using bcache.
- [Arch wiki](https://wiki.archlinux.org/index.php/Bcache) - The Bcache article on found on the Arch wiki.
- [Patrick's Blog](http://gentooexperimental.org/~patrick/weblog/archives/2014-09.html#e2014-09-21T13_59_16.txt) -  [Patrick Lauer (Patrick)](https://wiki.gentoo.org/wiki/User:Patrick) , a Gentoo developer, wrote a few short entries on his blog concerning the use of bcache and a multi-disk SATA array back in September, 2014. Read about it here.
- [Pommi's blog - SSD caching using Linux and bcache](https://cloud-infra.engineer/ssd-caching-using-linux-and-bcache/) - A 2013 blog entry containing details of setting up bcache.
