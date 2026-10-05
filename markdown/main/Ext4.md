<!-- source: https://wiki.gentoo.org/wiki/Ext4 | group: Gentoo Wiki (Main) | wiki-title: Ext4 -->
---
title: ext4
url: https://wiki.gentoo.org/wiki/Ext4
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-10-03"
fingerprint: ad8909b4dab6cef0
license: CC BY-SA 4.0
---

# ext4

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**ext4** (fourth extended file system) is an open source disk [filesystem](https://wiki.gentoo.org/wiki/Filesystem) and the most recent version of the extended series of filesystems. It is the primary file system in use by many Linux systems making it arguably the most stable and well tested file system supported in Linux.

Initially created as a fork of ext3, ext4 brings new features, performance improvements, and removal of size limits with moderate changes to the on-disk format. It can span volumes up to 1 Exabyte and with maximum file size of 16TB. Instead of the classic ext2/3 bitmap block allocation, ext4 uses extents, which improve large file performance and reduce fragmentation. Ext4 also provides more sophisticated block allocation algorithms (delayed allocation and multiblock allocation) giving the filesystem driver more ways to optimize the layout of data on the disk.

## Installation

### Kernel

Activate the following kernel options for the ext4 driver:

**Enabling ext4 support**

```
File systems  --->
  <*> The Extended 4 (ext4) filesystem 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_CONFIG_EXT4_FS</code> to find this item.
Support for optional ext4 features:

**Enabling optional features for ext4**

File systems  --->
  \[\*\]   Ext4 POSIX Access Control Lists [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CONFIG\_EXT4\_FS\_POSIX\_ACL\</code> to find this item.
  \[\*\]   Ext4 Security Labels [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CONFIG\_EXT4\_FS\_SECURITY\</code> to find this item.
  \[ \]   EXT4 debugging support

#### Ext3

The original ext3 driver was removed from the Linux kernel with version 4.3. There should remain only rare cases which make it necessary to use an ext3 filesystem, in which case the ext4 driver may be used.

Activate the following kernel options for ext3 driver:

**Enabling ext3 support**

Support for optional ext3 features:

**Enabling optional features for ext3**

#### Ext2

Activate the following kernel options for ext2 support using the original ext2 driver:

**Enabling ext2 support**

Support for optional ext2 features:

**Enabling optional features for ext2**

#### Large drive support

**Enabling large drives for**x86** kernels**

### USE flags


| [+tools](https://packages.gentoo.org/useflags/+tools) | Build extfs tools (mke2fs, e2fsck, tune2fs, etc.) | 
| [archive](https://packages.gentoo.org/useflags/archive) | Add support for mke2fs to read a tarball as input. This allows not needing privileges. Needs app-arch/libarchive. | 
| [cron](https://packages.gentoo.org/useflags/cron) | Install e2scrub\_all cron script | 
| [fuse](https://packages.gentoo.org/useflags/fuse) | Build fuse2fs, a FUSE file system client for ext2/ext3/ext4 file systems | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [static-libs](https://packages.gentoo.org/useflags/static-libs) | Build static versions of dynamic libraries as well | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

### Emerge

The [sys-fs/e2fsprogs](https://packages.gentoo.org/packages/sys-fs/e2fsprogs) package and should be available as part of the default [system set](<https://wiki.gentoo.org/wiki/System_set_(Portage)>). Despite the historical name, the package includes utilities for ext3 and ext4.

`root #``emerge --ask sys-fs/e2fsprogs`
## Usage

### Creation

To create an ext4 filesystem on the /dev/sdx5 partition:

`root #``mkfs.ext4 /dev/sdx5`
### Mounting

See [filesystem](https://wiki.gentoo.org/wiki/Filesystem#Mounting).

### Ext4 without a journal

In specific use-cases it may be desirable to create a journal-less filesystem. Even though ext2 does provide exactly that, it is also affected by the year 2038 problem regarding file timestamps. Only the ext4 filesystem has been made Y2K38-safe.

In order to get the current Extended filesystem without a journal, an ext4 filesystem can be created without the `has_journal` feature, or modified accordingly. (Note that this is not possible on ext3 filesystems.)

#### Removing the journal from an existing ext4 volume

To display filesystem features currently enabled on a specific ext2/3/4 volume:

`root #``dumpe2fs -h /dev/sdx5 | grep "^Filesystem features:"`
To disable the journal, the filesystem must be unmounted first:

`root #``umount /dev/sdx5` `root #``tune2fs -O ^has_journal /dev/sdx5`
The leading `^` disables the specified feature. Otherwise the feature would be enabled; i.e. to enable a journal on an existing ext2 or journal-less ext4 filesystem:

`root #``tune2fs -O has_journal /dev/sdx5`
Running dumpe2fs again, the `has_journal` feature should no longer be listed. The filesystem can now be mounted again:

`root #``mount /dev/sdx5` #### Create a new journal-less ext4 volume

To create (i.e. format) a new ext4 volume without a journal, the default options of the `ext4` filesystem type have to be overridden:

`root #``mke2fs -t ext4 -O ^has_journal /dev/sdx5`
## Utilities

Utilities included in [sys-fs/e2fsprogs](https://packages.gentoo.org/packages/sys-fs/e2fsprogs) consist of:

| Utility | Description | Man page | 
|---|---|---|
| [badblocks](https://wiki.gentoo.org/wiki/Ext4/badblocks) | Scans a disk for bad blocks. | [badblocks(8)](https://man.archlinux.org/man/badblocks.8.en)  | 
| debugfs | An ext2/ext3/ext4 file system debugger. | [debugfs(8)](https://man.archlinux.org/man/debugfs.8.en)  | 
| dumpe2fs | A tool to dump ext2/ext3/ext4 filesystem information. | [dumpe2fs(8)](https://man.archlinux.org/man/dumpe2fs.8.en)  | 
| e2fsck | A tool for checking ext2/ext3/ext4 filesystems. | [e2fsck(8)](https://man.archlinux.org/man/e2fsck.8.en)  | 
| e2image | A tool for saving critical ext2/ext3/ext4 filesystem metadata to a file. | [e2image(8)](https://man.archlinux.org/man/e2image.8.en)  | 
| e2label | A tool to change the label on an ext2/ext3/ext4 filesystem (symlinks to tune2fs). |  | 
| e2undo | A tool to replay an undo log for an ext2/ext3/ext4 filesystem. | [e2undo(8)](https://man.archlinux.org/man/e2undo.8.en)  | 
| fsck.ext2 | Checks, specifically, an ext2 filesystem (symlinks to e2fsck). |  | 
| fsck.ext3 | Checks, specifically, an ext3 filesystem (symlinks to e2fsck). |  | 
| fsck.ext4 | Checks, specifically, an ext4 filesystem (symlinks to e2fsck). |  | 
| fsck.ext4dev | Checks, specifically, an ext4dev filesystem (symlinks to e2fsck). |  | 
| logsave | A tool to save the output of a command in a logfile. | [logsave(8)](https://man.archlinux.org/man/logsave.8.en)  | 
| mke2fs | The base program for creating ext2/ext3/ext4 filesystems. Creation commands symlink here. | [mke2fs(8)](https://man.archlinux.org/man/mke2fs.8.en)  | 
| mkfs.ext2 | Creates, specifically, an ext2 filesystem (symlinks to mke2fs). |  | 
| mkfs.ext3 | Creates, specifically, an ext3 filesystem (symlinks to mke2fs). |  | 
| mkfs.ext4 | Creates, specifically, an ext4 filesystem (symlinks to mke2fs). |  | 
| mkfs.ext4dev | Creates, specifically, an ext24dev filesystem (symlinks to mke2fs). |  | 
| resize2fs | An ext2/ext3/ext4 filesystem resizer. | [resize2fs(8)](https://man.archlinux.org/man/resize2fs.8.en)  | 
| tune2fs | Adjust tunable filesystem parameters on ext2/ext3/ext4 filesystems. | [tune2fs(8)](https://man.archlinux.org/man/tune2fs.8.en)  | 
| chattr | Change file attributes on a Linux filesystem. | [chattr(1)](https://man.archlinux.org/man/chattr.1.en)  | 
| lsattr | List ext2/ext3/ext4 file attributes. | [lsattr(1)](https://man.archlinux.org/man/lsattr.1.en)  | 
| e2freefrag | Report free space fragmentation information. | [e2freefrag(8)](https://man.archlinux.org/man/e2freefrag.8.en)  | 
| e4defrag | An online defragmenter for ext4 filesystem. | [e4defrag(8)](https://man.archlinux.org/man/e4defrag.8.en)  | 
| filefrag | Report on file fragmentation. | [filefrag(8)](https://man.archlinux.org/man/filefrag.8.en)  | 
| mklost+found | Create a lost+found directory on a mounted ext2/ext3/ext4 file system. | [mklost+found(8)](https://man.archlinux.org/man/mklost+found.8.en)  | 

## See also

- [XFS](https://wiki.gentoo.org/wiki/XFS) — a high-performance journaling [filesystem](https://wiki.gentoo.org/wiki/Filesystem)
- [Btrfs](https://wiki.gentoo.org/wiki/Btrfs) — a copy-on-write (CoW) [filesystem](https://wiki.gentoo.org/wiki/Filesystem) for Linux aimed at implementing advanced features while focusing on fault tolerance, repair, and easy administration.
- [FAT](https://wiki.gentoo.org/wiki/FAT) — [filesystem](https://wiki.gentoo.org/wiki/Filesystem) originally created for use with MS-DOS (and later pre-NT Microsoft Windows).

## External resources

- [https://ext4.wiki.kernel.org/](https://ext4.wiki.kernel.org/) - The second, third, and fourth extended file system wiki.
