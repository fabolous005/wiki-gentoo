<!-- source: https://wiki.gentoo.org/wiki/ReiserFS | group: Gentoo Wiki (Main) | wiki-title: ReiserFS -->
---
title: ReiserFS
url: https://wiki.gentoo.org/wiki/ReiserFS
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-07-02"
fingerprint: "8d044fd1fdaddd50"
license: CC BY-SA 4.0
---

# ReiserFS

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**ReiserFS** is a journaling [filesystem](https://wiki.gentoo.org/wiki/Filesystem) for Linux licensed under the GPL-2 license and is the predecessor of [Reiser4](https://archive.kernel.org/oldwiki/reiser4.wiki.kernel.org). ReiserFS was originally developed by [Namesys](https://en.wikipedia.org/wiki/Namesys) and [Hans Reiser](https://en.wikipedia.org/wiki/Hans_Reiser), but is under maintenance by volunteers nowadays.

## Installation

### Kernel

Support for ReiserFS has to be enabled in the Kernel first.

**Enabling ReiserFS support**

### Emerge

Utilities for ReiserFS are available in [sys-fs/reiserfsprogs](https://packages.gentoo.org/packages/sys-fs/reiserfsprogs) package:

`root #``emerge --ask sys-fs/reiserfsprogs`
## Troubleshooting

### ReiserFS filesystem corruption issues

If the ReiserFS partition is corrupt, try booting the Gentoo Install CD and run reiserfsck --rebuild-tree on the corrupted filesystem. This should make the filesystem consistent again, although there may be some lost files or directories due to the corruption.

## See also

- [Ext4](https://wiki.gentoo.org/wiki/Ext4) — an open source disk [filesystem](https://wiki.gentoo.org/wiki/Filesystem) and the most recent version of the extended series of filesystems.
- [XFS](https://wiki.gentoo.org/wiki/XFS) — a high-performance journaling [filesystem](https://wiki.gentoo.org/wiki/Filesystem)
- [ZFS](https://wiki.gentoo.org/wiki/ZFS) — a next generation [filesystem](https://wiki.gentoo.org/wiki/Filesystem) created by Matthew Ahrens and Jeff Bonwick.
