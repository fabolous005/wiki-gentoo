<!-- source: https://wiki.gentoo.org/wiki/Btrfs/Encrypted_Btrfs_System_Root_Guide | group: Gentoo Wiki (Main) | wiki-title: Btrfs/Encrypted Btrfs System Root Guide -->
---
title: Btrfs/Encrypted Btrfs System Root Guide
url: https://wiki.gentoo.org/wiki/Btrfs/Encrypted_Btrfs_System_Root_Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-09-25"
fingerprint: "9b843213806d6ad9"
license: CC BY-SA 4.0
---

# Btrfs/Encrypted Btrfs System Root Guide

From Gentoo Wiki

\< [Btrfs](https://wiki.gentoo.org/wiki/Btrfs)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article discusses several aspects of using Dm-crypt for full disk encryption with Btrfs for daily usage.

## Disk preparation

This example will use GPT as disk partition schema and grub(or rEFInd) as boot loader.

## Create partitions

To create GRUB BIOS, issue the following command:

`root #``parted -a optimal /dev/sdX`
Set the default units to mebibytes:

`(parted)``unit mib`
Create a GPT partition table:

`(parted)``mklabel gpt`
Create the BIOS partition:

`root #``mkpart primary 1 3``root #``name 1 grub``root #``set 1 bios_grub on`
Create boot partition:

`root #``mkpart primary fat32 3 515``root #``name 2 boot``root #``set 2 BOOT on`
Create root partition:

`root #``mkpart primary 515 -1``root #``name 3 root`
Exit from parted:

`root #``quit`
## Create boot filesystem

Create filesystem for /dev/sdX2.This partition is read by UEFI bios. Most of motherboards can ready only FAT32 filesystems:

`root #``mkfs.vfat -F32 /dev/sdX2`
## Prepare encrypted partition

In the next step, configure dm-crypt for /dev/sdX3:

`root #``modprobe dm-crypt``root #``cryptsetup luksFormat /dev/sdX3`
Open encrypted device and create btrfs filesystem:

`root #``cryptsetup luksOpen /dev/sdX3 root``root #``mkfs.btrfs /dev/mapper/root`
