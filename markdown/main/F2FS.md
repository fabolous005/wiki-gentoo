<!-- source: https://wiki.gentoo.org/wiki/F2FS | group: Gentoo Wiki (Main) | wiki-title: F2FS -->
---
title: F2FS
url: https://wiki.gentoo.org/wiki/F2FS
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-15"
fingerprint: "7c8909b8f9943bf0"
license: CC BY-SA 4.0
---

# F2FS

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**F2FS** (**F**lash-**F**riendly **F**ile **S**ystem) is a [filesystem](https://wiki.gentoo.org/wiki/Filesystem) designed for NAND flash-based devices. It is available in Linux [kernels](https://wiki.gentoo.org/wiki/Kernel) 3.8.x and higher. This filesystem is a good choice when installing Gentoo on an eMMC, [SSD](https://wiki.gentoo.org/wiki/SSD), [SDCard](https://wiki.gentoo.org/wiki/SDCard), or a flash-based USB device.

## Installation

### Kernel

**Enabling basic F2FS filesystem options**

```
File systems  --->
   <*> F2FS filesystem support
   [ ]   F2FS Status Information
   [*]   F2FS extended attributes
   [*]     F2FS Access Control Lists
   [*]     F2FS Security Labels
   [ ]   F2FS consistency checking feature
   [ ]   F2FS fault injection facility
   [*]   F2FS compression feature
   [*]     LZO compression support
   [*]       LZO-RLE compression support
   [*]     LZ4 compression support
   [*]       LZ4HC compression support
   [*]     ZSTD compression support
   [*]   F2FS IO statistics information
   [ ]   F2FS unfair rw_semaphore
```
When enabling support to the filesystem in the Linux kernel, it is wise to enable at least "F2FS extended attributes" (`F2FS_FS_XATTR`) with "F2FS Access Control Lists" (`CONFIG_F2FS_FS_POSIX_ACL`) and "F2FS Security Labels" (`CONFIG_F2FS_FS_SECURITY`) suboptions.

"F2FS consistency checking feature" (`CONFIG_F2FS_CHECK_FS`) option in the list will enable F2FS's filesystem consistency checking. The checking will occur during run time and will decrease the filesystem's performance. This option provides an advantage when consistency is more important than speed.

### Emerge

Install the userspace tools for the F2FS filesystem:

`root #``emerge --ask sys-fs/f2fs-tools`
## Usage

### Creation

After emerging the userspace tools, create a filesystem by running the mkfs.f2fs command followed by the appropriate device and partition number:

`root #``mkfs.f2fs /dev/sdd1`
### Filesystem check

`root #``fsck.f2fs /dev/sdd1`
### Defragmentation

`root #``defrag.f2fs`
## See also

- [Ext4](https://wiki.gentoo.org/wiki/Ext4) — an open source disk [filesystem](https://wiki.gentoo.org/wiki/Filesystem) and the most recent version of the extended series of filesystems.
- [Btrfs](https://wiki.gentoo.org/wiki/Btrfs) — a copy-on-write (CoW) [filesystem](https://wiki.gentoo.org/wiki/Filesystem) for Linux aimed at implementing advanced features while focusing on fault tolerance, repair, and easy administration.
- [SquashFS](https://wiki.gentoo.org/wiki/SquashFS) — an open source, read only, extremely compressible filesystem.
