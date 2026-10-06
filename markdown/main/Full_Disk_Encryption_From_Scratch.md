<!-- source: https://wiki.gentoo.org/wiki/Full_Disk_Encryption_From_Scratch | group: Gentoo Wiki (Main) | wiki-title: Full Disk Encryption From Scratch -->
---
title: Full Disk Encryption From Scratch
url: https://wiki.gentoo.org/wiki/Full_Disk_Encryption_From_Scratch
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-31"
fingerprint: be011e5a9da4acd0
license: CC BY-SA 4.0
---

# Full Disk Encryption From Scratch

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Full disk encryption can be used to help protect data integrity and privacy. [dm-crypt](https://wiki.gentoo.org/wiki/Dm-crypt) can be used to configure drives to be encrypted with LUKS or other formats. This article is a guide which covers the process of configuring a drive to be encrypted using LUKS and btrfs. This process can be done as part of a fresh install, or could be performed on a new drive to migrate an existing install. It should be noted that most users don't require this level of encryption and is why the Handbook recommends to start with [Rootfs encryption](https://wiki.gentoo.org/wiki/Rootfs_encryption) first. This allows users to see if they require a stronger level of encryption more easily at a later date.

## Introduction

True *Full Disk Encryption* is **not** something that helps in most threat models, and it requires using separate storage to store all elements required to boot. These include, but are not limited to:

- The kernel image
- The initramfs image
- Detached headers
- Keyfiles
- The bootloader

Storing the kernel, initramfs, and bootloader on a separate device than the rootfs can present more problems than benefits. None of these components should contain sensitive data that would benefit from encryption, and LUKS does not provide data authentication. Storage on another device may increase or decrease the risk of tampering - depending on the threat model.

The authenticity of files used to boot is generally more important than the privacy.

The ultimate result of *Full Disk Encryption* is a device that when powered off, only has seemingly random data written to the storage. [Rootfs Encryption](https://wiki.gentoo.org/wiki/Rootfs_encryption) only reveals that the system likely uses Linux.

### LUKS headers

By default, LUKS uses a keyslot system, where volume contents are encrypted using a master key which is protected by any key installed into a LUKS keyslot for that header. This is helpful because multiple keys can be used to decrypt drive contents, and keys can be changed without re-encrypting the volume. This behavior is important to understand because it means that adding a weak key into a keyslot can degrade security, even if the rest of the keyslots are well protected.

#### Detached headers

LUKS headers can be detached, meaning they are stored in a file outside of the protected volume, resulting in a partition that truly only contains encrypted data. While this may be beneficial, protection of the headers must be carefully considered. If the headers were obtained, a keyslot could be cracked offline. Improper storage of keyfiles could result in worse protection than the default attached headers.

### Key files on a separate volume

Key files must ultimately be stored on an unencrypted volume in order to be accessed to decrypt data. Unless these key files are stored safely, they can reduce security. Ideally, key files should be encrypted. GPG works well because smartcards such as a [YubiKey/GPG](https://wiki.gentoo.org/wiki/YubiKey/GPG) can be used to decrypt the key file.

### Additional boot complexity

Without encryption, the boot process is already very complex. Adding any type of root filesystem encryption takes this complexity to another level, because some mechanism must decrypt the root filesystem so the kernel can start the *init* process. Although most LUKS operations are handled by the *dm-crypt* kernel module, userspace tools are required to perform them.

An early boot environment, such as an initramfs, is typically used to perform drive decryption during the boot process. This initramfs does not require much - depending on the encryption scheme. A very simple initramfs could just contain tools such as mount, cryptsetup, and switch\_root. First, /dev, /sys, and /proc must be mounted, then cryptsetup can be used to open and map the drive, and it can finally be mounted and switch\_root can be used to change to the newly mounted root filesystem and start the init process.

[sys-kernel/dracut](https://packages.gentoo.org/packages/sys-kernel/dracut) is able to handle this with some configuration, while [sys-kernel/ugrd](https://packages.gentoo.org/packages/sys-kernel/ugrd) is built for this specific purpose.

## Installation

### Emerge

`root #``emerge --ask sys-fs/cryptsetup`
### Additional software

If using GPG to further secure key files:

`root #``emerge --ask app-crypt/gnupg`
## System preparation

If this is being followed as part of a fresh Gentoo install, the install procedure can be followed until the following step: [AMD64 Handbook: Designing a partition scheme](https://wiki.gentoo.org/wiki/Handbook:AMD64/Full/Installation#Designing_a_partition_scheme)

If converting an existing system to an encrypted setup, either new storage must be added, or this procedure could be used to create new partitions using free space, where the data can then be copied after creation.

## Disk preparation

This example will use GPT as disk partition schema and [GRUB](https://wiki.gentoo.org/wiki/GRUB) as boot loader. fdisk will be used as the partitioning tool though any partitioning utility will work.

For full disk encryption with a boot USB:

For full disk encryption, using a separate boot drive with a split boot layout:

### Root device formatting

To create a partition layout using fdisk, start by creating a fresh partition table on the root disk, /dev/nvme0n1:

`root #``fdisk /dev/nvme0n1`
Welcome to fdisk (util-linux 2.38.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.
Device does not contain a recognized partition table.
Created a new DOS disklabel with disk identifier 0x81391dbc.
Command (m for help): g
Created a new GPT disklabel (GUID: 8D91A3C1-8661-2940-9076-65B815B36906).

With a partition table crated, a new partition spanning the drive can be created by using **n** and then accepting the defaults:

`Command (m for help):``n````
Partition number (1-128, default 1): 
First sector (2048-1953525134, default 2048): 
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-1953525134, default 1953523711): 
Created a new partition 1 of type 'Linux filesystem' and of size 931.5 GiB.
```
The **Linux Root (x86-64)** property can be set using **t**:

`Command (m for help):``t`
Partition number (1, default 1):
Partition type or alias (type L to list all): 23
Changed type of partition 'Linux filesystem' to 'Linux Root (x86-64)'.

Finally, the changes can be written with **w**:

`Command (m for help):``w`
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.

#### Boot device formatting

The boot disk can be setup using a similar process, the main difference is the boot flags will be set as a final step:

`root #``fdisk /dev/sda`
Welcome to fdisk (util-linux 2.38.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.
Device does not contain a recognized partition table.
Created a new DOS disklabel with disk identifier 0x81391dbc.
Command (m for help): g
Created a new GPT disklabel (GUID: 3E57DFCE-CDD9-6F42-8418-F0B6B4A08294).

##### Create the ESP

With a fresh partition table, a 1GB partition for the ESP can be created using:

`Command (m for help):``n````
Partition number (1-128, default 1): 
First sector (2048-121008094, default 2048): 
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-121008094, default 121006079): +1G
Created a new partition 1 of type 'Linux filesystem' and of size 1 GiB.
```
The **ESP** properties can be set using **t**:

`Command (m for help):``t`
Selected partition 1
Partition type or alias (type L to list all): 1
Changed type of partition 'Linux filesystem' to 'EFI System'.

##### Optional: Create the Extended Boot partition

The *Extended Boot* partition can be created with:

`Command (m for help):``n````
Partition number (2-128, default 2): 
First sector (2099200-121008094, default 2099200): 
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2099200-121008094, default 121008094): +1G
 
Created a new partition 2 of type 'Linux filesystem' and of size 1 GiB.
```
The **Linux Extended Boot** property can be set using **t**:

`Command (m for help):``t`
Partition number (1-2, default 2):
Partition type or alias (type L to list all): 142
Changed type of partition 'Linux filesystem' to 'Linux Extended Boot'.

##### Write changes

Changes can be written with **w**:

`Command (m for help):``w`
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.

## LUKS Preparation

To prepare the encrypted filesystem, cryptsetup can be used. Passwords and keys protect key slots on the LUKS header, which contains the master key that actually encrypts the partition data.

### Detached header

LUKS partitions can be created with a detached header using `--header {header_file}` ex:

`root #``cryptsetup luksFormat --header /media/sda2/luks_header.img /dev/nvme0n1p1`
### Passphrase secured LUKS Header

The most basic way to configure an encrypted volume is to use:

`root #``cryptsetup luksFormat --key-size 512 /dev/nvme0n1p1`
WARNING!
========
This will overwrite data on /dev/nvme0n1p1 irrevocably.
Are you sure? (Type 'yes' in capital letters): 
YES
Enter passphrase for /dev/nvme0n1p1:

#### Adding a Passphrase to Volume Using GPG Secured Key Files

To add a passphrase to a volume which is already secured using key files, a named pipe must be used for the key file contents:

`root #``mkfifo key_pipe`
Then the key can be decrypted to this pipe:

`root #``gpg --decrypt key_file > key_pipe &`
A passphrase can be added with:

`root #``cryptsetup luksAddKey --key-file key_pipe /dev/nvme0n1p1`
The named pipe can be cleaned up with:

`root #``rm key_pipe`
### Key File Secured Header

Instead of securing the LUKS keyslots with a passphrase, keyfiles can be used.

#### Key File Creation

Keyfiles can be created and stored in a variety of ways. It is recommended to encrypt keyfiles, so they cannot just be stolen and used.

##### Basic key file creation

A basic key file can be created using *dd* and /dev/urandom:

`/media/sda2/ #``dd bs=8388608 count=1 if=/dev/urandom of=crypt_key.luks`
1+0 records in
1+0 records out
8388608 bytes (8.4 MB, 8.0 MiB) copied, 0.014407 s, 582 MB/s

##### GPG Symmetrically Encrypted Key File

For increased security, the key file can be immediately encrypted with [GnuPG](https://wiki.gentoo.org/wiki/GnuPG).

`/media/sda2/ #``dd bs=8388608 count=1 if=/dev/urandom | gpg --symmetric --cipher-algo AES256 --output crypt_key.luks.gpg`
1+0 records in
1+0 records out
8388608 bytes (8.4 MB, 8.0 MiB) copied, 7.50139 s, 1.1 MB/s

##### GPG Asymmetrically Encrypted Key File

A key file can be protected using public key cryptography using a smartcard such as a [YubiKey](https://wiki.gentoo.org/wiki/YubiKey). [This YubiKey GPG guide](https://wiki.gentoo.org/wiki/YubiKey/GPG) can be used to generate [GPG](https://wiki.gentoo.org/wiki/GnuPG) keys on a YubiKey. With the public keys loaded, keys can be encrypted with the key holder as the recipient:

`/media/sda2/ #``dd bs=8388608 count=1 if=/dev/urandom | gpg --recipient larry@gentoo.org --output crypt_key.luks.gpg --encrypt`
1+0 records in
1+0 records out
8388608 bytes (8.4 MB, 8.0 MiB) copied, 7.50139 s, 1.1 MB/s

When booting, ensure the smartcard is inserted. Dracut will prompt for the PIN, and depending on the configuration, the device may need to be tapped to complete presence detection.

#### Key File Usage

##### luksFormat Using a Key File

To secure the partition using a plain key file:

`/media/sda2/ #``cryptsetup --key-size 512 luksFormat /dev/nvme0n1p1 crypt_key.luks`
WARNING!
========
This will overwrite data on /dev/nvme0n1p1 irrevocably.
Are you sure? (Type 'yes' in capital letters): YES

To add this keyfile to an already encrypted partition:

`/media/sda2/ #``cryptsetup luksAddKey /dev/nvme0n1p1 crypt_key.luks`
Enter any existing passphrase:

##### luksFormat Using a GPG protected keyfile

To secure the partition using a GPG protected key file:

`/media/sda2/ #``gpg --decrypt crypt_key.luks.gpg | cryptsetup luksFormat --key-size 512 /dev/nvme0n1p1 -`
gpg: AES256.CFB encrypted data
WARNING!
========
This will overwrite data on /dev/nvme0n1p1 irrevocably.
Are you sure? (Type 'yes' in capital letters): YES

To add the GPG encrypted keyfile to an already encrypted partition, named pipes must be used to avoid decrypting the key to disk, as both **gpg** and **cryptsetup** are expecting input from stdin:

`/media/sda2/ #``mkfifo crypt_key``/media/sda2/ #``mkfifo cryptsetup_pass`
Once the files are created, the key must be decrypted to *crypt\_key*, and the recovery passphrase must be passed to *cryptsetup\_pass*:

`/media/sda2/ #``gpg --decrypt crypt_key.luks.gpg > crypt_key &``/media/sda2/ #``read -s -r -p 'LUKS passphrase: ' CRYPT_PASS; echo "$CRYPT_PASS" > cryptsetup_pass &`
Finally, **cat** can be used to pass this information to **cryptsetup**:

`/media/sda2/ #``cat cryptsetup_pass crypt_key | cryptsetup luksAddKey /dev/nvme0n1p1 -`
gpg: AES256.CFB encrypted data
gpg: encrypted with 1 passphrase
\[1\]-  Done                    read -s -r -p 'LUKS passphrase: ' CRYPT\_PASS; echo "$CRYPT\_PASS" > cryptsetup\_pass
\[2\]+  Done                    gpg -d crypt\_key.luks.gpg > crypt\_key

### LUKS Header Backup

The headers can be backed up with:

`root #``cryptsetup luksHeaderBackup /dev/nvme0n1p1 --header-backup-file crypt_headers.img`
## Filesystem Preparation

With the LUKS volume created, it must be mapped so the underlying filesystems can be created.

### Open the LUKS volume

The encrypted device must be opened and mapped before it can be used, this can be done with:

`root #``cryptsetup luksOpen /dev/nvme0n1p1 root`
If using a key file:

`/media/sda2/ #``cryptsetup --key-file crypt_key.luks open /dev/nvme0n1p1 root`
If using a GPG encrypted key file:

`/media/sda2/ #``gpg --decrypt crypt_key.luks.gpg | cryptsetup --key-file - open /dev/nvme0n1p1 root`
### Format the Filesystems

To create a filesystem for /dev/sda1, the *EFI System Partition* which will contain GRUB. This partition is read by UEFI. Most motherboards can read only a [FAT32](https://wiki.gentoo.org/wiki/FAT) filesystem:

`root #``mkfs.vfat -F32 /dev/sda1`
To create an *Extended Boot* filesystem, which the bootloader will read the kernel and initramfs from:

`root #``mkfs.ext4 -L boot /dev/sda2`
To create the [btrfs](https://wiki.gentoo.org/wiki/Btrfs) root filesystem on the LUKS partition:

`root #``mkfs.btrfs -L rootfs /dev/mapper/root`
#### Create optional btrfs subvolumes

To create subvolumes for /etc, /home, and /var, the filesystem must first be mounted:

`root #``mount LABEL=rootfs /mnt/gentoo`
Then each subvolume can be created:

`root #````
btrfs subvolume create /mnt/gentoo/etc
```
`root #````
btrfs subvolume create /mnt/gentoo/home
```
`root #``btrfs subvolume create /mnt/gentoo/var`
## Initramfs configuration

[sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel) supports 2 initramfs generators:

- [Dracut](https://wiki.gentoo.org/wiki/Dracut) - A dynamic initramfs built on udev.
- [UgRD](https://wiki.gentoo.org/wiki/UgRD) - A minimalistic initramfs built specifically for the host.

### UgRD

[UgRD](https://wiki.gentoo.org/wiki/UgRD) or "microgram ramdisk" is designed specifically to decrypt LUKS volumes on Gentoo systems. It uses [app-misc/pax-utils](https://packages.gentoo.org/packages/app-misc/pax-utils) to resolve libraries for binaries needed in the early boot environment and generates a simple init script to handle this decryption.

Unlike Dracut, UGRD attempts to automatically configure mount and LUKS configuration, and validates the configuration before creating a CPIO. UGRD also makes smaller, simpler, and faster images than Dracut. This is possible because UGRD determines requirements and the boot process at build time, while Dracut is dynamic and responds to events at runtime.

UGRD is available in the Gentoo repository and can be installed with:

`root #``emerge --ask sys-kernel/ugrd``user $``ugrd --help````
ugrd --help
usage: ugrd [-h] [-d] [-dd] [-v] [--log-file LOG_FILE] [--log-level LOG_LEVEL] [--log-time] [--no-log-color] [--build-logging] [--no-build-logging] [-c CONFIG]
            [-m MODULES] [--kernel-version KERNEL_VERSION] [--clean] [--no-clean] [--compress] [--no-compress] [--rotate] [--no-rotate] [--validate]
            [--no-validate] [--hostonly] [--no-hostonly] [--lspci] [--no-lspci] [--lsmod] [--no-lsmod] [--firmware] [--no-firmware] [--autodetect-root]
            [--no-autodetect-root] [--autodetect-root-luks] [--no-autodetect-root-luks] [--autodetect-root-lvm] [--no-autodetect-root-lvm] [--autodetect-root-dm]
            [--no-autodetect-root-dm] [--no-kmod] [--print-config] [--print-init] [--test] [--test-kernel TEST_KERNEL] [--livecd-label LIVECD_LABEL]
            [out_file]
MicrogRAM disk initramfs generator
positional arguments:
  out_file              set the output image location
options:
  -h, --help            show this help message and exit
  -d, --debug           enable debug mode (level 10)
  -dd, --trace          enable trace debug mode (level 5)
  -v, --version         print the version and exit
  --log-file LOG_FILE   set the path to the log file
  --log-level LOG_LEVEL
                        set the log level
  --log-time            enable log timestamps
  --no-log-color        disable log color
  --build-logging       enable additional build logging
  --no-build-logging    disable additional build logging
  -c CONFIG, --config CONFIG
                        set the config file location
  -m MODULES, --modules MODULES
                        Define config modules to load, comma separated
  --kernel-version KERNEL_VERSION, --kver KERNEL_VERSION
                        set the kernel version
  --clean               clean the build directory at runtime
  --no-clean            disable build directory cleaning
  --compress            compress the final image
  --no-compress         don't compress the final image
  --rotate              rotate old cpio images
  --no-rotate           don't rotate old cpio images
  --validate            enable configuration validation
  --no-validate         disable config validation
  --hostonly            enable hostonly mode, required for automatic kmod detection
  --no-hostonly         disable hostonly mode
  --lspci               use lspci to auto-detect kmods
  --no-lspci            do not use lspci to auto-detect kmods
  --lsmod               use lsmod to auto-detect kmods
  --no-lsmod            do not use lsmod to auto-detect kmods
  --firmware            include firmware files found with modinfo
  --no-firmware         exclude firmware files
  --autodetect-root     autodetect the root partition
  --no-autodetect-root  do not autodetect the root partition
  --autodetect-root-luks
                        autodetect LUKS volumes under the root partition
  --no-autodetect-root-luks
                        do not autodetect root LUKS volumes
  --autodetect-root-lvm
                        autodetect LVM volumes
  --no-autodetect-root-lvm
                        do not autodetect LVM volumes
  --autodetect-root-dm  autodetect DM (LUKS/LVM) root partitions
  --no-autodetect-root-dm
                        do not autodetect root DM volumes
  --no-kmod             Allow images to be built without kmods/kernel info
  --print-config        print the final config dict
  --print-init          print the final init structure
  --test                Tests the image with qemu
  --test-kernel TEST_KERNEL
                        Tests the image with qemu using a specific kernel file.
  --livecd-label LIVECD_LABEL
                        Sets the label for the livecd
```
#### Passphrase protected volume

If using plain passphrase protection, ugrd should automatically detect it. Look for the following lines when running ugrd:

#### Detached headers

To use UGRD to decrypt a LUKS volume with detached headers, stored at /boot (Boot support):

**`/etc/ugrd/config.toml`**

```
# This configuration should autodetect root/luks info and use detached headers
modules = [
  "ugrd.kmod.usb",
  "ugrd.crypto.cryptsetup"
]
auto_mounts = ['/boot']
#[mounts.boot]
#type = "vfat"
#uuid = "BDF2-0139"
[cryptsetup.root]
header_file = "/boot/luks_header.img"
# partuuid = f0273847-2754-4961-b64e-307c30097396  # should be autodetected
```
#### Symmetrically encrypted GPG keyfile

To use UGRD with a GPG encrypted keyfile at /boot/crypt\_key.luks.gpg:

**`/etc/ugrd/config.toml`**

```
modules = [
  "ugrd.kmod.usb",
  "ugrd.crypto.gpg"
]
auto_mounts = ['/boot']
[cryptsetup.root]
#uuid = "4bb45bd6-9ed9-44b3-b547-b411079f043b"  # should be autodetected
key_type = "gpg"
key_file = "/boot/crypt_key.luks.gpg"
```
#### Yubikey Protected GPG keyfile

To use UGRD with a YubiKey to decrypt /boot/crypt\_key.luks.gpg with the public key at /etc/ugrd/pubkey.gpg:

**`/etc/ugrd/config.toml`**

```
modules = [
  "ugrd.kmod.usb",
  "ugrd.crypto.smartcard"
]
sc_public_key = "/etc/ugrd/pubkey.gpg"
auto_mounts = ['/boot']
[cryptsetup.root]
#uuid = "4bb45bd6-9ed9-44b3-b547-b411079f043b"  # should be autodetected
key_type = "gpg"
key_file = "/boot/crypt_key.luks.gpg"
try_nokey = true
```
#### Updating an initramfs image

If the **ugrd** USE flag is set on [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel), the initramfs can be regenerated and reinstalled with:

`root #``emerge --config gentoo-kernel`
#### Manually creating an initramfs image

UGRD can be used to create an initramfs image based on the current kernel version simply by running it as root:

`root #``ugrd`
INFO     | Intializing class: InitramfsGenerator
INFO     | Intializing class: InitramfsConfigDict
INFO     | Module version: 2.1.0
INFO     | Processing module: ugrd.base.base
INFO     | Processing module: ugrd.base.core
INFO     | Adding library path: /usr/lib64
INFO     | Processing module: ugrd.fs.mounts
INFO     | Processing module: ugrd.base.cmdline
INFO     | Processing module: ugrd.kmod.kmod
INFO     | Processing module: ugrd.fs.cpio
INFO     | Processing module: ugrd.base.checks
INFO     | Loading config file: /etc/ugrd/config.toml
INFO     | Processing module: ugrd.crypto.smartcard
INFO     | Processing module: ugrd.crypto.gpg
INFO     | Processing module: ugrd.base.console
INFO     | Registered custom init function: custom\_init
INFO     | Processing module: ugrd.crypto.cryptsetup
INFO     | Adding library path: /usr/lib/gcc/x86\_64-pc-linux-gnu/13
INFO     | Processing module: ugrd.kmod.nvme
INFO     | Processing module: ugrd.kmod.usb
INFO     | Processing module: ugrd.kmod.standard\_mask
INFO     | Processing module: ugrd.kmod.nosound
INFO     | Processing module: ugrd.kmod.novideo
INFO     | Processing module: ugrd.kmod.nonetwork
INFO     | \[ugrd.crypto.cryptsetup:root\] No retries specified, using default: 5
INFO     | Building initramfs
INFO     | Detected init at: /usr/bin/init
WARNING  | Cleaning build directory: /tmp/initramfs\_build
INFO     | Source path for libgcc\_s: /usr/lib/gcc/x86\_64-pc-linux-gnu/13/libgcc\_s.so.1
INFO     | Found device mapper devices: dm-0
INFO     | Auto-enabling kernel modules for device: dm\_mod
INFO     | Autodetected root type: btrfs
INFO     | Autodetected root source: uuid=8537843e-e479-4379-8fc2-a76139d59505
INFO     | \[mounts\] Updating mount: root
INFO     | Auto-enabling module: btrfs
INFO     | Processing module: ugrd.fs.btrfs
INFO     | Detected a device mapper mount: /dev/mapper/root
INFO     | \[root\] LUKS volume uuid: 20447cd7-5d64-477a-9c95-fdbd30fb61af
INFO     | \[root\] Configuring cryptsetup for LUKS mount (root) on: dm-0
root:
  key\_type: gpg
  key\_file: /boot/root.luks.gpg
  try\_nokey: True
  key\_command: gpg --decrypt /boot/root.luks.gpg
  reset\_command: gpgconf --reload && gpg --card-status
  retries: 5
  uuid: 20447cd7-5d64-477a-9c95-fdbd30fb61af
INFO     | Auto-enabling kernel modules for device: nvme
INFO     | Using detected kernel version: 6.6.47-custom
INFO     | Adding GPG public key file to dependencies: /etc/ugrd/pubkey.gpg
INFO     | \[deploy\_nodes\] Skipping real device node creation with mknod, as mknod\_cpio is not specified.
INFO     | Regenerating kernel module metadata files.
INFO     | Running init generator functions
INFO     | Init kernel modules: dm\_crypt, uhid, nvme, uas, usbcore, scsi\_mod, ehci\_hcd, ohci\_hcd, uhci\_hcd, xhci\_hcd, vfat, dm\_mod, btrfs
INFO     | Included kernel modules: nvme\_core, usb\_storage, ehci\_pci, fat, nvme\_common, crc32c
WARNING  | 'cryptsetup\_prompt' is disabled, if the 'quiet' kernel parameter is not set, the prompt may be hidden under log messages at runtime.
INFO     | Wrote file: /tmp/initramfs\_build/etc/profile
INFO     | Included functions: check\_var, setvar, readvar, prompt\_user, retry, edebug, einfo, ewarn, eerror, rd\_fail, rd\_restart, \_find\_init, mount\_root, parse\_cmdline\_bool, parse\_cmdline\_str, get\_crypt\_dev, mount\_base, export\_exports, parse\_cmdline, load\_modules, mount\_fstab, crypt\_init, mount\_cmdline\_root, do\_switch\_root
INFO     | Wrote file: /tmp/initramfs\_build/init\_main.sh
INFO     | Wrote file: /tmp/initramfs\_build/init
INFO     | Wrote file: /tmp/initramfs\_build/etc/fstab
WARNING  | Deleting old file: /tmp/initramfs\_out/ugrd-6.6.47-custom.old.1
INFO     | \[1\] Cycling file: /tmp/initramfs\_out/ugrd-6.6.47-custom.old -> /tmp/initramfs\_out/ugrd-6.6.47-custom.old.1
INFO     | \[0\] Cycling file: /tmp/initramfs\_out/ugrd-6.6.47-custom.cpio -> /tmp/initramfs\_out/ugrd-6.6.47-custom.old
INFO     | Wrote 63.27 MiB to: /tmp/initramfs\_out/ugrd-6.6.47-custom.cpio
INFO     | Completed checks.

To create an image for a specific kernel version, and output it to /boot/:

`root #``ugrd --kver 6.6.47-gentoo /boot/initramfs-6.6.47-gentoo.img`
#### Recovery

If a step fails in UGRD, it will generally try to re-exec the init from the top, and will keep attempting this until it is successful, or the device is rebooted.

UGRD attempts to halt and wait for user input before continuing after a failure.

### Dracut

#### Dracut module config

The following modules must be added to the *add\_dracutmodules* directive in /etc/dracut.conf:

**`/etc/dracut.conf`**

**Minimum required components to decrypt LUKS volumes using dracut**

```
add_dracutmodules+=" crypt dm rootfs-block "
```
##### GPG config

If GPG keys are being used, the following module must also be added: **crypt-gpg**

**`/etc/dracut.conf`**

**Minimum required components to decrypt LUKS volumes using dracut**

```
add_dracutmodules+=" crypt crypt-gpg dm rootfs-block "
```
#### Dracut cmdline config

Dracut can be configured to build with configuration for LUKS hardcoded, first disk information must be obtained:

`root #``lsblk -o name,uuid`
NAME        UUID
sda
├──sda1     BDF2-0139
└──sda2     0e86bef-30f8-4e3b-ae35-3fa2c6ae705b
nvme0n1
└─nvme0n1p1 4bb45bd6-9ed9-44b3-b547-b411079f043b
  └─root    cb070f9e-da0e-4bc5-825c-b01bb2707704

##### Passphrase encryption

To open a LUKS volume protected with passphrase encryption:

**`/etc/dracut.conf`**

```
kernel_cmdline+=" root=UUID=cb070f9e-da0e-4bc5-825c-b01bb2707704 rd.luks.uuid=4bb45bd6-9ed9-44b3-b547-b411079f043b "
```
##### GPG Keys

To open a gpg keyfile protected LUKS volume:

**`/etc/dracut.conf`**

**Embed cmdline parameters for rootfs decryption**

```
kernel_cmdline+=" root=LABEL=crypt rd.luks.uuid=4bb45bd6-9ed9-44b3-b547-b411079f043b rd.luks.key=/crypt_key.luks.gpg:UUID=0e86bef-30f8-4e3b-ae35-3fa2c6ae705b "
```
#### Systemd

For systemd systems, rebuild with the **cryptsetup** USE-flag:

**`/etc/portage/package.use/systemd`**

```
sys-apps/systemd cryptsetup
```
`root #``emerge --ask --newuse sys-apps/systemd`
#### Manually generating an image

Once Dracut is configured, a new initramfs can be generated by running:

`root #``dracut`
If the initramfs is being generated for a kernel other than the currently active one, **--kver** must be used:

`root #````
dracut --kver 6.1.28-gentoo
```
This can happen in a situation when the kernel version in the Gentoo Live CD differs from the emerged sys-kernel/gentoo-sources in the kernel compilation process.

Dracut has now generated an initramfs, but configuration is not complete. Command line parameters must be set, either by manually adding them to the kernel, or by configuring them into the bootloader.

#### Extracting the initramfs

It's possible to use **dracut** to generate an *initramfs* image, then extract this to be built into the kernel.

`/usr/src/initramfs #``/usr/lib/dracut/skipcpio  /boot/initramfs-6.1.28-gentoo-initramfs.img | zcat | cpio -ivd`
### Embedding the initramfs

An initramfs image can be embedded into the Linux kernel, or a path can be supplied to be packed and embedded.

#### Embedding a directory

With the *initramfs* unpacked in /usr/src/initramfs, the kernel can be configured to embed it:

**Embed the initramfs into the kernel**

```
General Setup --->
[*] Initial RAM filesystem and RAM disk (initramfs/initrd) support
    (/usr/src/initramfs) Initramfs source file(s)
[*]   Support initial ramdisk/ramfs compressed using gzip
```
.config equivalent:

With this configuration, the kernel will automatically embed whatever exists under /usr/src/initramfs into the kernel when it is built, and attempt to use it on boot. This is especially useful when [Secure Booting](https://wiki.gentoo.org/wiki/Secure_Boot).

#### Embedding an image

If the path to a CPIO image is provided instead of a directory, it will be packed into the kernel.

## Gentoo installation

If this procedure is being followed during a Gentoo install (in place of [Handbook:AMD64/Full/Installation#Designing\_a\_partition\_scheme](https://wiki.gentoo.org/wiki/Handbook:AMD64/Full/Installation#Designing_a_partition_scheme) through [Handbook:AMD64/Full/Installation#Mounting\_the\_root\_partition](https://wiki.gentoo.org/wiki/Handbook:AMD64/Full/Installation#Mounting_the_root_partition)), the following steps can be used to mount the created partition, to continue with the install.

### Mount the root partition

The logical volume for the root file system can be mounted at this created location with:

`root #``mount LABEL=rootfs /mnt/gentoo`
### fstab configuration

For consistent volume mounting, labels and UUIDs must be used.

Block devices and their associated partition IDs can be viewed with:

`root #``lsblk -o name,uuid`
NAME        UUID
sda
├──sda1     BDF2-0139
└──sda2     0e86bef-30f8-4e3b-ae35-3fa2c6ae705b
nvme0n1
└─nvme0n1p1 4bb45bd6-9ed9-44b3-b547-b411079f043b
  └─root    cb070f9e-da0e-4bc5-825c-b01bb2707704

With the partition UUIDs and labels identified, [/etc/fstab](https://wiki.gentoo.org/wiki/Fstab) can be edited to add relevant mounts:

**`/mnt/gentoo/etc/fstab`**

```
# <fs>                                          <mountpoint>    <type>          <opts>          <dump/pass>
UUID=BDF2-0139                                  /efi            vfat            noauto,noatime  0 1
LABEL=boot                                      /boot           ext4            noauto,noatime  0 1
LABEL=rootfs                                    /               btrfs           defaults        0 0
```
### Finalizing the Gentoo install

At this point, the Gentoo install can be continued normally: [Installing a stage tarball](https://wiki.gentoo.org/wiki/Handbook:AMD64/Full/Installation#Installing_a_stage_tarball)

## Additional information

### SSD tricks

[SSD § Partitioning](https://wiki.gentoo.org/wiki/SSD#Partitioning)

SSD [trim](<https://en.wikipedia.org/wiki/Trim_(computing)>) allows an operating system to inform a solid-state drive (SSD) which blocks of data are no longer considered in use and can be wiped internally. Because low-level operation of SSDs differs significantly from hard drives, the typical way in which operating systems handle operations like deletes and formats resulted in unanticipated progressive performance degradation of write operations on SSDs. Trimming enables the SSD to more efficiently handle garbage collection, which would otherwise slow future write operations to the involved blocks.

See [SSD § LUKS](https://wiki.gentoo.org/wiki/SSD#LUKS) for enabling discard passthrough.

See [SSD § LVM](https://wiki.gentoo.org/wiki/SSD#LVM) when using LVM.

## See also

- [Rootfs encryption](https://wiki.gentoo.org/wiki/Rootfs_encryption) — Encrypting the root [filesystem](https://wiki.gentoo.org/wiki/Filesystem) can enhance privacy, and prevent unauthorized access.
- [Dm-crypt](https://wiki.gentoo.org/wiki/Dm-crypt) — a disk encryption system using the kernels crypto API framework and device mapper subsystem.
- [Encrypted bootable media with SecureBoot/GRUB/LUKS](https://wiki.gentoo.org/wiki/Encrypted_bootable_media_with_SecureBoot/GRUB/LUKS) — Encrypting bootable media can enhance privacy, and prevent unauthorized access.
