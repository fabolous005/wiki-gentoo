<!-- source: https://wiki.gentoo.org/wiki/Aufs | group: Gentoo Wiki (Main) | wiki-title: Aufs -->
---
title: Aufs
url: https://wiki.gentoo.org/wiki/Aufs
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-19"
fingerprint: "9ed5473f9cae1d33"
license: CC BY-SA 4.0
---

# Aufs

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

As of

**2020-12-29**, this article is

**deprecated (obsolete)**. Contents are

<u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

**Aufs** (**A**nother **U**nion **F**ile **S**ystem) is an advanced multi-layered unification filesystem. Aufs was originally a re-design and re-implementation of the popular [UnionFS](https://wiki.gentoo.org/index.php?title=UnionFS&action=edit&redlink=1), however after adding many new original ideas it became entirely separate from UnionFS. Aufs is considered a UnionFS alternative since it supports many of the same features.

## Features

- The ability to unite several directories into a single virtual filesystem. Calling the member directory as a branch;
- Specification of the permission flags on each branch (readonly, readwrite, and whiteout-able);
- Via upper writable branch, internal copyup and whiteout is possible (files and directories on the readonly branch are logically modifiable);
- Dynamic branch manipulation (add, delete, etc.)

## Installation

Emerge the pre-patched ([sys-kernel/aufs-sources](https://packages.gentoo.org/packages/sys-kernel/aufs-sources)) package. This will install another set of kernel sources that have had the Aufs4 patches applied. The new sources will show up in /usr/src using a *aufs* suffixed name scheme.

If aufs sources are not yet emerged on the system, do so presently:

`root #``emerge --ask sys-kernel/aufs-sources`
After the emerge process is finished, list the available sources:

`root #``eselect kernel list`
\[1\]   linux-4.14.91-gentoo
  \[2\]   linux-4.14.91-aufs

Use eselect to set the symlink to the aufs kernel sources:

`root #``eselect kernel set 2`
### Kernel

**Enabling support for Aufs**

After the features have been set, build the kernel following the [kernel configuration guide](https://wiki.gentoo.org/wiki/Kernel/Configuration#Build).

## Configuration

In order to manage aufs, the [sys-fs/aufs-util](https://packages.gentoo.org/packages/sys-fs/aufs-util) package is needed.

`root #``emerge --ask sys-fs/aufs-util`
## Usage

Check [man aufs](http://aufs.sourceforge.net/aufs4/man.html)
