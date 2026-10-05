<!-- source: https://wiki.gentoo.org/wiki/Btrfs/Native_System_Root_Guide | group: Gentoo Wiki (Main) | wiki-title: Btrfs/Native System Root Guide -->
---
title: Btrfs/Native System Root Guide
url: https://wiki.gentoo.org/wiki/Btrfs/Native_System_Root_Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-24"
fingerprint: "329fe27a92ce5f41"
license: CC BY-SA 4.0
---

# Btrfs/Native System Root Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


This is a supplementary article to use alongside the Handbook to set up a basic btrfs rootfs system.
Btrfs is for users that require features such as snapshot rollback. If these features aren't required then they will be better off with normal XFS default as suggested by the Handbook.

## LiveGUI

When installing Gentoo using the LiveGUI, be aware that the kernel used by the installation media may be newer than the kernel used by the installed system by default. Generally, Btrfs filesystems are compatible across kernel versions, and a standard installation will normally work with an LTS kernel. However, some Btrfs features introduced in newer kernels are not supported by older kernels. If such features are enabled or used during installation, booting the installed system with an older kernel may fail or cause the filesystem to be unavailable.

## Partitioning

Follow the Handbook default partitions layouts at [Designing a partition scheme](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Disks#Designing_a_partition_scheme) for either GPT or DOS depending on needs, and return here when reaching [Creating filesystems](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Disks#Creating_file_systems)

## Filesystem creation

### ESP

The [UEFI](https://wiki.gentoo.org/wiki/UEFI) on most motherboards can only read [FAT32](https://wiki.gentoo.org/wiki/FAT) filesystems. To format the ESP:

`root #``mkfs.vfat -F32 /dev/nvme0n1p1`
### Root filesystem

To format the root filesystem with [Btrfs](https://wiki.gentoo.org/wiki/Btrfs):

`root #``mkfs.btrfs -L rootfs /dev/nvme0n1p3`
## Chrooting

`root #``mount --label rootfs /mnt/gentoo`
At this point, the Gentoo install can be continued: [Installing a stage tarball](https://wiki.gentoo.org/wiki/Handbook:AMD64/Full/Installation#Installing_a_stage_tarball).

## initramfs

For the most part this section is the same as Handbook.

A dracut example is provided below encase of need:

`root #``lsblk -o name,uuid`
NAME        UUID
sdb                                           
├─nvme0n1p1 BDF2-0139
├─nvme0n1p2 b0e86bef-30f8-4e3b-ae35-3fa2c6ae705b
└─nvme0n1p3 4bb45bd6-9ed9-44b3-b547-b411079f043b
  └─root    cb070f9e-da0e-4bc5-825c-b01bb2707704



**`/etc/dracut.conf.d/00-installkernel.conf`**

## Finalize

It is now safe to follow the Handbook as normal, only making sure [sys-fs/btrfs-progs](https://packages.gentoo.org/packages/sys-fs/btrfs-progs) is installed

`root #``emerge --ask sys-fs/btrfs-progs`
