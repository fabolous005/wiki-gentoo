<!-- source: https://wiki.gentoo.org/wiki/Orange_Pi_PC/Quick_Start | group: Gentoo Wiki (Main) | wiki-title: Orange Pi PC/Quick Start -->
---
title: Orange Pi PC/Quick Start
url: https://wiki.gentoo.org/wiki/Orange_Pi_PC/Quick_Start
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-21"
fingerprint: "1c31d01b14e4acda"
license: CC BY-SA 4.0
---

# Orange Pi PC/Quick Start

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This document describes how to install Gentoo on an Orange Pi PC without compiling the u-boot bootloader or the Allwinner Linux kernel.

## Prerequisites

- microSD card (at least 4 GB Class 6, better 16 GB+ Class 10)
- Orange Pi PC Linux SD card image (any will do, here we use the Debian\_jessie\_mini image from [the Orange Pi forum](http://www.orangepi.org/orangepibbsen/forum.php?mod=viewthread&tid=342))
- Gentoo armv7a\_hardfp stage3 from [\[1\]](http://distfiles.gentoo.org/releases/arm/autobuilds/current-stage3-armv7a_hardfp/) or your local mirror
- current snapshot of the portage tree from [\[2\]](http://distfiles.gentoo.org/snapshots/) or your local mirror

## Preparing the SD card

We need to transfer the boot loader and kernel from the image to the SD card. The boot loader resides in the unpartitioned space before the first partition, and the kernel resides in the first partition.

### Copying the boot loader and kernel

Uncompress the image if it is compressed. We assume that the image is called Debian\_jessie\_mini.img and the SD card is /dev/sdb. Adjust to match your configuration if necessary. Look at the image partition structure:

`root #``fdisk -l Debian_jessie_mini.img` Disk Debian\_jessie\_mini.img: 809 MiB, 848297984 bytes, 1656832 sectors
Units: sectors of 1 \* 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x9c2e57c2
Device                  Boot  Start     End Sectors  Size Id Type
Debian\_jessie\_mini.img1       40960  172031  131072   64M  b W95 FAT32
Debian\_jessie\_mini.img2      172032 1656832 1484801  725M 83 Linux

In this case we will copy the image right until the end of the boot partition to the microSD card.

`root #``dd if=Debian_jessie_mini.img of=/dev/sdb count=172032`
Next we will remove the partition table entry for the previous root partition, then create the new root and optionally swap partitions.

`root #``fdisk /dev/sdb````
Welcome to fdisk (util-linux 2.26.2).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.
Command (m for help): d
Partition number (1,2, default 2): 2
Partition 2 has been deleted.
Command (m for help): n
Partition type
   p   primary (1 primary, 0 extended, 3 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (2-4, default 2): 2
First sector (2048-32372735, default 2048): 172032
Last sector, +sectors or +size{K,M,G,T,P} (2048-32372735, default 32372735):
(choose here the size for your root partition, repeat with the swap partition if you want one, then set the appropriate partition types)
Command (m for help): w
```
### Setting up the root partition

We assume that the root partition is /dev/sdb2 and the swap partition is /dev/sdb3.

Create the root filesystem with label "linux" and the swap space.

`root #````
mkfs.ext4 -L linux /dev/sdb2
```
`root #``mkswap /dev/sdb3`
## Install Gentoo to the SD card

Mount the root partition and extract the stage3 tarball and portage snapshot.

`root #````
mkdir /mnt/opipc
```
`root #````
mount /dev/sdb2 /mnt/opipc
```
`root #````
tar xfa stage3-armv7a_hardfp-????????.tar.bz2 -C /mnt/opipc
```
`root #``tar xfa portage-latest.tar.xz -C /mnt/opipc/usr`
If the original Linux image supports multiple boards, mount the boot partition and choose the correct uImage and script.bin for the Orange Pi PC.

`root #````
mount /dev/sdb1 /mnt/opipc/boot
```
`root #````
cp /mnt/opipc/boot/script.bin.OPI-PC_1080p60 /mnt/opipc/boot/script.bin
```
`root #``cp /mnt/opipc/boot/uImage_OPI-2 /mnt/opipc/boot/uImage`
Now extract the kernel modules from the image and copy them to your Gentoo system:

`root #````
mkdir /mnt/opipc_image
```
`root #````
mount -o loop,offset=$((172032*512)) Debian_jessie_mini.img /mnt/opipc_image
```
`root #````
cp -a /mnt/opipc_image/lib/modules /mnt/opipc/lib
```
## Finish configuration

Edit fstab.

**`/etc/fstab`**

Set a root password.

`root #``sed -i "s|root:\*|root:$(openssl passwd -1)|" /mnt/opipc/etc/shadow`
Password: 
Verifying - Password:

Set compiler and make flags in make.conf.

**`/etc/portage/make.conf`**

To work via the debug ttl uart, change:

**`/etc/inittab`**

to

**`/etc/inittab`**

That's it! Now unmount everything, insert the SD card into the Orange Pi PC, and power it on.

How to compile application for arm gentoo on the desktop you can read in this article. [Embedded\_Handbook/General/Compiling\_with\_qemu\_user\_chroot](https://wiki.gentoo.org/wiki/Embedded_Handbook/General/Compiling_with_qemu_user_chroot)
