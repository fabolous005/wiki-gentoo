<!-- source: https://wiki.gentoo.org/wiki/Raspberry_Pi_Install_Guide/Preparing_the_disks | group: Gentoo Wiki (Main) | wiki-title: Raspberry Pi Install Guide/Preparing the disks -->
---
title: Raspberry Pi Install Guide/Preparing the disks
url: https://wiki.gentoo.org/wiki/Raspberry_Pi_Install_Guide/Preparing_the_disks
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-25"
fingerprint: bc199850ddb48cdb
license: CC BY-SA 4.0
---

# Raspberry Pi Install Guide/Preparing the disks

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Preparing the disks

This page assumes that the (micro)SD card is fitted to a card to USB adapter for the install part of work on the install host. Therefore it will appear as /dev/sdX. This page uses /dev/sdi.

It further assumes that at boot time, in the Pi, it will be /dev/mmcblk0. USB booting the Pi3 and later is possible but requires a microSD card boot first, so is deliberately not covered here.

Readers with a SD slot on the install host will find that target SD card is /dev/sdX or /dev/mmcblkY depending on the hardware in use by the host.

### Locate the target drive

The disk will be a (micro)SD card. The Pi3 and later can also boot from USB but that can require booting from SD first, to set up USB booting.

Modern Pi4 and Pi5 firmware work with both MSDOS and GPT partition tables

The first partition on the SD card must be /boot, formatted VFAT.

Root (/) can be anything that is built into the kernel. EXT4 is known to work.

If the Pi will be used for building, swap will be useful.

The GPU does all the hard work for loading the CPU boot files. Indeed, the ARM CPU is held reset until the kernel, device tree binary and optionally, the initrd are all loaded into RAM. There is no grub/lilo/syslinux equivalent for the Pi family.

Plug the drive in to any desktop system and find it

`root #``dmesg`
...
[ 1613.049729] scsi 11:0:0:0: Direct-Access     Samsung  SSD 870 EVO 1TB  0    PQ: 0 ANSI: 6
[ 1613.051603] sd 11:0:0:0: Attached scsi generic sg10 type 0
[ 1613.051811] sd 11:0:0:0: [sdi] 1953525168 512-byte logical blocks: (1.00 TB/932 GiB)
[ 1613.051938] sd 11:0:0:0: [sdi] Write Protect is off
[ 1613.051941] sd 11:0:0:0: [sdi] Mode Sense: 43 00 00 00
[ 1613.052096] sd 11:0:0:0: [sdi] Write cache: enabled, read cache: enabled, doesn't support DPO or FUA
[ 1613.052254] sd 11:0:0:0: [sdi] Preferred minimum I/O size 512 bytes
[ 1613.052256] sd 11:0:0:0: [sdi] Optimal transfer size 33553920 bytes
[ 1613.054759]  sdi: sdi1 sdi2 sdi3
[ 1613.055312] sd 11:0:0:0: [sdi] Attached SCSI disk

The Pi used for testing has already been configured for USB booting. Here, its /dev/sdi, on the Pi it will be /dev/sda.

### Make a new disk label and partition

On the Pi3 and older, the code to find the boot files is included within the GPU and cannot be updated.

The Pi4 and Pi5 use an EEPROM for this code. It still runs on the GPU. After the Pi4 was released, an EEPROM update to enable the Pi4 to boot from both DOS and GPT disk labels became available. The Pi5 can boot from both DOS and GPT disk labels.

As /dev/sdi has been used, start with a new disk label.

`root #``fdisk /dev/sdi`
Welcome to fdisk (util-linux 2.39.2).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Make sure its the right device.

`Command (m for help):``p`
Disk /dev/sdi: 931.51 GiB, 1000204886016 bytes, 1953525168 sectors
Disk model: SSD 870 EVO 1TB
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 33553920 bytes
Disklabel type: gpt
Disk identifier: 3D8E9053-4AD6-45AF-963A-96A42EB3CA89
Device         Start        End    Sectors   Size Type
/dev/sdi1	2048     501759     499712   244M EFI System
/dev/sdi2     501760  124999679  124497920  59.4G Linux filesystem
/dev/sdi3  124999680 1953523711 1828524032 871.9G Linux filesystem

Check against the dmesg output. If its OK, go ahead and destroy the data on the drive.

`Command (m for help):``g`
Created a new disklabel (GUID: BC6F5C13-002F-4B8C-B377-A26AFABDC45D)

Your disk label will have its own GUID.

### Make Partitions

Some good partition sizes are /boot, 256M, swap 8G, /home, if it will be separate 100G and the rest for root.

Make the partitions first and correct the types at the end.

Partitian 1 Must be VFAT. All the Pi foundation boot code will go here and a selection of kernels too. 256M is a good size.

`Command (m for help):``n````
Partition number (1-128, default 1): 1
First sector (2048-1953525134, default 2048):
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-1953525134, default 1953523711): +256M
Created a new partition 1 of type 'Linux filesystem' and of size 256 MiB.
Partition #1 contains a vfat signature.
Do you want to remove the signature? [Y]es/[N]o:
The signature will be removed by a write command.
```
The warning at the end is due to the drive being used and can be ignored.

Next 8G for swap.

`Command (m for help):``n````
Partition number (1-128, default 1): 2):
First sector (526336-1953525134, default 526336):
Last sector, +/-sectors or +/-size{K,M,G,T,P} (526336-1953525134, default 1953523711): +8G
Created a new partition 2 of type 'Linux filesystem' and of size 8 GiB.
```
then 100G for /home

`Command (m for help):``n````
Partition number (3-128, default 3):
First sector (17303552-1953525134, default 17303552):
Last sector, +/-sectors or +/-size{K,M,G,T,P} (17303552-1953525134, default 1953523711): +100G
Created a new partition 3 of type 'Linux filesystem' and of size 100 GiB.
```
Finally, the rest of the drive for root.

`Command (m for help):``n````
Partition number (4-128, default 4):
First sector (227018752-1953525134, default 227018752):
Last sector, +/-sectors or +/-size{K,M,G,T,P} (227018752-1953525134, default 1953523711):
Created a new partition 4 of type 'Linux filesystem' and of size 823.3 GiB.
```
Trust but verify.

`Command (m for help):``p`
Disk /dev/sdi: 931.51 GiB, 1000204886016 bytes, 1953525168 sectors
Disk model: SSD 870 EVO 1TB
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 33553920 bytes
Disklabel type: gpt
Disk identifier: BC6F5C13-002F-4B8C-B377-A26AFABDC45D
Device         Start        End    Sectors   Size Type
/dev/sdi1	2048     526335     524288   256M Linux filesystem
/dev/sdi2     526336   17303551   16777216     8G Linux filesystem
/dev/sdi3   17303552  227018751  209715200   100G Linux filesystem
/dev/sdi4  227018752 1953523711 1726504960 823.3G Linux filesystem
Filesystem/RAID signature on partition 1 will be wiped.

This drive was part of a mdadm raid set at one time. The warning can be ignored.

Set the types of partitions 1 and 2.

`Command (m for help):``t`
Partition number (1-4, default 4): 1
Partition type or alias (type L to list all): 11
Changed type of partition 'Linux filesystem' to 'Microsoft basic data'.
Command (m for help): t
Partition number (1-4, default 4): 2
Partition type or alias (type L to list all): 19
Changed type of partition 'Linux filesystem' to 'Linux swap'.

Check once more

`Command (m for help):``p`
Disk /dev/sdi: 931.51 GiB, 1000204886016 bytes, 1953525168 sectors
Disk model: SSD 870 EVO 1TB 
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 33553920 bytes
Disklabel type: gpt
Disk identifier: BC6F5C13-002F-4B8C-B377-A26AFABDC45D
Device         Start        End    Sectors   Size Type
/dev/sdi1	2048     526335     524288   256M Microsoft basic data
/dev/sdi2     526336   17303551   16777216     8G Linux swap
/dev/sdi3   17303552  227018751  209715200   100G Linux filesystem
/dev/sdi4  227018752 1953523711 1726504960 823.3G Linux filesystem
Filesystem/RAID signature on partition 1 will be wiped.

So far, the target drive has not been touched. The new disk label and partition table are in memory. After the next step, there is no going back.

Use 'q' to throw away the changes or 'w' to write them to disk

`Command (m for help):``w`
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.

### Make Filesystems

VFAT on the first partition, ext4 on /home and root and a swap label to swap

VFAT on /boot

`root #``mkfs -t vfat /dev/sdi1`

ext4 on /home

`root #``mkfs -t ext4 /dev/sdi3````
mke2fs 1.47.0 (5-Feb-2023)
Creating filesystem with 26214400 4k blocks and 6553600 inodes
Filesystem UUID: b7642a46-c15b-4453-9fe4-a2554c355fac
Superblock backups stored on blocks: 
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
        4096000, 7962624, 11239424, 20480000, 23887872
Allocating group tables: done
Writing inode tables: done
Creating journal (131072 blocks): done
Writing superblocks and filesystem accounting information: done
```


ext4 on root

`root #``mkfs -t ext4 /dev/sdi4````
mke2fs 1.47.0 (5-Feb-2023)
Creating filesystem with 215813120 4k blocks and 53960704 inodes
Filesystem UUID: c09f4b30-f30d-43b7-a7e6-67f7bf6b25d7
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
        4096000, 7962624, 11239424, 20480000, 23887872, 71663616, 78675968,
        102400000, 214990848
Allocating group tables: done
Writing inode tables: done
Creating journal (262144 blocks): done
Writing superblocks and filesystem accounting information: done
```

and a swap label on swap.

`root #``mkswap /dev/sdi2`
Setting up swapspace version 1, size = 8 GiB (8589930496 bytes)
no label, UUID=f351667c-3bf9-4bf5-a630-09da4fcc671f

the --pagesize option is required, as the install host probably uses 4096B pages.

`root #``mkswap --pagesize 16384 /dev/sdi2`
Setting up swapspace version 1, size = 8 GiB (8589930496 bytes)
no label, UUID=f351667c-3bf9-4bf5-a630-09da4fcc671f
