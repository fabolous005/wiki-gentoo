<!-- source: https://wiki.gentoo.org/wiki/Partition | group: Gentoo Wiki (Main) | wiki-title: Partition -->
---
title: Partition
url: https://wiki.gentoo.org/wiki/Partition
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-21"
categories: ['sys-fs']
fingerprint: "1c29a95f8defabc8"
license: CC BY-SA 4.0
---

# Partition

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

A **partition** is a means of splitting a block device up to sub-regions. It allows creating a more manageable and adaptive "logical" structure visible to the system. The PARTUUID (partition UUID) can be seen using blkid.

## Master Boot Record (MBR)

Used for a long time to organize data, also called DOS-Partitions. Partition information is stored in the first 512 bytes of the device.

- Widespread and supported in nearly all operating systems.
- Very well documented.
- Maximum of 4 primary partitions per device.
- Maximum size of the device 2 TiB.
- Using one primary partition as an extended partition, it is possible to create additional logical partitions to work around the problem of only 4 primary partitions.

### Kernel configuration

**Enable MBR support (CONFIG\_MSDOS\_PARTITION)**

### Available software

The following programs can be used to create, alter, and remove MBR partitions:

| Program | Package | GUI | Function | 
|---|---|---|---|
| [cfdisk](https://wiki.gentoo.org/wiki/Util-linux#cfdisk) | [sys-apps/util-linux](https://packages.gentoo.org/packages/sys-apps/util-linux) |  | Create, alter, and remove partitions. More intuitive interface than fdisk. | 
| [fdisk](https://wiki.gentoo.org/wiki/Util-linux) | [sys-apps/util-linux](https://packages.gentoo.org/packages/sys-apps/util-linux) |  | Create, alter, and remove partitions. | 
| gparted | [sys-block/gparted](https://packages.gentoo.org/packages/sys-block/gparted) |  | GNOME Partition Editor; create, alter, and remove partitions. | 
| parted | [sys-block/parted](https://packages.gentoo.org/packages/sys-block/parted) |  | Create, alter, remove, check, copy partitions and file systems. | 
| partitionmanager | [sys-block/partitionmanager](https://packages.gentoo.org/packages/sys-block/partitionmanager) |  | KDE Partition Manager; create, alter, and remove partitions. | 
| [sfdisk](https://wiki.gentoo.org/wiki/Util-linux) | [sys-apps/util-linux](https://packages.gentoo.org/packages/sys-apps/util-linux) |  | Non-interactive version of fdisk. | 

### Supported operating systems

- BSD (Mac OS X) - full support.
- DOS - full support.
- Linux - full support.
- Solaris - full support.
- Windows - full support.

## GUID Partition Table (GPT)

In GUID (**G**lobal **U**nique **ID**entifier) partition system, a small amount of disk space at the beginning of the device is used to store the partition information. Its main advantage is the supported size of storage devices and the creation of a backup of the partition table at the end of the device.

- Widespread and supported in most modern operating systems.
- Maximum of 128 primary partitions per device.
- Maximum size of the device 8 ZiB.

### Kernel configuration

**Enable GPT support (CONFIG\_EFI\_PARTITION)**

### Available software

The following programs can be used to create, alter, and remove **[GPT](https://en.wikipedia.org/wiki/GUID_Partition_Table)** partitions:

| Program | Package | GUI | Function | 
|---|---|---|---|
| [cfdisk](https://wiki.gentoo.org/wiki/Util-linux) | [sys-apps/util-linux](https://packages.gentoo.org/packages/sys-apps/util-linux) |  | Create, alter, and remove partitions. More intuitive interface than fdisk. | 
| [fdisk](https://wiki.gentoo.org/wiki/Util-linux) | [sys-apps/util-linux](https://packages.gentoo.org/packages/sys-apps/util-linux) |  | Create, alter, and remove partitions. | 
| GNOME Disks | [sys-apps/gnome-disk-utility](https://packages.gentoo.org/packages/sys-apps/gnome-disk-utility) |  | GNOME partition manager. | 
| gparted | [sys-block/gparted](https://packages.gentoo.org/packages/sys-block/gparted) |  | GNOME Partition Editor; create, alter, and remove partitions. | 
| gptfdisk | [sys-apps/gptfdisk](https://packages.gentoo.org/packages/sys-apps/gptfdisk) |  | Create, alter, remove, convert MBR to GPT, and recreate partition tables from backup. | 
| parted | [sys-block/parted](https://packages.gentoo.org/packages/sys-block/parted) |  | Create, alter, remove, check, copy partitions and file systems. | 
| partitionmanager | [sys-block/partitionmanager](https://packages.gentoo.org/packages/sys-block/partitionmanager) |  | KDE Partition Manager; create, alter, and remove partitions. | 
| [sfdisk](https://wiki.gentoo.org/wiki/Util-linux) | [sys-apps/util-linux](https://packages.gentoo.org/packages/sys-apps/util-linux) |  | Non-interactive version of fdisk. | 

### Supported operating systems

- BSD (Mac OS X) - full support.
- Linux - full support.
- Windows - Installs into the /boot/EFI/Microsoft/ subdirectory of the [ESP](https://wiki.gentoo.org/wiki/EFI_System_Partition).

## Logical Volume Manager (LVM)

LVM is a complete suite to dynamically manage partitions, storage devices or other underlying systems as volumes.

- Widespread and supported in most modern operating systems.
- Maximum size of the device depends on the underlying systems limitations.
- Maximum size of Logical Volumes is 8 EiB on 64-bit Linux and 16 TiB on 32-bit Linux.
- Storage devices, RAID system, network storage (e.g. [iSCSI](https://wiki.gentoo.org/wiki/ISCSI)) can be used as Physical Volumes (no need of partitioning).
- Provides basic forms of data redundancy (RAID 1, RAID 5) or stripset (RAID 0) for performance.

### Kernel configuration

**Enabling LVM**

### Available software

The following programs come with [sys-fs/lvm2](https://packages.gentoo.org/packages/sys-fs/lvm2)

| Program | GUI | Function | 
|---|---|---|
| lvcreate |  | Create, alter, and remove volumes. | 
| pvcreate |  | Create or remove Physical Volumes of storage devices/systems. | 
| vgcreate |  | Groups PV as Volume Group. | 

The following programs can be used to create, alter, and remove LVM partitions:

| Program | Package | GUI | Function | 
|---|---|---|---|
| partitionmanager | [sys-block/partitionmanager](https://packages.gentoo.org/packages/sys-block/partitionmanager) |  | KDE Partition Manager; create, alter, and remove LVM PVs, VGs, LVs. | 

### Supported operating systems

- BSD - cannot boot itself, needs Linux [GRUB](https://wiki.gentoo.org/wiki/GRUB) with dual boot.
- Linux - full support.

## ZFS

ZFS is a complete suite to dynamically manage storage and [filesystem](https://wiki.gentoo.org/wiki/Filesystem).

- Support in Linux (via ZFSOnLinux<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>), Solaris, FreeBSD.
- Needs [GRUB](https://wiki.gentoo.org/wiki/GRUB) bootloader.
- Maximum size of a single zpool is 256 ZiB
- Storage devices can be used complete as vdev (no need of partitioning)
- Zpools are created once and cannot be resized afterwards. Every volume has access to the full capacity of the zpool, this can be reduced via quota.
- It provides forms of redundancy like RAID 1 (mirroring), and RAID 0 (striping) for performance. Also supports RAID 5, RAID 6, etc.
- Has its own filesystem with features like compression, copy-on-write, and deduplication.

### Available software

The following programs come with [sys-fs/zfs](https://packages.gentoo.org/packages/sys-fs/zfs):

| Program | GUI | Function | 
|---|---|---|
| zfs |  | Create, alter (resize), and remove volumes. | 
| zpool |  | Manage and organize vdevs in zpools. | 

### Supported operating systems

- BSD - full support.
- Linux - built as external module because of the CDDL and GPL license conflict - mostly supported.
- Solaris - full support.

## Other software

[Busybox](https://wiki.gentoo.org/wiki/Busybox) also contains a version of fdisk.

There are some special versions of fdisk for specific system types in the [Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository), such as: [sys-fs/arm-fdisk](https://packages.gentoo.org/packages/sys-fs/arm-fdisk), [sys-fs/mac-fdisk](https://packages.gentoo.org/packages/sys-fs/mac-fdisk), or [sys-fs/atari-fdisk](https://packages.gentoo.org/packages/sys-fs/atari-fdisk).

See [sys-fs](https://packages.gentoo.org/categories/sys-fs) category for even more tools.

## See also

- [Complete Handbook/Putting the minimal environment in place](https://wiki.gentoo.org/wiki/Complete_Handbook/Putting_the_minimal_environment_in_place)
- [Filesystem/Security](https://wiki.gentoo.org/wiki/Filesystem/Security) — one of the basic means to harden a system.
- [Handbook:AMD64/Installation/Disks](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Disks)
