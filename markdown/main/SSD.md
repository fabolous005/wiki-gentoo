<!-- source: https://wiki.gentoo.org/wiki/SSD | group: Gentoo Wiki (Main) | wiki-title: SSD -->
---
title: SSD
url: https://wiki.gentoo.org/wiki/SSD
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-11-11"
fingerprint: "7e5e5f1485a72be0"
license: CC BY-SA 4.0
---

# SSD

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article provides guidelines for basic maintenance, such as enabling discard/trim support, for **SSD**s ([Solid State Drives](https://en.wikipedia.org/wiki/Solid-state_drive)) on Linux. It presumes the user has a basic understanding of partitioning and formatting disk drives.

## Introduction

The term Solid State Drive is commonly used for flash-based block devices. Compared to conventional [HDD](https://wiki.gentoo.org/wiki/HDD), flash-based technology offers a much faster access time, lower latency, silent operation, power savings (no moving parts), and more. However, the flash-based technology brings a few issues which require some special system attention and care.

### Dealing with empty blocks

Generally, traditional [filesystems](https://wiki.gentoo.org/wiki/Filesystem) do not erase deleted data blocks but only flags them as such. Due to nature of flash memory cells any write operation has to be done to empty cells only. Thus writing to physically non-empty cells, flagged as deleted by a filesystem, requires their erasure which makes the operation slower than writing to empty cells. This problem is further amplified by hardware limitations. Futhermore, the data still remains on the storage media, even when it is flagged as deleted, which is a non-issue on conventional storage media such as HDD. On SDD on the other hand, storing unused (deleted) data severely limits the availability of empty cells.

For modern [kernels](https://wiki.gentoo.org/wiki/Kernel) it is possible to hint the deleted (not-used) data blocks to SSD. The described mechanism is called **discard**. Names of implementations differ — **TRIM** for [ATAPI](https://en.wikipedia.org/wiki/ATA_Packet_Interface), **UNMAP** for [SCSI](https://en.wikipedia.org/wiki/SCSI), **Deallocate** for [NVMe](https://wiki.gentoo.org/wiki/NVMe); [MMC](https://en.wikipedia.org/wiki/MultiMediaCard) and [SD cards](https://wiki.gentoo.org/wiki/SDCard) (although not contained in the term "SSD" technically sharing the same technology: non-volatile [flash memory](https://en.wikipedia.org/wiki/flash_memory)) distinguish between TRIM and **ERASE**. Filesystem support is required in order to use discard. Majority of modern filesystems (like [ext4](https://wiki.gentoo.org/wiki/Ext4)<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>, [XFS](https://wiki.gentoo.org/wiki/XFS)<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>, [Btrfs](https://wiki.gentoo.org/wiki/Btrfs)<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup>, or [bcachefs](https://wiki.gentoo.org/wiki/Bcachefs)) support discard, and it has been implemented for traditional (existing, "old") filesystems as well (e.g. [FAT](https://wiki.gentoo.org/wiki/FAT), or [NTFS](https://wiki.gentoo.org/wiki/NTFS)). Also there are filesystems developed primarily for flash-based devices, such as [F2FS](https://wiki.gentoo.org/wiki/F2FS).

There are two basic approaches to issue the discard command — using mount `discard` option (`-o discard`) for continuous discard<sup>[\[4\]](https://wiki.gentoo.org#cite_note-4)</sup> or periodic calls of fstrim utility<sup>[\[5\]](https://wiki.gentoo.org#cite_note-fstrim-5)</sup>. Not all filesystems support both methods.

### Slowing wear out

Each write operation performed on a [NAND](https://en.wikipedia.org/wiki/Nand_memory) flash cell causes its wear. This fact limits the SSD lifespan. The cell endurance varies with used technology<sup>[\[6\]](https://wiki.gentoo.org#cite_note-lifetime-6)</sup>. On the other hand, read operations are straightforward and do not cause cell wear.

A basic method increasing SSD lifespan is to uniformly distribute writes across all the blocks. This method is called *wear leveling* and is deployed via SSD firmware.

From the system point of view, it is appropriate to generally reduce amount of writes.

## Considerations

### Discard (trim) support

Device support for discard (sometimes referred to as trim) should be verified before performing any form of discarding on the drive.

It is possible to use lsblk utility from [sys-apps/util-linux](https://packages.gentoo.org/packages/sys-apps/util-linux):

`user $``lsblk --discard`
NAME   DISC-ALN DISC-GRAN DISC-MAX DISC-ZERO
sda           0      512B       2G         0
├─sda1        0      512B       2G         0
├─sda2        0      512B       2G         0
└─sda3        0      512B       2G         0
sdb           0        0B       0B         0
└─sdb1        0        0B       0B         0

A device supporting discard has non-zero values in the columns of `DISC-GRAN` (discard granularity) and `DISC-MAX` (discard max bytes). In the example listing above, the /dev/sda device supports discard while /dev/sdb does not.

## Initial setup

### Partitioning

Sizes of SSD internal data structures (blocks and pages) varies across different devices. Filesystems operates on data structures of different sizes. For optimal performance filesystem data structures should aim not to cross boundaries of underlying SSD internal data structures. Thus effectively minimizing the number of required internal SSD operations. This can be achieved by aligning start of each partition — the common alignment is to 1 MiB.

Both parted and fdisk partitioning utilities support partition alignment. For parted, there is `-a optimal` option. Recent versions of fdisk should use optimal alignment by default<sup>[\[7\]](https://wiki.gentoo.org#cite_note-7)</sup>.

It is possible to easily check the alignment for given partition using parted:

`root #``parted /dev/sda`
(parted) align-check optimal 1
1 aligned

For further details about the partitioning, follow dedicated [handbook chapter](https://wiki.gentoo.org/wiki/Handbook:AMD64/Blocks/Disks).

#### blkdiscard

blkdiscard utility from [sys-apps/util-linux-2.23](https://packages.gentoo.org/packages/sys-apps/util-linux-2.23) (or later) discards all data blocks on given device.

#### LVM

[LVM](https://wiki.gentoo.org/wiki/LVM) aligns to MiB boundaries and passes discards to underlying devices by default. No additional configuration is required.

In order to discard all unused space in a [Volume Group](https://wiki.gentoo.org/wiki/LVM#VG_.28Volume_Group.29) ([VG](https://wiki.gentoo.org/wiki/LVM#VG_.28Volume_Group.29)) use the [blkdiscard](https://wiki.gentoo.org/wiki/SSD#blkdiscard) utility:

`root #````
lvcreate -l100%FREE -n trim yourvg
```
`root #````
blkdiscard /dev/yourvg/trim
```
`root #````
lvremove yourvg/trim
```
Alternatively, there is a discard option in lvm.conf which makes LVM discard entire [Logical Volume](https://wiki.gentoo.org/wiki/LVM#LV_.28Logical_Volume.29) ([LV](https://wiki.gentoo.org/wiki/LVM#LV_.28Logical_Volume.29)) on lvremove, lvreduce, pvmove and other actions that free Physical Extents (PE) in a VG.

**`/etc/lvm/lvm.conf`**

#### LUKS

For discards to pass through [fully encrypted devices](https://wiki.gentoo.org/wiki/Full_Disk_Encryption_From_Scratch) ([LUKS](https://wiki.gentoo.org/wiki/Dm-crypt)), they have to be opened with the `--allow-discards` option.

`root #``cryptsetup luksOpen --allow-discards /dev/thing <name>`
This can be applied permanently in LUKS2 by adding it to the header.

`root #``cryptsetup refresh --persistent --allow-discards <name>`
To check if discard is enabled for the current session, see if `flags: discards` is listed in:

`root #``cryptsetup status <name>`
To check if discard is permamently enabled, see if `Flags: allow-discards` is listed in:

`root #``cryptsetup luksDump /dev/thing`
##### LUKS1

When the root-device is encrypted with LUKS1, discards must be enabled by the [initramfs](https://wiki.gentoo.org/wiki/Initramfs).
When using [genkernel](https://wiki.gentoo.org/wiki/Genkernel) for creating your initramfs, pass the following kernel option:

**`/etc/default/grub`**

When using [dracut](https://wiki.gentoo.org/wiki/Dracut) for creating the initramfs, pass the following kernel option:

**`/etc/default/grub`**

To evaluate if discard is enabled on a LUKS device, check if the output of the following command contains the string `allow_discards`:

`root #``dmsetup table /dev/mapper/crypt_dev --showkeys`
### Formatting

Similarly to [partitions](https://wiki.gentoo.org/wiki/SSD#Partitioning), performance can be improved if a filesystem is configured the way it can align its data structures with device's internal structures sizes — namely its erase block size.

This configuration gets important in case of a software RAID, when one really should know the erase block size<sup>[\[8\]](https://wiki.gentoo.org#cite_note-8)</sup>. Consider this information when making a purchase.

#### Configuring for erase block size

When device's erase block size is known, it can be used when creating a filesystem.

For example for [ext4](https://wiki.gentoo.org/wiki/Ext4) using mkfs.ext4 on an average-sized partition, it will apply 4KiB blocks<sup>[\[9\]](https://wiki.gentoo.org#cite_note-9)</sup>. Using `-E stride` and `-E stripe-width` options, it is possible to set the alignment to erase block size. Both options should be set as *erase block size* / *block size*.

For a drive with 512KiB erase block size, it makes 512KiB / 4KiB = 128:

`root #``mkfs.ext4 -E stride=128,stripe-width=128 /dev/sda3`
##### List of devices with known erase block sizes

- *OCZ* drives; stride an stripe-width are 128

- *Crucial M500 240GB*; stride and stripe-width are 2048

- *SanDisk z400s*; stride an stripe-width are 4096

### Mounting

For rootfs it is usually recommended to periodically use fstrim utility. Using the `discard` mount option results in continuous discard that could potentially cause degradation of older or poor-quality SSDs<sup>[\[5\]](https://wiki.gentoo.org#cite_note-fstrim-5)</sup>.

The following command can be used manually or be setup as a [periodic job](https://wiki.gentoo.org/wiki/SSD#Periodic_fstrim_jobs) to run once a week<sup>[\[12\]](https://wiki.gentoo.org#cite_note-freq-12)</sup>:

`root #``fstrim -v /`
For mount points with a low amount of disk writes occurring on a SSD it should be safe to use the `discard` mount option in [/etc/fstab](https://wiki.gentoo.org/wiki//etc/fstab). Also it is recommended to use the mount option when maintaining performance is required<sup>[\[13\]](https://wiki.gentoo.org#cite_note-13)</sup>.

Given the considerations above, a discard-enabled /etc/fstab could look like this:

**`/etc/fstab`**

**fstab with discard enabled**

Once the /etc/fstab has been modified, remount all filesystems mentioned there via:

`root #``mount -a`
## Additional configuration

### Periodic fstrim jobs

There are multiple ways how to setup a periodic block discarding process. As of 2018, the default recommended frequency is once a week<sup>[\[12\]](https://wiki.gentoo.org#cite_note-freq-12)</sup>.

#### cron

Run fstrim on all mounted devices that support discard on a weekly basis:

**`/etc/crontab`**

**Run fstrim once per week**

Similarly, it is possible to run fstrim only for a selected mount point:

**`/etc/crontab`**

**Run fstrim once per week on rootfs**

#### SSDcronTRIM

There is also a semi-automatic cron job available on GitHub called [SSDcronTRIM](http://chmatse.github.io/SSDcronTRIM/) which has the following features:

- Distribution independent script (developed on a Gentoo system).
- The script decides every time depending on the disk usage how often (monthly, weekly, daily, hourly) each partition has to be trimmed.
- Recognizes if it should install itself into /etc/cron.{monthly,weekly,daily,hourly}, /etc/cron.d or any other defined directory and if it should make an entry into crontab.
- Checks if the kernel meets the requirements, the filesystem is able to and if the SSD supports trimming.

#### SSDcronTRIM-LUKS

There is also a semi-automatic cron job available on GitHub called [SSDcronTRIM-LUKS](https://github.com/spai-phoenix/SSDcronTRIM-L/) with dm-crypt/LUKS support.

#### systemd timer

[sys-apps/util-linux](https://packages.gentoo.org/packages/sys-apps/util-linux) on systemd-enabled systems comes with a timer unit executing a weekly fstrim. Enable it with:

`root #``systemctl enable fstrim.timer`
#### Without cron, on system shutdown (with OpenRC)

A [/etc/local.d](https://wiki.gentoo.org/wiki//etc/local.d) script may be used to trim on poweroff on Fridays:

**`/etc/local.d/date.stop`**

```
# From
# https://fitzcarraldoblog.wordpress.com/2018/01/13/running-a-shell-script-at-shutdown-only-not-at-reboot-a-comparison-between-openrc-and-systemd/
if [ `who -r | awk '{print $2}'` = "0" ] && [ "$(date +%a)" = "Fri" ]; then
    echo /etc/local.d/trim.stop: run SSD trim
    fstrim / --verbose
    sleep 5
fi
```
### Reducing amount of writes

The flash-based SSDs have a limited write lifetime - the number of writes performed<sup>[\[6\]](https://wiki.gentoo.org#cite_note-lifetime-6)</sup>. Thus when using a SSD, administrators generally want to reduce the amount of writes.

#### Portage `TMPDIR` on tmpfs

When building packages via [Portage](https://wiki.gentoo.org/wiki/Portage) it is possible to perform the operations in RAM by using a [tmpfs](https://wiki.gentoo.org/wiki/Tmpfs) or [zram](https://wiki.gentoo.org/wiki/Zram) mount. This has the theoretical benefit of reducing writes to the SSD. See [Portage `TMPDIR` on tmpfs](https://wiki.gentoo.org/wiki/Portage_TMPDIR_on_tmpfs) (or zram) guide.

#### Temporal files on tmpfs

It is possible to mount desired mount points as [tmpfs](https://wiki.gentoo.org/wiki/Tmpfs). Since tmpfs stores files in volatile memory all the I/O operations directed to the given mount points are not performed on the solid state disk. This reduces the amount of writes and also improves performance.

This is an example of both /tmp and /var/tmp being mounted as tmpfs:

#### XDG cache on tmpfs

When running a Gentoo desktop, many programs, using [X Window System](https://wiki.gentoo.org/wiki/Xorg) ([Chromium](https://wiki.gentoo.org/wiki/Chromium), [Firefox](https://wiki.gentoo.org/wiki/Firefox) etc.) make frequent disk I/O every few seconds to cache<sup>[\[14\]](https://wiki.gentoo.org#cite_note-14)</sup>.

The cache directory location usually complies to *XDG Base Directory Specification*<sup>[\[15\]](https://wiki.gentoo.org#cite_note-15)</sup>, namely to the `XDG_CACHE_HOME` environment variable. The default cache location is \~/.cache, which is usually mounted on a hard drive and could be moved to [tmpfs](https://wiki.gentoo.org/wiki/Tmpfs).

To remap the cache directory location create a script that exports to directory under /run:

**`/etc/profile.d/xdg_cache_home.sh`**

```
if [ ${LOGNAME} ]; then
  export XDG_CACHE_HOME="/run/user/${UID}/cache"
fi
```
#### Web browser profile(s) and cache on tmpfs

The web browser profile/s, cache, etc. can be relocated to [tmpfs](https://wiki.gentoo.org/wiki/Tmpfs). The corresponding I/O associated with using the browser gets redirected from the SSD drive to tmpfs' volatile memory, resulting in reduced wear to the physical drive and also improving browser speed and responsiveness.

It is possible to relocate the browser components mentioned above with the utility [www-misc/profile-sync-daemon](https://packages.gentoo.org/packages/www-misc/profile-sync-daemon):

`root #``emerge --ask www-misc/profile-sync-daemon`
##### systemd

Close all the browsers, start and enable the daemon:

`user $``systemctl --user enable --now psd`
Now it is possible to view all symlinks by printing the status of the started daemon:

`user $``psd p`
##### OpenRC

Next add the users whose browser(s) profile(s) will get symlinked to a tmpfs or another mountpoint in the variable `USERS`:

**`/etc/psd.conf`**

Finally, close all the browsers, start and enable the daemon:

`root #````
rc-update add psd default
```
`root #````
rc-service psd start
```
Now it is possible to view all symlinks by printing the status of the started daemon:

`user $``psd p`
## See also

- [HDD](https://wiki.gentoo.org/wiki/HDD) — describes the setup of an internal SATA or PATA (IDE) rotational **hard disk drive**.
- [NVMe](https://wiki.gentoo.org/wiki/NVMe) — flash memory chips connected to a system via the PCI-E bus (use four-lane max).

## External resources

- [Aligning an SSD on Linux](http://blog.nuclex-games.com/2009/12/aligning-an-ssd-on-linux/) — Drives internal structures explained.
- [Aligning filesystems to an SSD’s erase block size](https://tytso.livejournal.com/2009/02/20/) — Aligning explained by Ted T'so.
- [Magic soup: ext4 with SSD, stripes and strides](https://thelastmaimou.wordpress.com/2013/05/04/magic-soup-ext4-with-ssd-stripes-and-strides/) — ext4 aligning discussion
- [Arch Linux Profile-Sync-Daemon](https://wiki.archlinux.org/index.php/profile-sync-daemon)

## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) [Performance of TRIM command on ext4 filesystem](https://people.redhat.com/lczerner/discard/ext4_discard.html), people.redhat.com. Retrieved on October 29, 2018
2. [↑](https://wiki.gentoo.org#cite_ref-2) [FITRIM/discard](http://xfs.org/index.php/FITRIM/discard), XFS.org. Retrieved on October 29, 2018
3. [↑](https://wiki.gentoo.org#cite_ref-3) [FAQ - btrfs Wiki](https://btrfs.wiki.kernel.org/index.php/FAQ#Does_Btrfs_support_TRIM.2Fdiscard.3F), btrfs.wiki.kernel.org. Retrieved on October 29, 2018
4. [↑](https://wiki.gentoo.org#cite_ref-4) [mount(8) - Linux manual page](http://man7.org/linux/man-pages/man8/mount.8.html), man7.org. Retrieved on October 29, 2018
5. ↑ <sup>[5.0](https://wiki.gentoo.org#cite_ref-fstrim_5-0)</sup> <sup>[5.1](https://wiki.gentoo.org#cite_ref-fstrim_5-1)</sup> [fstrim(8) - Linux manual page](http://man7.org/linux/man-pages/man8/fstrim.8.html), man7.org. Retrieved on October 29, 2018
6. ↑ <sup>[6.0](https://wiki.gentoo.org#cite_ref-lifetime_6-0)</sup> <sup>[6.1](https://wiki.gentoo.org#cite_ref-lifetime_6-1)</sup> [Hard Drive - Why Do Solid State Devices (SSD) Wear Out](https://www.dell.com/support/article/cz/cs/czdhs1/sln156899/hard-drive-why-do-solid-state-devices-ssd-wear-out?lang=en), Dell. Retrieved on October 29, 2018
7. [↑](https://wiki.gentoo.org#cite_ref-7) [fdisk(8) - Linux manual page](http://man7.org/linux/man-pages/man8/fdisk.8.html), man7.org. Retrieved on October 31, 2018
8. [↑](https://wiki.gentoo.org#cite_ref-8) [RAID setup - Linux Raid Wiki](https://raid.wiki.kernel.org/index.php/RAID_setup#Performance), wiki.kernel.org. Retrieved on November 1, 2018
9. [↑](https://wiki.gentoo.org#cite_ref-9) [mke2fs(8) - Linux manual page](http://man7.org/linux/man-pages/man8/mke2fs.8.html), man7.org. Retrieved on November 1, 2018
10. [↑](https://wiki.gentoo.org#cite_ref-10) [Partition Alignment Spreadsheet](https://www.techpowerup.com/forums/threads/partition-alignment-spreadsheet.107126/), techpowerup.com. Retrieved on November 1, 2018
11. [↑](https://wiki.gentoo.org#cite_ref-11) [The Crucial/Micron M500 Review (960GB, 480GB, 240GB, 120GB)](http://www.anandtech.com/show/6884/crucial-micron-m500-review-960gb-480gb-240gb-120gb)
12. ↑ <sup>[12.0](https://wiki.gentoo.org#cite_ref-freq_12-0)</sup> <sup>[12.1](https://wiki.gentoo.org#cite_ref-freq_12-1)</sup> [fstrim.timer\sys-utils - util-linux/util-linux.git - The util-linux code repository](https://git.kernel.org/pub/scm/utils/util-linux/util-linux.git/tree/sys-utils/fstrim.timer), kernel.org. Retrieved on October 30, 2018
13. [↑](https://wiki.gentoo.org#cite_ref-13) [2.4. Discard unused blocks](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Storage_Administration_Guide/ch02s04.html), Red Hat. Retrieved on October 30, 2018
14. [↑](https://wiki.gentoo.org#cite_ref-14) [Firefox is eating your SSD - here is how to fix it](https://www.servethehome.com/firefox-is-eating-your-ssd-here-is-how-to-fix-it/), Loyolan Ventures. Retrieved on October 28, 2018
15. [↑](https://wiki.gentoo.org#cite_ref-15) [XDG Base Directory Specification](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html), freedesktop.org. Retrieved on October 28, 2018
