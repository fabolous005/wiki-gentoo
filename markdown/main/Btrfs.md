<!-- source: https://wiki.gentoo.org/wiki/Btrfs | group: Gentoo Wiki (Main) | wiki-title: Btrfs -->
---
title: Btrfs
url: https://wiki.gentoo.org/wiki/Btrfs
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-23"
fingerprint: "1f833b1ba0f15ba1"
license: CC BY-SA 4.0
---

# Btrfs

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Btrfs** is a copy-on-write (CoW) [filesystem](https://wiki.gentoo.org/wiki/Filesystem) for Linux aimed at implementing advanced features while focusing on fault tolerance, self-healing properties, and easy administration. Jointly developed at Oracle, Red Hat, Fujitsu, Intel, SUSE, STRATO, and many others, Btrfs is licensed under the [GPL](https://en.wikipedia.org/wiki/GNU_General_Public_License) and open for contribution from anyone.

## Features

Ext4 is safe and stable and can handle large filesystems with extents, but why switch?  Btrfs has established features such as subvolumes, snapshots, checksumming, and compression; maturity varies by feature, and RAID5/6 remains unstable.<sup>[\[1\]](https://wiki.gentoo.org#cite_note-upstream-status-1)</sup> Some Linux distributions have already begun to switch to it with their current releases. Btrfs has a number of advanced features in common with ZFS, which is what made the ZFS filesystem popular with BSD distributions and NAS devices.

- **Copy on Write (CoW) and snapshotting** - Make incremental backups painless even from a "hot" filesystem or virtual machine (VM).
- **Data and metadata checksums** - Btrfs checksums data and metadata blocks by default. Checksums detect corruption; repair requires a valid redundant copy or recoverable redundant data.<sup>[\[2\]](https://wiki.gentoo.org#cite_note-upstream-checksums-2)</sup><sup>[\[3\]](https://wiki.gentoo.org#cite_note-upstream-autorepair-3)</sup>
- **Compression** - Files may be compressed and decompressed on the fly, which speeds up read performance.
- **Auto defragmentation** - The optional autodefrag mount option queues small random writes for defragmentation while the filesystem is in use; it is disabled by default.<sup>[\[4\]](https://wiki.gentoo.org#cite_note-upstream-mountopts-4)</sup>
- **Subvolumes** - Filesystems can share a single pool of space instead of being put into their own partitions.
- **RAID** - Btrfs does its own RAID implementations so [LVM](https://wiki.gentoo.org/wiki/LVM) or mdadm are not required to have RAID. RAID0, RAID1, RAID1C3, RAID1C4, and RAID10 are supported; RAID5 and RAID6 are considered unstable.<sup>[\[5\]](https://wiki.gentoo.org#cite_note-upstream-profiles-5)</sup>
- **Partitions are optional** - While Btrfs can work with partitions, it has the potential to use raw devices (/dev/\<device>) directly.
- **Data deduplication** - Btrfs supports out-of-band [deduplication](https://wiki.gentoo.org/wiki/Deduplication) through external tools. The filesystem checks candidate ranges byte for byte before allowing them to share storage.<sup>[\[6\]](https://wiki.gentoo.org#cite_note-upstream-dedupe-6)</sup>
- **Quotas** - Btrfs offers quota support, which allows for grouping of subvolumes in quotas.

Down the road, new clustered filesystems will readily take advantage of Btrfs with its copy on write and other advanced features for their object stores. [Ceph](https://wiki.gentoo.org/wiki/Ceph) is one example of a clustered filesystem that looks very promising, and can take advantage of Btrfs.

## Caveats

Btrfs gradually allocates storage in block groups (also called chunks) that are then filled with extents. Typical sizes are 1 GiB for data and 256 MiB or 1 GiB for metadata, depending on filesystem size. Partially filled block groups remain allocated; deleting a file does not necessarily free a whole block group.<sup>[\[5\]](https://wiki.gentoo.org#cite_note-upstream-profiles-5)</sup> This approach can lead to a scenario where

1. The underlying storage backend is fully allocated with chunks
2. Non-full data chunks exist
3. A new metadata chunk is needed for a file operation but cannot be allocated, resulting in ENOSPC status code.

In practice: df reports free space but file operations can stall/fail. See [#Maintenance](https://wiki.gentoo.org#Maintenance) on how to avoid reaching this situation.

Additionally, a single 4K reference to a 128M extent inside Btrfs can cause free space to be present, but unavailable for allocations. This can also cause Btrfs to return ENOSPC when free space is reported by df.

On top of the potential issue described above, information on the issues present in Btrfs in the latest kernel branches is available in [btrfs status page](https://btrfs.readthedocs.io/en/latest/Status.html).

## Installation

### Kernel

In order for the Linux kernel to support Btrfs, the filesystem has to be enabled:

#### Versions \<6.14.0

**Enable Btrfs and CRC32c hardware acceleration**

File systems  --->
   \<\*> Btrfs filesystem support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_BTRFS\_FS\</code> to find this item.
-\*- Cryptographic API --->
   Accelerated Cryptographic Algorithms for CPU (x86) --->
       \<\*> CRC32c (SSE4.2/PCLMULQDQ) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CRYPTO\_CRC32C\_INTEL\</code> to find this item.

#### Versions >=6.14.0

**Enable Btrfs and CRC32c hardware acceleration**

File systems  --->
   \<\*> Btrfs filesystem support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_BTRFS\_FS\</code> to find this item.
Library routines --->
   \[\*\] Enable optimized CRC implementations [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CRC\_OPTIMIZATIONS\</code> to find this item.

#### Snippet

**`/etc/kernel/config.d/btrfs-linux6-1-111.config`**

```
CONFIG_BTRFS_FS=y
CONFIG_XOR_BLOCKS=y
CONFIG_RAID6_PQ=y
CONFIG_RAID6_PQ_BENCHMARK=y
CONFIG_ZSTD_COMPRESS=y
```
### Emerge

To work with the [sys-fs/btrfs-progs](https://packages.gentoo.org/packages/sys-fs/btrfs-progs) utilities package issue:

`root #``emerge --ask sys-fs/btrfs-progs`
## Usage

Typing long Btrfs commands can quickly become a hassle. Each command (besides the initial btrfs command) can be reduced to a very short set of instructions. This method is helpful when working from the command line to reduce the amount of characters typed.

For example, to defragment certain internal trees of the filesystem mounted at /, the following shows the long command. Without -r, this directory argument does not recursively defragment file contents; see [#Defragmentation](https://wiki.gentoo.org#Defragmentation) for that operation:[\[8\]](https://wiki.gentoo.org#cite_note-upstream-defrag-8)

`root #``btrfs filesystem defragment -v /` Shorten each of the longer commands after the btrfs command by reducing them to their unique, shortest prefix. In this context, unique means that no *other* btrfs commands will match the command at the command's shortest length. The shortened version of the above command is:

`root #``btrfs fi de -v /` No other btrfs commands start with `fi`; `filesystem` is the only one. The same goes for the `de` sub-command under the `filesystem` command.

### Creation

To create a Btrfs filesystem on the /dev/sdXN partition:

`root #``mkfs.btrfs /dev/sdXN`
In the example above, replace `N` with the partition number and `X` with the disk letter that is to be formatted. For example, to format the third partition of the first drive in the system with Btrfs, run:

`root #``mkfs.btrfs /dev/sda3`
### Labels

Labels can be added to Btrfs filesystems, making mounting and organization easier.

Labels can be added to a Btrfs filesystem after it has been created by using:

`root #``btrfs filesystem label /dev/sda1 rootfs`
Labels can be added when the Btrfs filesystem is created with:

`root #``mkfs.btrfs -L rootfs /dev/sda1`
### Mount

After creation, filesystems can be mounted in several ways:

- [mount](https://wiki.gentoo.org/wiki/Mount) - Manual mount.
- [/etc/fstab](https://wiki.gentoo.org/wiki//etc/fstab) - Defining mount points in /etc/fstab enables automatic mounts on system boot.
- [Removable media](https://wiki.gentoo.org/wiki/Removable_media) - Automatic mounts on demand (useful for USB drives).
- [AutoFS](https://wiki.gentoo.org/wiki/AutoFS) - Automatic mount on filesystem access.

### Maintenance

To check the current space usage of a mounted Btrfs filesystem:

`root #``btrfs filesystem usage <mountpoint>````
Overall:
    Device size:                   1.35TiB
    Device allocated:              1.12TiB
    Device unallocated:          232.31GiB
    Device missing:                  0.00B
    Device slack:                    0.00B
    Used:                          1.01TiB
    Free (estimated):            338.76GiB      (min: 222.60GiB)
    Free (statfs, df):           338.75GiB
    Data ratio:                       1.00
    Metadata ratio:                   2.00
    Global reserve:              512.00MiB      (used: 0.00B)
    Multiple profiles:                  no
```
To reduce the risk described in [#Caveats](https://wiki.gentoo.org#Caveats), monitor unallocated device space as well as data and metadata usage. Keeping more than approximately 1 GiB unallocated is only a rough guideline, not a guarantee: workspace requirements depend on the profiles and available space on individual devices. One way to reclaim space on a live system is to relocate data and metadata block groups that are less than 50% full:[\[10\]](https://wiki.gentoo.org#cite_note-upstream-balance-10)

`root #``btrfs balance start -dusage=50 -musage=50 <mountpoint>`
Done, had to relocate 90 out of 486 chunks

The -d flag selects **d**ata and -m selects **m**etadata. If the above fails because relocation workspace is unavailable, first try reclaiming completely empty block groups. This does not require relocation workspace:[\[10\]](https://wiki.gentoo.org#cite_note-upstream-balance-10)

`root #``btrfs balance start -dusage=0 -musage=0 <mountpoint>`
If more space is needed, consider deleting unneeded large files before retrying. Extents still referenced by snapshots or reflinked files remain allocated.

The above (and more) can be automated with [sys-fs/btrfsmaintenance](https://packages.gentoo.org/packages/sys-fs/btrfsmaintenance).

### Converting ext\* based file systems

It is possible to convert ext2, ext3, and ext4 filesystems to Btrfs using the btrfs-convert utility.

The following instructions only support the conversion of filesystems that are unmounted. To convert the root partition, boot to a system rescue disk (SystemRescueCD works nicely) and run the conversion commands on the root partition.

First, be sure the filesystem is unmounted:

`root #``umount` *<mounted_device>*
Check the integrity of the filesystem using the appropriate fsck tool. In the next example, the filesystem is ext4:

`root #``fsck.ext4 -f` *<unmounted_device>*
Use btrfs-convert to convert the ext\* formatted device into a Btrfs-formatted device:

`root #``btrfs-convert` *<unmounted_device>*
Be sure to edit /etc/fstab after the device has been formatted to change the filesystem column from ext4 to Btrfs:

**`/etc/fstab`**

**Changing ext4 to btrfs**

```
<device>   <mountpoint>  btrfs  defaults  0 0
```
### Defragmentation

Another feature of Btrfs is online defragmentation. To recursively defragment files under /, run the following command. It does not descend into nested subvolumes, other mount points, or directory symlinks; those require separate consideration.[\[8\]](https://wiki.gentoo.org#cite_note-upstream-defrag-8)

`root #``btrfs filesystem defragment -r -v /`
The `autodefrag` mount option sets the default behavior to online defragmentation.

### Compression

Btrfs supports transparent compression using zlib, lzo, and zstd. Zstd support was introduced in Linux 4.14; configurable positive zstd levels were added in Linux 5.1.[\[12\]](https://wiki.gentoo.org#cite_note-12)[\[13\]](https://wiki.gentoo.org#cite_note-upstream-compression-13)

It is possible to compress specific files using the file attributes:

`user $``chattr +c <`*filename*> [<*filename*> ...]
or

`user $``btrfs prop set <`*filename*> compression <*type*>
where `type` is one of zlib, lzo, and zstd. When applied to a directory, all files and subdirectories created *afterwards* will be compressed.

The `compress` mount option enables compression for newly written data; existing extents are not rewritten merely by changing this option. Compression may be skipped for incompressible data. To request lzo compression while recursively defragmenting files under /, run the following command. It does not cross nested subvolume or mount-point boundaries:[\[13\]](https://wiki.gentoo.org#cite_note-upstream-compression-13)[\[8\]](https://wiki.gentoo.org#cite_note-upstream-defrag-8)

`root #``btrfs filesystem defragment -r -v -clzo /`
Depending on the CPU and disk performance, using lzo compression could improve the overall throughput.

As alternatives to lzo it is possible to use the zlib or zstd compression algorithms. Zlib is slower but has a higher compression ratio, whereas zstd has a good ratio between the two<sup>[\[14\]](https://wiki.gentoo.org#cite_note-14)</sup>.

To request zlib compression for the same recursive traversal:

`root #``btrfs filesystem defragment -r -v -czlib /`
Substitute zstd for zlib in the example above to activate zstd compression.

#### Compression level

zlib supports levels 1-9. zstd supports levels 1-15 and, since Linux 6.15, faster levels -15 through -1; these negative levels are available on Linux 6.18. Level 0 selects the default. For example, to set zlib to maximum compression at mount time:[\[13\]](https://wiki.gentoo.org#cite_note-upstream-compression-13)

`root #``mount -o compress=zlib:9 /dev/sdXY /path/to/btrfs/mountpoint`
Or to set minimal compression:

`root #``mount -o compress=zlib:1 /dev/sdXY /path/to/btrfs/mountpoint`
Or adjust compression by remounting:

`root #``mount -o remount,compress=zlib:3 /path/to/btrfs/mountpoint`
The compression level should be visible in /proc/mounts, or by checking the most recent dmesg output using the following command:

`root #``dmesg | grep -i btrfs` \[    0.495284\] Btrfs loaded, crc32c=crc32c-intel
\[ 3010.727383\] BTRFS: device label My Passport devid 1 transid 31 /dev/sdd1
\[ 3111.930960\] BTRFS info (device sdd1): disk space caching is enabled
\[ 3111.930973\] BTRFS info (device sdd1): has skinny extents
\[ 9428.918325\] BTRFS info (device sdd1): use zlib compression, level 3

#### Adjust fstab for compression

Once a drive has been remounted or adjusted to compress data, be sure to add the appropriate modifications to the /etc/fstab file. In this example, the device is set to noatime and forced zstd level 1 for higher data throughput<sup>[\[16\]](https://wiki.gentoo.org#cite_note-16)</sup> at mount time:

**`/etc/fstab`**

**Add Btrfs compression for zstd**

```
/dev/sdb                /srv            btrfs           defaults,noatime,compress-force=zstd:1,rw     0 0
```
#### Compression ratio and disk usage

The usual userspace tools for determining used and free space like du and df may provide inaccurate results on a *Btrfs* partition due to inherent design differences in the way files are written compared to, for example, *ext2/3/4*<sup>[\[17\]](https://wiki.gentoo.org#cite_note-17)</sup>.

It is therefore advised to use the du/df alternatives provided by the Btrfs userspace tool `btrfs filesystem`. In addition, the compsize tool found in the [sys-fs/compsize](https://packages.gentoo.org/packages/sys-fs/compsize) package can be helpful in providing additional information regarding compression ratios and the disk usage of compressed files. The following are example uses of these tools for a Btrfs partition mounted under /media/drive.

`user $``btrfs filesystem du -s /media/drive` Total   Exclusive  Set shared  Filename
 848.12GiB   848.12GiB       0.00B  /media/drive/

`user $``btrfs filesystem df /media/drive` Data, single: total=846.00GiB, used=845.61GiB
System, DUP: total=8.00MiB, used=112.00KiB
Metadata, DUP: total=2.00GiB, used=904.30MiB
GlobalReserve, single: total=512.00MiB, used=0.00B

`user $``compsize /media/drive` Processed 2262 files, 112115 regular extents (112115 refs), 174 inline.
Type       Perc     Disk Usage   Uncompressed Referenced  
TOTAL       99%      845G         848G         848G       
none       100%      844G         844G         844G       
zlib        16%      532M         3.2G         3.2G

### Multiple devices (RAID)

Btrfs can be used with multiple block devices in order to create RAIDs. Using Btrfs to create filesystems that span multiple devices is much easier than creating using mdadm, since there is no initialization time needed for creation.

Btrfs handles data and metadata separately. This is important to keep in mind when using a multi-device filesystem. It is possible to use separate profiles for data and metadata block groups. For example, metadata could be configured across multiple devices in RAID1, while data could be configured to RAID5. This combination is possible with three or more block devices. Btrfs also permits two-device RAID5, but upstream discourages that arrangement because it provides RAID1-like redundancy with additional parity overhead.[\[5\]](https://wiki.gentoo.org#cite_note-upstream-profiles-5)

With RAID1 metadata, each metadata block has two copies on different devices, regardless of the total device count. RAID5 stripes data and distributed parity across devices. RAID1 metadata consumes space for two copies, and parity updates add work to RAID5 writes. Actual throughput depends on the workload.[\[5\]](https://wiki.gentoo.org#cite_note-upstream-profiles-5)

#### Creation

The simplest method is to use the entirety of unpartitioned block devices to create a filesystem spanning multiple devices. For example, to create a filesystem in RAID1 mode across two devices:

`root #``mkfs.btrfs -m raid1 -d raid1` *<device1> <device2>*
#### Conversion

Converting between RAID profiles is possible with the balance sub-command. For example, say three block devices are presently configured for RAID1 and mounted at /srv. It is possible to convert the data in this profile from RAID1 to RAID5 with the following command:

`root #``btrfs balance start -dconvert=raid5 --force /srv`
Conversion can be performed while the filesystem is online and in use. Possible RAID profiles in Btrfs include RAID0, RAID1, RAID1C3, RAID1C4, RAID5, RAID6, and RAID10.<sup>[\[5\]](https://wiki.gentoo.org#cite_note-upstream-profiles-5)</sup> See the [upstream Btrfs wiki](https://btrfs.wiki.kernel.org/index.php/Using_Btrfs_with_Multiple_Devices) for more information.

#### Addition

Additional devices can be added to a mounted Btrfs filesystem to increase capacity. In the example below, /srv is the mounted filesystem and /dev/sdd is the new device. Check the device identity and ensure that no contents on it need to be kept. Do not remove an existing member just to add capacity.[\[22\]](https://wiki.gentoo.org#cite_note-upstream-device-22)

`root #``btrfs device add /dev/sdd /srv`
To redistribute existing data and metadata onto the newly added device, run a balance. Device addition itself makes the device available for new allocations and does not require a full balance:

`root #``btrfs balance start /srv`
#### Replacement

To replace an existing member, use btrfs replace on the mounted filesystem. The target must be at least as large as the source and its contents will be overwritten. Keep a readable source connected during replacement when possible. The source is removed from the filesystem after replacement completes.[\[23\]](https://wiki.gentoo.org#cite_note-upstream-replace-23)

For example, after verifying that /dev/sdb is the source and /dev/sdd is an unused replacement device (not a device already added to the filesystem):

`root #``btrfs replace start -B /dev/sdb /dev/sdd /srv`
Progress can be checked from another terminal:

`root #``btrfs replace status /srv`
If the source has already failed or been disconnected, identify its device ID with btrfs filesystem show and use that ID in place of the source path. Mounting with -o degraded may be necessary, and replacement is possible only if all required data can still be read or reconstructed from the remaining devices. Follow [#Multi\_device\_filesystem\_mount\_fails](https://wiki.gentoo.org#Multi_device_filesystem_mount_fails) for the degraded-mount prerequisites.[\[23\]](https://wiki.gentoo.org#cite_note-upstream-replace-23)[\[4\]](https://wiki.gentoo.org#cite_note-upstream-mountopts-4)

#### Removal

##### By device path

Block devices (disks) can be removed from mounted multi-device filesystems using the btrfs device remove subcommand. Remaining devices must have enough space and satisfy the active profile constraints; wait for the operation to finish before disconnecting the removed device:[\[22\]](https://wiki.gentoo.org#cite_note-upstream-device-22)

`root #``btrfs device remove /dev/sde /srv`
##### By device ID

Use the usage subcommand to determine the device IDs:

`root #``btrfs device usage /srv`
/dev/sdb, ID: 3
   Device size:             1.82TiB
   Device slack:              0.00B
   Data,RAID1:             25.00GiB
   Data,RAID5:            497.00GiB
   Data,RAID5:              5.00GiB
   Metadata,RAID5:         17.00GiB
   Metadata,RAID5:        352.00MiB
   System,RAID5:           32.00MiB
   Unallocated:             1.29TiB
 
/dev/sdc, ID: 1
   Device size:             1.82TiB
   Device slack:              0.00B
   Data,RAID1:             25.00GiB
   Data,RAID5:            497.00GiB
   Data,RAID5:              5.00GiB
   Metadata,RAID5:         17.00GiB
   Metadata,RAID5:        352.00MiB
   System,RAID5:           32.00MiB
   Unallocated:             1.29TiB
 
/dev/sdd, ID: 4
   Device size:             1.82TiB
   Device slack:              0.00B
   Data,RAID1:             25.00GiB
   Data,RAID5:            497.00GiB
   Data,RAID5:              5.00GiB
   Metadata,RAID5:         17.00GiB
   Metadata,RAID5:        352.00MiB
   System,RAID5:           32.00MiB
   Unallocated:             1.29TiB
 
/dev/sde, ID: 5
   Device size:               0.00B
   Device slack:              0.00B
   Data,RAID1:             75.00GiB
   Data,RAID5:              5.00GiB
   Metadata,RAID5:        352.00MiB
   Unallocated:             1.74TiB

Next use the device ID to remove the device. In this case /dev/sde will be removed:

`root #``btrfs device remove 5 /srv`
### Resizing

Btrfs partitions can be resized while online using the built-in resize subcommand.

Set the size of the root filesystem to 128gb:

`root #``btrfs filesystem resize 128g /`
Add 50 gigabytes of space to the rootfs:

`root #``btrfs filesystem resize +50g /`
The command can also fill all available space:

`root #``btrfs filesystem resize max /`
### Subvolumes

A **subvolume** of Btrfs is a directory with special properties. Most notably a subvolume can be mounted, in particular without mounting the entire Btrfs volume.

The top directory of a Btrfs itself is always a subvolume. Thus a subvolume can contain other subvolumes. A subvolume has to be created by the `btrfs subvolume` command, as explained below. Another feature is that one can set the quota for a subvolume.

A subvolume shares the filesystem's storage pool and Btrfs-specific mount options with other subvolumes. For two subvolumes X and Y:

- X can be mounted read-write and Y read-only, provided the filesystem itself is writable. A read-only mount is distinct from the subvolume's persistent ro property.
- X and Y cannot have different compress mount options, but files and directories can have different compression properties.
- X and Y cannot have different nodatacow mount options, but the NOCOW file attribute can be set for individual files or inherited from a directory.<sup>[\[24\]](https://wiki.gentoo.org#cite_note-upstream-subvolumes-24)</sup><sup>[\[4\]](https://wiki.gentoo.org#cite_note-upstream-mountopts-4)</sup>

Subvolume space accounting is complicated by shared extents. Quotas provide referenced and exclusive accounting through qgroups. Without quotas, btrfs filesystem du can still report total, exclusive, and shared file extents for a directory tree, but this is not the same as complete per-subvolume accounting.[\[25\]](https://wiki.gentoo.org#cite_note-upstream-usage-25)

Thus a Btrfs subvolume is completely different from an [LVM](https://wiki.gentoo.org/wiki/LVM) volume. A subvolume cannot be created *across* different Btrfs filesystems. The snapshot can be *moved* from one filesystem to another, but it cannot span across the two.

#### Create

To create a subvolume, issue the following command inside a Btrfs filesystem's name space:

`root #``btrfs subvolume create` *<dest-name>*
Replace *\<dest-name>*

`root #``btrfs subvolume create /mnt/btrfs/subvolume1`
#### List

To see the subvolume(s) that have been created, use the `subvolume list` command followed by a Btrfs filesystem location. If the current directory is somewhere inside a Btrfs filesystem, the following command will display the subvolume(s) that exist on the filesystem:

`root #``btrfs subvolume list .`
If a Btrfs filesystem with subvolumes exists at the mount point created in the example command above, the output from the list command will look similar to the following:

`root #``btrfs subvolume list /mnt/btrfs`
ID 309 gen 102913 top level 5 path subvolume1

#### Remove

All available subvolume paths in a Btrfs filesystem can be seen using the list command above.

Subvolumes can be properly removed by using the `subvolume delete` command, followed by the path to the subvolume:

`root #``btrfs subvolume delete` *<subvolume-path>*
As above, replace *\<subvolume-path>*

`root #``btrfs subvolume delete /mnt/btrfs/subvolume1`
Delete subvolume (no-commit): '/mnt/btrfs/subvolume1'

#### Snapshots

Snapshots are subvolumes that share data and metadata with other subvolumes. This is made possible by Btrfs' Copy on Write (CoW) ability.<sup>[\[26\]](https://wiki.gentoo.org#cite_note-26)</sup> Snapshots preserve a subvolume at a point in time and can be used for rollback or as a source for backups.

If / is a Btrfs subvolume and the parent directory /mnt/backup is on the same Btrfs filesystem, create a snapshot using the `subvolume snapshot` commands below. The snapshot path /mnt/backup/rootfs must not already exist; create only its parent directory:

`root #````
mkdir -p /mnt/backup
```
`root #````
btrfs subvolume snapshot / /mnt/backup/rootfs
```
The following small shell script can be added to a timed cron job to create timestamped local snapshots of a Btrfs root subvolume. As above, /mnt/backup must be on the same Btrfs filesystem, and nested subvolumes are not included. The timestamps can be adjusted to whatever is preferred by the user.

**`btrfs_snapshot.sh`**

**Btrfs rootfs snapshot cron job example**

```
#!/bin/bash
NOW=$(date +"%Y-%m-%d_%H:%M:%S")
 
if [ ! -e /mnt/backup ]; then
mkdir -p /mnt/backup
fi
 
cd /
/sbin/btrfs subvolume snapshot / "/mnt/backup/backup_${NOW}"
```
Alternatively, it is possible to use a [portage hook](https://wiki.gentoo.org/wiki/Handbook:AMD64/Portage/Advanced#Hooking_into_the_emerge_process) to create a new snapshot every time a package is installed. This could be useful for users that run testing keywords or experimental packages. See [Snapper](https://wiki.gentoo.org/wiki/Snapper) for one example on enabling this setup.

#### Mounting

A subvolume can be mounted in a location different from where it was created, or users can choose to not mount them at all. For example, a user could create a Btrfs filesystem in /mnt/btrfs and create /mnt/btrfs/home and /mnt/btrfs/gentoo-repo subvolumes. The subvolumes could then be mounted at /home and /var/db/repos/gentoo, with the original top level subvolume left unmounted. This results in a configuration where the subvolumes' relative path from the top level subvolume is different from their actual path.

To mount a subvolume, perform the following command, where *\<rel-path>*`subvolume list` command:

`root #``mount -o subvol=`*<rel-path> <device> <mountpoint>*
Similarly, the filesystem tab can be updated to mount a Btrfs subvolume:

**`/etc/fstab`**

**Mounting Subvolumes**

```
<device>  <mountpoint>  btrfs  subvol=<rel-path>  0 0
```
### Scrub

Scrub detects damage to the filesystem against stored checksums.<sup>[\[27\]](https://wiki.gentoo.org#cite_note-27)</sup> A directory path selects the entire mounted filesystem, not just that directory subtree. To start scrubbing all devices of the filesystem mounted at /:[\[28\]](https://wiki.gentoo.org#cite_note-upstream-scrub-28)

`root #``btrfs scrub start /`
Scrub repairs damaged copies when a verified good redundant copy is available. It cannot verify file data that has no data checksums, such as NOCOW file data.[\[28\]](https://wiki.gentoo.org#cite_note-upstream-scrub-28)

To see the scrub progress:

`root #``watch -cn 10 btrfs scrub status /`
## Troubleshooting

### Filesystem check

With a failing disk or corrupted data, it may be necessary to run a filesystem check. Typically filesystem check commands are handled through the fsck. prefix, but for Btrfs filesystems, structural checks are handled via the btrfs check subcommand. Unmount the filesystem first. This command is read-only by default:[\[29\]](https://wiki.gentoo.org#cite_note-upstream-check-29)

`root #``btrfs check --progress /dev/<device>`
### Multi device filesystem mount fails

After ungracefully removing one or more devices from a multi device filesystem, attempting to mount the filesystem will fail:

`root #``mount /srv`
mount: /srv: wrong fs type, bad option, bad superblock on /dev/sdb, missing codepage or helper program, or other error.

This type of mount failure could be caused by missing one or more devices from the multi device filesystem. Missing devices can be detected by using the filesystem show subcommand. In the following example /dev/sdb is one of the devices still connected to the multi device filesystem:

`root #``btrfs filesystem show /dev/sdb````
Label: none  uuid: 9e7e9824-d66b-4a9c-a05c-c4245accabe99
        Total devices 5 FS bytes used 2.50TiB
        devid    1 size 1.82TiB used 817.03GiB path /dev/sdc
        devid    3 size 1.82TiB used 817.00GiB path /dev/sdb
        devid    5 size 10.91TiB used 2.53TiB path /dev/sde
        devid    6 size 10.91TiB used 2.53TiB path /dev/sdd
        *** Some devices missing
```
To remove a missing device, first mount the filesystem read-write with -o degraded, using a surviving device. This succeeds only if the remaining block groups permit a degraded mount. For the example above:[\[4\]](https://wiki.gentoo.org#cite_note-upstream-mountopts-4)

`root #``mount -o degraded /dev/sdb /srv`
Then remove the missing member only if all required data can still be read or reconstructed, sufficient space is available, and the remaining devices satisfy the profiles. The command below does not forcibly discard unavailable data:[\[22\]](https://wiki.gentoo.org#cite_note-upstream-device-22)

`root #``btrfs device delete missing /srv`
### Using with VM disk images

For virtual machine disk images with frequent overwrites, disabling data copy-on-write can improve I/O performance. The NOCOW attribute can only be set or cleared on empty files. Setting it on a directory makes newly created files inherit it; existing populated images are not converted. For example, using the chattr command:[\[30\]](https://wiki.gentoo.org#cite_note-upstream-attributes-30)

`root #``chattr +C /var/lib/libvirt/images`
### Clear the free space cache

It is possible to clear Btrfs' free space cache with the `clear_cache` mount option on the first read-write mount. With the default v2 cache, it clears the entire cache; with legacy v1, it clears only the caches of block groups modified during that mount. For an unmounted filesystem, for example:[\[4\]](https://wiki.gentoo.org#cite_note-upstream-mountopts-4)

`root #``mount -o clear_cache /path/to/device /path/to/mountpoint`
### Btrfs hogging memory (disk cache)

When utilizing some of Btrfs' special abilities (like making many `--reflink` copies or creating high amounts of snapshots), a lot of memory can be consumed and not freed fast enough by the kernel's inode cache. This issue can go undiscovered since memory dedicated to the disk cache might not be clearly visible in traditional system monitoring utilities. The slabtop utility (available as part of the [sys-process/procps](https://packages.gentoo.org/packages/sys-process/procps) package) was specifically created to determine how much memory kernel objects are consuming:

`root #``slabtop`
Active / Total Objects (% used)    : 5011373 / 5052626 (99.2%)
Active / Total Slabs (% used)      : 1158843 / 1158843 (100.0%)
Active / Total Caches (% used)     : 103 / 220 (46.8%)
Active / Total Size (% used)       : 3874182.66K / 3881148.34K (99.8%)
Minimum / Average / Maximum Object : 0.02K / 0.77K / 4096.00K
 
OBJS ACTIVE  USE OBJ SIZE  SLABS OBJ/SLAB CACHE SIZE NAME
2974761 2974485  99%    1.10K 991587        3   3966348K btrfs\_inode
1501479 1496052  99%    0.19K  71499       21    285996K dentry

If the inode cache is consuming too much memory, the kernel can be manually instructed to drop the cache by echoing an integer value to the /proc/sys/vm/drop\_caches file<sup>[\[31\]](https://wiki.gentoo.org#cite_note-31)</sup>.

Running sync before the echo commands below writes dirty data and can make more cached objects eligible for reclamation. It is not required to make drop\_caches non-destructive, because dirty objects are not discarded:[\[32\]](https://wiki.gentoo.org#cite_note-kernel-drop-caches-32)

`user $``sync`
For testing or troubleshooting, echo 2 requests reclamation of eligible slab objects, including dentries and inodes:

`root #``echo 2 > /proc/sys/vm/drop_caches`
To request reclamation of both eligible slab objects and clean page-cache data, use echo 3 instead:

`root #``echo 3 > /proc/sys/vm/drop_caches`
More information on kernel slabs can be found in this [dedoimedo blog entry](https://www.dedoimedo.com/computers/slabinfo.html).

### Mounting Btrfs fails, returning mount: unknown filesystem type 'btrfs'

The [original solution by Tim on Stack Exchange](http://unix.stackexchange.com/questions/121611/gentoo-does-not-seem-to-be-booting-new-kernel) inspired the following solution: build the kernel manually instead of using [genkernel](https://wiki.gentoo.org/wiki/Genkernel):

`#````
cd /usr/src/linux
```
`#````
make menuconfig
```
`#````
make && make modules_install
```
`#````
cp arch/x86_64/boot/bzImage /boot
```
`#````
mv /boot/bzImage /boot/whatever_kernel_filename
```
`#````
genkernel --install initramfs
```
### Btrfs root doesn't boot

Genkernel's initramfs as created with the command below doesn't load Btrfs:

`root #``genkernel --btrfs initramfs` Compile support for Btrfs in the kernel rather than as a module, or use [Dracut](https://wiki.gentoo.org/wiki/Dracut) to generate the initramfs.

### lsblk doesn't show mountpoint for all devices in btrfs RAID

This is unfortunately a known bug at least since 2014.

What you may expect:

`root #``lsblk````
NAME                           MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINTS
sda                              8:0    0   3,6T  0 disk  
└─sda1                           8:1    0   3,6T  0 part  
  ├─raid0vg0-raid0lv0_rmeta_0  253:0    0     4M  0 lvm   
  │ └─raid0vg0-raid0lv0        253:4    0   3,6T  0 lvm   
  │   └─home                   253:5    0   3,6T  0 crypt /home
  └─raid0vg0-raid0lv0_rimage_0 253:1    0   3,6T  0 lvm   
    └─raid0vg0-raid0lv0        253:4    0   3,6T  0 lvm   
      └─home                   253:5    0   3,6T  0 crypt /home
sdb                              8:16   0   3,6T  0 disk  
└─sdb1                           8:17   0   3,6T  0 part  
  ├─raid0vg0-raid0lv0_rmeta_1  253:2    0     4M  0 lvm   
  │ └─raid0vg0-raid0lv0        253:4    0   3,6T  0 lvm   
  │   └─home                   253:5    0   3,6T  0 crypt /home
  └─raid0vg0-raid0lv0_rimage_1 253:3    0   3,6T  0 lvm   
    └─raid0vg0-raid0lv0        253:4    0   3,6T  0 lvm   
      └─home                   253:5    0   3,6T  0 crypt /home
```
What you get:

`root #``lsblk`
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
sda           8:0    0   3,6T  0 disk /home
sdb           8:16   0   3,6T  0 disk

It's because */proc/#/mountinfo* contains reference to the one device only.[\[33\]](https://wiki.gentoo.org#cite_note-GitHub_docs:_update_TODO_.C2.B7_util-linux.2Futil-linux.402b1322f-33)[\[34\]](https://wiki.gentoo.org#cite_note-Redhat_Bugzilla-34)

## See also

- [Btrfs/snapshots](https://wiki.gentoo.org/wiki/Btrfs/snapshots) — script to **make automatic snapshots with [Btrfs]** filesystem, using  btrfs subvolume list-new function to create snapshots only when files have changed, so as to create fewer snapshots.
- [Btrfs/System Root Guide](https://wiki.gentoo.org/wiki/Btrfs/System_Root_Guide) — one example for re-basing a Gentoo installation's root filesystem to use btrfs
- [Btrfs/Native System Root Guide](https://wiki.gentoo.org/wiki/Btrfs/Native_System_Root_Guide)
- [Ext4](https://wiki.gentoo.org/wiki/Ext4) — an open source disk [filesystem](https://wiki.gentoo.org/wiki/Filesystem) and the most recent version of the extended series of filesystems.
- [Btrbk](https://wiki.gentoo.org/wiki/Btrbk) — a tool for creating incremental snapshots and remote backups of [Btrfs] subvolumes.
- [Samba shadow copies](https://wiki.gentoo.org/wiki/Samba_shadow_copies) — expose Shadow Copies as 'Previous Versions' to Windows clients.
- [Snapper](https://wiki.gentoo.org/wiki/Snapper) — a command-line program to create and manage filesystem snapshots, allowing viewing or reversion of changes.
- [ZFS](https://wiki.gentoo.org/wiki/ZFS) — a next generation [filesystem](https://wiki.gentoo.org/wiki/Filesystem) created by Matthew Ahrens and Jeff Bonwick.

## External resources

- [https://wiki.debian.org/Btrfs](https://wiki.debian.org/Btrfs) - As described by the Debian wiki.
- [https://wiki.archlinux.org/index.php/Btrfs](https://wiki.archlinux.org/index.php/Btrfs) Btrfs article - As described by the Arch Linux wiki.
- [http://www.funtoo.org/BTRFS\_Fun](http://www.funtoo.org/BTRFS_Fun) - BTRFS Fun on the Funtoo wiki.
- [http://marc.merlins.org/perso/btrfs/post\_2014-05-04\_Fixing-Btrfs-Filesystem-Full-Problems.html](http://marc.merlins.org/perso/btrfs/post_2014-05-04_Fixing-Btrfs-Filesystem-Full-Problems.html) - Tips and tricks on fixing niche Btrfs filesystem problems in some situations.

## References

1. ↑ <sup>[1.0](https://wiki.gentoo.org#cite_ref-upstream-status_1-0)</sup> <sup>[1.1](https://wiki.gentoo.org#cite_ref-upstream-status_1-1)</sup> [Btrfs feature status](https://btrfs.readthedocs.io/en/latest/Status.html); [btrfs-progs v7.1 source](https://github.com/kdave/btrfs-progs/blob/v7.1/Documentation/Status.rst).
2. ↑ <sup>[2.0](https://wiki.gentoo.org#cite_ref-upstream-checksums_2-0)</sup> <sup>[2.1](https://wiki.gentoo.org#cite_ref-upstream-checksums_2-1)</sup> <sup>[2.2](https://wiki.gentoo.org#cite_ref-upstream-checksums_2-2)</sup> [Btrfs checksumming](https://btrfs.readthedocs.io/en/latest/Checksumming.html); [btrfs-progs v7.1 source](https://github.com/kdave/btrfs-progs/blob/v7.1/Documentation/ch-checksumming.rst).
3. [↑](https://wiki.gentoo.org#cite_ref-upstream-autorepair_3-0) [Btrfs automatic repair](https://btrfs.readthedocs.io/en/latest/Auto-repair.html); [btrfs-progs v7.1 source](https://github.com/kdave/btrfs-progs/blob/v7.1/Documentation/Auto-repair.rst).
4. ↑ <sup>[4.0](https://wiki.gentoo.org#cite_ref-upstream-mountopts_4-0)</sup> <sup>[4.1](https://wiki.gentoo.org#cite_ref-upstream-mountopts_4-1)</sup> <sup>[4.2](https://wiki.gentoo.org#cite_ref-upstream-mountopts_4-2)</sup> <sup>[4.3](https://wiki.gentoo.org#cite_ref-upstream-mountopts_4-3)</sup> <sup>[4.4](https://wiki.gentoo.org#cite_ref-upstream-mountopts_4-4)</sup> <sup>[4.5](https://wiki.gentoo.org#cite_ref-upstream-mountopts_4-5)</sup> [Btrfs mount options](https://btrfs.readthedocs.io/en/latest/btrfs-man5.html#btrfs-specific-mount-options); [btrfs-progs v7.1 source](https://github.com/kdave/btrfs-progs/blob/v7.1/Documentation/ch-mount-options.rst).
5. ↑ <sup>[5.0](https://wiki.gentoo.org#cite_ref-upstream-profiles_5-0)</sup> <sup>[5.1](https://wiki.gentoo.org#cite_ref-upstream-profiles_5-1)</sup> <sup>[5.2](https://wiki.gentoo.org#cite_ref-upstream-profiles_5-2)</sup> <sup>[5.3](https://wiki.gentoo.org#cite_ref-upstream-profiles_5-3)</sup> <sup>[5.4](https://wiki.gentoo.org#cite_ref-upstream-profiles_5-4)</sup> <sup>[5.5](https://wiki.gentoo.org#cite_ref-upstream-profiles_5-5)</sup> [Btrfs allocation profiles](https://btrfs.readthedocs.io/en/latest/mkfs.btrfs.html#profiles); [btrfs-progs v7.1 source](https://github.com/kdave/btrfs-progs/blob/v7.1/Documentation/mkfs.btrfs.rst).
6. [↑](https://wiki.gentoo.org#cite_ref-upstream-dedupe_6-0) [Btrfs deduplication](https://btrfs.readthedocs.io/en/latest/Deduplication.html); [btrfs-progs v7.1 source](https://github.com/kdave/btrfs-progs/blob/v7.1/Documentation/Deduplication.rst).
7. [↑](https://wiki.gentoo.org#cite_ref-arstechnica_examining-btrfs_2021_7-0) [Examining btrfs, Linux’s perpetually half-finished filesystem](https://arstechnica.com/gadgets/2021/09/examining-btrfs-linuxs-perpetually-half-finished-filesystem/)
8. ↑ <sup>[8.0](https://wiki.gentoo.org#cite_ref-upstream-defrag_8-0)</sup> <sup>[8.1](https://wiki.gentoo.org#cite_ref-upstream-defrag_8-1)</sup> <sup>[8.2](https://wiki.gentoo.org#cite_ref-upstream-defrag_8-2)</sup> <sup>[8.3](https://wiki.gentoo.org#cite_ref-upstream-defrag_8-3)</sup> [Btrfs defragmentation](https://btrfs.readthedocs.io/en/latest/btrfs-filesystem.html#man-filesystem-cmd-defragment); [btrfs-progs v7.1 source](https://github.com/kdave/btrfs-progs/blob/v7.1/Documentation/btrfs-filesystem.rst).
9. [↑](https://wiki.gentoo.org#cite_ref-upstream-fsck_9-0) [Btrfs boot-time filesystem checks](https://btrfs.readthedocs.io/en/latest/fsck.btrfs.html); [btrfs-progs v7.1 source](https://github.com/kdave/btrfs-progs/blob/v7.1/Documentation/fsck.btrfs.rst).
10. ↑ <sup>[10.0](https://wiki.gentoo.org#cite_ref-upstream-balance_10-0)</sup> <sup>[10.1](https://wiki.gentoo.org#cite_ref-upstream-balance_10-1)</sup> [Balance workspace and ENOSPC](https://btrfs.readthedocs.io/en/latest/btrfs-balance.html#enospc); [btrfs-progs v7.1 source](https://github.com/kdave/btrfs-progs/blob/v7.1/Documentation/btrfs-balance.rst).
11. [↑](https://wiki.gentoo.org#cite_ref-11) [man page for btrfs-filesystem(8)](https://btrfs.readthedocs.io/en/latest/btrfs-filesystem.html), [Btrfs wiki](https://btrfs.readthedocs.io). Retrieved on 6th February, 2017.
12. [↑](https://wiki.gentoo.org#cite_ref-12) [https://btrfs.readthedocs.io/en/latest/Compression.html#compression-levels](https://btrfs.readthedocs.io/en/latest/Compression.html#compression-levels)
13. ↑ <sup>[13.0](https://wiki.gentoo.org#cite_ref-upstream-compression_13-0)</sup> <sup>[13.1](https://wiki.gentoo.org#cite_ref-upstream-compression_13-1)</sup> <sup>[13.2](https://wiki.gentoo.org#cite_ref-upstream-compression_13-2)</sup> <sup>[13.3](https://wiki.gentoo.org#cite_ref-upstream-compression_13-3)</sup> <sup>[13.4](https://wiki.gentoo.org#cite_ref-upstream-compression_13-4)</sup> [Btrfs compression](https://btrfs.readthedocs.io/en/latest/Compression.html); [btrfs-progs v7.1 source](https://github.com/kdave/btrfs-progs/blob/v7.1/Documentation/ch-compression.rst).
14. [↑](https://wiki.gentoo.org#cite_ref-14) [https://git.kernel.org/pub/scm/linux/kernel/git/mason/linux-btrfs.git/commit/?h=next&id=5c1aab1dd5445ed8bdcdbb575abc1b0d7ee5b2e7](https://git.kernel.org/pub/scm/linux/kernel/git/mason/linux-btrfs.git/commit/?h=next&id=5c1aab1dd5445ed8bdcdbb575abc1b0d7ee5b2e7)
15. [↑](https://wiki.gentoo.org#cite_ref-upstream-property_15-0) [Btrfs properties](https://btrfs.readthedocs.io/en/latest/btrfs-property.html); [btrfs-progs v7.1 source](https://github.com/kdave/btrfs-progs/blob/v7.1/Documentation/btrfs-property.rst).
16. [↑](https://wiki.gentoo.org#cite_ref-16) braindevices. [btrfs benchmark for daily used desktop os](https://gist.github.com/braindevices/fde49c6a8f6b9aaf563fb977562aafec), Retrieved on September 27, 2024.
17. [↑](https://wiki.gentoo.org#cite_ref-17) [https://btrfs.wiki.kernel.org/index.php/Compression#How\_can\_I\_determine\_compressed\_size\_of\_a\_file.3F](https://btrfs.wiki.kernel.org/index.php/Compression#How_can_I_determine_compressed_size_of_a_file.3F)
18. [↑](https://wiki.gentoo.org#cite_ref-18) [Article mentioning that parity RAID code has multiple serious data-loss bugs](https://btrfs.readthedocs.io/en/latest/btrfs-man5.html#raid56-status-and-recommended-practices), [Btrfs wiki](https://btrfs.readthedocs.io). Retrieved on January 1st, 2017.
19. [↑](https://wiki.gentoo.org#cite_ref-19) Michael Larabel, [Btrfs RAID56 "Mostly OK"](http://www.phoronix.com/scan.php?page=news_item&px=Linux-4.12-Btrfs-RAID-Mostly-OK), Phoronix. July 8, 2017.
20. [↑](https://wiki.gentoo.org#cite_ref-20) [btrfs: scrub: Fix RAID56 recovery race condition](https://git.kernel.org/pub/scm/linux/kernel/git/mason/linux-btrfs.git/commit/?h=for-linus-4.12&id=28d70e237dac905cd8d1896af10216b7d2bced24), source commit, April 18th 2017.
21. [↑](https://wiki.gentoo.org#cite_ref-21) [GIT PULL Btrfs from Chris Mason](http://lkml.iu.edu/hypermail/linux/kernel/1705.1/01197.html), [Linux kernel mailing list](http://lkml.iu.edu/hypermail/linux/kernel/index.html), May 9th 2017.
22. ↑ <sup>[22.0](https://wiki.gentoo.org#cite_ref-upstream-device_22-0)</sup> <sup>[22.1](https://wiki.gentoo.org#cite_ref-upstream-device_22-1)</sup> <sup>[22.2](https://wiki.gentoo.org#cite_ref-upstream-device_22-2)</sup> [Btrfs device management](https://btrfs.readthedocs.io/en/latest/btrfs-device.html); [btrfs-progs v7.1 source](https://github.com/kdave/btrfs-progs/blob/v7.1/Documentation/btrfs-device.rst).
23. ↑ <sup>[23.0](https://wiki.gentoo.org#cite_ref-upstream-replace_23-0)</sup> <sup>[23.1](https://wiki.gentoo.org#cite_ref-upstream-replace_23-1)</sup> [Btrfs device replacement](https://btrfs.readthedocs.io/en/latest/btrfs-replace.html); [btrfs-progs v7.1 source](https://github.com/kdave/btrfs-progs/blob/v7.1/Documentation/btrfs-replace.rst).
24. ↑ <sup>[24.0](https://wiki.gentoo.org#cite_ref-upstream-subvolumes_24-0)</sup> <sup>[24.1](https://wiki.gentoo.org#cite_ref-upstream-subvolumes_24-1)</sup> [Btrfs subvolumes and snapshots](https://btrfs.readthedocs.io/en/latest/btrfs-subvolume.html); [btrfs-progs v7.1 source](https://github.com/kdave/btrfs-progs/blob/v7.1/Documentation/ch-subvolume-intro.rst).
25. [↑](https://wiki.gentoo.org#cite_ref-upstream-usage_25-0) [Btrfs filesystem space reporting](https://btrfs.readthedocs.io/en/latest/btrfs-filesystem.html); [btrfs-progs v7.1 source](https://github.com/kdave/btrfs-progs/blob/v7.1/Documentation/btrfs-filesystem.rst).
26. [↑](https://wiki.gentoo.org#cite_ref-26) [Page explaining the differences between subvolumes and logical volumes in LVM](https://btrfs.readthedocs.io/en/latest/Subvolumes.html), [Btrfs wiki](https://btrfs.readthedocs.io). Retrieved on April 2nd, 2023.
27. [↑](https://wiki.gentoo.org#cite_ref-27) [https://btrfs.readthedocs.io/en/latest/Scrub.html](https://btrfs.readthedocs.io/en/latest/Scrub.html). Retrieved on October 5, 2025.
28. ↑ <sup>[28.0](https://wiki.gentoo.org#cite_ref-upstream-scrub_28-0)</sup> <sup>[28.1](https://wiki.gentoo.org#cite_ref-upstream-scrub_28-1)</sup> [Btrfs scrub](https://btrfs.readthedocs.io/en/latest/btrfs-scrub.html); [btrfs-progs v7.1 source](https://github.com/kdave/btrfs-progs/blob/v7.1/Documentation/btrfs-scrub.rst).
29. ↑ <sup>[29.0](https://wiki.gentoo.org#cite_ref-upstream-check_29-0)</sup> <sup>[29.1](https://wiki.gentoo.org#cite_ref-upstream-check_29-1)</sup> [Btrfs filesystem check](https://btrfs.readthedocs.io/en/latest/btrfs-check.html); [btrfs-progs v7.1 source](https://github.com/kdave/btrfs-progs/blob/v7.1/Documentation/btrfs-check.rst).
30. [↑](https://wiki.gentoo.org#cite_ref-upstream-attributes_30-0) [Btrfs file attributes](https://btrfs.readthedocs.io/en/latest/btrfs-man5.html#file-attributes); [btrfs-progs v7.1 source](https://github.com/kdave/btrfs-progs/blob/v7.1/Documentation/ch-file-attributes.rst).
31. [↑](https://wiki.gentoo.org#cite_ref-31) [Documentation for /proc/sys/vm/\*](https://www.kernel.org/doc/Documentation/sysctl/vm.txt), [Kernel.org](https://www.kernel.org). Retrieved on January 1st, 2017.
32. ↑ <sup>[32.0](https://wiki.gentoo.org#cite_ref-kernel-drop-caches_32-0)</sup> <sup>[32.1](https://wiki.gentoo.org#cite_ref-kernel-drop-caches_32-1)</sup> [Linux drop\_caches documentation](https://docs.kernel.org/admin-guide/sysctl/vm.html#drop-caches); [Linux 6.18.52 documentation source](https://github.com/gregkh/linux/blob/v6.18.52/Documentation/admin-guide/sysctl/vm.rst#drop_caches).
33. [↑](https://wiki.gentoo.org#cite_ref-GitHub_docs:_update_TODO_.C2.B7_util-linux.2Futil-linux.402b1322f_33-0) [GitHub docs: update TODO · util-linux/util-linux@2b1322f](https://github.com/util-linux/util-linux/commit/2b1322f478d0149d3d570025dbec7cdf897b99a1)
34. [↑](https://wiki.gentoo.org#cite_ref-Redhat_Bugzilla_34-0) [1084453 – lsblk does not show mountpoints for raid-1 btrfs](https://bugzilla.redhat.com/show_bug.cgi?id=1084453)
