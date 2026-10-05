<!-- source: https://wiki.gentoo.org/wiki/XFS | group: Gentoo Wiki (Main) | wiki-title: XFS -->
---
title: XFS
url: https://wiki.gentoo.org/wiki/XFS
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-25"
fingerprint: "3e8d48b4faa67c60"
license: CC BY-SA 4.0
---

# XFS

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The **XFS** filesystem is a high-performance journaling [filesystem](https://wiki.gentoo.org/wiki/Filesystem). It is [ACL](https://wiki.gentoo.org/wiki/Filesystem/Access_Control_List_Guide) (POSIX) compliant for use with Linux.

XFS has a reputation for reliability and led to the creation of the venerable xfstests Linux kernel test suite which now tests regressions in various filesystems.

## Installation

### Kernel

**Enable XFS support**

```
 File systems  --->
  <*> XFS filesystem support 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_XFS_FS</code> to find this item.
Optional:

**Enable optional XFS features**

File systems  --->
  \<\*> XFS filesystem support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_XFS\_FS\</code> to find this item.
    \[\*\]   XFS Quota support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_XFS\_QUOTA\</code> to find this item.
    \[\*\]   XFS POSIX ACL support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_XFS\_POSIX\_ACL\</code> to find this item.
    \[\*\]   XFS Realtime subvolume support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_XFS\_RT\</code> to find this item.
  \[ \]   XFS Verbose Warnings
  \[ \]   XFS Debugging support
  \[ \]   XFS online metadata check support
     \[ \]   XFS online metadata check usage data collection
     \[ \]   XFS online metadata repair support

### Emerge

The [sys-fs/xfsprogs](https://packages.gentoo.org/packages/sys-fs/xfsprogs) package is needed for XFS userspace utilities:

`root #``emerge --ask sys-fs/xfsprogs`
## Usage

### Mount

Mount XFS filesystems with the [mount](https://wiki.gentoo.org/wiki/Mount) command.

### Creation

To make an XFS filesystem with mkfs.xfs from [sys-fs/xfsprogs](https://packages.gentoo.org/packages/sys-fs/xfsprogs):

`root #``mkfs.xfs -L 'label' /dev/sda1`
The label is optional. Further tuning on creation might be interesting for use as a RAID, multi-terabyte drives, and placing the journal for a [HDD](https://wiki.gentoo.org/wiki/HDD) on a separate [SSD](https://wiki.gentoo.org/wiki/SSD).

Additionally to target stable kernels, it is recommended to add an option so the format does not enable unsupported or experimental features in that LTS kernel. [bug #969909](https://bugs.gentoo.org/show_bug.cgi?id=969909)

`root #``mkfs.xfs -c options=/usr/share/xfsprogs/mkfs/lts_6.12.conf /dev/sda1`
The [GRUB](https://wiki.gentoo.org/wiki/GRUB) bootloader will not tolerate unknown features enabled where /boot resides and will fail to install when grub-install is invoked.  For example, the sys-boot/grub-2.12 release needs to use no later than lts\_6.6.conf in the above command for the partition where /boot will reside. Be it a separate partition with the Handbook's Legacy BIOS layout or the rootfs in the UEFI layout.

### Filesystem information

xfs\_spaceman can be used to display information about the space available and to run a report on the health of a filesystem.

`root #``xfs_spaceman -c info /path/to/mountpoint`
### Changing parameters

The parameters of an XFS filesystem can be changed using xfs\_admin. For the full list of options, view the manpage: [xfs\_admin(8)](https://man.archlinux.org/man/xfs_admin.8.en)

`root #``xfs_admin -L 'label' /dev/sda1`
### Expanding a filesystem

To grow an XFS filesystem to N amount, use xfs\_growfs.

`root #``xfs_growfs -D N /path/to/mountpoint`
### Freezing

To suspend access to a filesystem, use the xfs\_freeze command.

`root #``xfs_freeze -f /path/to/mountpoint`
## Utilities

| Utility | Description <sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> | Man page | 
|---|---|---|
| fsck.xfs | Checks a filesystem for corruption | [fsck.xfs(8)](https://man.archlinux.org/man/fsck.xfs.8.en)  | 
| mkfs.xfs | Creates a new filesystem | [mkfs.xfs(8)](https://man.archlinux.org/man/mkfs.xfs.8.en)  | 
| xfs\_admin | Changes the parameters of a filesystem | [xfs\_admin(8)](https://man.archlinux.org/man/xfs_admin.8.en)  | 
| xfs\_bmap | Prints block mapping for an XFS file | [xfs\_bmap(8)](https://man.archlinux.org/man/xfs_bmap.8.en)  | 
| xfs\_copy | Copies contents of a filesystem to one or more targets in parallel | [xfs\_copy(8)](https://man.archlinux.org/man/xfs_copy.8.en)  | 
| xfs\_estimate | Estimate the amount of space a directory would consume if it were copied to an XFS filesystem | [xfs\_estimate(8)](https://man.archlinux.org/man/xfs_estimate.8.en)  | 
| xfs\_db | Used to debug an XFS filesystem | [xfs\_db(8)](https://man.archlinux.org/man/xfs_db.8.en)  | 
| xfs\_freeze | Suspends access to a filesystem | [xfs\_freeze(8)](https://man.archlinux.org/man/xfs_freeze.8.en)  | 
| xfs\_fsr | Improves organization of mounted filesystems, compacting or improving the layout of extents | [xfs\_fsr(8)](https://man.archlinux.org/man/xfs_fsr.8.en)  | 
| xfs\_growfs | Increases a filesystem's size | [xfs\_growfs(8)](https://man.archlinux.org/man/xfs_growfs.8.en)  | 
| xfs\_info | Equivalent to invoking xfs\_growfs but does not change any aspects about the filesystem | [xfs\_info(8)](https://man.archlinux.org/man/xfs_info.8.en)  | 
| xfs\_io | Used for debugging, like xfs\_db but for regular file paths than raw volumes | [xfs\_io(8)](https://man.archlinux.org/man/xfs_io.8.en)  | 
| xfs\_logprint | Prints the log of an XFS filesystem | [xfs\_logprint(8)](https://man.archlinux.org/man/xfs_logprint.8.en)  | 
| xfs\_mdrestore | Restores an XFS metadump image to a filesystem image | [xfs\_mdrestore(8)](https://man.archlinux.org/man/xfs_mdrestore.8.en)  | 
| xfs\_metadump | Copies filesystem metadata to a file | [xfs\_metadump(8)](https://man.archlinux.org/man/xfs_metadump.8.en)  | 
| xfs\_mkfile | Creates an XFS file (padded by zeroes by default) | [xfs\_mkfile(8)](https://man.archlinux.org/man/xfs_mkfile.8.en)  | 
| xfs\_ncheck | Generates pathnames from inode numbers | [xfs\_ncheck(8)](https://man.archlinux.org/man/xfs_ncheck.8.en)  | 
| xfs\_quota | Used for reporting and editing different aspects of filesystem quotas | [xfs\_quota(8)](https://man.archlinux.org/man/xfs_quota.8.en)  | 
| xfs\_repair | Repairs corrupted or damaged XFS filesystems | [xfs\_repair(8)](https://man.archlinux.org/man/xfs_repair.8.en)  | 
| xfs\_rtcp | Copies a file to a real-time partition | [xfs\_rtcp(8)](https://man.archlinux.org/man/xfs_rtcp.8.en)  | 
| xfs\_scrub | Checks and repairs contents of a mounted filesystem | [xfs\_scrub(8)](https://man.archlinux.org/man/xfs_scrub.8.en)  | 
| xfs\_scrub\_all | Scrubs all mounted XFS filesystems | [xfs\_scrub\_all(8)](https://man.archlinux.org/man/xfs_scrub_all.8.en)  | 
| xfs\_spaceman | Reports and controls free space usage | [xfs\_spaceman(8)](https://man.archlinux.org/man/xfs_spaceman.8.en)  | 

## Maintenance

### Year 2038 timestamp support (bigtime)

Older partitions (created with \<xfsprogs-5.15) will not have `bigtime` enabled by default. Mounting such partitions results in a warning like:

`root #``dmesg`
...
\[    4.036258\] xfs filesystem being mounted at /home supports timestamps until 2038 (0x7fffffff)
...

To check the current version of xfsprogs, run mkfs.xfs -V. There's no need for this on up-to-date Gentoo systems, but it might be necessary if using install media from another distribution with older userland.

`bigtime` support was enabled by default in xfsprogs 5.15, so manual setting is not required in newer versions.

Beginning with [kernel](https://wiki.gentoo.org/wiki/Kernel) 5.10, XFS gained `bigtime` support to extend the maximum recorded date stamps from 2038 to 2486 for the V5 on-disk format. [\[2\]](https://wiki.gentoo.org#cite_note-2)

To upgrade an older filesystem to `bigtime`, first cleanly unmount the file system. The upgrade will refuse to run if the unmount was not completely clean.

Then run:

`root #``xfs_admin -O bigtime=1 /dev/sda1`
Replacing /dev/sda1 with the device path.

#### Using Dracut initramfs to perform the upgrade

First, [Dracut](https://wiki.gentoo.org/wiki/Dracut) needs additional files included in the initramfs in order to perform the upgrade.  This can be accomplished with either the `--install` option or inside a configuration file using the `install_items` option.

`root #``dracut --install "/usr/sbin/xfs_admin /usr/bin/expr" ...`
Then, the kernel command line option can be modified to include `rd.break=pre-mount` to stop the initramfs just before it would mount the root filesystem. Ensure this is done temporarily and removed on subsequent reboots after upgrade.

## Removal

To schedule removal at the next run:

`root #``emerge --ask --depclean --verbose sys-fs/xfsprogs`
## See also

- [Deduplication](https://wiki.gentoo.org/wiki/Deduplication) — a mechanism for reducing the space taken by multiple identical copies of a file are stored on a [filesystem](https://wiki.gentoo.org/wiki/Filesystem)
- [FAT](https://wiki.gentoo.org/wiki/FAT) — [filesystem](https://wiki.gentoo.org/wiki/Filesystem) originally created for use with MS-DOS (and later pre-NT Microsoft Windows).
- [Ext4](https://wiki.gentoo.org/wiki/Ext4) — an open source disk [filesystem](https://wiki.gentoo.org/wiki/Filesystem) and the most recent version of the extended series of filesystems.
- [Btrfs](https://wiki.gentoo.org/wiki/Btrfs) — a copy-on-write (CoW) [filesystem](https://wiki.gentoo.org/wiki/Filesystem) for Linux aimed at implementing advanced features while focusing on fault tolerance, repair, and easy administration.
