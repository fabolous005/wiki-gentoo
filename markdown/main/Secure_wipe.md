<!-- source: https://wiki.gentoo.org/wiki/Secure_wipe | group: Gentoo Wiki (Main) | wiki-title: Secure wipe -->
---
title: Secure wipe
url: https://wiki.gentoo.org/wiki/Secure_wipe
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-03-16"
fingerprint: ae4c5d584973a1bd
license: CC BY-SA 4.0
---

# Secure wipe

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Once data has been stored on a disk, it may not be straightforward to remove that data, being sure that it cannot be read again. Securely wiping memory can differ depending on the memory type, as well as how it is interfaced with. The main obstacle in securely erasing a drive is [Data Remanence](https://en.wikipedia.org/wiki/Data_remanence), or residual data remaining on the drive.

## Introduction

Erasing portions of storage is often unnecessary, this is because it is simpler faster to just unlink data instead of wiping it. Wiping data requires the region of memory to be totally overwritten, and this can be a very slow process.

### Secure against whom or what

Like all combat areas, being it defending your system from the internet or copy protection, data erasure is another. Start with the premise that nothing is perfect.

The methods discussed here are probably adequate to defend against an attacker armed with the same tools you have. To defend against a well equipped, well funded determined attacker, media destruction is the only sure way.

### Crypto shredding

Planning in advance, and never writing plaintext data on a drive can help in preventing data recovery, especially recovery using inaccessible data regions. If only encrypted data has been written to a disk, through the use of [Full Disk Encryption](https://wiki.gentoo.org/wiki/Full_Disk_Encryption), or other methods, deleting the key is typically enough to prevent data recovery.

An analogy to this approach is locking something into a container and throwing away the key.

### Wiping woes

Or, why wiping storage is not always simple.

#### Advanced data recovery

Even if an entire drive is overwritten with 0s, that does not guarantee that data cannot be recovered, especially if transparent compression is being used. Doing multiple overwrite passes with random data is typically required to be reasonably certain that data cannot be recovered. Wiping using random data is typically slower, and also requires good entropy sources for any amount of speed.

#### Inaccessible memory regions

Modern hard drives often come with more available storage than the controller allows usage of. This allows the drive to reallocate data when it detects that memory is degrading. This over-provision is especially relevant to SSDs, which can physically only write each section of memory a limited amount of times before it fails. Depending on the drive, software methods used to overwrite all memory may have no way to touch this now deallocated memory.

#### Write once read many (WORM) memory

Data written to any form of WORM memory, such as CD-Rs cannot be overwritten or erased, so the storage must be destroyed to prevent data recovery.

## Installation

### USE flags



### Emerge

`root #``emerge --ask sys-apps/hdparm``root #``emerge --ask sys-apps/nvme-cli`
## Methods of destruction

### Crypto shredding

If data recovery is ever a concern for a storage device, encryption should be the first line of defense. In the event the device is obtained before it could be wiped, the data should already be useless, unless the keys were stored with the device, making decryption easy.

If a device is encrypted using LUKS, wiping the headers is enough to render the data useless, so long as backups do not exist.

If /dev/sda1 is LUKS encrypted, all keys can be removed from the header with:

`root #``cryptsetup erase /dev/sda1`
With no keys installed, there is no way the partition can be decrypted. To totally remove the LUKS header:

`root #``wipefs --all /dev/sda`
### Erasing

#### Generic methods

**shred** is part of *GNU coreutils* and should be preinstalled on most Gentoo based systems.

**shred** can be used to erase /dev/sda using one pass of pseudorandom data sourced from /dev/urandom:

`root #``shred --verbose --random-source /dev/urandom --iterations 1 /dev/sda`
#### SATA

**hdparm** can be used to execute an [ATA Secure Erase command](https://tinyapps.org/docs/wipe_drives_hdparm.html). A Secure Erase command should handle wiping inaccessible regions.

In order to execute a SATA Secure Erase, a password must be set for the device, this password will be removed during the erase process:

`root #``hdparm --security-set-pass NULL /dev/sda`
security\_password: ""
/dev/sda:
 Issuing SECURITY\_SET\_PASS command, password="", user=user, mode=high

With the password set, the drive can be erased using:

`root #``hdparm --security-erase NULL /dev/sda`
security\_password: ""
/dev/sda:
 Issuing SECURITY\_ERASE command, password="", user=user

#### NVMe

A firmware secure erase can be executed on a [NVMe](https://wiki.gentoo.org/wiki/NVMe) drive using the [sys-apps/nvme-cli](https://packages.gentoo.org/packages/sys-apps/nvme-cli)-provided utility:

`root #``nvme format --ses 1 /dev/nvmeXnY`
