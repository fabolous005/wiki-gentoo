<!-- source: https://wiki.gentoo.org/wiki/JFS | group: Gentoo Wiki (Main) | wiki-title: JFS -->
---
title: JFS
url: https://wiki.gentoo.org/wiki/JFS
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-03-11"
fingerprint: "3f89583cf3b44c70"
license: CC BY-SA 4.0
---

# JFS

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**JFS** (**J**ournaled **F**ile **S**ystem) is a 64-bit journaling [filesystem](https://wiki.gentoo.org/wiki/Filesystem) created by IBM. An implementation for the Linux kernel is available as free software under the terms of the GNU General Public License. It is low on resource usage and comparatively fast doing all kinds of filesystem operations (as opposed to being specialized in some, e.g. [XFS](https://wiki.gentoo.org/wiki/XFS) is fast with big files, but slower with small ones). As such JFS is especially good for usage with battery-powered devices such as laptops.

## Installation

### Kernel

JFS is supported in the standard Linux kernel:

**Enabling JFS support**

Optional JFS features:

**Adding optional JFS features**

### Emerge

Filesystem utilities are available in the [sys-fs/jfsutils](https://packages.gentoo.org/packages/sys-fs/jfsutils) package:

`root #``emerge --ask sys-fs/jfsutils`
## Usage

### Creation

`root #``mkfs.jfs /dev/sda1`
### Mount

`root #``mount -t jfs /dev/sda1 /path/to/mountpoint`
### Extracting a Fsck Log

jfs\_fscklog can extract the fsck log from a JFS device.

`root #``jfs_fscklog -d /dev/sda1 -f fsck.log`
### Tuning

jfs\_tune can change different parameters, to change the UUID:

`root #``jfs_tune -l -U random /dev/sda1`
## Utilities

| Utility | Description <sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> | Man page | 
|---|---|---|
| fsck.jfs | A hard link to jfs\_fsck. |  | 
| jfs\_fsck | Checks a JFS filesystem for corruption. | [jfs\_fsck(8)](https://man.archlinux.org/man/jfs_fsck.8.en)  | 
| mkfs.jfs | A hard link to jfs\_mkfs. |  | 
| jfs\_mkfs | Creates a new JFS filesystem. | [jfs\_fsck(8)](https://man.archlinux.org/man/jfs_fsck.8.en)  | 
| jfs\_debugfs | A utility to perform low-level actions on a JFS filesystem. | [jfs\_debugfs(8)](https://man.archlinux.org/man/jfs_debugfs.8.en)  | 
| jfs\_fscklog | Extracts the fsck log from a JFS filesystem. | [jfs\_fscklog(8)](https://man.archlinux.org/man/jfs_fscklog.8.en)  | 
| jfs\_logdump | Dumps the journal of a filesystem into ./jfslog.dmp. | [jfs\_logdump(8)](https://man.archlinux.org/man/jfs_logdump.8.en)  | 
| jfs\_tune | Adjusts tunable parameters of a filesystem. | [jfs\_tune(8)](https://man.archlinux.org/man/jfs_tune.8.en)  | 

## Troubleshooting

### Fsck

To check a JFS filesystem for corruption, run fsck.jfs:

`root #``fsck.jfs /dev/sda1`
### Debugfs

jfs\_debugfs can be used to perform low-level actions on a JFS filesystem.

In this example, a JFS filesystem with the layout:

First, the inode needs to be known for the root of the directory.

`user $``ls -id`
2 .

Next, enter the debugfs interface with jfs\_debugfs:

`root #``jfs_debugfs /dev/sda1`
Now list the directory using the inode number:

`>``dir 2`
idotdot = 2
 
4096	test

**4096** is the inode of the test directory, now list the contents of that directory:

`>``dir 4096`
idotdot = 2
 
4097	a
4098	b
4099	c

To see everything that the debugfs interface do, read the man-page [jfs\_debugfs(8)](https://man.archlinux.org/man/jfs_debugfs.8.en)[.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

## See also

- [XFS](https://wiki.gentoo.org/wiki/XFS) — a high-performance journaling [filesystem](https://wiki.gentoo.org/wiki/Filesystem)
- [Ext4](https://wiki.gentoo.org/wiki/Ext4) — an open source disk [filesystem](https://wiki.gentoo.org/wiki/Filesystem) and the most recent version of the extended series of filesystems.
