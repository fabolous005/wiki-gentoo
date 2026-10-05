<!-- source: https://wiki.gentoo.org/wiki/LiveUSB/ISOLINUX | group: Gentoo Wiki (Main) | wiki-title: LiveUSB/ISOLINUX -->
---
title: LiveUSB/ISOLINUX
url: https://wiki.gentoo.org/wiki/LiveUSB/ISOLINUX
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-10-23"
fingerprint: b245364b85e7ba95
license: CC BY-SA 4.0
---

# LiveUSB/ISOLINUX

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

As of **2023-07-20**, this article is **deprecated (obsolete)**. Contents are <u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

This article details creating a bootable LiveUSB drive from a ISOLINUX-based live image.

### Automatic drive-wide installation script

This script will assist in writing a LiveUSB stick suitable for both BIOS and UEFI systems.

**`gentoo-usb.sh`**

```
#!/bin/bash
set -e
image=${1:?Supply the .iso image of a Gentoo installation medium}
target=${2:?Supply the target device}
echo Checking for the necessary tools presence...
which syslinux
which sfdisk
which mkfs.vfat
echo Mounting Gentoo CD image...
cdmountpoint=/mnt/gentoo-cd
mkdir -p "$cdmountpoint"
trap 'echo Unmounting Gentoo CD image...; umount "$cdmountpoint"' EXIT
mount -o loop,ro "$image" "$cdmountpoint"
echo Creating a disk-wide EFI FAT partition on "$target"...
echo ',,U,*' | sfdisk --wipe always "$target"
echo Installing syslinux MBR on "$target"...
dd if=/usr/share/syslinux/mbr.bin of="$target"
sleep 1
echo Creating file system on "$target"1...
mkfs.vfat "$target"1 -n GENTOO
echo Mounting file system...
mountpoint=/mnt/gentoo-usb
mkdir -p "$mountpoint"
mount "$target"1 "$mountpoint"
echo Copying files...
cp -r "$cdmountpoint"/* "$mountpoint"/
mv "$mountpoint"/isolinux/* "$mountpoint"
mv "$mountpoint"/isolinux.cfg "$mountpoint"/syslinux.cfg
rm -rf "$mountpoint"/isolinux*
mv "$mountpoint"/memtest86 "$mountpoint"/memtest
sed -i -e "s:cdroot:cdroot slowusb:" -e "s:kernel memtest86:kernel memtest:" "$mountpoint"/syslinux.cfg
echo Unmounting file system...
umount "$mountpoint"
echo Installing syslinux on "$target"1
syslinux "$target"1
echo Syncing...
sync
echo 'Done!'
```
### Manually preparing a LiveUSB drive

#### Preparing the USB drive

##### Partitioning the drive

Create a FAT16 partition on the USB drive and mark it bootable using fdisk. An example partitioning scheme can be seen below:

`root #``fdisk -l /dev/sdc`
Disk /dev/sdc: 2063 MB, 2063597056 bytes
255 heads, 63 sectors/track, 250 cylinders
Units = cylinders of 16065 \* 512 = 8225280 bytes
  
   Device Boot      Start         End      Blocks   Id  System
/dev/sdc1   \*           1         250     2008124+   6  FAT16

##### Creating the filesystem

Create a FAT16 filesystem on the USB drive using mkfs.fat:

`root #``emerge --ask sys-fs/dosfstools``root #``mkfs.fat -F 16 /dev/sdc1`
mkfs.fat 3.0.22 (2013-07-19)

##### Installing a Master Boot Record (MBR)

Install the precompiled Master Boot Record (MBR) from syslinux on the USB drive:

`root #``emerge --ask --verbose sys-boot/syslinux``root #``dd if=/usr/share/syslinux/mbr.bin of=/dev/sdc`
0+1 records in
0+1 records out
440 bytes (440 B) copied, 0.00522668 s, 84.2 kB/s

#### Copying the files

##### Mounting the Gentoo Minimal Installation CD

Download a Gentoo Minimal Installation CD for the system's architecture from a the main site's [download page](https://www.gentoo.org/downloads/) and mount the ISO image on /mnt/cdrom as shown below:

`root #````
mkdir -p /mnt/cdrom
```
`root #````
mount -o loop,ro -t iso9660 /path/to/isofile.iso /mnt/cdrom
```
Adjust the `/path/to/isofile.iso` as necessary to the location of the downloaded Minimal Installation CD ISO.

##### Mounting the LiveUSB

Mount the newly formatted USB drive on /mnt/usb as shown below:

`root #````
mkdir -p /mnt/usb
```
`root #````
mount -t vfat /dev/sdc1 /mnt/usb
```
##### Copying the files

Copy the files from the Minimal Installation CD to the LiveUSB. The files need to be reordered since syslinux will be used as the bootloader:

`root #````
cp -r /mnt/cdrom/* /mnt/usb
```
Unmount the ISO image:

`root #``umount /mnt/cdrom`
##### Create the bootloader configuration

create the syslinux configuration file .

**`/mnt/usb/syslinux.cfg`**

#### Installing a bootloader

##### Unmounting the drive

Make sure the USB drive has been unmounted before installing the bootloader:

`root #``umount /mnt/usb`
##### Installing syslinux

Finally install the syslinux bootloader on the USB drive:

`root #``syslinux /dev/sdc1`
