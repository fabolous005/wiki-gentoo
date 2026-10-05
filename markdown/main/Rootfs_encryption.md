<!-- source: https://wiki.gentoo.org/wiki/Rootfs_encryption | group: Gentoo Wiki (Main) | wiki-title: Rootfs encryption -->
---
title: Rootfs encryption
url: https://wiki.gentoo.org/wiki/Rootfs_encryption
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-04"
fingerprint: "9d01241a83ecbcc8"
license: CC BY-SA 4.0
---

# Rootfs encryption

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Encrypting the root [filesystem](https://wiki.gentoo.org/wiki/Filesystem) can enhance privacy, and prevent unauthorized access.

## Installation

### Emerge

`root #``emerge --ask sys-fs/cryptsetup`
## System preparation

This guide is designed to be followed as part of a fresh Gentoo install, the install procedure can be followed until the following step: [AMD64 Handbook: Designing a partition scheme](https://wiki.gentoo.org/wiki/Handbook:AMD64/Full/Installation#Designing_a_partition_scheme)

## Disk preparation

This example will use [GPT](https://wiki.gentoo.org/wiki/GPT) as disk partition schema. [fdisk](https://wiki.gentoo.org/wiki/Fdisk) will be used as the partitioning tool though any partitioning utility will work.

### Simple EFI System Partition Layout

In most cases, only an ESP is required, to create one on the same disk as the encrypted root:

#### Configure GPT label

First, a fresh partition table is created on /dev/nvme0n1 with:

`root #``fdisk /dev/nvme0n1`
Welcome to fdisk (util-linux 2.38.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.
 
Device does not contain a recognized partition table.
Created a new DOS disklabel with disk identifier 0x81391dbc.

`Command (m for help):``g`
Created a new GPT disklabel (GUID: 8D91A3C1-8661-2940-9076-65B815B36906).

#### Create the ESP

With a GPT partition table created, the [EFI System Partition](https://wiki.gentoo.org/wiki/EFI_System_Partition) (ESP) can be added using **n**:

`Command (m for help):``n````
Partition number (1-128, default 1): 
First sector (2048-134217694, default 2048): 
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-134217694, default 134215679): +1G
 
Created a new partition 1 of type 'Linux filesystem' and of size 1 GiB.
```
The **ESP** property can be set using **t**:

`Command (m for help):``t`
Selected partition 1
Partition type or alias (type L to list all): 1
Changed type of partition 'Linux filesystem' to 'EFI System'.

#### Create the Root partition

The root partition can be created with:

`Command (m for help):``n````
Partition number (2-128, default 2):
First sector (2099200-134217694, default 2099200): 
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2099200-134217694, default 134215679):
 
Created a new partition 2 of type 'Linux filesystem' and of size 62 GiB.
```
The **Linux Root (x86-64)** property can be set using **t**:

`Command (m for help):``t`
Partition number (1-2, default 2):
Partition type or alias (type L to list all): 23
Changed type of partition 'Linux filesystem' to 'Linux Root (x86-64)'.

#### Apply changes

Finally, the changes can be written with **w**:

`Command (m for help):``w`
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.

### Split EFI/BOOTx Grub layout

If an additional boot partition is needed, it can be created in addition to an ESP.

#### Configure GPT label

To create a partition layout using fdisk, start by creating a fresh partition table on the root disk:

`root #``fdisk /dev/nvme0n1`
Welcome to fdisk (util-linux 2.38.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.
 
Device does not contain a recognized partition table.
Created a new DOS disklabel with disk identifier 0x81391dbc.

`Command (m for help):``g`
Created a new GPT disklabel (GUID: 8D91A3C1-8661-2940-9076-65B815B36906).

#### Create the ESP

With a GPT partition table created, the [EFI System Partition](https://wiki.gentoo.org/wiki/EFI_System_Partition) (ESP) can be added using **n**:

`Command (m for help):``n````
Partition number (1-128, default 1): 
First sector (2048-134217694, default 2048): 
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-134217694, default 134215679): +1G
 
Created a new partition 1 of type 'Linux filesystem' and of size 1 GiB.
```
The **ESP** property can be set using **t**:

`Command (m for help):``t`
Selected partition 1
Partition type or alias (type L to list all): 1
Changed type of partition 'Linux filesystem' to 'EFI System'.

#### Create the Extended Boot partition

The boot partition can be created with:

`Command (m for help):``n````
Partition number (2-128, default 2): 
First sector (2099200-134217694, default 2099200): 
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2099200-134217694, default 134215679): +1G
 
Created a new partition 2 of type 'Linux filesystem' and of size 1 GiB.
```
The **Linux Extended Boot** property can be set using **t**:

`Command (m for help):``t`
Partition number (1-2, default 2):
Partition type or alias (type L to list all): 142
Changed type of partition 'Linux filesystem' to 'Linux Extended Boot'.

#### Create the Root partition

The root partition can be created with:

`Command (m for help):``n````
Partition number (3-128, default 3): 
First sector (4196352-134217694, default 4196352): 
Last sector, +/-sectors or +/-size{K,M,G,T,P} (4196352-134217694, default 134215679):
 
Created a new partition 3 of type 'Linux filesystem' and of size 62 GiB.
```
The **Linux Root (x86-64)** property can be set using **t**:

`Command (m for help):``t`
Partition number (1-3, default 3):
Partition type or alias (type L to list all): 23
Changed type of partition 'Linux filesystem' to 'Linux Root (x86-64)'.

#### Apply changes

Finally, the changes can be written with **w**:

`Command (m for help):``w`
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.

## LUKS setup

Once partitions have been created, cryptsetup can be used to format the LUKS volumes.

### Create the LUKS encrypted partition

To prepare the encrypted filesystem, [dm-crypt](https://wiki.gentoo.org/wiki/Dm-crypt) can be used:

To format the root partition (/dev/nvme0n1p2) using LUKS, secured with a passphrase:

`root #``cryptsetup luksFormat /dev/nvme0n1p2`
WARNING!
========
This will overwrite data on /dev/nvme0n1p2 irrevocably.
 
Are you sure? (Type 'yes' in capital letters): 
YES
Enter passphrase for /dev/nvme0n1p2:

#### LUKS Header Backup

The headers can be backed up with:

`root #``cryptsetup luksHeaderBackup /dev/nvme0n1p2 --header-backup-file root_headers.img`
### Open the LUKS volume

The encrypted device must be opened and mapped before it can be used, this can be done with:

`root #``cryptsetup luksOpen /dev/nvme0n1p2 root`
### Set LUKS flags

On [SSDs](https://wiki.gentoo.org/wiki/SSD), allow discard instructions to pass through from the file system by setting the `allow-discards` flag:

`root #``cryptsetup refresh --persistent --allow-discards root`
## Filesystems Preparation

Once partitions are formatted, and LUKS volumes are unlocked, they must be formatted before they can be mounted and used.

### ESP

The [UEFI](https://wiki.gentoo.org/wiki/UEFI) on most motherboards can only read [FAT32](https://wiki.gentoo.org/wiki/FAT) filesystems. To format the ESP:

`root #``mkfs.vfat -F32 /dev/nvme0n1p1`
### Root filesystem

To format the root filesystem with [Btrfs](https://wiki.gentoo.org/wiki/Btrfs):

`root #``mkfs.btrfs -L rootfs /dev/mapper/root`
### Optional: Extended boot formatting

If used, the extended boot partition must be formatted. Any filesystem which the bootloader supports can be used.

To format the BOOTx partition at /dev/nvme0n1p2 with [Ext4](https://wiki.gentoo.org/wiki/Ext4):

`root #``mkfs.ext4 -L boot /dev/nvme0n1p2`
## Gentoo installation

If this procedure is being followed during a Gentoo install (in place of [Designing a partition scheme](https://wiki.gentoo.org/wiki/Handbook:AMD64/Full/Installation#Designing_a_partition_scheme) through [Mounting the root partition](https://wiki.gentoo.org/wiki/Handbook:AMD64/Full/Installation#Mounting_the_root_partition)), the install can be completed using the handbook with a few important considerations:

The root file system can be mounted at /mnt/gentoo to continue the install with:

`root #``mount --label rootfs /mnt/gentoo`
At this point, the Gentoo install can be continued: [Installing a stage tarball](https://wiki.gentoo.org/wiki/Handbook:AMD64/Full/Installation#Installing_a_stage_tarball).

## Initramfs configuration

An [initramfs](https://wiki.gentoo.org/wiki/Initramfs) must be used to decrypt and mount the root partition. This can be accomplished using an image generated by a tool such as [UgRD](https://wiki.gentoo.org/wiki/UgRD) or [Dracut](https://wiki.gentoo.org/wiki/Dracut).

### UGRD

ugrd can be installed directly, but is best installed by adding it as a USE flag for installkernel:

**`/etc/portage/package.use/ugrd`**

Once configured, installkernel can be re-emerged and will pull UGRD:

`root #``emerge -1 sys-kernel/installkernel`
To force an initramfs rebuild, emerge --config can be used on dist-kernel packages:

`root #``emerge --config sys-kernel/gentoo-kernel-bin`
### Dracut

In order to properly decrypt LUKS volumes, Dracut must be configured to use the `crypt` module, and cmdline parameters specifying the LUKS information must be configured:

The following modules must be added to the `add_dracutmodules` directive in /etc/dracut.conf.d/luks.conf:

#### Module configuration

**`/etc/dracut.conf.d/luks.conf`**

**Minimum required component to decrypt LUKS volumes using dracut**

#### LUKS target configuration

Dracut can be configured to build with configuration for LUKS hardcoded, first disk information must be obtained:

`root #``lsblk -o name,uuid`
NAME        UUID
sdb                                           
├─nvme0n1p1 BDF2-0139
├─nvme0n1p2 b0e86bef-30f8-4e3b-ae35-3fa2c6ae705b
└─nvme0n1p3 4bb45bd6-9ed9-44b3-b547-b411079f043b
  └─root    cb070f9e-da0e-4bc5-825c-b01bb2707704

Dracut expects to find the specified root drive on the kernel cmdline. For setups where a UKI is in use, and dracut is used for UKI generation, this information can be provided like so:

**`/etc/dracut.conf.d/luks.conf`**

**Embed cmdline parameters for rootfs decryption**

For (default) setups without a UKI, refer to the specific page for the bootloader of choice to append the rootfs decryption cmdline parameters to its specific configuration location.

#### Systemd

When using [systemd](https://wiki.gentoo.org/wiki/Systemd), rebuild it with the USE-flag `cryptsetup`:

**`/etc/portage/package.use/systemd`**

`root #``emerge --ask sys-apps/systemd`
**`/etc/dracut.conf.d/luks.conf`**

**Embed cmdline parameters for rootfs decryption**

#### Building the image

Once Dracut is configured, the new initramfs is ready to be generated. To force an initramfs rebuild, emerge --config can be used on dist-kernel packages:

`root #``emerge --config sys-kernel/gentoo-kernel-bin`
## Booting with the initramfs

### efibootmgr

Extensible Firmware Interface systems may boot an EFI stub kernel with initramfs using [efibootmgr](https://wiki.gentoo.org/wiki/Efibootmgr). A page relevant example is provided:

`root #``efibootmgr --create --disk /dev/nvme0n1 --label "Gentoo" --loader "vmlinuz-6.1.28-gentoo" --unicode "initrd=initramfs-6.1.28-gentoo"`
#### dracut with systemd

If using dracut with systemd, the rd.luks.uuid parameter should be passed in the cmdline, not embedded:

`root #``efibootmgr --create --disk /dev/nvme0n1 --label "Gentoo" --loader "vmlinuz-6.1.28-gentoo" --unicode "initrd=initramfs-6.1.28-gentoo rd.luks.uuid=4bb45bd6-9ed9-44b3-b547-b411079f043b"`
### GRUB

[GRUB](https://wiki.gentoo.org/wiki/GRUB) should be installed into the EFI System Partition. On Newer systems this is:

`root #``grub-install --efi-directory=/efi`
On DOS/Legacy BIOS systems and UEFI systems installed longer ago, run:

`root #``grub-install --efi-directory=/boot`
## See also

- [Dm-crypt](https://wiki.gentoo.org/wiki/Dm-crypt) — a disk encryption system using the kernels crypto API framework and device mapper subsystem.
- [Efibootmgr](https://wiki.gentoo.org/wiki/Efibootmgr) — a tool for managing [UEFI](https://wiki.gentoo.org/wiki/UEFI) boot entries.
- [Full Disk Encryption From Scratch](https://wiki.gentoo.org/wiki/Full_Disk_Encryption_From_Scratch) — a guide which covers the process of configuring a drive to be encrypted using LUKS and btrfs.
- [Unlocking Rootfs encryption over SSH](https://wiki.gentoo.org/wiki/Unlocking_Rootfs_encryption_over_SSH)
