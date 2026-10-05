<!-- source: https://wiki.gentoo.org/wiki/Raspberry_Pi/Install | group: Gentoo Wiki (Main) | wiki-title: Raspberry Pi/Install -->
---
title: Raspberry Pi/Install
url: https://wiki.gentoo.org/wiki/Raspberry_Pi/Install
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-11-21"
fingerprint: bd28b0db2dcdcbcb
license: CC BY-SA 4.0
---

# Raspberry Pi/Install

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

### Formatting The Disks

Once you're booted to either a Gentoo installation medium or a working Gentoo system, insert the SD card into a USB port and type the following -

`root #` `parted -l`
This will list the available disks and partitions. The particular one you're looking for should say "Mass Storage Device (scsi)" and the capacity should match the size of your SD card -

`root #` `parted -l`
Model: ATA WDC WD800BB-22JH (scsi)

Disk /dev/sda: 80.0GB

Sector size (logical/physical): 512B/512B

Partition Table: gpt

Disk Flags:


Number Start End Size File system Name Flags

1      1049kB  3146kB  2097kB                  grub    bios\_grub
2      3146kB  137MB   134MB   ext2            boot    boot, esp
3      137MB   4432MB  4295MB  linux-swap(v1)  swap
4      4432MB  80.0GB  75.6GB  ext4            rootfs


Model: Mass Storage Device (scsi)

Disk /dev/sdb: 31.9GB

Sector size (logical/physical): 512B/512B

Partition Table: msdos

Disk Flags:


Number Start End Size Type File system Flags

1      1049kB  1605MB  1604MB  primary                lba
2      1605MB  31.9GB  30.3GB  extended               lba
5      1606MB  3753MB  2146MB  logical
6      3754MB  31.9GB  28.2GB  logical

As shown above, my SD card would be device /dev/sdb.

The boot partition on a Raspberry Pi SD card must be a FAT32 partition. However, unlike a standard hard drive, the boot partition does not need to be made bootable.

Next we'll start Parted to make our partitions. We'll use my table above as our example partition table. You'll notice that I have a primary partition as the boot partition, while I then create an extended partition with two logical partitions for the swap and root partitions.

`root #` `parted -a optimal /dev/sdb`
First, let's make our label, which will be "msdos" -

parted| mklabel msdos
