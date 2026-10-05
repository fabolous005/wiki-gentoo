<!-- source: https://wiki.gentoo.org/wiki/Bcachefs | group: Gentoo Wiki (Main) | wiki-title: Bcachefs -->
---
title: bcachefs
url: https://wiki.gentoo.org/wiki/Bcachefs
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-04-02"
fingerprint: "37854b10de219bc0"
license: CC BY-SA 4.0
---

# bcachefs

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Resources**

**bcachefs** is a fully-featured [B-tree](https://en.wikipedia.org/wiki/B-tree) [filesystem](https://wiki.gentoo.org/wiki/Filesystem) based on [bcache](https://wiki.gentoo.org/wiki/Bcache). It includes features such as Copy-on-Write (CoW), compression, encryption, and erasure coding. Bcachefs is comparable to [Btrfs](https://wiki.gentoo.org/wiki/Btrfs) and [ZFS](https://wiki.gentoo.org/wiki/ZFS).

A noteworthy feature is native tiered storage support, enabling use of one or more fast disk drives (such as flash-based [SSD](https://wiki.gentoo.org/wiki/SSD) or [NVMe](https://wiki.gentoo.org/wiki/NVMe) disks) to act as a cache for one or more slower disk drives in a pool while transparently managing *hot* and *cold* files based on activity.

## Installation

The [modules](https://packages.gentoo.org/useflags/modules) [flag is enabled by default on](https://wiki.gentoo.org/wiki/USE_flag) [sys-fs/bcachefs-tools](https://packages.gentoo.org/packages/sys-fs/bcachefs-tools), contains kernel config checks, and will build and install the corresponding kernel module version.

### USE flags


| [+initramfs](https://packages.gentoo.org/useflags/+initramfs) | Include kernel modules in the initramfs, and re-install the kernel (only effective for distribution kernels) | 
| [+modules](https://packages.gentoo.org/useflags/+modules) | Build the kernel modules | 
| [+strip](https://packages.gentoo.org/useflags/+strip) | Allow symbol stripping to be performed by the ebuild for special files | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [dist-kernel](https://packages.gentoo.org/useflags/dist-kernel) | Enable subslot rebuilds on Distribution Kernel upgrades | 
| [fuse](https://packages.gentoo.org/useflags/fuse) | Enable bcachefs FUSE support (experimental!) | 
| [llvm-libunwind](https://packages.gentoo.org/useflags/llvm-libunwind) | Use llvm-runtimes/libunwind instead of sys-libs/libunwind | 
| [modules-compress](https://packages.gentoo.org/useflags/modules-compress) | Install compressed kernel modules (if kernel config enables module compression) | 
| [modules-sign](https://packages.gentoo.org/useflags/modules-sign) | Cryptographically sign installed kernel modules (requires CONFIG\_MODULE\_SIG=y in the kernel) | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

### Emerge

`root #``emerge --ask sys-fs/bcachefs-tools`
### Shell completions

Emerging the package does not automatically install shell completions, to install shell completions for bcachefs, use the command bcachefs completions. Currently only the following shells have completions: Bash, Evlish, Fish, Powershell, and ZSH.

`root #``bcachefs completions <shell>`
## Usage

### Creation

To format and use a single filesystem with bcachefs:

`root #``bcachefs format /dev/sda1`
### Multi-device filesystems

The most basic multi-device filesystem would look something like:

`root #``bcachefs format /dev/sda /dev/sdb`
#### Caching, targets, and data placement

By default, writes are striped across all devices in a filesystem, but they may be directed to a specific device or set of devices with the various target options. The allocator only prefers to allocate from devices matching the specified target; if those devices are full, it will fall back to allocating from any device in the filesystem.[\[1\]](https://wiki.gentoo.org#cite_note-bcfs-docs-devlabels-1)

Four target options exist. These options all may be set at the filesystem level (at format time, at mount time, or at runtime via sysfs), or on a particular file or directory<sup>[\[1\]](https://wiki.gentoo.org#cite_note-bcfs-docs-devlabels-1)</sup>:

- `foreground_target`: normal foreground data writes, and metadata if metadata\_target is not set
- `metadata_target`: btree writes
- `background_target`: If set, user data (not metadata) will be moved to this target in the background
- `promote_target`: If set, a cached copy will be added to this target on read, if none exists

For a basic multi device filesystem, with /dev/sda caching /dev/sdb, device names can be used directly:

`root #``bcachefs format /dev/sd[ab] --foreground_target /dev/sda  --promote_target /dev/sda  --background_target /dev/sdb --metadata_target /dev/sda``root #``mount -t bcachefs /dev/sda:/dev/sdb /mnt`
For a multi device filesystem where multiple devices need to be assigned to target, it is required to label the devices.

Labels are paths, with dot delimiters, which allows devices to be grouped into a hierarchy.

For example, formatting with the following labels

bcachefs format --label=ssd.ssd1 /dev/sda1 --label=ssd.ssd2 /dev/sdb1 \ --label=hdd.hdd1 /dev/sdc1 --label=hdd.hdd2 /dev/sdd1
Then target options could refer to any of:

- `--foreground_target=/dev/sda1`
- `--foreground_target=ssd` (both sda1 and sdb1)
- `--foreground_target=ssd.ssd1` (alias for sda1)

- —bcachefs/Target options and disk labels<sup>[\[2\]](https://wiki.gentoo.org#cite_note-bcfs-caching-2)</sup>


For writeback caching (the most common configuration), we want foreground writes to go to the fast device, data to be moved in the background to the slow device, and additionally any time we read if the data isn't already on the fast device we want a copy to be stored there. Continuing with the previous example, you'd use the following options:

- `--foreground_target=ssd`
- `--background_target=hdd`
- `--promote_target=ssd`

The rebalance thread will continually move data to the background\_target device(s). When doing so, the copy on the original device will be kept but marked as cached; also, when promoting data to the promote target the newly-written copy will be marked as cached.

- —bcachefs/Caching<sup>[\[2\]](https://wiki.gentoo.org#cite_note-bcfs-caching-2)</sup>


For a filesystem with multiple background devices, using /dev/nvme0n1 (as fast) caching /dev/sda and /dev/sdb (as slow)

`root #``bcachefs format --label=fast.nvme1 /dev/nvme0n1 --label=slow.hdd1 /dev/sda --label=slow.hdd2 /dev/sdb --foreground_target fast  --promote_target fast  --background_target slow --metadata_target fast``root #``mount -t bcachefs /dev/nvme0n1:/dev/sda:/dev/sdb /mnt`
The additional options `data_allowed` and `durability` can be used as follows:

`data_allowed`

The target options are best-effort; if the specified devices are full the allocator will fall back to allocating from any device that has space.

The per-device data\_allowed option can be used to restrict devices to be used for only journal, btree, or user data, and this is a hard restriction.

`durabiliity`

Some devices may already have internal redundancy, e.g. a hardware raid controller. The durability option may be used to indicate that a replicas on a device should count as being worth n replicas towards the desired total.

Also, specifying `--durability=0` allows a device to be used for true writethrough caching, where we consider a device to be untrusted: allocations will ensure that the device can be yanked at any time without losing data.

- —bcachefs/Caching<sup>[\[2\]](https://wiki.gentoo.org#cite_note-bcfs-caching-2)</sup>


#### Replication

bcachefs supports standard RAID1/10 style redundancy with the `data_replicas` and `metadata_replicas` options. Layout is not fixed as with RAID10: a given extent can be replicated across any set of devices.

The bcachefs fs usage command shows how data is replicated within a filesystem.

#### Erasure Coding

bcachefs supports Reed-Solomon erasure coding. When enabled with the `ec` option, the desired redundancy is taken from the `data_replicas` option. Erasure coding of metadata is not supported.[\[4\]](https://wiki.gentoo.org#cite_note-4)

### Filesystem options

To set options on a filesystem after creation, use bcachefs set-option:

`root #``bcachefs set-option --compression=lz4 /dev/sdb`
### Mount

There are multiple ways to mount a bcachefs filesystem once it has been created, manually mounting and using the fstab.

#### Single-device bcachefs

`root #``mount -t bcachefs /dev/sdb /mnt`
Or to mount with bcachefs:

`root #``bcachefs mount /dev/sdb /mnt`
To add it to the fstab:

**`/etc/fstab`**

#### Multi-device bcachefs

Systemd does currently not support multi-device fstab entries (see [https://github.com/systemd/systemd/issues/8234](https://github.com/systemd/systemd/issues/8234)). As workaround, you can use OLD\_BLK\_UUID

**`/etc/fstab`**

The UUID could be obtained, for example, via

`user $``lsblk -f`
### Resizing

Resizing the filesystem can be done with the device resize command:

`root #``bcachefs device resize /dev/sda [size]`
To resize the journal on a device, use resize-journal:

`root #``bcachefs device resize-journal /dev/sda [size]`
### Compression

Currently, bcachefs supports gzip, lz4, and zstd for compression. To compress a filesystem on format, add the option as an argument:

`root #``bcachefs format --compression=zstd /dev/sdb`
### Multiple devices

#### Adding

To add a device to an existing bcachefs filesystem, use device add:

`root #``bcachefs device add <External UUID> /dev/sdb`
#### Removing

To remove the device just added, use `remove`

`root #``bcachefs device remove /dev/sdb`
#### Connecting

To add a device to a mounted filesystem that did not have the device when mounted, use online:

`root #``bcachefs device online /dev/sdb`
#### Disconnecting

To remove a device from a mounted filesystem without removing it, use offline:

`root #``bcachefs device offline /dev/sdb`
#### Evacuating

To prepare a drive for removal and migrate data off of it, use evacuate:

`root #``bcachefs device evacuate /dev/sdb`
#### Device state

A device can be in one of four states: `rw`, `ro`, `failed`, `spare`. A failed device has zero durability and replicas do not count towards the number an extent should have.

To set a device in the `failed` state, use set-state:

`root #``bcachefs device set-state failed /dev/sdb`
### Subvolumes

Subvolumes in Bcachefs can currently be interacted with in three different ways: creation, deletion, and snapshots. They also do not need to be mounted as the filesystem handles it when the main volume is mounted.

#### Create

`root #``bcachefs subvolume create <name>`
#### Delete

`root #``bcachefs subvolume delete <name>`
#### Snapshots

`root #``bcachefs subvolume create /path/to/subvolume /path/to/snapshots/name`
### Encryption

#### Changing the passphrase

To change the passphrase on an encrypted filesystem:

`root #``bcachefs set-passphrase /dev/sda`
#### Unlocking

The simplest way to decrypt a bcachefs volume (or pool) is to use the following command on a single member:

`root #``bcachefs unlock /dev/sdx`
To decrypt a bcachefs volume while using systemd, insert '-k session' into the unlock command:

`root #``bcachefs unlock -k session /dev/sdx`
It is also possible to permanently unlock a filesystem using the remove-passphrase command:

`root #``bcachefs remove-passphrase /dev/sda`
### Labels and target options

By default, bcachefs stripes writes across all devices in a filesystem. For more control over the placement of data (or to improve performance) it is possible to direct particular filesystem activity to a disk or collection of disks using labels.

In bcachefs these activities are categorised as target options. Four target options exist which may be set at the filesystem level (at format time, at mount time, or at runtime via sysfs), or on a particular file or directory:

- foreground target: normal foreground data writes, and metadata if metadata target is not set
- metadata target: btree writes
- background target: If set, user data (not metadata) will be moved to this target in the background
- promote target: If set, a cached copy will be added to this target on read, if none exists

Label names are arbitrary - `ssd.ssd1`  works just as well as `ssd.1` or `fast.1`. Labels are also hierarchical: to refer to all disks labelled `ssd.ssd#`, `ssd` may be used. Labels are not required and it is possible to target to a device directly (e.g. /dev/sda1) however this is not recommended; udev naming is not reliable. In larger pools it is advised to instead use a label for any target that needs to be configured.

Target options may be set as file attributes (i.e. controlled per-file). The bcachefs setattr command is used for this, e.g.:

`root #``bcachefs setattr --background_target=ssd /path/to/file`
### Filesystem information

#### Showing the superblock

Displaying information about the superblock shows everything needed to determine what a bcachefs device does, i.e. it displays: compression type, device members, quotas, if ACLs are enabled, and more.

`root #``bcachefs show-super /dev/sdb`
#### Data usage

To display information regarding the usage of the filesystem, use fs usage:

`root #``bcachefs fs usage`
### Advanced usage

Bcachefs supports a a number of additional features, including compression, encryption, and disk labels; an example configuration using these features may be found below:

`root #``bcachefs format --compression=zstd \````
    --encrypted \
    --replicas=2 \
    --label=hdd.hdd1 /dev/sdc \
    --label=hdd.hdd2 /dev/sdd \
    --label=hdd.hdd3 /dev/sde \
    --label=hdd.hdd4 /dev/sdf \
    --label=hdd.hdd5 /dev/sdg \
    --label=hdd.hdd6 /dev/sdh \
    --label=hdd.hdd7 /dev/sdi \
    --label=hdd.hdd8 /dev/sdj \
    --label=hdd.hdd9 /dev/sdk \
    --label=ssd.ssd1 /dev/sdl \
    --label=ssd.ssd2 /dev/sdm \
    --label=ssd.ssd3 /dev/sdn \
    --label=ssd.ssd4 /dev/sdo \
    --label=ssd.ssd5 /dev/sdp \
    --label=ssd.ssd6 /dev/sdq \
    --foreground_target=ssd \
    --promote_target=ssd \
    --background_target=hdd \
    --metadata_target=ssd
```
## Troubleshooting

### Filesystem check

It is possible to check for corruption on a bcachefs filesystem either in userspace or when being mounted by the kernel. In either case, the same fsck implementation is executed, just in a different environment. Running fsck in the kernel at mount time has better performance, while the userspace implementation can be stopped by the user, and enables user input for resolving errors.[\[5\]](https://wiki.gentoo.org#cite_note-5)

To check in userspace, use fsck:

`root #``bcachefs fsck /dev/sdb`
Or a "dry-run" can be ran using the arguments `-ny`

`root #``bcachefs fsck -ny /dev/sdb`
To run fsck on filesystem mount, add `-o fsck` to the mount options. To make changes to the filesystem to fix errors, add `-o fix_errors`.

### Debugging information

To get debugging information for a bcachefs filesystem, the dump, list, and list\_journal commands will be useful.

Dumping a bcachefs filesystem will dump its metadata into a .qcow2 image file

`root #``bcachefs dump`
Listing a filesystem will give the same functionality as the debugfs interface, listing btree nodes and contents but for offline filesystems.

`root #``bcachefs list`
Listing the contents of the journal will show the records of btree updates ordered by when they occurred

`root #``bcachefs list_journal`
## See also

- [Bcache](https://wiki.gentoo.org/wiki/Bcache) — a Linux [kernel](https://wiki.gentoo.org/wiki/Kernel) block layer cache.
- [Btrfs](https://wiki.gentoo.org/wiki/Btrfs) — a copy-on-write (CoW) [filesystem](https://wiki.gentoo.org/wiki/Filesystem) for Linux aimed at implementing advanced features while focusing on fault tolerance, repair, and easy administration.
- [ZFS](https://wiki.gentoo.org/wiki/ZFS) — a next generation [filesystem](https://wiki.gentoo.org/wiki/Filesystem) created by Matthew Ahrens and Jeff Bonwick.
