<!-- source: https://wiki.gentoo.org/wiki/Tahoe-LAFS | group: Gentoo Wiki (Main) | wiki-title: Tahoe-LAFS -->
---
title: Tahoe-LAFS
url: https://wiki.gentoo.org/wiki/Tahoe-LAFS
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-06-16"
fingerprint: a74de95dfcbdc0cd
license: CC BY-SA 4.0
---

# Tahoe-LAFS

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Tahoe-LAFS** (**L**east **A**uthority **F**ile **S**ystem) is an encrypted, secure, distributed (fault-tolerant) file system). It does not require trust between parties in order to keep data safe and secure; the only caveat being involved parties must leave their system's connected to the storage network.

## Installation

### Kernel

(Optional section. Remove if not applicable.)

### USE flags

There are only a couple USE flags available for Tahoe-LAFS, neither of them incorporate additional functionality. Essentially when more documentation is desired make sure the `doc` flag is enabled in /etc/portage/package.use.



### Emerge

Install Tahoe:

`root #``emerge --ask net-fs/tahoe-lafs`
## Configuration

(Needs written...)

## Usage

(Needs written...)

### Invocation

Tahoe can be invoked via:

`user $``tahoe`
## Troubleshooting

### Emerge fails

emerge tahoe-lafs fails with a `pkg_resources.DistributionNotFound` error similar to the following:

The work around to this problem is to roll back the [dev-python/setuptools](https://packages.gentoo.org/packages/dev-python/setuptools) package to an earlier version. It appears version 12.0.1 which is currently marked as stable on **amd64** as an issue determining versioning for Python dependencies. Rolling back to version 7.0 should do the trick:

`root #``emerge --ask =dev-python/setuptools-7.0`
After the new version of [dev-python/setuptools](https://packages.gentoo.org/packages/dev-python/setuptools) is installed proceed with [installation process](https://wiki.gentoo.org#Installation) as normal.

## See also

- [Ceph](https://wiki.gentoo.org/wiki/Ceph) — a distributed object store and filesystem designed to provide excellent performance, reliability, and scalability.
- [ISCSI](https://wiki.gentoo.org/wiki/ISCSI) — an IP-based network standard and a [Storage Area Network](https://en.wikipedia.org/wiki/Storage_area_network) (SAN) protocol.

## External resources

- [https://www.linux.com/learn/tutorials/546799:weekend-project-get-started-with-tahoe-lafs-storage-grids](https://www.linux.com/learn/tutorials/546799:weekend-project-get-started-with-tahoe-lafs-storage-grids) - An article explaining what Tahoe-LAFS is and how it works.
- [https://www.lowendguide.com/3/networking/how-to-set-up-your-own-distributed-redundant-and-encrypted-storage-grid-in-a-few-easy-steps-tahoe-lafs/](https://www.lowendguide.com/3/networking/how-to-set-up-your-own-distributed-redundant-and-encrypted-storage-grid-in-a-few-easy-steps-tahoe-lafs/) - An independently written Tahoe-LAFS setup guide.
