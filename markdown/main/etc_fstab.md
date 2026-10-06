<!-- source: https://wiki.gentoo.org/wiki//etc/fstab | group: Gentoo Wiki (Main) | wiki-title: /etc/fstab -->
---
title: "/etc/fstab"
url: https://wiki.gentoo.org/wiki//etc/fstab
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-03-03"
fingerprint: be0b409bafc619f9
license: CC BY-SA 4.0
---

# /etc/fstab

[/etc](https://wiki.gentoo.org/wiki/Special:MyLanguage//etc)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Resources**

The **fstab** (**f**ile **s**ystem **tab**le) file (/etc/fstab) is a configuration file that defines how and where the main [filesystems](https://wiki.gentoo.org/wiki/Filesystem) are to be mounted, especially at boot time.

## Syntax

Each line of /etc/fstab contains the necessary settings to mount one partition, drive or network share. The line has six columns, separated by whitespaces or tabs. The columns are as follows:

1. The [device file](https://wiki.gentoo.org/wiki/Device_file), [UUID or label](https://wiki.gentoo.org/wiki//etc/fstab#UUIDs_and_labels) or other means of locating the partition or data source.
2. The mount point, where the data is to be attached to the filesystem.
3. The filesystem type. See man 5 fstab for more supported file system types.
4. Options, including if the filesystem should be mounted at boot.
5. Adjusts the archiving schedule for the partition (used by [app-arch/dump](https://packages.gentoo.org/packages/app-arch/dump) package). `0` disables, `1` enables the feature.
6. Controls the order in which fsck checks the device/partition for errors at boot time. The root device should be `1`. Other partitions should be either `2` (to check after root) or `0` (to disable checking for that partition altogether).

An example for the root device:

**`/etc/fstab`**

```
/dev/sda1   /   ext4   defaults   0   1
```
Special characters can be escaped by using their octal representation from an ASCII table. For example, if the name of the mount point contains spaces or tabs these can be escaped as \040 and \011 respectively.

For more detailed information see man 5 fstab.

## UUIDs and labels

In the first column, a [UUID](https://en.wikipedia.org/wiki/Universally_unique_identifier) can be used instead of a device file:

**`/etc/fstab`**

**Using a UUID for the root partition**

```
UUID=339df6e7-91a8-4cf9-a43f-7f7b3db533c6   /   ext4   defaults   0   1
```
Alternatively, a LABEL can be used:

**`/etc/fstab`**

**Using a label for the root partition**

```
LABEL=Gentoo   /   ext4   defaults   0   1
```
Depending on the partition table (e.g. the GUID Partition Table "GPT"), PARTLABEL can be used:

**`/etc/fstab`**

**Using a label for the root partition**

```
PARTLABEL=Gentoo   /   ext4   defaults   0   1
```
Please read [this](https://wiki.gentoo.org/wiki/Removable_media#UUIDs_and_labels) for details on how to retrieve UUIDs and labels.

## Services

The following [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) services read the fstab to mount or manage the filesystems:

- **localmount** - Mount disks and swap according to fstab.
- **netmount** - Mount network shares according to fstab.
- **fsck** - Check and repair filesystems according to fstab.
- **root** - Mount the root filesystem read/write.

These services supplement the fstab, if the filesystems are not explicitly stated:

- **sysfs** - Mount the /sys filesystem.
- **devfs** - Mount system critical filesystems in /dev.

Check that they are enabled to start at boot time:

`root #``rc-update show`
## See also

- [AutoFS](https://wiki.gentoo.org/wiki/AutoFS) — a program that uses the Linux [kernel](https://wiki.gentoo.org/wiki/Kernel) automounter to automatically [mount](https://wiki.gentoo.org/wiki/Mount) [filesystems](https://wiki.gentoo.org/wiki/Filesystem) on demand.
- [Disk Quotas (Security Handbook)](https://wiki.gentoo.org/wiki/Security_Handbook/User_and_group_limitations#Quotas)
- [fstab (AMD64 Handbook)](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/System#About_fstab)
- [Mounting partitions (Security Handbook)](https://wiki.gentoo.org/wiki/Security_Handbook/Mounting_partitions)
- [mount](https://wiki.gentoo.org/wiki/Mount) — the attaching of an additional [filesystem](https://wiki.gentoo.org/wiki/Filesystem) to the currently accessible filesystem of a computer.
- [removable media](https://wiki.gentoo.org/wiki/Removable_media) — any media that is easily removed from a system.
- [SSD](https://wiki.gentoo.org/wiki/SSD) — provides guidelines for basic maintenance, such as enabling discard/trim support, for **SSD**s ([Solid State Drives](https://en.wikipedia.org/wiki/Solid-state_drive)) on Linux.
