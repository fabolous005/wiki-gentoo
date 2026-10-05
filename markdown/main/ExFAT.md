<!-- source: https://wiki.gentoo.org/wiki/ExFAT | group: Gentoo Wiki (Main) | wiki-title: ExFAT -->
---
title: exFAT
url: https://wiki.gentoo.org/wiki/ExFAT
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-25"
fingerprint: "8e0949bccca7b8f5"
license: CC BY-SA 4.0
---

# exFAT

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


exFAT (**Ex**tended **F**ile **A**llocation **T**able) is a Microsoft file system optimized for flash memory storage such as USB sticks.

The availability of the exFAT filesystem had long been poor, because of its proprietary, unpublished specification. The situation, however, was improved after release of Linux kernel 5.7 with native exFAT driver implementation.[\[1\]](https://wiki.gentoo.org#cite_note-1)

## Installation

### Kernel

Enable exFAT support in the kernel:

**Enable support for CONFIG\_EXFAT\_FS**

### Emerge

Install the [sys-fs/exfatprogs](https://packages.gentoo.org/packages/sys-fs/exfatprogs) package:

`root #``emerge --ask sys-fs/exfatprogs`
## Usage

### Formatting

To create an exFAT file system, use mkfs.exfat:

`user $``mkfs.exfat````
exfatprogs 1.0.4
Usage: mkfs.exfat
        -L | --volume-label=label                              Set volume label
        -c | --cluster-size=size(or suffixed by 'K' or 'M')    Specify cluster size
        -b | --boundary-align=size(or suffixed by 'K' or 'M')  Specify boundary alignment
        -f | --full-format                                     Full format
        -V | --version                                         Show version
        -v | --verbose                                         Print debug
        -h | --help                                            Show help
```
For instance, to create it on a removable device present at /dev/sde1 while assigning "Flash" as the file system label:

`root #``mkfs.exfat -L Flash /dev/sde1`
### Mounting

With native support, standard mount commands work perfectly:

`root #``mount /dev/sde1 /mnt/flash`
### Integrity checking

To check the integrity of an exFAT filesystem, use fsck.exfat:

`root #``fsck.exfat /dev/sde1`
## See also

- [FAT](https://wiki.gentoo.org/wiki/FAT) — [filesystem](https://wiki.gentoo.org/wiki/Filesystem) originally created for use with MS-DOS (and later pre-NT Microsoft Windows).
- [NTFS](https://wiki.gentoo.org/wiki/NTFS) — a proprietary disk [filesystem](https://wiki.gentoo.org/wiki/Filesystem) by Microsoft for Windows (NT-based) and WindowsNT-based operating systems.
- [Ext4](https://wiki.gentoo.org/wiki/Ext4) — an open source disk [filesystem](https://wiki.gentoo.org/wiki/Filesystem) and the most recent version of the extended series of filesystems.
