<!-- source: https://wiki.gentoo.org/wiki/Fdisk | group: Gentoo Wiki (Main) | wiki-title: Fdisk -->
---
title: Fdisk
url: https://wiki.gentoo.org/wiki/Fdisk
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-05-13"
fingerprint: bcbd19428d6786c9
license: CC BY-SA 4.0
---

# Fdisk

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

fdisk is a dialog-driven command-line utility commonly found on Linux systems that is used for disk partitioning. It is one of the most commonly used tools for creating and managing disk partitions.

## Overview

The fdisk utility allows users to create and manipulate partition tables on a hard disk. It supports GPT, MBR, Sun, SGI, and BSD partition tables. Users can create, resize, modify, delete, and move disk partitions.

## Installation

### Emerge

The fdisk utility should already be available on any Gentoo systems, since it is a part of [util-linux](https://wiki.gentoo.org/wiki/Util-linux) package, which is in [@system](<https://wiki.gentoo.org/wiki/System_set_(Portage)>). In case it's not, emerge the package with:

`root #``emerge --ask sys-apps/util-linux`
## Usage

To list the partition table of a device, the fdisk command is invoked with the `-l` option, followed by the device name. For example, to list the /dev/nvme0n1 partition table and partitions, the command would be `fdisk -l /dev/nvme0n1`.

`root #``fdisk -l /dev/nvme0n1`
Disk /dev/nvme0n1: 931.51 GiB, 1000204886016 bytes, 1953525168 sectors
Disk model: Samsung SSD 980 PRO 1TB                 
Units: sectors of 1 \* 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 4BABCAFE-B0BA-DEAD-BEEF-A0B105F00D0A
Device           Start        End    Sectors   Size Type
/dev/nvme0n1p1    2048    8390655    8388608     4G EFI System
/dev/nvme0n1p2 8390656 1953523711 1945133056 927.5G Linux filesystem

To start partitioning a drive, fdisk is run with the device name. For example, to work on /dev/nvme0n1, the command would be `fdisk /dev/nvme0n1`.
