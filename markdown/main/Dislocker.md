<!-- source: https://wiki.gentoo.org/wiki/Dislocker | group: Gentoo Wiki (Main) | wiki-title: Dislocker -->
---
title: Dislocker
url: https://wiki.gentoo.org/wiki/Dislocker
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-07-13"
fingerprint: "14a56898bfe7fa74"
license: CC BY-SA 4.0
---

# Dislocker

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Dislocker** is [FUSE](https://wiki.gentoo.org/wiki/FUSE)-based [filesystem](https://wiki.gentoo.org/wiki/Filesystem) driver capable of reading [NTFS](https://wiki.gentoo.org/wiki/NTFS) BitLocker encrypted partitions.

## Installation

### Emerge

`root #``emerge --ask sys-fs/dislocker`
## Configuration

### Files

**A note on fstab**

BitLocker partitions can be mount-ed using the /etc/fstab file and dislocker's long options. The line below is an example line, which has to be adapted to each case:

FILE **`/etc/fstab`**

```
/dev/sda2 /mnt/dislocker fuse.dislocker user-password=blah,nofail 0 0
```
## Example

With the sample /etc/fstab as above, make sure the two mount points /mnt/dislocker and /mnt/clear exist. Then:

`root #````
mount /dev/sda2
```
`root #````
mount -o loop /mnt/dislocker/dislocker-file /mnt/clear
```
The first mount creates the file /mnt/dislocker/dislocker-file, which is the raw unencrypted NTFS from inside the BitLocker partition. The second command mounts that NTFS partition onto /mnt/clear.

## See also

- [NTFS](https://wiki.gentoo.org/wiki/NTFS) — a proprietary disk [filesystem](https://wiki.gentoo.org/wiki/Filesystem) by Microsoft for Windows (NT-based) and WindowsNT-based operating systems.
- [FAT](https://wiki.gentoo.org/wiki/FAT) — [filesystem](https://wiki.gentoo.org/wiki/Filesystem) originally created for use with MS-DOS (and later pre-NT Microsoft Windows).
