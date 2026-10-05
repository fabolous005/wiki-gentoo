<!-- source: https://wiki.gentoo.org/wiki/Bcachefs/rootfs | group: Gentoo Wiki (Main) | wiki-title: Bcachefs/rootfs -->
---
title: bcachefs/rootfs
url: https://wiki.gentoo.org/wiki/Bcachefs/rootfs
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-09"
fingerprint: "1d04ef5a572198ec"
license: CC BY-SA 4.0
---

# bcachefs/rootfs

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


This article is focused on using [bcachefs](https://wiki.gentoo.org/wiki/Bcachefs) as the root file system on Gentoo and is intended to be followed alongside the Handbook.

## Install media

Due to bcachefs becoming an out of tree module again, Gentoo boot media no longer supports bcachefs. As of 2026-01-09 there appears to be no major Linux distribution planning long term support in their boot media to recommend an alterative for installation at this time.

## Disk setup

The disk layout for bcachefs is similar to a normal disk layout using ext4 or XFS: a vfat boot partition, a swap partition, and the bcachefs partition.

`root #``mkfs.vfat -F 32 /dev/sda1``root #``mkswap /dev/sda2``root #``mkfs.bcachefs /dev/sda3`
Or, using bcachefs's cli tools:

`root #``bcachefs format /dev/sda3`
Before proceeding, follow the [handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64) and resume this guide after reaching [Applying a filesystem to a partition](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Disks#Applying_a_filesystem_to_a_partition).

### Subvolumes

Subvolumes in bcachefs are similar to those in btrfs, but with one benefit: they don't need to be mounted! When the root file system is mounted, the subvolumes are mounted along with it.

To create a subvolume for /home, run the following:

`root /mnt/gentoo #``bcachefs subvolume create home`
Continue following the Handbook and return upon reaching [Configuring the Linux kernel](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Kernel)

## Kernel Config

For configuring the kernel, following the [manual configuration](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Kernel#Alternative:_Manual_configuration) guide in the Handbook should be sufficient, all that needs to be changed for bcachefs is:

**Adding bcachefs support**

If a lscpu shows **ssse3** and/or **avx2** it is recommended to enable also:

**Adding bcachefs support - Accelerated Cryptographic Algorithms**

After building the Kernel, if the Distribution Kernel config was used, install the initramfs with

`root #``dracut --kver=6.12.16-gentoo`
## /etc/fstab

An example fstab for a bcachefs rootfs looks like:

**`/etc/fstab`**

and finally finish the Handbook, resuming at [configuring the system](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/System).

## Userspace Tools

To manage bcachefs in userspace, the package [sys-fs/bcachefs-tools](https://packages.gentoo.org/packages/sys-fs/bcachefs-tools) will need to be installed.

`root #``emerge --ask sys-fs/bcachefs-tools`
## Issues

### Multi Device Root FS

Using the command to add a second partition to one bcachefs pool currently leaves the system unbootable when used on the rootfs, so should not be used until bcachefs support is added to [sys-boot/grub](https://packages.gentoo.org/packages/sys-boot/grub) and other bootloaders. This will work fine if you have the rootfs on one pool and create a second pool which uses multiple partitions.

To tracker this bug please follow this [bug report](https://github.com/koverstreet/bcachefs/issues/630).
