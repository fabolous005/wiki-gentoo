<!-- source: https://wiki.gentoo.org/wiki/HFS | group: Gentoo Wiki (Main) | wiki-title: HFS -->
---
title: HFS
url: https://wiki.gentoo.org/wiki/HFS
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-02-23"
fingerprint: "9d8d593bb3c5bced"
license: CC BY-SA 4.0
---

# HFS

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The **Hierarchical File System**, abbreviated *HFS*, is the native filesystem of the Apple Macintosh and its operating system up to Mac OS 8. It was the primary base [filesystem](https://wiki.gentoo.org/wiki/Filesystem) of the Macintosh on the Motorola 68000 architecture. The filesystem is case-insensitive.

Its successor [HFS+](https://wiki.gentoo.org/wiki/HFS%2B) became the standard base filesystem in Mac OS 8.1 on PowerPC as well as Mac OS X on PowerPC and Intel x86. However, on PowerPC-based Macs a HFS partition is still used for the bootstrap partition.

## Installation

### Kernel

**Enable HFS support (`CONFIG_HFS_FS`)**

Since HFS partitions are normally only found on Apple Partition Maps (APM), enable this partitioning scheme in the kernel:

**Enable APM support (`CONFIG_MAC_PARTITION`)**

### Emerge

#### diskdev\_cmds

The [sys-fs/diskdev\_cmds](https://packages.gentoo.org/packages/sys-fs/diskdev_cmds) package is a port of HFS/HFS+ utilities from OpenDarwin, the BSD core operating system of macOS. It includes mkfs and fsck userspace utilities.

`root #``emerge --ask sys-fs/diskdev_cmds`
The commands have names common to OpenDarwin: newfs\_hfs and fsck\_hfs. The package creates symlinks to mkfs.hfs and fsck.hfs. To create a new filesystem on a partition, e.g. on /dev/sda3, use:

`root #``newfs_hfs -h /dev/sda3`
To use newfs\_hfs to create the NewWorld Bootblock from the example below, the command would be:

`root #``newfs_hfs -h -v "NewWorld Bootblock" /dev/sda3`
#### hfsutils

The [sys-fs/hfsutils](https://packages.gentoo.org/packages/sys-fs/hfsutils) package traditionally provides means to access the Hierarchical File System on m68k and ppc/ppc64 Macs. The included utilities provide limited access to a filesystem on a selected partition.

`root #``emerge --ask sys-fs/hfsplusutils`
Instead of mounting the filesystem as a subfolder under the root directory /, the various utilities of package [sys-fs/hfsutils](https://packages.gentoo.org/packages/sys-fs/hfsutils) access the filesystem directly. hmount selects the partition for theese utilities and humount releases it again. The use of [sys-fs/mac-fdisk](https://packages.gentoo.org/packages/sys-fs/mac-fdisk) is highly recommended.

`root #``emerge --ask sys-fs/mac-fdisk``root #``mac-fdisk -l /dev/sda` ```
/dev/sda
        #                    type name                  length   base      ( size )  system
/dev/sda1     Apple_partition_map Apple                     63 @ 1         ( 31.5k)  Partition map
/dev/sda2              Apple_Free Extra                 262144 @ 64        (128.0M)  Free space
/dev/sda3              Apple_Boot NewWorld Bootblock   1834944 @ 262208    (896.0M)  Unknown
/dev/sda4              Apple_Free Extra                 262144 @ 2097152   (128.0M)  Free space
/dev/sda5               Apple_HFS Tiger              209715200 @ 2359296   (100.0G)  HFS
/dev/sda6              Apple_Free Extra                 262144 @ 212074496 (128.0M)  Free space
/dev/sda7               Apple_HFS Leopard            314572800 @ 212336640 (150.0G)  HFS
/dev/sda8              Apple_Free Extra                 262144 @ 526909440 (128.0M)  Free space
/dev/sda9              Apple_Boot Apple Hardware Test   100898 @ 527171584 ( 49.3M)  Unknown
/dev/sda10             Apple_Free Extra                 262144 @ 527272482 (128.0M)  Free space
/dev/sda11        Apple_UNIX_SVR2 Linux Boot           2097152 @ 527534626 (  1.0G)  Linux native
/dev/sda12             Apple_Free Extra                 262144 @ 529631778 (128.0M)  Free space
/dev/sda13        Apple_UNIX_SVR2 Gentoo Linux       134217728 @ 529893922 ( 64.0G)  Linux native
/dev/sda14             Apple_Free Extra                 262144 @ 664111650 (128.0M)  Free space
/dev/sda15        Apple_UNIX_SVR2 Debian Linux       134217728 @ 664373794 ( 64.0G)  Linux native
/dev/sda16             Apple_Free Extra                 262144 @ 798591522 (128.0M)  Free space
/dev/sda17        Apple_UNIX_SVR2 Linux Local         71216270 @ 798853666 ( 34.0G)  Linux native
/dev/sda18             Apple_Free Extra                 262144 @ 870069936 (128.0M)  Free space
/dev/sda19        Apple_UNIX_SVR2 Linux swap          67108864 @ 870332080 ( 32.0G)  Linux native
/dev/sda20             Apple_Free Extra                 262144 @ 937440944 (128.0M)  Free space
Block size=512, Number of Blocks=937703088
DeviceType=0x0, DeviceId=0x0
```
`root #``parted /dev/sda print` Model: ATA TOSHIBA-TR200 (scsi)
Disk /dev/sda: 480GB
Sector size (logical/physical): 512B/512B
Partition Table: mac
Disk Flags: 
Number  Start   End     Size    File system     Name                 Flags
 1      512B    32.8kB  32.3kB                  Apple
 3      134MB   1074MB  939MB   hfs             NewWorld Bootblock   boot
 5      1208MB  109GB   107GB   hfs+            Tiger
 7      109GB   270GB   161GB   hfs+            Leopard
 9      270GB   270GB   51.7MB  hfs+            Apple Hardware Test  boot
11      270GB   271GB   1074MB  ext2            Linux Boot
13      271GB   340GB   68.7GB  btrfs           Gentoo Linux
15      340GB   409GB   68.7GB  btrfs           Debian Linux
17      409GB   445GB   36.5GB  ext4            Linux Local
19      446GB   480GB   34.4GB  linux-swap(v1)  Linux swap

Select the partition with hmount:

`root #``hmount /dev/sda4`
On the selected partition, the commands h\[format|vol|fsck|ls|dir|pwd|mkdir|cd|rmdir|attrib|copy|rename|del\] can be used for file and filesystem operations.

`root #``hls`
Apple Extras     Desktop Folder   Develop

`root #``hcp /path/to/source_file_from_linux_filesystem :path:to:target_file_on_hfs_volume``root #``hcp :path:to:source_file_on_hfs_volume /path/to/target_file_from_linux_filesystem`
This will copy a file from the Linux filesystem to the HFS volume and vice versa. The utilities will automatically determine if the source or target is on the HFS volume or the Linux filesystem, thus the pathname should be unambiguous and use the slash "/" for Linux and the colon ":" for HFS.

The partition is released (unmounted) with the humount utility:

`root #``humount /dev/sda4`
### NewWorld Bootblock

A specialty of the PowerPC-based NewWorld Macs is the use of a NewWorld Bootblock for yaboot or GRUB. NewWorld Macs are PowerPC-based Macs with Open Firmware version 3.0 (OF3) or later. OF3+ will automatically look for bootable partitions, like those of the type Apple\_Boot. When such a partition contains a filesystem of the type HFS, and this filesystem contains a "blessed" file (attribute ":tbxi"), this file will be selectable as a boot option from OF3+.

First, a NewWorld Bootblock has to be created using mac-fdisk:

`root #``mac-fdisk /dev/sda`
At the interactive command prompt this partition can either be created manually e.g. by using the C command for "create new partition, specifying the partition type", or semi-automatic by using the b command for "create new 800k Apple\_Bootstrap partition (used by yaboot)". Either way you should get a bootable HFS partition of either the type Apple\_Boot or Apple\_Bootstrap.

Assuming this partition is /dev/sda3, to bless a bootloader such as yaboot or GRUB, hattrib can be used.

`root #``hformat -l "NewWorld Bootblock" /dev/sda3``root #``hmount /dev/sda3``root #``hcopy /boot/grub/powerpc-ieee1275/core.elf :core.elf``root #``hattrib -c UNIX -t tbxi :core.elf``root #``hattrib -b :``root #``humount /dev/sda3`
## See also

- [HFS+](https://wiki.gentoo.org/wiki/HFS%2B) — the native filesystem of Mac OS 8.1+ and Mac OS X up to 10.12 Sierra from Apple for Mac computers.
- [Filesystem](https://wiki.gentoo.org/wiki/Filesystem) — a means to organize data to be retained after a program terminates.
- [Mount](https://wiki.gentoo.org/wiki/Mount) — the attaching of an additional [filesystem](https://wiki.gentoo.org/wiki/Filesystem) to the currently accessible filesystem of a computer.
- [Removable media](https://wiki.gentoo.org/wiki/Removable_media) — any media that is easily removed from a system.
- [/etc/fstab](https://wiki.gentoo.org/wiki//etc/fstab) — a configuration file that defines how and where the main [filesystems](https://wiki.gentoo.org/wiki/Filesystem) are to be mounted, especially at boot time.
