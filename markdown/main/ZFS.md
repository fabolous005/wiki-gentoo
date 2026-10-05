<!-- source: https://wiki.gentoo.org/wiki/ZFS | group: Gentoo Wiki (Main) | wiki-title: ZFS -->
---
title: ZFS
url: https://wiki.gentoo.org/wiki/ZFS
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-01"
fingerprint: fc1d68fb65aba6e0
license: CC BY-SA 4.0
---

# ZFS

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

- write "raidz and draid" section

**ZFS**  is a next generation [filesystem](https://wiki.gentoo.org/wiki/Filesystem) created by Matthew Ahrens and Jeff Bonwick. It was designed around a few key ideas:

- Administration of storage should be simple.
- Redundancy should be handled by the filesystem.
- Filesystems should never be taken offline for repair.
- Automated simulations of worst case scenarios before shipping code is important.
- Data integrity is paramount.

Development of ZFS started in 2001 at Sun Microsystems. It was released under the [CDDL](https://opensource.org/licenses/CDDL-1.0) in 2005 as part of OpenSolaris. Pawel Jakub Dawidek ported ZFS to FreeBSD in 2007. Brian Behlendorf at [LLNL](https://www.llnl.gov/) started the ZFSOnLinux project in 2008 to port ZFS to Linux for High Performance Computing. Oracle purchased Sun Microsystems in 2010 and discontinued OpenSolaris later that year.

The Illumos project started to replace OpenSolaris and roughly 2/3 of the core ZFS team resigned, including Matthew Ahrens and Jeff Bonwick. Many took jobs at companies which continue to develop OpenZFS, initially as part of the Illumos project. The 1/3 of the ZFS core team at Oracle that did not resign continue development of an incompatible proprietary branch of ZFS in Oracle Solaris.

The first release of Solaris included a few innovative changes that were under development prior to the mass resignation. Subsequent releases of Solaris have included fewer and less ambitious changes. Today, a growing community continues development of OpenZFS across multiple platforms, including FreeBSD, Illumos, Linux and Mac OS X.

A detailed list of features can be found in a [separate article](https://wiki.gentoo.org/wiki/ZFS/Features).

There are out-of-tree Linux kernel modules available from the [ZFSOnLinux Project](https://zfsonlinux.org/).

Since version 0.6.1, ZFS is considered ["ready for wide scale deployment on everything from desktops to super computers"](http://www.h-online.com/open/news/item/ZFS-on-Linux-is-ready-for-wide-scale-deployment-1832848.html) stable for wide scale deployment, by the OpenZFS Project.


| [+dist-kernel-cap](https://packages.gentoo.org/useflags/+dist-kernel-cap) | Prevents upgrading to an unsupported kernel version when combined with USE=dist-kernel | 
| [+initramfs](https://packages.gentoo.org/useflags/+initramfs) | Include kernel modules in the initramfs, and re-install the kernel (only effective for distribution kernels) | 
| [+modules](https://packages.gentoo.org/useflags/+modules) | Build the kernel modules | 
| [+rootfs](https://packages.gentoo.org/useflags/+rootfs) | Enable dependencies required for booting off a pool containing a rootfs | 
| [+strip](https://packages.gentoo.org/useflags/+strip) | Allow symbol stripping to be performed by the ebuild for special files | 
| [custom-cflags](https://packages.gentoo.org/useflags/custom-cflags) | Build with user-specified CFLAGS (unsupported) | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [dist-kernel](https://packages.gentoo.org/useflags/dist-kernel) | Enable subslot rebuilds on Distribution Kernel upgrades | 
| [kernel-builtin](https://packages.gentoo.org/useflags/kernel-builtin) | Disable dependency on sys-fs/zfs-kmod under the assumption that ZFS is part of the kernel source tree | 
| [minimal](https://packages.gentoo.org/useflags/minimal) | Don't install python scripts (arcstat, dbufstat etc) and avoid dependency on dev-lang/python | 
| [modules-compress](https://packages.gentoo.org/useflags/modules-compress) | Install compressed kernel modules (if kernel config enables module compression) | 
| [modules-sign](https://packages.gentoo.org/useflags/modules-sign) | Cryptographically sign installed kernel modules (requires CONFIG\_MODULE\_SIG=y in the kernel) | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [pam](https://packages.gentoo.org/useflags/pam) | Install zfs\_key pam module, for automatically loading zfs encryption keys for home datasets | 
| [python](https://packages.gentoo.org/useflags/python) | Add optional support/bindings for the Python language | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [split-usr](https://packages.gentoo.org/useflags/split-usr) | Enable behavior to support maintaining /bin, /lib\*, /sbin and /usr/sbin separately from /usr/bin and /usr/lib\* | 
| [test-suite](https://packages.gentoo.org/useflags/test-suite) | Install regression test suite | 
| [unwind](https://packages.gentoo.org/useflags/unwind) | Add support for call stack unwinding and function name resolution | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

The following command installs ZFS:

`root #``emerge --ask sys-fs/zfs``root #``emerge -va @module-rebuild`
The ZED (ZFS Event Daemon) monitors events generated by the ZFS kernel module. When a zevent (ZFS Event) is posted, the ZED will run any ZEDLETs (ZFS Event Daemon Linkage for Executable Tasks) that have been enabled for the corresponding zevent class.

**`/etc/zfs/zed.d/zed.rc`**

Add the zfs scripts to runlevels for initialization at boot:

`root #````
rc-update add zfs-import boot
```
`root #````
rc-update add zfs-mount boot
```
`root #````
rc-update add zfs-load-key boot
```
`root #````
rc-update add zfs-share default
```
`root #````
rc-update add zfs-zed default
```
To enable the service so it is automatically started at boot time:

`root #``systemctl enable zfs.target`
To manually start the daemon:

`root #``systemctl start zfs.target`
In order to mount zfs pools automatically on boot, the following services and targets need to be enabled:

`root #````
systemctl enable zfs-import-cache
```
`root #````
systemctl enable zfs-mount
```
`root #````
systemctl enable zfs-import.target
```
[sys-fs/zfs](https://packages.gentoo.org/packages/sys-fs/zfs) requires Zlib kernel support (module or builtin).

It may be beneficial to use files in the package.mask folder to mask certain versions of zfs-kmod and zfs. It will avoid other versions of those two packages from being pulled in. Furthermore, sys-fs/zfs also must match the same version as zfs-kmod.

If using an initramfs that was generated by either dracut or genkernel, please (re)generate it after (re)compiling the module. When using a custom initramfs, follow the next section:

`root #``mkdir -vp /usr/src/initramfs/lib/modules/$(uname -r)`
Copy over the binary files so that modprobe can find them during init:

`root #``cp -Prv /lib/modules/$(uname -r)/extra/* /usr/src/initramfs/lib/modules/$(uname -r)/extra/`
These libraries are also required in order to boot properly:

`root #` `lddtree --copy-to-tree /usr/src/initramfs /sbin/zfs` `root #` `lddtree --copy-to-tree /usr/src/initramfs /sbin/zpool` `root #` `lddtree --copy-to-tree /usr/src/initramfs /sbin/zed` `root #` `lddtree --copy-to-tree /usr/src/initramfs /sbin/zgenhostid``root #` `lddtree --copy-to-tree /usr/src/initramfs /sbin/zvol_wait` The following example shows the creation of an initramfs for 6.6.2, avoiding files for version 6.1.18:

`root #````
cd /usr/src/initramfs
```
`root #````
find . -not -path "./lib/modules/6.1.118/*" -not -path "./lib/modules/6.1.118" -print0 | cpio --null --create --verbose --format=newc | gzip --best > /boot/initramfs-custom.img
```
Now that the initramfs is ready, (re)configure the boot-loader to use it.

For a collection of tips on some advanced ways to improve edge case needs for ZFS, then please take a look at the following sub article:

ZFS includes already all programs to manage the hardware and the filesystems, there are no additional tools needed.

ZFS supports the use of either block devices or files. Administration is the same in both cases, but for production use, the ZFS developers recommend the use of block devices (preferably whole disks). Users can use whichever way is most convenient while experimenting with the various commands detailed here.

A ZFS storage pool (or zpool) is an aggregate of one or several virtual devices (or vdevs each composed of one or several underlying physical devices grouped together in a certain geometry. Three very important rules:

1. The zpool being striped across vdevs, if one of the latter becomes unavailable, the whole zpool also becomes unavailable
2. Once a dev has been created, its geometry is set in the stone and cannot be modified. the only way to reconfigure a vdev is to destroy and recreate it
3. To able to repair a damaged zpool some level of redundancy is required in each of the underlying vdevs

#### Hardware considerations

- ZFS needs to **directly** see and steer the physical devices supporting a zpool (it has its own I/O scheduler). It is highly unadvised to use hardware RAID, even multiple single RAID-0 virtual disks because that approach mitigates the benefits brought by ZFS. Always use JBODs. Some controllers propose an option, some other like older LSI HBAs have to be re-flashed with a specific firmware ("IT mode firmware" in the case of LSI) to be able to do JBOD. Unfortunately some RAID controllers only propose RAID levels without any alternative and are unsuitable for ZFS.
- ZFS can be extremely I/O intensive:
  - A very popular option in NAS boxes is to use second-hand SAS HBAs attached to several SATA "rusty" platter drives (on a bigger scale, using near-line SATA drives is recommended)
  - If a zpool is created with NVMe modules, **ensure they are not DRAM-less** (check the datasheet). Two reasons for that: avoid a significant bottleneck but also avoid hardware crashes as some of those DRAM-less modules tends to freeze when being overwhelmed by I/O requests<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.
  - Do not use the so called SMR *(Shingled Magnetic Drives)* drives in zpools as those are very slow especially at writing and can timeout on a zpool resilvering thus preventing a recovery. **Always use CMR drives with ZFS.**
- Consider using power loss protected NVMe/SSDs (i.e with on-board capacitors) unless the storage is protected by an uninterruptible power supply (UPS) or has a very reliable power source
- SATA port multipliers should be avoided, unless there is no other option, because of major bottlenecks multiplexing traffic between the drives connected effectively dividing available bandwidth. I/O latency also increases.
- ZFS is able to use vectorized SSE/AVX instructions found on X86/64 processors to speed up checksums calculations and automatically selects the fastest implementation. It is possible (under Linux) to see the microbenchmarks results in /proc/spl/kstat/zfs/fletcher\_4\_bench and vdev\_raidz\_bench. Example:

`root #``cat /proc/spl/kstat/zfs/fletcher_4_bench`
0 0 0x01 -1 0 2959173570 60832614852064
implementation   native         byteswap       
scalar           11346741272    8813266429     
superscalar      13447646071    11813021922    
superscalar4     15452304506    13531748022    
sse2             24205728004    11368002233    
ssse3            24301258139    22109806540    
avx2             43631210878    41199229117    
avx512f          47804283343    19883061560    
avx512bw         47806962319    42246291561    
fastest          avx512bw       avx512bw

#### Redundancy considerations

Just like RAID, ZFS offers several options to create a redundant (or not) vdev which are:

- striped ("RAID-0"): The fatest option but offers no protection against a drive loss. If a drives dies, not only the vdev dies but also the whole zpool dies as well (remember: a zpool is always a striped thing).
- mirror ("RAID 1") : one drive is the mirror of the other. Usually two drives are used but it is possible to have a 3-faces (or more) mirror at the cost of space use efficiency. Tolerates the loss of one drive only (or two for 3-face mirror)
- raidz1 or also raidz ("RAID-5"): a single parity is used and distributed in the vdev and thus that latter does only tolerate the loss of a single drive. The total vdev capacity is equal to the sum all individual drives capacity (minus the capacity of one drive).
- raidz2 ("RAID-6"): same concept as raidz1 but with two parities. The total vdev capacity is equal to the sum all individual drives capacity (minus the capacity of two drives). Tolerates the loss of two drives.
- raidz3: same concept as raidz1 but with three parities. The total vdev capacity is equal to the sum all individual drives capacity (minus the capacity of three drives). Tolerates the loss of three drives. Use for extreme data security.

#### Preparing the storage pool

The following commands create three 2GB sparse image files in /var/lib/zfs\_img used as hard drives. This uses at most 8GB disk space, but in practice will use very little because only written areas are allocated:

`root #````
mkdir /var/lib/zfs_img
```
`root #````
truncate -s 2G /var/lib/zfs_img/zfs0.img
```
`root #````
truncate -s 2G /var/lib/zfs_img/zfs1.img
```
`root #````
truncate -s 2G /var/lib/zfs_img/zfs2.img
```
`root #``truncate -s 2G /var/lib/zfs_img/zfs3.img`
If a ramdisk is more convenient the equivalent of above is:

`root #````
modprobe brd rd_nr=4 rd_size=1953125
```
`root #````
ls -l /dev/ram*
```
brw-rw---- 1 root disk 1, 0 Apr 29 12:32 /dev/ram0
brw-rw---- 1 root disk 1, 1 Apr 29 12:32 /dev/ram1
brw-rw---- 1 root disk 1, 2 Apr 29 12:32 /dev/ram2
brw-rw---- 1 root disk 1, 2 Apr 29 12:32 /dev/ram3

That latter solution involves having activated `CONFIG_BLK_DEV_RAM=m` in the Linux kernel configuration. As a last resort, loopback devices combined to files stored in a tmpfs filesystem can be used. The first part is to create the tmpfs where the raw images files will be put:

`root #````
mkdir /var/lib/zfs_img
```
Then create the raw images files:

`root #````
dd if=/dev/null of=/var/lib/zfs0.img bs=1024 count=2097153 seek=2097152
```
`root #````
dd if=/dev/null of=/var/lib/zfs1.img bs=1024 count=2097153 seek=2097152
```
`root #````
dd if=/dev/null of=/var/lib/zfs2.img bs=1024 count=2097153 seek=2097152
```
`root #``dd if=/dev/null of=/var/lib/zfs3.img bs=1024 count=2097153 seek=2097152`
Then assign a loopback device to each file created above:

`root #````
losetup /dev/loop0 /var/lib/zfs_img/zfs0.img
```
`root #````
losetup /dev/loop1 /var/lib/zfs_img/zfs1.img
```
`root #````
losetup /dev/loop2 /var/lib/zfs_img/zfs2.img
```
`root #````
losetup /dev/loop3 /var/lib/zfs_img/zfs3.img
```
`root #````
losetup -a
```
/dev/loop1: \[0067\]:3 (/tmp/zfs\_img/zfs1.img)
/dev/loop2: \[0067\]:4 (/tmp/zfs\_img/zfs2.img)
/dev/loop0: \[0067\]:2 (/tmp/zfs\_img/zfs0.img)
/dev/loop3: \[0067\]:5 (/tmp/zfs\_img/zfs3.img)

Every ZFS story starts with the program /usr/sbin/zpool which is used to create and tweak ZFS storage pools (zpools). A zpool is a kind of logical storage space, grouping one or several vdevs together. That logical storage can contain both of:

- datasets : A set of files and directories mounted in the VFS, just like any traditional filesystem like xfs or ext4
- zvolumes (or zvols): virtual volumes which can be accessed as virtual block devices and used just like any other physical hard drive on the machine lying under /dev.

Once a zpool has been created, it is possible to take point-in-time pictures (snapshots) of its different zvolumes and datasets. Those snapshots can be browsed or even rolled-back to restore the content as it was back in time. Another paradigm with ZFS is that a zpool, a dataset and a zvolume can be governed by acting on several attributes: Encryption, data compression, usage quotas and many other aspects are just examples on what can be tweaked by attributes.

Remember: a zpool is striped across its vdevs so even a single loss implies the loss of the whole zpool. The redundancy happens at the vdev level, not the zpool level. When losing a zpool, the only option is to rebuild it from scratch and restore its contents from backups. Taking snapshots is not sufficient to ensure a crash recovery, those have to be sent elsewhere. More on this in following sections.

The general syntax is:

`root #` `zpool create <poolname> [{striped,mirror,raidz,raidz2,raidz3}] /dev/blockdevice1 /dev/blockdevice2 ... [{striped,mirror,raidz,raidz2,raidz3}] /dev/blockdeviceA /dev/blockdeviceB ... [{striped,mirror,raidz,raidz2,raidz3}] /dev/blockdeviceX /dev/blockdeviceY ...` Some examples:

- `zpool create zfs_test /dev/sda`: creates a zpool composed of a single vdev containing only one physical device (no redundancy at all!)
- `zpool create zfs_test /dev/sda /dev/sdb`: creates a zpool composed of a single vdev containing two striped physical devices (no redundancy at all!)
- `zpool create zfs_test mirror /dev/sda /dev/sdb`: creates a zpool composed of a single vdev containing two physical devices in mirror
- `zpool create zfs_test mirror /dev/sda /dev/sdb mirror /dev/sdc /dev/sdd`: creates a *striped mirror* zpool composed of a two vdevs each containing two physical devices in mirror (one device in each vdev can die without losing the zpool).
- `zpool create zfs_test raidz /dev/sda /dev/sdb /dev/sdc`: creates a zpool composed of a single vdev containing three physical devices in RAID-Z1 (one loss can be tolerated)

If files rather than block devices are used, just substitute the block device path by the full path of the files used. So:

- `zpool create zfs_test /dev/sda` becomes `zpool create zfs_test /var/lib/zfs_img/zfs0.img`
- `zpool create zfs_test /dev/sda /dev/sdb` becomes `zpool create zfs_test /var/lib/zfs_img/zfs0.img /var/lib/zfs_img/zfs1.img`

and so on.

Once a zpool has been created, whatever its architecture is, a dataset having the same name is also automatically created and mounted directly under the VFS root:

`root #````
mount | grep zpool_test
```
zpool\_test on /zpool\_test type zfs (rw,xattr,noacl)

For the moment, that dataset is an empty eggshell containing no data.

The list of the currently known zpools and some usage statistics can be checked by:

`root #``zpool list`
NAME         SIZE  ALLOC   FREE  CKPOINT  EXPANDSZ   FRAG    CAP  DEDUP    HEALTH  ALTROOT
rpool       5.45T   991G  4.49T        -         -     1%    17%  1.00x    ONLINE  -
zpool\_test  99.5G   456K  99.5G        -         -     0%     0%  1.00x    ONLINE  -

Some brief explanations on what is displayed:

- NAME: The name of the pool
- SIZE: The total size of the pool. In the case of RAID-Z{1,3} vdevs, that size takes into account the parity. As `zpool_test` is a mirrored vdev, the reported size here is the size of a single disk.
- ALLOC: The current size of stored data (same remark as above for RAID-Z{1,3} vdevs)
- FREE: The available free space (FREE = SIZE - ALLOC)
- CKPOINT: The size taken by an eventual checkpoint (i.e. a global zpool snapshot)
- EXPANDSZ: The space available to expand the zpool. Usually nothing is listed here but on particular scenarios
- FRAG: Free space fragmentation expressed in percents. Fragmentation is not an issue with ZFS as it has been designed to deal with that and there is currently no way to defragment a zpool but recreating it from scratch and restoring its data from backups.
- CAP: Used space expressed as percentage
- DEDUP: Data deduplication factor. Always `1.00x` unless a zpool had data de-duplication active.
- HEALTH: Current pool state, "ONLINE" meaning the pool is in an optimal (i.e. non-degraded or, worse, unavailable) state
- ALTROOT: Alternate root path (not persistent across reboots). This is handy in the case a zpool has to be handled from a live media  environment: all datasets are mounted relatively to this path which can be set via the `-R` option of the `zpool import` command.


It is also possible to have bit more details using the option `-v`:

`root #``zpool list -v````
NAME                 SIZE  ALLOC   FREE  CKPOINT  EXPANDSZ   FRAG    CAP  DEDUP    HEALTH  ALTROOT
rpool               5.45T   991G  4.49T        -         -     1%    17%  1.00x    ONLINE  -
  raidz1-0          5.45T   991G  4.49T        -         -     1%  17.8%      -    ONLINE
    /dev/nvme1n1p1  1.82T      -      -        -         -      -      -      -    ONLINE
    /dev/nvme2n1p1  1.82T      -      -        -         -      -      -      -    ONLINE
    /dev/nvme0n1p1  1.82T      -      -        -         -      -      -      -    ONLINE
zpool_test          99.5G   540K  99.5G        -         -     0%     0%  1.00x    ONLINE  -
  mirror-0          99.5G   540K  99.5G        -         -     0%  0.00%      -    ONLINE
    /dev/sdd1       99.9G      -      -        -         -      -      -      -    ONLINE
    /dev/sdg1       99.9G      -      -        -         -      -      -      -    ONLINE
```
Another very useful command is zpool status, which provides a detailed report on what is going on on the zpools and if something has to be fixed:

`root #``zpool status````
zpool status
  pool: rpool
 state: ONLINE
  scan: scrub repaired 0B in 00:03:18 with 0 errors on Sat Apr 29 15:11:19 2023
config:
        NAME                STATE     READ WRITE CKSUM
        rpool               ONLINE       0     0     0
          raidz1-0          ONLINE       0     0     0
            /dev/nvme1n1p1  ONLINE       0     0     0
            /dev/nvme2n1p1  ONLINE       0     0     0
            /dev/nvme0n1p1  ONLINE       0     0     0
errors: No known data errors
  pool: zpool_test
 state: ONLINE
status: One or more devices are configured to use a non-native block size.
        Expect reduced performance.
action: Replace affected devices with devices that support the
        configured block size, or migrate data to a properly configured
        pool.
config:
        NAME           STATE     READ WRITE CKSUM
        zpool_test     ONLINE       0     0     0
          mirror-0     ONLINE       0     0     0
            /dev/sdd1  ONLINE       0     0     0  block size: 4096B configured, 65536B native
            /dev/sdg1  ONLINE       0     0     0  block size: 4096B configured, 65536B native
errors: No known data errors
```
A zpool does not magically appear, it has to be *imported* to be known and usable by the system. Once a zpool is imported, the following happens:

1. All datasets, including the root dataset, are mounted in the VFS unless the `mountpoint` attribute tells otherwise (i.e. `mountpoint=none`). If the datasets are nested and have to be mounted in a hierarchical manner, ZFS handles this case automatically.
2. Block devices entries are created for zvolumes under /dev/zvol/\<poolname>

To search for and list all available (and not already imported) zpools in the system issue the command:

`root #````
zpool import
```
```
   pool: zpool_test
     id: 5283656651466890926
  state: ONLINE
status: One or more devices are configured to use a non-native block size.
        Expect reduced performance.
 action: The pool can be imported using its name or numeric identifier.
 config:
        zpool_test     ONLINE
          mirror-0     ONLINE
            /dev/sdd1  ONLINE
            /dev/sdg1  ONLINE
```
To effectively import a zpool just name it and ZFS will automatically probe all attached drives for it:

`root #````
zpool import zfs_test
```
or use the `-a` option to import all available zpools:

`root #````
zpool import -a
```
If all the vdevs composing a zpool are available, either in a clean or in a degraded (but acceptable) state, the import should complete successfully. In the case of a single vdev is missing, the zpool cannot be imported, thus has to be restored from a backup.

Time to demonstrate the famous ALTROOT invoked earlier: suppose *zfs\_test* is to be imported but with all of its mountable datasets relatively to /mnt/gentoo rather than the VFS root. This is achieved like this:

`root #````
zpool import -R /mnt/gentoo zpool_test
```
`root #````
zpool list
```
NAME         SIZE  ALLOC   FREE  CKPOINT  EXPANDSZ   FRAG    CAP  DEDUP    HEALTH  ALTROOT
rpool       5.45T   991G  4.49T        -         -     1%    17%  1.00x    ONLINE  -
zpool\_test  99.5G   948K  99.5G        -         -     0%     0%  1.00x    ONLINE  /mnt/gentoo

Confirmed by:

`root #````
mount | grep zpool_test
```
zpool\_test on /mnt/gentoo/zpool\_test type zfs (rw,xattr,noacl)

The reverse operation of importing a zpool is called: *exporting* the pool. If nothing uses the pool (i.e no open files on datasets and no zvolume in use), the zpool can be exported simply with:

`root #````
zpool export zpool_test
```
At this time, all mounted datasets are unmounted from the VFS and all block device entries corresponding to zvolumes are cleared from /dev.

#### Editing Zpools properties

A zpool has several attributes also named properties. While some can be changed and are "knobs" to govern aspects like data compression or encryption, some others are intrinsic and can only be read. Two options of the `zpool` command can be used to interact with properties:

- `zpool get propertyname zpoolname` to view `propertyname` (The special *all* property name allows to view everything)
- `zpool set propertyname=propertyvalue zpoolname` to set `propertyname` to `propertyvalue`

`root #````
zpool get all zpool_test
```
NAME        PROPERTY                       VALUE                          SOURCE
zpool\_test  size                           99.5G                          -
zpool\_test  capacity                       0%                             -
zpool\_test  altroot                        -                              default
zpool\_test  health                         ONLINE                         -
zpool\_test  guid                           14297858677399026207           -
zpool\_test  version                        -                              default
zpool\_test  bootfs                         -                              default
zpool\_test  delegation                     on                             default
zpool\_test  autoreplace                    off                            default
zpool\_test  cachefile                      -                              default
zpool\_test  failmode                       wait                           default
zpool\_test  listsnapshots                  off                            default
zpool\_test  autoexpand                     off                            default
zpool\_test  dedupratio                     1.00x                          -
zpool\_test  free                           99.5G                          -
zpool\_test  allocated                      6.56M                          -
zpool\_test  readonly                       off                            -
zpool\_test  ashift                         16                             local
zpool\_test  comment                        -                              default
zpool\_test  expandsize                     -                              -
zpool\_test  freeing                        0                              -
zpool\_test  fragmentation                  0%                             -
zpool\_test  leaked                         0                              -
zpool\_test  multihost                      off                            default
zpool\_test  checkpoint                     -                              -
zpool\_test  load\_guid                      6528179821128937184            -
zpool\_test  autotrim                       off                            default
zpool\_test  compatibility                  off                            default
zpool\_test  feature@async\_destroy          enabled                        local
zpool\_test  feature@empty\_bpobj            enabled                        local
zpool\_test  feature@lz4\_compress           active                         local
zpool\_test  feature@multi\_vdev\_crash\_dump  enabled                        local
zpool\_test  feature@spacemap\_histogram     active                         local
zpool\_test  feature@enabled\_txg            active                         local
zpool\_test  feature@hole\_birth             active                         local
zpool\_test  feature@extensible\_dataset     active                         local
zpool\_test  feature@embedded\_data          active                         local
zpool\_test  feature@bookmarks              enabled                        local
zpool\_test  feature@filesystem\_limits      enabled                        local
zpool\_test  feature@large\_blocks           enabled                        local
zpool\_test  feature@large\_dnode            enabled                        local
zpool\_test  feature@sha512                 enabled                        local
zpool\_test  feature@skein                  enabled                        local
zpool\_test  feature@edonr                  enabled                        local
zpool\_test  feature@userobj\_accounting     active                         local
zpool\_test  feature@encryption             enabled                        local
zpool\_test  feature@project\_quota          active                         local
zpool\_test  feature@device\_removal         enabled                        local
zpool\_test  feature@obsolete\_counts        enabled                        local
zpool\_test  feature@zpool\_checkpoint       enabled                        local
zpool\_test  feature@spacemap\_v2            active                         local
zpool\_test  feature@allocation\_classes     enabled                        local
zpool\_test  feature@resilver\_defer         enabled                        local
zpool\_test  feature@bookmark\_v2            enabled                        local
zpool\_test  feature@redaction\_bookmarks    enabled                        local
zpool\_test  feature@redacted\_datasets      enabled                        local
zpool\_test  feature@bookmark\_written       enabled                        local
zpool\_test  feature@log\_spacemap           active                         local
zpool\_test  feature@livelist               enabled                        local
zpool\_test  feature@device\_rebuild         enabled                        local
zpool\_test  feature@zstd\_compress          enabled                        local
zpool\_test  feature@draid                  enabled                        local

The output follows a tabular format where the last two columns are:

- `VALUE`:  The current value for the property. For a boolean property can be `on`/`off`, for zpool features can be `active` (enabled and *currently* in use), `enabled` (enabled but *currently* **not** in use) and `disabled` (disabled, not in use). Depending on the system version of OpenZFS and the enabled zpool features a zpool has, that latter can be imported or not. More explanation on this in a moment.
- `SOURCE`: mentions if the property value has been overridden (`local`) or if the default value is used (`default`). In the case a property cannot be changed and can only be read, a dash is displayed.

While not all will be described, something immediately catches the eye: a couple of properties have names starting with `feature@`. What is that? History<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>.

The legacy zpools version numbering schema has been abandoned in 2012 (shortly after Oracle discontinued Open Solaris and continued the development of ZFS behind closed-doors) and switched to *features flags* (the zpool version number was also set to 5000 and never changed since), hence the various `feature@` figuring in the command output above.

At the time the version numbering scheme was in use, when a zpool was created, the operating system tagged the zpool with the highest supported zpool version number<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup>. Those were numbered from 1  (ZFS initial release) to 49 which is supported by Solaris 11.4 SRU 51. As the zpool version number always increased between Solaris releases, so: a newer Solaris version can always import a zpool coming from an older Solaris version. There is however a catch: the zpool must have its version number upgraded (this is a manual operation) to get the benefits of the various news ZFS features and enhancements brought by a newer Solaris version. Once that zpool version number has been upgraded, older Solaris versions can no longer import the zpool.  E.g. a zpool created under Solaris 10 9/10 (supporting zpool versions up to 22) can be imported under Solaris 10 1/13 (supporting zpool versions up to 32) and even be imported again on Solaris 10 1/13 after that, at least as long as the zpool version number have not been upgraded under Solaris 10 1/13 (would tag the zpool at version 32).

The same remains true with the features flags: if the zpool has a feature flag enabled but not supported by the ZFS version in use on the system, the zpool cannot be imported. Likewise with the (legacy) version number system: to get the benefits of the new features flags provided by later OpenZFS/ZFS releases, the zpool must be upgraded (manual operation) to be able to support those new feature flags. Once this has been done, older OpenZFS/ZFS releases can no longer import the zpool.

If the zpool lies on SSD or NVMe drives, it might be convenient to enable the `autotrim` feature to let ZFS handles the "house keeping" on its own:

`root #````
zpool get autotrim zpool_test
```
NAME        PROPERTY  VALUE     SOURCE
zpool\_test  autotrim  off       default

`root #````
zpool set autotrim=on zpool_test
```
`root #````
zpool get autotrim zpool_test
```
NAME        PROPERTY  VALUE     SOURCE
zpool\_test  autotrim  on        local

Another useful feature is data compression. ZFS uses LZ4 by default and if switching to ZSTD is desired, a naïve approach would be:

`root #````
zpool set feature@zstd_compress=active zpool_test
```
cannot set property for 'zpool\_test': property 'feature@zstd\_compress' can only be set to 'enabled' or 'disabled

Indeed, even that property support is `enabled` on the zpool, activating it is not done by just setting the value that way but by changing the value of a property at a dataset level or a zvolume level via some "magic command". Once this "magic command" has been run, the ZSTD compression algorithm becomes effectively in use (`active`):

`root #````
zpool get feature@zstd_compress zpool_test
```
NAME        PROPERTY               VALUE                  SOURCE
zpool\_test  feature@zstd\_compress  active                 local

As invoked before, a zpool has to be *upgraded* to enable the usage of the new features provided by newer OpenZFS/ZFS versions (or whatever ZFS implementation used by the operating system). When an *upgrade* is due, `zpool status` will report this and invite to run `zpool upgrade` (the full command in this case is `zpool upgrade zpool_test`):

`root #````
zpool status zpool_test
```
```
  pool: zpool_test
 state: ONLINE
status: Some supported and requested features are not enabled on the pool.
        The pool can still be used, but some features are unavailable.
action: Enable all features using 'zpool upgrade'. Once this is done,
        the pool may no longer be accessible by software that does not support
        the features. See zpool-features(7) for details.
config:
(...)
```
Once the zpool upgrade has been done:

`root #````
zpool status zpool_test
```
pool: zpool\_test
 state: ONLINE
config:
(...)

To display the list of currently supported feature flags by the ZFS version in use by the operating system:

`root #``zpool upgrade -v````
This system supports ZFS pool feature flags.
 
The following features are supported:
 
FEAT DESCRIPTION
-------------------------------------------------------------
async_destroy                         (read-only compatible)
     Destroy filesystems asynchronously.
empty_bpobj                           (read-only compatible)
     Snapshots use less space.
lz4_compress
     LZ4 compression algorithm support.
multi_vdev_crash_dump
     Crash dumps to multiple vdev pools.
spacemap_histogram                    (read-only compatible)
     Spacemaps maintain space histograms.
enabled_txg                           (read-only compatible)
     Record txg at which a feature is enabled
hole_birth
     Retain hole birth txg for more precise zfs send
extensible_dataset
     Enhanced dataset functionality, used by other features.
embedded_data
     Blocks which compress very well use even less space.
bookmarks                             (read-only compatible)
     "zfs bookmark" command
filesystem_limits                     (read-only compatible)
     Filesystem and snapshot limits.
large_blocks
     Support for blocks larger than 128KB.
large_dnode
     Variable on-disk size of dnodes.
sha512
     SHA-512/256 hash algorithm.
skein
     Skein hash algorithm.
edonr
     Edon-R hash algorithm.
userobj_accounting                    (read-only compatible)
     User/Group object accounting.
encryption
     Support for dataset level encryption
project_quota                         (read-only compatible)
     space/object accounting based on project ID.
device_removal
     Top-level vdevs can be removed, reducing logical pool size.
obsolete_counts                       (read-only compatible)
     Reduce memory used by removed devices when the blocks are freed or remapped.
zpool_checkpoint                      (read-only compatible)
     Pool state can be checkpointed, allowing rewind later.
spacemap_v2                           (read-only compatible)
     Space maps representing large segments are more efficient.
allocation_classes                    (read-only compatible)
     Support for separate allocation classes.
resilver_defer                        (read-only compatible)
     Support for deferring new resilvers when one is already running.
bookmark_v2
     Support for larger bookmarks
redaction_bookmarks
     Support for bookmarks which store redaction lists for zfs redacted send/recv.
redacted_datasets
     Support for redacted datasets, produced by receiving a redacted zfs send stream.
bookmark_written
     Additional accounting, enabling the written#<bookmark> property(space written since a bookmark), and estimates of send stream sizes for incrementals from bookmarks.
log_spacemap                          (read-only compatible)
     Log metaslab changes on a single spacemap and flush them periodically.
livelist                              (read-only compatible)
     Improved clone deletion performance.
device_rebuild                        (read-only compatible)
     Support for sequential device rebuilds
zstd_compress
     zstd compression algorithm support.
draid
     Support for distributed spare RAID
 
The following legacy versions are also supported:
 
VER  DESCRIPTION
---  --------------------------------------------------------
 1   Initial ZFS version
 2   Ditto blocks (replicated metadata)
 3   Hot spares and double parity RAID-Z
 4   zpool history
 5   Compression using the gzip algorithm
 6   bootfs pool property
 7   Separate intent log devices
 8   Delegated administration
 9   refquota and refreservation properties
 10  Cache devices
 11  Improved scrub performance
 12  Snapshot properties
 13  snapused property
 14  passthrough-x aclinherit
 15  user/group space accounting
 16  stmf property support
 17  Triple-parity RAID-Z
 18  Snapshot user holds
 19  Log device removal
 20  Compression using zle (zero-length encoding)
 21  Deduplication
 22  Received properties
 23  Slim ZIL
 24  System attributes
 25  Improved scrub stats
 26  Improved snapshot deletion performance
 27  Improved snapshot creation performance
 28  Multiple vdev replacements
```
Hardware is not perfect and sometimes a drive can fail for some reason or the data on it can become corrupt. ZFS has countermeasures to detect corruption and prevent even double-flipped bits errors. If the underlying vdevs are redundant, damaged data can be repaired as if nothing had ever happened.

Remember that zpools are striped across all of the vdev and two scenarios can happen when a device fails:

- Either enough redundancy exists and the zpool is still available but under a "DEGRADED" state
- Either there is not enough redundancy exists and the zpool becomes "SUSPENDED" (at this point data must be restored from a backup)

A `zpool status` reports what the situation is:

`root #````
zpool status zpool_test
```
```
  pool: zpool_test
 state: DEGRADED
status: One or more devices could not be used because the label is missing or
        invalid.  Sufficient replicas exist for the pool to continue
        functioning in a degraded state.
action: Replace the device using 'zpool replace'.
   see: https://openzfs.github.io/openzfs-docs/msg/ZFS-8000-4J
config:
        NAME           STATE     READ WRITE CKSUM
        zpool_test     DEGRADED     0     0     0
          mirror-0     DEGRADED     0     0     0
            /dev/sde1  UNAVAIL      3    80     0
            /dev/sdh1  ONLINE       0     0     0
```
The defective drive (/dev/sde) has to be physically replaced by another one (here /dev/sdg). That task is accomplished by first off-lining the failed drive from the zpool:

`root #````
zpool offline zpool_test /dev/sde
```
`root #````
zpool status zpool_test
```
```
  pool: zpool_test
 state: DEGRADED
status: One or more devices has experienced an unrecoverable error.  An
        attempt was made to correct the error.  Applications are unaffected.
action: Determine if the device needs to be replaced, and clear the errors
        using 'zpool clear' or replace the device with 'zpool replace'.
   see: https://openzfs.github.io/openzfs-docs/msg/ZFS-8000-9P
config:
        NAME           STATE     READ WRITE CKSUM
        zpool_test     DEGRADED     0     0     0
          mirror-0     DEGRADED     0     0     0
            /dev/sde1  OFFLINE      3    80     0
            /dev/sdh1  ONLINE       0     0     0
errors: No known data errors
```
Then proceeding with the replacement (ZFS will partition the new drive automatically) :

`root #````
zpool replace zpool_test /dev/sde /dev/sdg
```
`root #````
zpool status zpool_test
```
```
  pool: zpool_test
 state: ONLINE
  scan: resilvered 28.9M in 00:00:01 with 0 errors on Sat May  6 11:12:55 2023
config:
        NAME           STATE     READ WRITE CKSUM
        zpool_test     ONLINE       0     0     0
          mirror-0     ONLINE       0     0     0
            /dev/sdg1  ONLINE       0     0     0
            /dev/sdh1  ONLINE       0     0     0
```
Once the drive has been replaced, ZFS starts a rebuilding process in background known as *resilvering* which can take several hours or even days depending on the amount of data stored on the zpool. Here the amount is very small so the revilvering process just takes a couple of seconds. On large zpools the resilvering process duration can even be prohibitive and this is where draid kicks-in (a totally different beast than raidz), more on this later.

Rather than being a manual operation, what if the zpool can have hot-spare drives that can automatically replace the failed ones? This is exactly the reason of having a special vdev known as... spare. There are two ways of having spare vdevs in a zpool: whenever the zpool is created or added on the fly to an existing zpool. Replacement hot-spare drives should be identical to faulty drives.

To add a spare vdev, simply add it by issuing a `zpool add` command (here again, ZFS will partition the hot-spare drive on its own). Once added to a zpool, the hot-spare remains in stand-by until a drive failure happens:

`root #````
zpool add zpool_test spare /dev/sdd
```
`root #````
zpool status zpool_test
```
```
  pool: zpool_test
 state: ONLINE
  scan: resilvered 28.9M in 00:00:01 with 0 errors on Sat May  6 11:12:55 2023
config:
        NAME           STATE     READ WRITE CKSUM
        zpool_test     ONLINE       0     0     0
          mirror-0     ONLINE       0     0     0
            /dev/sdg1  ONLINE       0     0     0
            /dev/sdh1  ONLINE       0     0     0
        spares
          /dev/sdd1    AVAIL   
errors: No known data errors
```
At this point the hot-spare is available (`AVAIL`). If for some reason /dev/sdh (or /dev/sdg would fail, /dev/sdd will immediately take over the failed drive (so it becomes `INUSE`) thus triggering a zpool resilvering in background with no downtime for the zpool:

`root #````
zpool status
```
```
  pool: zpool_test
 state: DEGRADED
status: One or more devices could not be used because the label is missing or
        invalid.  Sufficient replicas exist for the pool to continue
        functioning in a degraded state.
action: Replace the device using 'zpool replace'.
   see: https://openzfs.github.io/openzfs-docs/msg/ZFS-8000-4J
  scan: resilvered 40.7M in 00:00:01 with 0 errors on Sat May  6 11:39:28 2023
config:
        NAME             STATE     READ WRITE CKSUM
        zpool_test       DEGRADED     0     0     0
          mirror-0       DEGRADED     0     0     0
            spare-0      DEGRADED     0     0     0
              /dev/sdg1  UNAVAIL      3    83     0
              /dev/sdd1  ONLINE       0     0     0
            /dev/sdh1    ONLINE       0     0     0
        spares
          /dev/sdd1      INUSE     currently in use
```
The original drive will automatically get removed asynchronously. If this is not the case, it may need to be manually detached with the `zpool detach` command:

`root #````
zpool detach zpool_test /dev/sdg
```
`root #````
zpool status
```
```
  pool: zpool_test
 state: ONLINE
  scan: resilvered 40.7M in 00:00:01 with 0 errors on Sat May  6 11:39:28 2023
config:
        NAME           STATE     READ WRITE CKSUM
        zpool_test     ONLINE       0     0     0
          mirror-0     ONLINE       0     0     0
            /dev/sdd1  ONLINE       0     0     0
            /dev/sdh1  ONLINE       0     0     0
errors: No known data errors
```
Having a mirrored vdev under the hand in the present example: it is also possible to add a third actively used drive to it rather than using a hot-spare drive. This is accomplished by attaching the new drive (`zpool attach`) to one of the existing drives composing the mirrored vdev:

`root #````
zpool attach zpool_test /dev/sdh /dev/sdg
```
`root #````
zpool status zpool_test
```
```
  pool: zpool_test
 state: ONLINE
  scan: resilvered 54.4M in 00:00:00 with 0 errors on Sat May  6 11:55:42 2023
config:
        NAME           STATE     READ WRITE CKSUM
        zpool_test     ONLINE       0     0     0
          mirror-0     ONLINE       0     0     0
            /dev/sdd1  ONLINE       0     0     0
            /dev/sdh1  ONLINE       0     0     0
            /dev/sdg1  ONLINE       0     0     0
errors: No known data errors
```
It is a good practice to periodically *scrub* a zpool to ensure what it contains is "healthy" (if something is corrupt and if the zpool have enough redundancy the damage will be repaired). To start a manual scrub:

`root #````
zpool scrub zpool_test
```
`root #````
zpool status zpool_test
```
```
  pool: zpool_test
 state: ONLINE
  scan: scrub repaired 0B in 00:00:01 with 0 errors on Sat May  6 12:06:18 2023
config:
        NAME           STATE     READ WRITE CKSUM
        zpool_test     ONLINE       0     0     0
          mirror-0     ONLINE       0     0     0
            /dev/sdd1  ONLINE       0     0     0
            /dev/sdh1  ONLINE       0     0     0
            /dev/sdg1  ONLINE       0     0     0
errors: No known data errors
```
In the case of something corrupt is detected the output of the above command would be:

`root #````
zpool scrub zpool_test
```
`root #````
zpool status zpool_test
```
```
  pool: zpool_test
 state: ONLINE
status: One or more devices has experienced an unrecoverable error.  An
        attempt was made to correct the error.  Applications are unaffected.
action: Determine if the device needs to be replaced, and clear the errors
        using 'zpool clear' or replace the device with 'zpool replace'.
   see: https://openzfs.github.io/openzfs-docs/msg/ZFS-8000-9P
  scan: scrub repaired 256K in 00:00:00 with 0 errors on Sat May  6 12:09:22 2023
config:
        NAME           STATE     READ WRITE CKSUM
        zpool_test     ONLINE       0     0     0
          mirror-0     ONLINE       0     0     0
            /dev/sdd1  ONLINE       0     0     4
            /dev/sdh1  ONLINE       0     0     0
            /dev/sdg1  ONLINE       0     0     0
errors: No known data errors
```
Once the damage has been repaired, issue a `zpool clear` command as suggested:

`root #````
zpool clear zpool_test
```
`root #````
zpool status zpool_test
```
```
  pool: zpool_test
 state: ONLINE
  scan: scrub repaired 256K in 00:00:00 with 0 errors on Sat May  6 12:09:22 2023
config:
        NAME           STATE     READ WRITE CKSUM
        zpool_test     ONLINE       0     0     0
          mirror-0     ONLINE       0     0     0
            /dev/sdd1  ONLINE       0     0     0
            /dev/sdh1  ONLINE       0     0     0
            /dev/sdg1  ONLINE       0     0     0
errors: No known data errors
```
Speaking about three-faced mirrors, having only one of the three drives healthy still allows a functional zpool:

`root #````
zpool scrub zpool_test
```
`root #````
zpool status zpool_test
```
```
  pool: zpool_test
 state: ONLINE
status: One or more devices has experienced an unrecoverable error.  An
        attempt was made to correct the error.  Applications are unaffected.
action: Determine if the device needs to be replaced, and clear the errors
        using 'zpool clear' or replace the device with 'zpool replace'.
   see: https://openzfs.github.io/openzfs-docs/msg/ZFS-8000-9P
  scan: scrub in progress since Sat May  6 13:43:09 2023
        5.47G scanned at 510M/s, 466M issued at 42.3M/s, 5.47G total
        0B repaired, 8.31% done, 00:02:01 to go
config:
        NAME           STATE     READ WRITE CKSUM
        zpool_test     ONLINE       0     0     0
          mirror-0     ONLINE       0     0     0
            /dev/sdd1  ONLINE       0     0     0
            /dev/sdh1  ONLINE       0     0 68.9K
            /dev/sdg1  ONLINE       0     0 68.9K
errors: No known data errors
```
Once a zpool is no longer needed, it can be destroyed with `zpool destroy`. If the zpool is not empty and contains zvolumes and/or datasets, the `-r` option is required. There is no need to export the zpool first as ZFS will  handles this automatically.

If the zpool has just been destroyed and all of its vdevs are still intact, it is possible to re-import it by issuing a `zpool import` this time using the option `-D`:

`root #````
zpool destroy zpool_test
```
`root #````
zpool import -D
```
```
   pool: zpool_test
     id: 628141733996329543
  state: ONLINE (DESTROYED)
 action: The pool can be imported using its name or numeric identifier.
 config:
        zpool_test     ONLINE
          mirror-0     ONLINE
            /dev/sdd1  ONLINE
            /dev/sdh1  ONLINE
            /dev/sdg1  ONLINE
```
Importing the destroyed zpool that way also "undestroy" it:

`root #````
zpool list zpool_test
```
cannot open 'zpool\_test': no such pool

`root #````
zpool import -D zpool_test
```
`root #````
zpool list zpool_test
```
NAME         SIZE  ALLOC   FREE  CKPOINT  EXPANDSZ   FRAG    CAP  DEDUP    HEALTH  ALTROOT
zpool\_test  99.5G  5.48G  94.0G        -         -     0%     5%  1.00x    ONLINE  -

Sometimes it is extremely useful to get the list of what happened in a zpool life and here is where `zpool history` comes at rescue. As `zpool_test` is brand new and so has nothing really interesting to show, let's take an example of what is going on with a real zpool used in production on workstation recently restored from remote TrueNAS backups:

`root #````
zpool history rpool
```
History for 'rpool':
2023-05-02.19:24:36 zpool create rpool raidz /dev/nvme0n1 /dev/nvme1n1 /dev/nvme2n1
2023-05-02.19:27:32 zfs create -o mountpoint=none rpool/ROOT
2023-05-02.19:27:36 zfs create -o mountpoint=none rpool/HOME
2023-05-02.19:33:59 zfs receive -vu rpool/ROOT/gentoo-gcc13-avx512
2023-05-02.19:39:57 zfs receive -vu rpool/HOME/user111
2023-05-02.20:05:52 zpool set autotrim=on rpool
2023-05-02.20:08:12 zfs destroy -r rpool/HOME/user111
2023-05-02.20:33:27 zfs receive -vu rpool/HOME/user1
2023-05-02.20:35:12 zpool import -a
2023-05-02.20:58:19 zpool scrub rpool
2023-05-02.21:22:25 zpool import -N rpool
2023-05-02.21:23:18 zpool set cachefile=/etc/zfs/zpool.cache rpool
2023-05-03.07:02:25 zfs snap rpool/ROOT/gentoo-gcc13-avx512@20230503
2023-05-03.07:02:36 zfs snap rpool/HOME/user1@20230503
2023-05-03.07:04:16 zfs send -I rpool/HOME/user1@20230430 rpool/HOME/user1@20230502
2023-05-07.16:10:15 zpool import -N -c /etc/zfs/zpool.cache rpool
(...)

It is possible to have the same information in full-format (who/what/where/when):

`root #````
zpool history -l rpool
```
History for 'rpool':
2023-05-02.19:24:36 zpool create rpool raidz /dev/nvme0n1 /dev/nvme1n1 /dev/nvme2n1 \[user 0 (root) on livecd:linux\]
2023-05-02.19:27:32 zfs create -o mountpoint=none rpool/ROOT \[user 0 (root) on livecd:linux\]
2023-05-02.19:27:36 zfs create -o mountpoint=none rpool/HOME \[user 0 (root) on livecd:linux\]
2023-05-02.19:33:59 zfs receive -vu rpool/ROOT/gentoo-gcc13-avx512 \[user 0 (root) on livecd:linux\]
2023-05-02.19:39:57 zfs receive -vu rpool/HOME/user111\[user 0 (root) on livecd:linux\]
(...)
2023-05-02.20:35:12 zpool import -a \[user 0 (root) on wks.myhomenet.lan:linux\]

Note that:

1. The zpool as been created using a Linux live media environment (in that case the Gentoo LiveCD) and that environment had set

`livecd` has being the hostname at that time

1. Two placeholders (non-mountable) datasets named `rpool/ROOT` and `rpool/HOME` have been created by the user `root` under that live  media environment
2. The dataset `rpool/ROOT/gentoo-gcc13-avx512` (obviously the Gentoo system) has been restored from a remote snapshot by the user `root` under that live  media environment
3. The dataset `rpool/HOME/user111` (obviously a user home directory) has been restored from a remote snapshot by the user `root` under that live  media environment
4. At some point, the workstation rebooted under the restored Gentoo environment (notice the hostname change for `wks.myhomenet.lan`) that imported the zpool
5. and so on.

- It is sometimes not possible to shrink a zpool after initial creation - **if** the pool has no raidz vdevs and the pool has all vdevs of the same ashift, the "device removal" feature in 0.8 and above can be used. There are performance implications to doing this, however, so **always be careful** when creating pools or adding vdevs/disks!
- It is possible to add more disks to a MIRROR after its initial creation. Use the following command (/dev/loop0 is the first drive in the MIRROR):

`root #``zpool attach zfs_test /dev/loop0 /dev/loop2`
- Sometimes, a wider RAIDZ vdev can be less suitable than two (or more) smaller RAIDZ vdevs. Try testing the intended use before settling on one and moving all data entirely.
- RAIDZ vdevs currently can not be resized after initial creation. Only additional hot spares may be added. However, the hard drives can be replaced with bigger ones (one at a time), e.g. replace 1T drives with 2T drives to double the available space in the zpool.
- It is possible to mix MIRROR and RAIDZ vdevs in a zpool. For example to add two more disks as a MIRROR vdev in a zpool with a RAIDZ1 vdev named zfs\_test, use:

- `root #``zpool add -f zfs_test mirror /dev/loop4 /dev/loop5`

It is possible to import zpools coming from other platforms even of a different bit ordering as long as:

- the version is 28 (legacy) or earlier
- The ZFS implementation used support at least the features indicated by the zpools in the case of a zpool at version 5000.

In the following example, a zpool named *oldpool* had been created on an emulated FreeBSD 10/SPARC64 environment emulated with [app-emulation/qemu](https://packages.gentoo.org/packages/app-emulation/qemu). The zpool had been created with legacy version 28 (`zpool create -o version=28`) which is basically what a SPARC64 machine running Solaris 10 would also have done:

`root #````
zpool import
```
```
   pool: oldpool
     id: 11727962949360948576
  state: ONLINE
status: The pool is formatted using a legacy on-disk version.
 action: The pool can be imported using its name or numeric identifier, though
        some features will not be available without an explicit 'zpool upgrade'.
 config:
        oldpool         ONLINE
          raidz1-0      ONLINE
            /dev/loop0  ONLINE
            /dev/loop1  ONLINE
            /dev/loop2  ONLINE
```
`root #````
zpool import -f oldpool
```
`root #````
zpool status oldpool
```
```
  pool: oldpool
 state: ONLINE
status: The pool is formatted using a legacy on-disk format.  The pool can
        still be used, but some features are unavailable.
action: Upgrade the pool using 'zpool upgrade'.  Once this is done, the
        pool will no longer be accessible on software that does not support
        feature flags.
config:
        NAME            STATE     READ WRITE CKSUM
        oldpool         ONLINE       0     0     0
          raidz1-0      ONLINE       0     0     0
            /dev/loop0  ONLINE       0     0     0
            /dev/loop1  ONLINE       0     0     0
            /dev/loop2  ONLINE       0     0     0
errors: No known data errors
```
Notice the values of all *feature@* (all are `disabled`) and *version* (`28`) properties:

`root #````
zpool get all oldpool
```
NAME     PROPERTY                       VALUE                          SOURCE
oldpool  size                           11.9G                          -
oldpool  capacity                       0%                             -
oldpool  altroot                        -                              default
oldpool  health                         ONLINE                         -
oldpool  guid                           11727962949360948576           -
oldpool  version                        28                             local
oldpool  bootfs                         -                              default
oldpool  delegation                     on                             default
oldpool  autoreplace                    off                            default
oldpool  cachefile                      -                              default
oldpool  failmode                       wait                           default
oldpool  listsnapshots                  off                            default
oldpool  autoexpand                     off                            default
oldpool  dedupratio                     1.00x                          -
oldpool  free                           11.9G                          -
oldpool  allocated                      88.8M                          -
oldpool  readonly                       off                            -
oldpool  ashift                         0                              default
oldpool  comment                        -                              default
oldpool  expandsize                     -                              -
oldpool  freeing                        0                              -
oldpool  fragmentation                  -                              -
oldpool  leaked                         0                              -
oldpool  multihost                      off                            default
oldpool  checkpoint                     -                              -
oldpool  load\_guid                      11399183722859374527           -
oldpool  autotrim                       off                            default
oldpool  compatibility                  off                            default
oldpool  feature@async\_destroy          disabled                       local
oldpool  feature@empty\_bpobj            disabled                       local
oldpool  feature@lz4\_compress           disabled                       local
oldpool  feature@multi\_vdev\_crash\_dump  disabled                       local
oldpool  feature@spacemap\_histogram     disabled                       local
oldpool  feature@enabled\_txg            disabled                       local
oldpool  feature@hole\_birth             disabled                       local
oldpool  feature@extensible\_dataset     disabled                       local
oldpool  feature@embedded\_data          disabled                       local
oldpool  feature@bookmarks              disabled                       local
oldpool  feature@filesystem\_limits      disabled                       local
oldpool  feature@large\_blocks           disabled                       local
oldpool  feature@large\_dnode            disabled                       local
oldpool  feature@sha512                 disabled                       local
oldpool  feature@skein                  disabled                       local
oldpool  feature@edonr                  disabled                       local
oldpool  feature@userobj\_accounting     disabled                       local
oldpool  feature@encryption             disabled                       local
oldpool  feature@project\_quota          disabled                       local
oldpool  feature@device\_removal         disabled                       local
oldpool  feature@obsolete\_counts        disabled                       local
oldpool  feature@zpool\_checkpoint       disabled                       local
oldpool  feature@spacemap\_v2            disabled                       local
oldpool  feature@allocation\_classes     disabled                       local
oldpool  feature@resilver\_defer         disabled                       local
oldpool  feature@bookmark\_v2            disabled                       local
oldpool  feature@redaction\_bookmarks    disabled                       local
oldpool  feature@redacted\_datasets      disabled                       local
oldpool  feature@bookmark\_written       disabled                       local
oldpool  feature@log\_spacemap           disabled                       local
oldpool  feature@livelist               disabled                       local
oldpool  feature@device\_rebuild         disabled                       local
oldpool  feature@zstd\_compress          disabled                       local
oldpool  feature@draid                  disabled                       local

To have this zpool being able to use the very latest features of OpenZFS/ZFS or the ZFS implementation used on the system, upgrade the zpool like explained in the above sections:

`root #````
zpool upgrade oldpool
```
This system supports ZFS pool feature flags.
 
Successfully upgraded 'oldpool' from version 28 to feature flags.
Enabled the following features on 'oldpool':
  async\_destroy
  empty\_bpobj
  lz4\_compress
  multi\_vdev\_crash\_dump
  spacemap\_histogram
  enabled\_txg
  hole\_birth
  extensible\_dataset
  embedded\_data
  bookmarks
  filesystem\_limits
  large\_blocks
  large\_dnode
  sha512
  skein
  edonr
  userobj\_accounting
  encryption
  project\_quota
  device\_removal
  obsolete\_counts
  zpool\_checkpoint
  spacemap\_v2
  allocation\_classes
  resilver\_defer
  bookmark\_v2
  redaction\_bookmarks
  redacted\_datasets
  bookmark\_written
  log\_spacemap
  livelist
  device\_rebuild
  zstd\_compress
  draid

As a test, scrub the pool:

`root #````
zpool scrub oldpool
```
`root #````
zpool status oldpool
```
```
  pool: oldpool
 state: ONLINE
  scan: scrub repaired 0B in 00:00:00 with 0 errors on Sun May  7 16:35:57 2023
config:
 
        NAME            STATE     READ WRITE CKSUM
        oldpool         ONLINE       0     0     0
          raidz1-0      ONLINE       0     0     0
            /dev/loop0  ONLINE       0     0     0
            /dev/loop1  ONLINE       0     0     0
            /dev/loop2  ONLINE       0     0     0
 
errors: No known data errors
```
What about the contained data?

`root #````
tar -tvf /oldpool/boot-fbsd10-sparc64.tar 
```
drwxr-xr-x root/wheel        0 2017-09-29 04:09 boot/
-r--r--r-- root/wheel     3557 2017-09-29 04:09 boot/beastie.4th
-r--r--r-- root/wheel     8192 2017-09-29 04:09 boot/boot1
-r--r--r-- root/wheel     2809 2017-09-29 04:09 boot/brand.4th
-r--r--r-- root/wheel     2129 2017-09-29 04:09 boot/brand-fbsd.4th
-r--r--r-- root/wheel     6208 2017-09-29 04:09 boot/check-password.4th
-r--r--r-- root/wheel     1870 2017-09-29 04:09 boot/color.4th
drwxr-xr-x root/wheel        0 2017-09-29 04:09 boot/defaults/
-r--r--r-- root/wheel     4056 2017-09-29 04:09 boot/delay.4th
-r--r--r-- root/wheel       89 2017-09-29 04:09 boot/device.hints
drwxr-xr-x root/wheel        0 2017-09-29 04:09 boot/dtb/
drwxr-xr-x root/wheel        0 2017-09-29 04:09 boot/firmware/
-r--r--r-- root/wheel     4179 2017-09-29 04:09 boot/frames.4th
drwxr-xr-x root/wheel        0 2017-09-29 04:09 boot/kernel/
-r--r--r-- root/wheel     6821 2017-09-29 04:09 boot/loader.4th
-r-xr-xr-x root/wheel   211768 2017-09-29 04:09 boot/loader
(...)

A zpool is perfectly importable even if coming for a totally different world (SPARC64 is a MSB bit-ordering architecture whereas ARM/X86\_64 are LSB bit-ordering architectures). To clean everything up (as created in the second part of this section hereafter):

`root #````
zpool export oldpool
```
`root #````
losetup -D
```
`root #````
rm /tmp/fbsd-sparc64-ada{0,1,2}.raw
```
Now the zpool has been upgraded, what happens if an import is attempted on the older FreeBSD 10/SPARC64 environment?

`root #``zpool import -o altroot=/tmp -f oldpool````
This pool uses the following feature(s) not supported by this system:
        com.delphix:spacemap_v2 (Space maps representing large segments are more efficient.)
        com.delphix:log_spacemap (Log metaslab changes on a single spacemap and flush them periodically.)
All unsupported features are only required for writing to the pool.
The pool can be imported using '-o readonly=on'.
```
`root #``zpool import -o readonly=on -o altroot=/tmp -f oldpool`
NAME      SIZE  ALLOC   FREE  EXPANDSZ   FRAG    CAP  DEDUP  HEALTH  ALTROOT
oldpool  11.9G  88.7M  11.9G         -     0%     0%  1.00x  ONLINE  /tmp

The system sees it but refuses to import it a read/write state. Issuing `zpool status` or `zpool list` command will not show if the zpool but a `zpool get all` will, not obviously, give a hint:

`root #``zpool get all`
NAME     PROPERTY                                       VALUE                                          SOURCE
oldpool  size                                           11.9G                                          -
oldpool  capacity                                       0%                                             -
oldpool  altroot                                        /tmp                                           local
oldpool  health                                         ONLINE                                         -
oldpool  guid                                           11727962949360948576                           default
oldpool  version                                        -                                              default
oldpool  bootfs                                         -                                              default
oldpool  delegation                                     on                                             default
oldpool  autoreplace                                    off                                            default
oldpool  cachefile                                      none                                           local
oldpool  failmode                                       wait                                           default
oldpool  listsnapshots                                  off                                            default
oldpool  autoexpand                                     off                                            default
oldpool  dedupditto                                     0                                              default
oldpool  dedupratio                                     1.00x                                          -
oldpool  free                                           11.9G                                          -
oldpool  allocated                                      88.7M                                          -
oldpool  readonly                                       on                                             -
oldpool  comment                                        -                                              default
oldpool  expandsize                                     -                                              -
oldpool  freeing                                        0                                              default
oldpool  fragmentation                                  0%                                             -
oldpool  leaked                                         0                                              default
oldpool  feature@async\_destroy                          enabled                                        local
oldpool  feature@empty\_bpobj                            enabled                                        local
oldpool  feature@lz4\_compress                           active                                         local
oldpool  feature@multi\_vdev\_crash\_dump                  enabled                                        local
oldpool  feature@spacemap\_histogram                     active                                         local
oldpool  feature@enabled\_txg                            active                                         local
(...)
oldpool  unsupported@com.delphix:zpool\_checkpoint       inactive                                       local
oldpool  unsupported@com.delphix:spacemap\_v2            readonly                                       local
oldpool  unsupported@com.delphix:log\_spacemap           readonly                                       local
(...)

Notice the value of properties `unsupported@com.delphix:spacemap_v2` and `unsupported@com.delphix:log_spacemap`. Also notice that all unsupported features have names starting with `unsupported@`.

For the completeness here is what happens when an attempt to import a totally incompatible zpool. The following example shows what happens when a Linux system (OpenZFS 2.1.x) attempts to import a zpool created under Solaris 11.4 (Solaris still uses the legacy version scheme and tagged the zpool at version 42).

`root #````
zpool import -f rpool
```
cannot import 'rpool': pool is formatted using an unsupported ZFS version

To create the zpool `oldpool` used above:

- Install [app-emulation/qemu](https://packages.gentoo.org/packages/app-emulation/qemu) (QEMU 8.0 was used) ensuring the variable `QEMU_SOFTMMU_TARGETS` contains at least `sparc64` build the "full" SPARC64 system emulator: `root #``emerge --ask app-emulation/qemu`
- Download a FreeBSD 10/SPARC64 DVD ISO image from one of the mirrors (FreeBSD 9 crashes at boot under QEMU, so FreeBSD 10 is the minimum knowing that FreeBSD 12 has some issues while loading the required modules)
- Create some images (here three of 4G each) for the future vdevs: `for i in {0..2}; do qemu-img create -f raw /tmp/fbsd-sparc64-ada${i}.raw 4G; done;`
- Fire QEmu up (using `-nographic` to not have FreeBSD hangs, also Sun4u machines have 2 IDE channels so 3 hard drives + 1 ATAPI CD/DVD ROM drive):

`root #````
qemu-system-sparc64 -nographic -m 2G -drive format=raw,file=/tmp/fbsd-sparc64-ada0.raw,media=disk,index=0 -drive format=raw,file=/tmp/fbsd-sparc64-ada1.raw,media=disk,index=1 -drive format=raw,file=/tmp/fbsd-sparc64-ada2.raw,media=disk,index=2 -drive format=raw,file=/tmp/FreeBSD-10.4-RELEASE-sparc64-dvd1.iso,media=cdrom,readonly=on 
```
OpenBIOS for Sparc64
Configuration device id QEMU version 1 machine id 0
kernel cmdline 
CPUs: 1 x SUNW,UltraSPARC-IIi
UUID: 00000000-0000-0000-0000-000000000000
Welcome to OpenBIOS v1.1 built on Mar 7 2023 22:22
  Type 'help' for detailed information
Trying disk:a...
No valid state has been set by load or init-program
0 >

- At the OpenBIOS prompt `0 >` enter: `boot cdrom:f`
- Now FreeBSD should boot, as emulation is slow it can take awhile, let the boot process complete and check if all the virtual disks (ada*X*) are mentioned:

```
Not a bootable ELF image
Loading a.out image...
Loaded 7680 bytes
entry point is 0x4000
 
>> FreeBSD/sparc64 boot block
   Boot path:   /pci@1fe,0/pci@1,1/ide@3/ide@1/cdrom@1:f
   Boot loader: /boot/loader
Consoles: Open Firmware console  
FreeBSD/sparc64 bootstrap loader, Revision 1.0
(root@releng1.nyi.freebsd.org, Fri Sep 29 07:57:55 UTC 2017)
bootpath="/pci@1fe,0/pci@1,1/ide@3/ide@1/cdrom@1:a"
Loading /boot/defaults/loader.conf
/boot/kernel/kernel data=0xc25140+0xe62a0 syms=[0x8+0xcd2c0+0x8+0xbf738]
Hit [Enter] to boot immediately, or any other key for command prompt.
Booting [/boot/kernel/kernel]...               
jumping to kernel entry at 0xc00a8000.
Copyright (c) 1992-2017 The FreeBSD Project.
Copyright (c) 1979, 1980, 1983, 1986, 1988, 1989, 1991, 1992, 1993, 1994
        The Regents of the University of California. All rights reserved.
FreeBSD is a registered trademark of The FreeBSD Foundation.
FreeBSD 10.4-RELEASE #0 r324094: Fri Sep 29 08:00:33 UTC 2017
    root@releng1.nyi.freebsd.org:/usr/obj/sparc64.sparc64/usr/src/sys/GENERIC sparc64
(...)
ada0 at ata2 bus 0 scbus0 target 0 lun 0
ada0: <QEMU HARDDISK 2.5+> ATA-7 device
ada0: Serial Number QM00001
ada0: 33.300MB/s transfers (UDMA2, PIO 8192bytes)
ada0: 4096MB (8388608 512 byte sectors)
ada0: Previously was known as ad0
cd0 at ata3 bus 0 scbus1 target 1 lun 0
cd0: <QEMU QEMU DVD-ROM 2.5+> Removable CD-ROM SCSI device
cd0: Serial Number QM00004
cd0: 33.300MB/s transfers (UDMA2, ATAPI 12bytes, PIO 65534bytes)
cd0: 2673MB (1368640 2048 byte sectors)
ada1 at ata2 bus 0 scbus0 target 1 lun 0
ada1: <QEMU HARDDISK 2.5+> ATA-7 device
ada1: Serial Number QM00002
ada1: 33.300MB/s transfers (UDMA2, PIO 8192bytes)
ada1: 4096MB (8388608 512 byte sectors)
ada1: Previously was known as ad1
ada2 at ata3 bus 0 scbus1 target 0 lun 0
ada2: <QEMU HARDDISK 2.5+> ATA-7 device
ada2: Serial Number QM00003
ada2: 33.300MB/s transfers (UDMA2, PIO 8192bytes)
ada2: 4096MB (8388608 512 byte sectors)
ada2: Previously was known as ad2
(...)
Console type [vt100]:  
```
- When the kernel has finished to boot, FreeBSD asks the console type, just press `ENTER` to accept the default choice (VT100)
- The FreeBSD installer is spawned, use the `TAB` key twice to highlight `LiveCD` and press the `ENTER` key to exit the FreeBSD installer and reach a login prompt
- at the login prompt, enter `root` (no password) *et voila!* a BASH shell prompt
- Issue the command `kldload opensolaris` *THEN* `kldload zfs` (some warnings can be ignored as `kldstat` will show what was loaded)

```
root@:~ # kldload opensolaris
root@:~ # kldload zfs
ZFS NOTICE: Prefetch is disabled by default if less than 4GB of RAM is present;
            to enable, add "vfs.zfs.prefetch_disable=0" to /boot/loader.conf.
ZFS filesystem version: 5
ZFS storage pool version: features support (5000)
root@:~ # kldstat
Id Refs Address            Size     Name
 1   10 0xc0000000 e97de8   kernel
 2    2 0xc186e000 106000   opensolaris.ko
 3    1 0xc1974000 3f4000   zfs.ko
```
- Create a zpool with a legacy version scheme (version at 28) named `oldpool` by issuing `zpool create -o altroot=/tmp  -o version=28 oldpool raidz /dev/ada0 /dev/ada1 /dev/ada2`
- Checking the zpool status should report `oldpool` as being `ONLINE` (ZFS should also complain that the legacy format is used, don't upgrade yet!):

```
root@:~ # zpool status
  pool: oldpool
 state: ONLINE
status: The pool is formatted using a legacy on-disk format.  The pool can
        still be used, but some features are unavailable.
action: Upgrade the pool using 'zpool upgrade'.  Once this is done, the
        pool will no longer be accessible on software that does not support feature
        flags.
  scan: none requested
config:
        NAME        STATE     READ WRITE CKSUM
        oldpool     ONLINE       0     0     0
          raidz1-0  ONLINE       0     0     0
            ada0    ONLINE       0     0     0
            ada1    ONLINE       0     0     0
            ada2    ONLINE       0     0     0
errors: No known data errors
```
- As the zpool has been (temporarily) mounted under /tmp/oldpool, copy some data on it by issuing a command like `tar cvf /tmp/oldpool/boot-fbsd10-sparc64.tar /boot`
- Export the zpool: `zpool export oldpool`
- Press `Ctrl`+`A X` to terminate QEMU (no need of a graceful shutdown, this is a live environment) and return to the Gentoo host shell
- The last step is to map some loop devices to the raw images:

`root #````
for i in {0..2}; do losetup -f /tmp/fbsd-sparc64-ada${i}.raw; done;
```
`root #````
losetup -l
```
NAME       SIZELIMIT OFFSET AUTOCLEAR RO BACK-FILE                  DIO LOG-SEC
/dev/loop1         0      0         0  0 /tmp/fbsd-sparc64-ada1.raw   0     512
/dev/loop2         0      0         0  0 /tmp/fbsd-sparc64-ada2.raw   0     512
/dev/loop0         0      0         0  0 /tmp/fbsd-sparc64-ada0.raw   0     512

#### Concepts

Just like /usr/sbin/zpool is used to manage zpools, the second ZFS tool is /usr/sbin/zfs which used for any operation regarding datasets. It is extremely important to understand several concepts detailed in  [zfsconcepts(7)](https://openzfs.github.io/openzfs-docs/man/7/zfsconcepts.7.html) and [zfs(8)](https://openzfs.github.io/openzfs-docs/man/8/zfs.8.html).

- datasets are named logical data containers managed as a single entity by the /usr/sbin/zfs command. A dataset has a unique name (path) within a given zpool and can be either a filesystem, a snapshot, a bookmark or a zvolume.

- filesystems are POSIX filesystems that can be mounted in the system VFS and appears, just like an ext4 or a XFS filesystem, as a set of files and directories. An extended form of POSIX ACLs (NFSv4 ACLs) and extended attributes are supported.

- snapshots are frozen, read-only point-in-time states of what a filesystem or a zvolume contains. ZFS being a copy-on-write beast, a snapshot only retains what has been changed since the previous snapshot and as such, are approximately free to take if nothing has changed. Unlike LVM snapshots, ZFS snapshots do not require space reservation and can be of an unlimited number. A snapshot can be browsed via a "magic path" to retrieve a filesystem or a zvolume contents as it was at the time it had been taken or used to rollback the whole filesystem or zvolume it related to and are named: *zpoolname/datasetname@snapshotname*.

- bookmarks are like snapshots but with no data held and simply store point in time references. This is extremely useful for doing incremental transfers to a backup system without keeping all the previous snapshots of a given dataset on the source system. Just like a bookmark left in a page of a book. Another use case of bookmarks is to make redacted datasets named: *zpoolname/datasetname#bookmarkname*

- clones are a writable "views" of snapshots that can be mounted in the system VFS just like a filesystem can. The catch is introducing a parent-child dependency with the filesystem. The consequence is that once a clone has been created, the original dataset which can no longer be destroyed as long as the clone exists. That parent-child relationship can be reversed by *promoting* the clone: the clone becomes the parent and the original filesystem becomes the child (this also impacts the original filesystem snapshots which are also "reversed").

- zvolumes are emulated block devices usable just like any other physical hard drive as special block device entries under the directory /dev/zvol/zpoolname.

#### Filesystems datasets

Whenever a  zpool is created, a first filesystem dataset with the same name as the zpool is also created. This root dataset can never be destroyed, unless destroying the zpool itself. All subsequently created datasets are uniquely identified by "path" and are always situated *somewhere* under this root dataset. *Somewhere* because  subsequently created filesystems datasets can be nested and so are forming a hierarchy.

To put an illustration on the explanation, let's (re)create the zpool `zpool_test`:

`root #````
zpool create zpool_test raidz /dev/ram0 /dev/ram1 /dev/ram2 /dev/ram3
```
`root #````
zfs list
```
NAME                                USED  AVAIL     REFER  MOUNTPOINT
zpool\_test                          541K  5.68T      140K  /zpool\_test

It can be observed that name given to the root dataset (column *NAME*) is `zpool_test` which is the same name as the zpool). As this dataset has a mountpoint defined (mentioned in the column *MOUNTPOINT*) this one is a filesystem dataset. Let's check that:

`root #``mount` `root #````
 grep zpool_test
```
zpool\_test on /zpool\_test type zfs (rw,xattr,noacl)

What does it contains?

`root #````
ls -la /zpool_test
```
total 33
drwxr-xr-x  2 root root  2 May  8 19:15 .
drwxr-xr-x 24 root root 26 May  8 19:15 ..

*Nothing!* Now, create additional filesystem datasets by issuing the commands below (notice the "paths" are specified without a leading slash):

`root #````
zfs create zpool_test/fds1
```
`root #````
zfs create zpool_test/fds1/fds1_1
```
`root #````
zfs create zpool_test/fds1/fds1_1/fds1_1_1
```
`root #````
zfs create zpool_test/fds1/fds1_1/fds1_1_2
```
`root #````
zfs create zpool_test/fds2
```
`root #``zfs create zpool_test/fds2/fds2_1`
The result of those latter operations being:

`root #````
zfs list
```
NAME                                USED  AVAIL     REFER  MOUNTPOINT
zpool\_test                         2.44M  5.68T      947K  /zpool\_test
zpool\_test/fds1                     418K  5.68T      140K  /zpool\_test/fds1
zpool\_test/fds1/fds1\_1              279K  5.68T      140K  /zpool\_test/fds1/fds1\_1
zpool\_test/fds1/fds1\_1/fds1\_1\_1     140K  5.68T      140K  /zpool\_test/fds1/fds1\_1/fds1\_1\_1
zpool\_test/fds1/fds1\_1/fds1\_1\_2     140K  5.68T      140K  /zpool\_test/fds1/fds1\_1/fds1\_1\_2
zpool\_test/fds2                     279K  5.68T      140K  /zpool\_test/fds2
zpool\_test/fds2/fds2\_1              140K  5.68T      140K  /zpool\_test/fds2/fds2\_1

Interestingly, the mountpoints hierarchy, by default, is similar the very same hierarchy the filesystem datasets have. The mountpoints can be customized for something else at convenience if needed and can even follow a totally different schema. The filesystem datasets hierarchy can be altered by renaming (`zfs rename`). For example, if `zpool_test/fds1/fds1_1/fds_1/fds1_1_2` should now be `zpool_test/fds3`:

`root #````
zfs rename zpool_test/fds1/fds1_1/fds1_1_2 zpool_test/fds3
```
`root #````
tree /zpool_test
```
/zpool\_test
├── fds1
│   └── fds1\_1
│       └── fds1\_1\_1
├── fds2
│   └── fds2\_1
└── fds3
7 directories, 0 files

A `zfs list` giving:

`root #````
zfs list
```
NAME                                USED  AVAIL     REFER  MOUNTPOINT
zpool\_test                         1.71M  5.68T      151K  /zpool\_test
zpool\_test/fds1                     430K  5.68T      140K  /zpool\_test/fds1
zpool\_test/fds1/fds1\_1              291K  5.68T      151K  /zpool\_test/fds1/fds1\_1
zpool\_test/fds1/fds1\_1/fds1\_1\_1     140K  5.68T      140K  /zpool\_test/fds1/fds1\_1/fds1\_1\_1
zpool\_test/fds2                     279K  5.68T      140K  /zpool\_test/fds2
zpool\_test/fds2/fds2\_1              140K  5.68T      140K  /zpool\_test/fds2/fds2\_1
zpool\_test/fds3                     140K  5.68T      140K  /zpool\_test/fds3

Better: rename a filesystem dataset with children under it like zpool\_test/fds2 which becomes zpool\_test/fds3\_1:

`root #````
zfs rename zpool_test/fds2 zpool_test/fds3/fds3_1
```
`root #````
zfs list
```
NAME                                USED  AVAIL     REFER  MOUNTPOINT
zpool\_test                         1.70M  5.68T      151K  /zpool\_test
zpool\_test/fds1                     418K  5.68T      140K  /zpool\_test/fds1
zpool\_test/fds1/fds1\_1              279K  5.68T      140K  /zpool\_test/fds1/fds1\_1
zpool\_test/fds1/fds1\_1/fds1\_1\_1     140K  5.68T      140K  /zpool\_test/fds1/fds1\_1/fds1\_1\_1
zpool\_test/fds3                     418K  5.68T      140K  /zpool\_test/fds3
zpool\_test/fds3/fds3\_1              279K  5.68T      140K  /zpool\_test/fds3/fds3\_1
zpool\_test/fds3/fds3\_1/fds2\_1       140K  5.68T      140K  /zpool\_test/fds3/fds3\_1/fds2\_1

Here again, ZFS adjusted the mountpoints in consequence but not the names of what is under zpool\_test/fds3/fds3\_1, requiring individual changes.

Rather than using `zfs create` commands, the very same hierarchy could have been created with a serie of `mkdir` commands. What makes things different here, just like several ext4/xfs would have been created on several disk partitions and mounted that way, is that each of those  filesystem datasets are different *standalone boxes* where a collection of files and directories are stored and share the same properties. ZFS cannot compress or encrypt or even share a single file, neither it can snapshot/rollback a single file but it can only do this on a filesystem dataset at whole. In other words, any action (property change, snapshot, rollback, etc) concerning `zpool_test/fds1` impacts *only* `zpool_test/fds1`, not the rest including its children and its children's children, if any. *What happens in the box, stays in the box.*

The structure exists but is somewhat empty. properties will be covered in the next section, however, before putting some data activate the compression on zpool\_test/fds4 (to be created) and copy a significant amount of easily compressible data like a Linux kernel source tree:

`root #````
zfs create -o compression=on zpool_test/fds4
```
`root #````
cp -a /usr/src/linux/* /zpool_test/fds4
```
Looking at the content of /zpool\_test/fds4 gives:

`root #````
ls -l /zpool_test/fds4
```
ls -l /zpool\_test/fds4
total 667007
-rw-r--r--   1 root root       496 Feb 19 17:24 COPYING
-rw-r--r--   1 root root    102088 Feb 19 17:24 CREDITS
drwxr-xr-x  88 root root       103 May 13 09:26 Documentation
-rw-r--r--   1 root root      2573 Feb 19 17:24 Kbuild
-rw-r--r--   1 root root       580 May 13 09:26 Kconfig
drwxr-xr-x   6 root root         6 May 13 09:26 LICENSES
-rw-r--r--   1 root root    698607 May 14 18:48 MAINTAINERS
-rw-r--r--   1 root root     71857 May 13 09:26 Makefile
-rw-r--r--   1 root root   1225558 May 15 00:31 Module.symvers
-rw-r--r--   1 root root       727 Feb 19 17:24 README
-rw-r--r--   1 root root   4144614 May 15 00:29 System.map
drwxr-xr-x  25 root root        27 May 15 00:29 arch
drwxr-xr-x   3 root root       213 May 15 00:31 block
(...)

Nothing really exciting here, just a bunch a files and directories. Where things starts to be interesting however is when ZFS is queried for some usage statistics:

`root #````
zfs get logicalreferenced,referenced,refcompressratio zpool_test/fds4
```
NAME             PROPERTY           VALUE     SOURCE
zpool\_test/fds4  logicalreferenced  8.97G     -
zpool\_test/fds4  referenced         4.58G     -
zpool\_test/fds4  refcompressratio      2.13x     -

`logicalreferenced` gives the logical ("true") amount of data seen by the dataset, independently of an eventual compression while `referenced` gives the size of data as written on the physical storage. As compression is active, `logicalreferenced` is greater than `referenced`.

Another manner to see the effect of compression is the following one:

`root #````
du -sh /zpool_test/fds4; du -sh --apparent-size /zpool_test/fds4
```
4.7G    /zpool\_test/fds4
8.5G    /zpool\_test/fds4

#### Destroying a filesystem dataset

Enough for the basis. It is now time to destroy zpool\_test/fds4 by issuing:

`root #````
zfs destroy zpool_test/fds4
```
#### Filesystem dataset properties

Likewise zpools that have properties so does datasets and filesystem datasets in the present case. Just like the zpool counterparts, properties hold by filesystem datasets can be either intrinsic (thus not being settable) or not and in that latter case can be changed for whatever suitable value in the context. As datasets follow a hierarchy so do the values of properties: a child can dataset inherits from the properties of its parent dataset, that parent inheriting from its own parent so far and so forth until the root dataset is reached. That latter also dataset inherits of some properties of the zpool it is located in. If a given dataset overrides at its level a property value, that value is propagated to the whole hierarchy lying underneath it until another dataset overrides it again on its own. Rince and repeat.

To view the properties of a dataset, the logic is similar to what has been seen for zpools: `zfs get all zpool/the/dataset/to/examine` to view all properties or `zfs get theproperty zpool/the/dataset/to/examine` for a specific one. Let's start by examining that now so famous root dataset:

`root #````
zfs get all zpool_test
```
NAME        PROPERTY              VALUE                  SOURCE
zpool\_test  type                  filesystem             -
zpool\_test  creation              Tue May  9 16:07 2023  -
zpool\_test  used                  1.69M                  -
zpool\_test  available             5.68T                  -
zpool\_test  referenced            140K                   -
zpool\_test  compressratio         1.00x                  -
zpool\_test  mounted               yes                    -
zpool\_test  quota                 none                   default
zpool\_test  reservation           none                   default
zpool\_test  recordsize            128K                   default
zpool\_test  mountpoint            /zpool\_test            default
zpool\_test  sharenfs              off                    default
zpool\_test  checksum              on                     default
zpool\_test  compression           off                    default
zpool\_test  atime                 on                     default
zpool\_test  devices               on                     default
zpool\_test  exec                  on                     default
zpool\_test  setuid                on                     default
zpool\_test  readonly              off                    default
zpool\_test  zoned                 off                    default
zpool\_test  snapdir               hidden                 default
zpool\_test  aclmode               discard                default
zpool\_test  aclinherit            restricted             default
zpool\_test  createtxg             1                      -
zpool\_test  canmount              on                     default
zpool\_test  xattr                 on                     default
zpool\_test  copies                1                      default
zpool\_test  version               5                      -
zpool\_test  utf8only              off                    -
zpool\_test  normalization         none                   -
zpool\_test  casesensitivity       sensitive              -
zpool\_test  vscan                 off                    default
zpool\_test  nbmand                off                    default
zpool\_test  sharesmb              off                    default
zpool\_test  refquota              none                   default
zpool\_test  refreservation        none                   default
zpool\_test  guid                  17939734310320752336   -
zpool\_test  primarycache          all                    default
zpool\_test  secondarycache        all                    default
zpool\_test  usedbysnapshots       0B                     -
zpool\_test  usedbydataset         140K                   -
zpool\_test  usedbychildren        1.55M                  -
zpool\_test  usedbyrefreservation  0B                     -
zpool\_test  logbias               latency                default
zpool\_test  objsetid              54                     -
zpool\_test  dedup                 off                    default
zpool\_test  mlslabel              none                   default
zpool\_test  sync                  standard               default
zpool\_test  dnodesize             legacy                 default
zpool\_test  refcompressratio      1.00x                  -
zpool\_test  written               140K                   -
zpool\_test  logicalused           449K                   -
zpool\_test  logicalreferenced     42K                    -
zpool\_test  volmode               default                default
zpool\_test  filesystem\_limit      none                   default
zpool\_test  snapshot\_limit        none                   default
zpool\_test  filesystem\_count      none                   default
zpool\_test  snapshot\_count        none                   default
zpool\_test  snapdev               hidden                 default
zpool\_test  acltype               off                    default
zpool\_test  context               none                   default
zpool\_test  fscontext             none                   default
zpool\_test  defcontext            none                   default
zpool\_test  rootcontext           none                   default
zpool\_test  relatime              off                    default
zpool\_test  redundant\_metadata    all                    default
zpool\_test  overlay               on                     default
zpool\_test  encryption            off                    default
zpool\_test  keylocation           none                   default
zpool\_test  keyformat             none                   default
zpool\_test  pbkdf2iters           0                      default
zpool\_test  special\_small\_blocks  0                      default

To start with an obvious and already seen property: `type`. That one describes what the dataset nature is and as discussed above, this one is a `filesystem` dataset. Nothing really exciting there but this also another manner to determine the fact.

Going on with the *SOURCE* column: while some values are set to  `defaults` some others are set to a single dash (`-`). There is a little secret here being: any property not having a dash in the *SOURCE* column can be changed, all "dashed" properties are intrinsic. The dataset *creation time*, its *compression ratio* or the *available space* left are intrinsic and should not be changed.

Some intrinsic properties can be focused on at first glance:

| Property | Function | 
|---|---|
| type | Describes the dataset type. In that case it is a *filesystem* but for some other found types are *volume*, *snapshot*, etc. | 
| used | The current physical size (i.e. as stored on the physical storage) of the dataset including its children regardless of type. As the dataset currently contains only some metadata (no files put inside yet), just a tiny amount is shown here. If the compression is in use, the value of *used* can be smaller than the value of *logical* (`df` also reports that). | 
| logical | The current apparent size (i.e. the "true" size) the dataset including its children whatever regardless of type. As the dataset currently contains only some metadata (no files put inside yet), just a tiny amount is shown here. If the compression is in use, the value of *logical* can be greater than the value of *used* (`df --apparent size` also reports that). | 
| available | The current size available for data storage. If a dataset has been told to handle several copies of a file and/or if snapshots retains a lot of data, the available space can be hogged very quickly. | 
| compressratio | The ratio between the "true" data size and its size as stored physically speaking. As the dataset is not compressed, the ratio is 1 but with compressed datasets that value can be more: if 110MB of data takes 100MB physically speaking because of the compression is slightly efficient, 1.10X would be shown here. Data that be aggressively compressed will show higher figures here. | 
| mounted | Indicates if the filesystem dataset is currently mounted somewhere within the VFS or not (also visible with the `mount` command). | 
| referenced | Amount of data accessible (true physical size) by this dataset on the zpool . With no files on the dataset yet but only some metadata, the amount is very tiny and similar to what is shown for *used*. When clones and snapshots will appear, this value will vary. If compression is used, *referenced* can be smaller than *logicalreferenced*. | 
| logicalreferenced | Amount of data accessible (apparent size) by this dataset on the zpool . With no files on the dataset yet but only some metadata, the amount is very tiny and similar to what is shown for *used*. When clones and snapshots will appear, this value will vary. If compression is used, *logicalreferenced* can be greater than *referenced*. | 
| version | It is not the version of the dataset but indicates the version of the ZFS on-disk format used by this dataset. For more than a decade, that number is still at version 5, even under Solaris 11.4 (now at its sunset to not say defunct since September 2017). If the ZFS on-disk format specification would evolve in the future, that version will be incremented. Not to be confused with zpools legacy version numbers as well. | 
| written | The amount of space that was written since the previous snapshot or in other words,the amount of data referenced by the previous dataset snapshot. | 

Some other settable properties of interest:

| Property | Value | Function | 
|---|---|---|
| mountpoint | path\|legacy\|none | Specifies the mountpoint for the dataset as being *path*. A mountpoint set to *none* prevents the dataset from being mounted. When mountpoint set to *legacy*: the mountpoint information will be searched in /etc/fstab AND it is possible to use traditional mount command with the dataset (`mount -t path/to/the/dataset /where/to/mount/it`). This is perhaps the most commonly changed attribute (along with *recordsize*, *quota* and *compression*). | 
| canmount | yes\|no | Specifies if a dataset can be mounted ( `yes`) or not (`no`). This property allows to "hide" a dataset without altering the current value of its property *mountpoint*. (`canmount=no` is an equivalent of `mountpoint=none`). | 
| quota | quota\_value\|none | Set a maximum space usage for the dataset including its descendants (see also *refquota*). E.g. `zfs set quota=4G` sets a 4GB space usage limit. If the value is *none* the dataset has no size limit (remember how tedious/tricky the task can be on "traditional" filesystems?). Also, take note that ZFS has support for user quota, group quota and project quota. | 
| recordsize | recordsize\_size | Specifies the block size for files as being *recordsize\_size* in that filesystem. This property is especially useful in the case of databases where the filesystem I/O operations must be carefully aligned with what the database requires. By default, blocks are of 128k each. | 
| compression | on\|off\|lzjb\|lz4\|zle\|gzip{,-N}\|zstd{,-fast}{,-N} | Specifies if the dataset content has compressed (and decompressed) on the fly. *off* means no compression, *on* means compression with the default method which is *lz4* on zpools having `feature@lz4_compress=enabled` and *lzjb* otherwise. *zstd* can be used on zpools having `feature@zstd_compress=enabled` (it is also possible to give an explicit compression level for *gzip* and *zstd*). Compression and decompression are handled behind the scene in a transparent manner. Compressing datasets can drastically reduce the amount of I/O operations (at the cost of spending CPU cycles in the compression/decompression process). | 
| sharenfs | on\|off\|nfsoptions | Shares the dataset via NFS (automatically adjusts /etc/exports and invokes *exportfs*). | 
| sharesmb | on\|off\|smboptions | shares the dataset via Samba (automatically adjusts /etc/samba/smb.conf) | 
| exec | on\|off | Controls if programs can be executed on the dataset and shared libraries (.so files) can be loaded as well. This has the same effect of a `mount -o noexec ...` if the value is `off`. | 
| setuid | on\|off | Controls if the SUID or GUID bits are honored on the dataset  This has the same effect of a `mount -o nosuid ...` if the value is `off`. | 
| readonly | on\|off | Controls whether a filesystem allows writes ( `off`) or has to be mounted as being read-only (`on`). | 
| atime | on\|off | Controls whether atime is updated for files on the filesystem. If keeping the access time to a file is not used, turning that property off can remove some overhead especially if a lot of files are accessed. | 
| xattr | on\|off\|sa | Controls whether extended attributes are enabled for this filesystem. By default ( `on`) enables directory-based attributes which can result in significant I/O overhead. Setting to `sa` can help to mitigate this. MDAC systems like SELinux require enabling extended attributes support, the same with POSIX ACL (Linux specific). The NFSv4 (FreeBSD only) are not stored in extended attributes. | 
| encryption | on\|off\|algorithm | Controls whether the data is encrypted ( `on`) or not (`off`). By default, AES-256-GCM is used unless a specific encryption algorithm is set. The zpool must have `feature@encrypted=enabled` and a *keyformat* must also be provided at  dataset creation time. When a dataset is encrypted, ZFS encrypts the data and and some metadata as well but **not the zpool structure**. Refer to *zfs-load-key (8)* for details. It is possible to change the encryption keys at a later time if needed (compromised key or any other reason). | 

First things first, start by enabling data compression on the root dataset and see what happens:

`root #````
zfs get compression zpool_test
```
`root #````
zfs get compression zpool_test/fds1
```
NAME             PROPERTY     VALUE           SOURCE
zpool\_test/fds1  compression  on              inherited from zpool\_test

For what lies under the result, summarized below, is:

NAME                               PROPERTY     VALUE           SOURCE
zpool\_test                         compression  on   local
zpool\_test/fds1                    compression  on   inherited from zpool\_test
zpool\_test/fds1/fds1\_1             compression  on   inherited from zpool\_test
zpool\_test/fds1/fds1\_1/fds1\_1\_1\_1  compression  on   inherited from zpool\_test

For the filesystem dataset property `zpool_test` the `local` tells us that the default value has been overridden here. Because no override was done for `zpool_test/fds1` and all of its children inherits from that value and ZFS being smart, it mentions from where the inherited value comes from.

Now, turning the value of `compression` to `off` for `zpool_test/fds1/fds_1_1` gives:

NAME                               PROPERTY     VALUE           SOURCE
zpool\_test                         compression  on   local
zpool\_test/fds1                    compression  on   inherited from zpool\_test
zpool\_test/fds1/fds1\_1             compression  off  local
zpool\_test/fds1/fds1\_1/fds1\_1\_1\_1  compression  on   inherited from zpool\_test/fds1/fds1\_1

To reset the inheritance from a given level to the lowest level of the hierarchy, a very useful command is this one:

`root #````
 zfs inherit -r compression zpool_test/fds1
```
NAME                    PROPERTY     VALUE           SOURCE
zpool\_test/fds1/fds1\_1  compression  on              inherited from zpool\_test

Another demonstration before closing the section: setting usage quotas. Set the quota of `zpool_test/fds1` and `zpool_test/fds1/fds1_1` like below:

`root #````
zfs set quota=5G zpool_test/fds1
```
`root #````
zfs set quota=2G zpool_test/fds1/fds1_1
```
First test, put a file bigger than 2GB directly under zpool\_test/fds1/fds1\_1\_1:

`root #````
dd if=/dev/random of=/zpool_test/fds1/fds1_1/fds1_1_1/test1.dd bs=1G count=3
```
dd: error writing '/zpool\_test/fds1/fds1\_1/fds1\_1\_1/test1.dd': Disk quota exceeded
2+0 records in
1+0 records out
1073741824 bytes (1.1 GB, 1.0 GiB) copied, 3.51418 s, 306 MB/s

Remember, the value set for zpool\_test/fds1/fds1\_1 also applies to zpool\_test/fds1/fds1\_1/fds1\_1\_1 because of inheritance so both  zpool\_test/fds1/fds1\_1 **AND** zpool\_test/fds1/fds1\_1/fds1\_1\_1 have a 2G `quota` limit. Let's ajust things a little bit and set the `quota` value of zpool\_test/fds1/fds1\_1/fds1\_1\_1 to `none` (unlimited) and try again:

`root #````
zfs set quota=none zpool_test/fds1/fds1_1/fds1_1_1
```
`root #````
dd if=/dev/random of=/zpool_test/fds1/fds1_1/fds1_1_1/test1.dd bs=1G count=3
```
3+0 records in
3+0 records out
3221225472 bytes (3.2 GB, 3.0 GiB) copied, 5.39856 s, 597 MB/s

This ends the grand tour covering how to create and rename a filesystem dataset, along with an overview of how properties can be set and inherited between a parent and its children.

To free some space before the next section:

`root #````
find /zpool_test -name '*.dd' -delete
```
#### Sharing a ZFS dataset via NFS/CIFS

A ZFS filesystem dataset can be simply shared via NFS and/or SMBFS/CIFS. No magic here, ZFS will adjust the contents of /etc/exports.d/zfs.exports and /etc/samba/smb.conf (Samba) for the administrator behind the scenes.
To share a dataset, simply set the properties `sharenfs` and/or `sharesmb` to the desired protocol. Both protocols are not mutually exclusive but be careful especially in the case of files opened in read/write mode as the locking mechanisms of NFS and SMBFS/CIFS will not be aware about what the other locks.

Let's create a shared filesystem dataset named `zpool_test/share_test` and share it via NFS to start with:

`root #````
zfs create -o sharenfs=on zpool_test/share_test
```
`root #````
zfs get sharenfs test/share_test
```
NAME                   PROPERTY  VALUE     SOURCE
zpool\_test/share\_test  sharenfs  on        local

On the machine itself, notice what has been added in /etc/exports.d/zfs.exports:

**`/etc/exports.d/zfs.exports`**

Now, listing what the host exports to NFS clients gives:

`root #````
showmount --exports 127.0.0.1
```
Export list for 127.0.0.1:
/zpool\_test/share\_test \*

If the filesystem dataset has to be also visible as a CIFS/SMB Samba share, the idea is the same assuming an already functional Samba general configuration is already present on the machine. Before going any further, ensure at least the following parameters set in /etc/samba/smb.conf or ZFS will silently fail to add shares:

**`/etc/samba/smb.conf`**

Also ensure that the /var/lib/samba/usershares directory exists and has adequate ownership (e.g. `root:root`) and permissions (`01770`).

`root #````
zfs set sharesmb=on zpool_test/share_test
```
NAME                   PROPERTY  VALUE     SOURCE
zpool\_test/share\_test  sharenfs  on        local

Once the share has been defined, ZFS will add a file whose name is the same as the shared dataset in /var/lib/samba/usershares:

`root #````
ls -l /var/lib/samba/usershares
```
total 6
-rw-r--r-- 1 root root 146 Jul  1 10:35 zpool\_test\_share\_test

**`/var/lib/samba/usershares`**

Of course it is possible to not use those facilities and defines the NFS/Samba shares by hand in the adequate configuration files if it more convenient in the context. However, using properties is more convenient and tends to keep the management easier.

#### Zvolumes datasets

A zvolume is nothing more than an emulated block device over ZFS and it can be used just like any other block device: it can be partitioned with fdisk then a filesystem (like ext4,xfs or anything else at convenience) can be created on it and then be mounted. Technically speaking, it is even possible to create another zpool on a zvolumes!

Likewise filesystems datasets, zvolumes also handles a set of properties that may be altered. Data management is the true power: just like filesystems datasets, zvolumes can be snapshotted to be then either rolledback or cloned.

To create a zvolume, just use `zfs create` like below:

- Normal zvolume  (space entirely pre-allocated) zvolume: `zfs create -V <zvol_size> ...`
- Sparse zvolume (thin provisioned): `zfs create -s -V  <zvol_size> ...` (notice `-s`)

The following creates a 1G zvolume zpool\_test/zvol1 will all space pre-allocated and another 2G zvolume zpool\_test/zvol2 thin-provisioned this time:

`root #````
zfs create -V 1G zpool_test/zvol1
```
`root #````
zfs create -s -V 2G zpool_test/zvol2
```
`root #````
zfs list -t all
```
NAME                                         USED  AVAIL     REFER  MOUNTPOINT
zpool\_test                                  1.37G  2.17G      128K  /zpool\_test
zpool\_test/zvol1                            1.37G  3.54G     74.6K  -
zpool\_test/zvol2                            74.6K  2.17G     74.6K  -

It is also possible to create a hierarchy with "placeholders" (i.e. non-mountable filesystems datasets) to get better organization:

`root #````
zfs create -V 1G zpool_test/VM/zvol1
```
`root #````
zfs create -s -V 2G zpool_test/VM/zvol2
```
`root #````
zfs list -t all
```
NAME                                         USED  AVAIL     REFER  MOUNTPOINT
zpool\_test                                  1.37G  2.17G      128K  /zpool\_test
zpool\_test/VM                               1.37G  2.17G      128K  none
zpool\_test/VM/zvol1                         1.37G  3.54G     74.6K  -
zpool\_test/VM/zvol2                         74.6K  2.17G     74.6K  -

Whatever the organization of the zvolumes is, several block devices entries are created in parallel under /dev and /dev/zvol/\<zpool\_name>:

`root #````
ls -l /dev/zvol/zpool_test/VM
```
total 0
lrwxrwxrwx 1 root root 12 May 18 09:05 zvol1 -> ../../../zd0
lrwxrwxrwx 1 root root 13 May 18 09:05 zvol2 -> ../../../zd16

The referenced special entries being:

`root #````
ls -l /dev/zd*
```
brw-rw---- 1 root disk 230,  0 May 18 09:05 /dev/zd0
brw-rw---- 1 root disk 230, 16 May 18 09:05 /dev/zd16

Run fdisk or parted to check what it reports:

`root #````
parted /dev/zvol/zpool_test/VM/zvol1
```
GNU Parted 3.6
Using /dev/zd0
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted) p                                                                
Error: /dev/zd0: unrecognised disk label
Model: Unknown (unknown)                                                  
Disk /dev/zd0: 1074MB
Sector size (logical/physical): 512B/8192B
Partition Table: unknown
Disk Flags: 
(parted)

From parted, create a primary partition that will hold an ext4 filesystem by, first creating a GPT (or MSDOS) label, then create the partition itself:

- `mklabel gpt`
- `mkpart primary ext4 2048s -1`
- `q` (to exit from parted)

(parted) mklabel gpt
(parted) mkpart primary ext4 2048s -1
(parted) p                                                                
Model: Unknown (unknown)
Disk /dev/zd0: 1074MB
Sector size (logical/physical): 512B/8192B
Partition Table: gpt
Disk Flags: 
Number  Start   End     Size    File system  Name     Flags
 1      1049kB  1073MB  1072MB  ext4         primary
(parted) q

Now just format new partition as ususal:

`root #````
mkfs -t ext4 /dev/zd0p1
```
```
mke2fs 1.47.0 (5-Feb-2023)
Discarding device blocks: done                            
Creating filesystem with 261632 4k blocks and 65408 inodes
Filesystem UUID: 9c8ae6ae-780a-413b-a3f9-ab45d97fd8b8
Superblock backups stored on blocks: 
        32768, 98304, 163840, 229376
Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (4096 blocks): done
Writing superblocks and filesystem accounting information: done
```
.. and mount it where desired:

`root #````
mkdir /mnt/ext4test
```
`root #````
mount -t ext4 /dev/zd0p1 /mnt/ext4test
```
`root #``df -h` `root #````
 grep ext4test
```
/dev/zd0p1                         988M   24K  921M   1% /mnt/ext4test

#### ZFS Snapshots (and rolling datasets back)

Just like a relational database can be backed-up and recovered at precise points in time, a ZFS dataset state can be "photographed" and then restored just like if nothing has ever happened since the snapshot had been taken. That kind of feature is not exclusive to ZFS: BTRFS and NILFS2 offer a similar functionality and LVM also supports snapshots. The major difference between snapshots in Copy-on-Write filesystems like ZFS, BTRFS or NILFS2 and LVM counterparts is not having a pre-established size. This never becomes invalid when too much data changes and it is possible to have, virtually, an infinite number, each one holding the data difference with its predecessor. As such, if nothing changes between two snapshots, the only used space is a couple of metadata (a matter of just some kilobytes).

To take a snapshot, the ZFS command is `zfs snapshot` followed by a snapshot name following the convention `zpool/the/dataset/to/be/snaphotted@snapshotname`. Any alphanumeric characters sequence can be used as a snapshot name: `mysnap`, `20221214` or `test123` are all valid snapshots names. The only constraint to be respected is the usage of the arobase character as being the separator between the dataset name and the snapshot name.

Once a snapshot has been taken, there is no need to do anything. ZFS will manage the differences behind the scenes in a transparent manner. As long as snapshots are not destroyed, no cleanup is done and all of the changes history will be retained, whatever the changes are (addition/deletion or modification of existing data). Once a snapshot is destroyed, ZFS will inspect the dataset snapshots timeline and will free all blocks not referenced anymore.

Quota management can be a bit tricky with snapshots: if a filesystem having small files but with a huge number of changes between snapshots, the total size of a snapshot timeline can also be huge and the quota limit can be quickly reached even the "current" dataset state is way below the threshold. The same would happen if just a couple of huge files changes between snapshots.

Enough said, time to demonstrate. First let's create a new dataset and copy some files in it:

`root #````
zfs create zpool_test/testfsds1
```
`root #````
cp -r /usr/src/linux/drivers/* /zpool_test/testfsds1
```
`root #````
df -h
```
Filesystem                         Size  Used Avail Use% Mounted on
zpool\_test                         433M  256K  433M   1% /zpool\_test
zpool\_test/testfsds1               5.0G  4.6G  433M  92% /zpool\_test/testfsds1

Nothing special to say at that point. Just notice the reported used size of the root root dataset is the same as the size of `zpool_test/testfsds1` (433M). As the root dataset contains just a couple of metadata and no files, it occupies only a couple of KB.

Now let's take a first snapshot named `snap1`:

`root #````
zfs snapshot zpool_test/testfsds1@snap1
```
`root #````
zfs list -t all
```
NAME                         USED  AVAIL     REFER  MOUNTPOINT
zpool\_test                                         4.50G   433M      151K  /zpool\_test
zpool\_test/testfsds1                               4.50G   433M     4.50G  /zpool\_test/testfsds1
zpool\_test/testfsds1@snap1                            0B      -     4.50G  -

So far so good, at first glance it can be noticed that the snapshot uses 0 bytes (`USED` shows `0`) because no data changed. In other words, the filesystem dataset `zpool_test/testfsds1` and its snapshot `zpool_test/testfsds1@snap1` refers to the exactly same data blocks.

Let's present this from another prospective:

- The filesystem dataset `zpool_test/testfsds1` has as size of (column `USED`) 4.50G.
- The filesystem dataset `zpool_test/testfsds1` refers to (column `REFER`) 4.50G of data. At this time both columns column `USED` and `REFER` shows identical values, this is perfectly normal
- The snapshot also references the very same 4.50G of data so its size is... zero (the snapshot "sees" the very same data than the filesystem dataset also "sees", so the difference between both are zero).

Remember that `zfs get` command seen earlier? Let's it on a snapshot:

`root #````
zfs get all zpool_test/testfsds1@snap1
```
NAME                        PROPERTY              VALUE                  SOURCE
zpool\_test/testfsds1@snap1  type                  snapshot               -
zpool\_test/testfsds1@snap1  creation              Sat Jul  1 17:52 2023  -
zpool\_test/testfsds1@snap1  used                  0B                     -
zpool\_test/testfsds1@snap1  referenced            4.50G                  -
zpool\_test/testfsds1@snap1  compressratio         1.00x                  -
zpool\_test/testfsds1@snap1  devices               on                     default
zpool\_test/testfsds1@snap1  exec                  on                     default
zpool\_test/testfsds1@snap1  setuid                on                     default
zpool\_test/testfsds1@snap1  createtxg             6470                   -
zpool\_test/testfsds1@snap1  xattr                 on                     default
zpool\_test/testfsds1@snap1  version               5                      -
zpool\_test/testfsds1@snap1  utf8only              off                    -
zpool\_test/testfsds1@snap1  normalization         none                   -
zpool\_test/testfsds1@snap1  casesensitivity       sensitive              -
zpool\_test/testfsds1@snap1  nbmand                off                    default
zpool\_test/testfsds1@snap1  guid                  17727341261301786665   -
zpool\_test/testfsds1@snap1  primarycache          all                    default
zpool\_test/testfsds1@snap1  secondarycache        all                    default
zpool\_test/testfsds1@snap1  defer\_destroy         off                    -
zpool\_test/testfsds1@snap1  userrefs              0                      -
zpool\_test/testfsds1@snap1  objsetid              2085                   -
zpool\_test/testfsds1@snap1  mlslabel              none                   default
zpool\_test/testfsds1@snap1  refcompressratio      1.00x                  -
zpool\_test/testfsds1@snap1  written               4.50G                  -
zpool\_test/testfsds1@snap1  logicalreferenced     4.31G                  -
zpool\_test/testfsds1@snap1  acltype               off                    default
zpool\_test/testfsds1@snap1  context               none                   default
zpool\_test/testfsds1@snap1  fscontext             none                   default
zpool\_test/testfsds1@snap1  defcontext            none                   default
zpool\_test/testfsds1@snap1  rootcontext           none                   default
zpool\_test/testfsds1@snap1  encryption            off                    default

Without commenting every property shown here is the most important:

- `type`: as this is a snaphot(dataset), the reported type is obviously... `snapshot`
- `creation`: The date/time the snapshot has been taken
- `used`: *nearly* zero as the snapshot and the corresponding filesystem dataset both point to the same contents (the  8.24M corresponds to the snapshot metadata).
- `referenced`: the same value seen before, 4.50G (see explanation above)
- `createtxg`: the transaction group (TXG) number in which the snapshot has been created (TXG numbers are always increased over time). This is useful to back-trace the order in which the snapshots of a given filesystem dataset have been taken for inconsistent naming (e.g. `zpool_test/testfsds1@snap1` then `zpool_test/testfsds1@mytest` then `zpool_test/testfsds1@bug412` and so on).

Now, a snapshot exists, let's delete some files but first things first determine the amount of data to be deleted:

`root #````
du -csh /zpool_test/testfsds1/{a,b,c}*
```
16M     /zpool\_test/testfsds1/accel
580K    /zpool\_test/testfsds1/accessibility
51M     /zpool\_test/testfsds1/acpi
81K     /zpool\_test/testfsds1/amba
433K    /zpool\_test/testfsds1/android
8.4M    /zpool\_test/testfsds1/ata
1016K   /zpool\_test/testfsds1/atm
274K    /zpool\_test/testfsds1/auxdisplay
19M     /zpool\_test/testfsds1/base
299K    /zpool\_test/testfsds1/bcma
39M     /zpool\_test/testfsds1/block
44M     /zpool\_test/testfsds1/bluetooth
71K     /zpool\_test/testfsds1/built-in.a
7.1M    /zpool\_test/testfsds1/bus
739K    /zpool\_test/testfsds1/cdrom
45M     /zpool\_test/testfsds1/char
26M     /zpool\_test/testfsds1/clk
1.7M    /zpool\_test/testfsds1/clocksource
4.3M    /zpool\_test/testfsds1/comedi
1.3M    /zpool\_test/testfsds1/connector
3.9M    /zpool\_test/testfsds1/counter
9.6M    /zpool\_test/testfsds1/cpufreq
2.1M    /zpool\_test/testfsds1/cpuidle
30M     /zpool\_test/testfsds1/crypto
537K    /zpool\_test/testfsds1/cxl
309M    total

Now remove the files and take a second snapshot:

`root #````
rm -rf /zpool_test/testfsds1/{a,b,c}*
```
`root #````
zfs snap zpool_test/testfsds1@snap2
```
`root #````
zfs list -t all
```
NAME                                                USED  AVAIL     REFER  MOUNTPOINT
zpool\_test                                         4.51G   432M      151K  /zpool\_test
zpool\_test/testfsds1                               4.50G   432M     4.20G  /zpool\_test/testfsds1
zpool\_test/testfsds1@snap1                          307M      -     4.50G  -
zpool\_test/testfsds1@snap2                            0B      -     4.20G  -

At first glance, the snapshot size of `zpool_test/testfsds1@snap1` roughly increased from 0 byte to, roughly, the size of the deleted data. In other words:

- `zpool_test/testfsds2@snap1` holds no differences with the current state of `zpool_test/testfsds1`, hence a 0 byte used. That latter snapshot references ("sees") to 4.20G of data (4.50G-307M \~ 4.20G), hence the 4.20G shown in the column `REFER`
- As 307M of data has been deleted since `zpool_test/testfsds1` has been taken, the difference between `zpool_test/testfsds1@snap1` and `zpool_test/testfsds1@snap2` is also 307M (the calculation is not exactly the same between ZFS and `df`)
- As the original size of `zpool_test/testfsds1` (before the files deletion) was 4.50G, `zpool_test/testfsds1@snap1` still sees that original state as it was hence the 4.50G shown in the column `REFER`).

Before going one step further, a (magic) trick: although the snapshot is shown as being non-mountable (a dash is present in the column `MOUNTPOINT`), it is possible to access its contents in a read-only form (snapshots are not writable) under the path /zpool\_test/testfsds1/.zfs/snapshot/snap1:

`root #````
ls -l /zpool_test/testfsds1/.zfs/snapshot
```
total 0
drwxrwxrwx 1 root root 0 Jul  1 17:52 snap1
drwxrwxrwx 1 root root 0 Jul  1 18:15 snap2

`root #````
ls -l /zpool_test/testfsds1/.zfs/snapshot/snap1
```
total 1749
-rw-r--r--  1 root root  4004 Jul  1 17:48 Kconfig
-rw-r--r--  1 root root  5543 Jul  1 17:48 Makefile
drwxr-xr-x  4 root root     7 Jul  1 17:48 accel
drwxr-xr-x  4 root root     6 Jul  1 17:48 accessibility
drwxr-xr-x 10 root root   304 Jul  1 17:48 acpi
drwxr-xr-x  2 root root    10 Jul  1 17:48 amba
drwxr-xr-x  2 root root    15 Jul  1 17:48 android
drwxr-xr-x  3 root root   152 Jul  1 17:48 ata
drwxr-xr-x  2 root root    32 Jul  1 17:48 atm
drwxr-xr-x  2 root root    23 Jul  1 17:48 auxdisplay
drwxr-xr-x  6 root root   110 Jul  1 17:48 base
drwxr-xr-x  2 root root    26 Jul  1 17:48 bcma
drwxr-xr-x  9 root root   100 Jul  1 17:48 block
drwxr-xr-x  2 root root   251 Jul  1 17:48 bluetooth
-rw-r--r--  1 root root 67032 Jul  1 17:48 built-in.a
drwxr-xr-x  4 root root    39 Jul  1 17:48 bus
drwxr-xr-x  2 root root    11 Jul  1 17:48 cdrom
drwxr-xr-x 10 root root   102 Jul  1 17:48 char
drwxr-xr-x 51 root root   161 Jul  1 17:48 clk
drwxr-xr-x  2 root root   102 Jul  1 17:48 clocksource
drwxr-xr-x  4 root root    16 Jul  1 17:48 comedi
(...)

Notice the presence of files and directories deleted by `rm -rf {a,b,c}*` just after the snapshot had been created. With that in hands, it is possible to copy or inspect the files at the precise point in time the snapshot had been taken. Extremely useful to restore altered or deleted files!

When inspecting the second snapshot, no file or directory names start with "a","b" or "c" because of deletion just before taking `zpool_test/testfsds1@snap2`:

`root #````
ls -l /zpool_test/testfsds1/.zfs/snapshot/snap2
```
total 1749
-rw-r--r--  1 root root  4004 Jul  1 17:48 Kconfig
-rw-r--r--  1 root root  5543 Jul  1 17:48 Makefile
drwxr-xr-x  4 root root    53 Jul  1 17:48 dax
drwxr-xr-x  2 root root    21 Jul  1 17:48 dca
drwxr-xr-x  3 root root    20 Jul  1 17:48 devfreq
drwxr-xr-x  2 root root     6 Jul  1 17:48 dio
drwxr-xr-x 20 root root   113 Jul  1 17:48 dma
drwxr-xr-x  3 root root    49 Jul  1 17:48 dma-buf
drwxr-xr-x  2 root root   209 Jul  1 17:48 edac
drwxr-xr-x  2 root root     9 Jul  1 17:48 eisa
(...)

Great! But no so much indeed. It would be much more convenient to know the precise differences between two snapshots directly rather than manually inspecting each one. Fortunately, ZFS brought a solution and allows to *diff* between snapshots (the output has been truncated):

`root #````
zfs diff zpool_test/testfsds1@snap1 zpool_test/testfsds1@snap2 | sort
```
\-       /zpool\_test/testfsds1/accel
-       /zpool\_test/testfsds1/accel/Kconfig
-       /zpool\_test/testfsds1/accel/Makefile
-       /zpool\_test/testfsds1/accel/drm\_accel.c
-       /zpool\_test/testfsds1/accel/habanalabs
(...)
-       /zpool\_test/testfsds1/base
-       /zpool\_test/testfsds1/base/.attribute\_container.o.cmd
-       /zpool\_test/testfsds1/base/.auxiliary.o.cmd
-       /zpool\_test/testfsds1/base/.built-in.a.cmd
-       /zpool\_test/testfsds1/base/.bus.o.cmd
-       /zpool\_test/testfsds1/base/.cacheinfo.o.cmd
-       /zpool\_test/testfsds1/base/.class.o.cmd
-       /zpool\_test/testfsds1/base/.component.o.cmd
-       /zpool\_test/testfsds1/base/.container.o.cmd
(...)
-       /zpool\_test/testfsds1/clk
-       /zpool\_test/testfsds1/clk/.built-in.a.cmd
-       /zpool\_test/testfsds1/clk/.clk-bulk.o.cmd
-       /zpool\_test/testfsds1/clk/.clk-composite.o.cmd
-       /zpool\_test/testfsds1/clk/.clk-devres.o.cmd
-       /zpool\_test/testfsds1/clk/.clk-divider.o.cmd
-       /zpool\_test/testfsds1/clk/.clk-fixed-factor.o.cmd
-       /zpool\_test/testfsds1/clk/.clk-fixed-rate.o.cmd
(...)
M       /zpool\_test/testfsds1/

What the command tells is basically: all files and directories whose name starts with "a", "b" and "c" has been removed (notice the dash on the left). Hence due to all of the changes inside, the path /zpool\_test/testfsds1 has been modified (notice the `M` on the left of the very last line).

At this point, let's destroy even more files and create another snapshot again (`zpool_test/testfsds1@snap3`):

`root #````
find -name Makefile -delete
```
`root #````
zfs snap zpool_test/testfsds1@snap3
```
`root #````
zfs list -t all
```
NAME                                                USED  AVAIL     REFER  MOUNTPOINT
zpool\_test                                         4.53G   410M      151K  /zpool\_test
zpool\_test/testfsds1                               4.53G   410M     4.19G  /zpool\_test/testfsds1
zpool\_test/testfsds1@snap1                          307M      -     4.50G  -
zpool\_test/testfsds1@snap2                          186K      -     4.20G  -
zpool\_test/testfsds1@snap3                            0B      -     4.19G  -

Notice that:

1. because `zpool_test/testfsds1@snap3` reflects the current state of `zpool_test/testfsds1`, its size is zero (more or less some metadata of negligible size) *relatively* to the current state of the dataset `zpool_test/testfsds1`.
2. The relative of `zpool_test/testfsds1@snap2` relatively to `zpool_test/testfsds1@snap3` corresponds to the deleted Makefiles (186K), metadata included
3. The size of `zpool_test/testfsds1@snap2` relatively to `zpool_test/testfsds1@snap1` remains the same (307M)

If `zfs diff` is used again (the `grep` command in the middle cleans the list up):

`root #````
 zfs diff zpool_test/testfsds1@snap2 zpool_test/testfsds1@snap3 | grep -v '^M' | sort 
```
\-       /zpool\_test/testfsds1/Makefile
-       /zpool\_test/testfsds1/dax/Makefile
-       /zpool\_test/testfsds1/dax/hmem/Makefile
-       /zpool\_test/testfsds1/dax/pmem/Makefile
-       /zpool\_test/testfsds1/dca/Makefile
-       /zpool\_test/testfsds1/devfreq/Makefile
-       /zpool\_test/testfsds1/devfreq/event/Makefile
(...)

Now, remove everything in `/zpool_test/testfsds1` and observe how each snapshot size evolves:

`root #````
rm -rf /zpool_test/testfsds1/*
```
`root #````
zfs list -t all
```
NAME                                                USED  AVAIL     REFER  MOUNTPOINT
zpool\_test                                         4.53G   406M      151K  /zpool\_test
zpool\_test/testfsds1                               4.53G   406M     12.2M  /zpool\_test/testfsds1
zpool\_test/testfsds1@snap1                          307M      -     4.50G  -
zpool\_test/testfsds1@snap2                          186K      -     4.20G  -
zpool\_test/testfsds1@snap3                         22.0M      -     4.19G  -

So:

`root #````
ls -la /zpool_test/testfsds1
```
total 1
drwxr-xr-x 2 root root 2 Jul  1 19:04 .
drwxr-xr-x 3 root root 3 Jul  1 17:47 ..

Oooops... Empty directory!

A way to restore the contents just before the deletion happened, is to copy the contents of /zpool\_test/testfsds1/.zfs/snapshot/snap3 but this will duplicate data blocks (this might not be true with OpenZFS 2.2 and its block cloning feature) but anyway this is an inelegant way to recover the data. What can be done as this point is a rollback.

A rollback restores a dataset state to what is was at a previous snapshot. To restore the state of `zpool_test/testfsds1` it was at the time the snapshot `zpool_test/testfsds1@snap3` has been taken. To achieve this, the command to use is `zfs rollback`:

`root #````
zfs rollback zpool_test/testfsds1@snap3
```
`root #````
zfs list -t all
```
NAME                                                USED  AVAIL     REFER  MOUNTPOINT
zpool\_test                                         4.53G   410M      151K  /zpool\_test
zpool\_test/testfsds1                               4.53G   410M     4.19G  /zpool\_test/testfsds1
zpool\_test/testfsds1@snap1                          307M      -     4.50G  -
zpool\_test/testfsds1@snap2                          186K      -     4.20G  -
zpool\_test/testfsds1@snap3                            0B      -     4.19G  -

*Et voila!* Notice the size of `zpool_test/testfsds1@snap3` relatively to  `zpool_test/testfsds1`: as the dataset has been rolled back, the content "seen" by both is identical hence the 0-byte difference. Listing /zpool\_test/testfsds1 shows some files again.

`root #````
ls -la /zpool_test/testfsds1
```
|total 1344
drwxr-xr-x 116 root root   118 Jul  1 18:41 .
drwxr-xr-x   3 root root     3 Jul  1 17:47 ..
-rw-r--r--   1 root root  4004 Jul  1 17:48 Kconfig
drwxr-xr-x   4 root root    52 Jul  1 18:41 dax
drwxr-xr-x   2 root root    20 Jul  1 18:41 dca
drwxr-xr-x   3 root root    19 Jul  1 18:41 devfreq
drwxr-xr-x   2 root root     5 Jul  1 18:41 dio
drwxr-xr-x  20 root root   112 Jul  1 18:41 dma
drwxr-xr-x   3 root root    48 Jul  1 18:41 dma-buf
drwxr-xr-x   2 root root   208 Jul  1 18:41 edac
drwxr-xr-x   2 root root     8 Jul  1 18:41 eisa
drwxr-xr-x   2 root root    55 Jul  1 18:41 extcon
drwxr-xr-x   2 root root    22 Jul  1 18:41 firewire

*"Is it possible to rollback to an older snapshot?"*
The answer is: Yes

Going back in time to `zpool_test/testfsds1@snap1` sounds as easy as what has just been demonstrated but with a nice side effect this time:

`root #````
zfs rollback zpool_test/testfsds1@snap1
```
cannot rollback to 'zpool\_test/testfsds1@snap1': more recent snapshots or bookmarks exist
use '-r' to force deletion of the following snapshots and bookmarks:
zpool\_test/testfsds1@snap2
zpool\_test/testfsds1@snap3

ZFS is smart enough to warn about the effect of the operation: if the dataset is rolled-back in this case, `zpool_test/testfsds1@snap2` and `zpool_test/testfsds1@snap3` would make absolutely no sense and require destruction. Proceeding:

`root #````
zfs rollback -r zpool_test/testfsds1@snap1
```
`root #````
zfs list -t all
```
NAME                                                USED  AVAIL     REFER  MOUNTPOINT
zpool\_test                                         4.50G   432M      151K  /zpool\_test
zpool\_test/testfsds1                               4.50G   432M     4.50G  /zpool\_test/testfsds1
zpool\_test/testfsds1@snap1                            0B      -     4.50G  -

Back to what things were at the beginning of the current section!

Last but not least, `zpool_test/testfsds1@snap1` can be destroyed via the `zfs destroy` command, just like a filesystem dataset or a zvolume can be:

`root #````
zfs destroy zpool_test/testfsds1@snap1
```
`root #````
zfs list
```
NAME                                                USED  AVAIL     REFER  MOUNTPOINT
zpool\_test                                         4.50G   432M      151K  /zpool\_test
zpool\_test/testfsds1                               4.50G   432M     4.50G  /zpool\_test/testfsds1

#### Important considerations on ZFS snapshots

Further important considerations on snapshots before moving on:

- Working with zvolumes snapshots follows the exact same idea and will not be demonstrated here
- Before rolling a dataset back:
  - ensure there are no open files on the corresponding filesystem dataset
  - in a case of a zvolume, ensure it is unmounted first
- It is perfectly possible to destroy an intermediary snapshot, not only the latest one: in that case ZFS will determine and free data blocks no longer referenced in all remaining snapshots (and the corresponding filesystem dataset). The same zvolumes snapshots.
- Once a  snapshot has been destroyed, there is no way to recover it but a `zfs send/receive` from a backup.

#### ZFS clones

ZFS clones are the continuity of ZFS snapshots. In the ZFS world, a snapshot is an immutable object and it can only be read. There is a way, however, to get a writable snapshot: a ZFS clone.

Intuitively speaking, as a clone is a writable snapshot is derived from a snapshot. The way clone are created follows that idea!

First things first, recreate a snapshot of our test filesystem dataset used in the previous section dedicated to snapshots management as it has been destroyed at the end:

`root #````
zfs snap zpool_test/testfsds1@snap1
```
NAME                                                USED  AVAIL     REFER  MOUNTPOINT
zpool\_test                                         4.50G   432M      151K  /zpool\_test
zpool\_test/testfsds1                               4.50G   432M     4.50G  /zpool\_test/testfsds1
zpool\_test/testfsds1@snap1                            0B      -     4.50G  -

Seeing a 0-byte snapshot should not be a novelty or a surprise at this point. It should also not be a surprise to read that a snapshot cannot be mounted (altough it can be accessed via the the pseudo=filesystem `/zpool_test/testfsds1/.zfs/snapshot/snap1`) nor be modified:

`root #````
touch /zpool_test/testfsds1/.zfs/snapshot/snap1/helloworld
```
touch: cannot touch '/zpool\_test/testfsds1/.zfs/snapshot/snap1/helloworld': Read-only file system

A clone of this latter snapshot can created with the following command:

`root #````
zfs clone zpool_test/testfsds1@snap1 zpool_test/testfsds1_clone
```
`root #````
zfs list -t all
```
NAME                                                USED  AVAIL     REFER  MOUNTPOINT
zpool\_test                                         4.50G   432M      151K  /zpool\_test
zpool\_test/testfsds1                               4.50G   432M     4.50G  /zpool\_test/testfsds1
zpool\_test/testfsds1@snap1                          163K      -     4.50G  -
zpool\_test/testfsds1\_clone                            0B   432M     4.50G  /zpool\_test/testfsds1\_clone

Notice the slight size change of `zpool_test/testfsds1@snap1` due to some additional metadata required for the clone creation and how the mountpoint has been defined. Nothing really new here.

Let's examine what has been created, here again not all of the properties will be commented:

`root #````
zfs get all zpool_test/testfsds1_clone
```
`root #````
zfs list -t all
```
NAME                                                USED  AVAIL     REFER  MOUNTPOINT
NAME                        PROPERTY              VALUE                        SOURCE
zpool\_test/testfsds1\_clone  type                  filesystem                   -
zpool\_test/testfsds1\_clone  creation              Sat Jul  1 20:44 2023        -
zpool\_test/testfsds1\_clone  used                  0B                           -
zpool\_test/testfsds1\_clone  available             432M                         -
zpool\_test/testfsds1\_clone  referenced            4.50G                        -
zpool\_test/testfsds1\_clone  compressratio         1.00x                        -
zpool\_test/testfsds1\_clone  mounted               yes                          -
zpool\_test/testfsds1\_clone  origin                zpool\_test/testfsds1@snap1   -
(...)

Guessing `clone` as the `type` would be definitively wrong! Despite being what is is, a clone behaves in fact just like any regular filesystem dataset and it is considered as such by ZFS. So seeing `filesystem` as being the `` type `is perfectly normal.` ``

So how a clone can be distinguished from a regular filesystem dataset then? Examine what is set for the property `origin`: it refers the snapshot the clone. As a comparison, notice what is set for `origin` in the case of `zpool_test/testfsds1`:

`root #````
zfs get type,origin zpool_test/testfsds1
```
NAME                        PROPERTY  VALUE                       SOURCE
zpool\_test/testfsds1  type      filesystem  -
zpool\_test/testfsds1  origin    -           -
(...)

A dash! In other words: a big *nothing*.

While `zpool_test/testfsds1` is a standalone filesystem dataset, there is parent/child relationship between  `zpool_test/testfsds1@snap1` and  `zpool_test/testfsds1_clone`. Because of this relationship exists, destroying `zpool_test/testfsds1@snap1` or even `zpool_test/testfsds1` is not possible unless destroying `zpool_test/testfsds1_clone` (and all of its own eventual snapshots) as a prior operation.

The structure at this point can be summarized by the following diagram:

![ZFS clone flow 1.png](https://wiki.gentoo.org/images/e/e8/ZFS_clone_flow_1.png)


As the clone is writable, why not add some content to it?

`root #````
dd if=/dev/random  of=/zpool_test/testfsds1_clone/test.dd bs=1M count=20
```
20+0 records in
20+0 records out
20971520 bytes (21 MB, 20 MiB) copied, 0.0316403 s, 663 MB/s

`root #````
zfs list -t all
```
NAME                                                USED  AVAIL     REFER  MOUNTPOINT
zpool\_test                                         4.52G   412M      163K  /zpool\_test
zpool\_test/testfsds1                               4.50G   412M     4.50G  /zpool\_test/testfsds1
zpool\_test/testfsds1@snap1                          163K      -     4.50G  -
zpool\_test/testfsds1\_clone                         20.2M   412M     4.52G  /zpool\_test/testfsds1\_clone

Observe the size change of a (roughly) 20MB. Because the clone now lives its own (separate) life, the added data does not impact the size of the snapshot it derives from.

Time to destroy the snapshot the clone has been derived from:

`root #````
zfs destroy zpool_test/testfsds1@snap1
```
cannot destroy 'zpool\_test/testfsds1@snap1': snapshot has dependent clones
use '-R' to destroy the following datasets:
zpool\_test/testfsds1\_clone

Because of the parent/child relationship between the clone and the filesystem dataset that latter derives from, ZFS is unable to delete the clone. Several options can be used to overcome that:

- Option #1: Break the relationship between the clone and its parent snapshot so each one exists as standalone ZFS entities. This requires to:
  - make a backup by `zfs send`-ing a full copy of `zpool_test/testfsds1_clone`
  - destroy `zpool_test/testfsds1_clone`
  - restore `zpool_test/testfsds1_clone` from the backup previously made using `zfs receive`
- Option #2: Invert the parent/child relationship so `zpool_test/testfsds1_clone` becomes the parent of `zpool_test/testfsds1@snap1`

The first option is of no interest here because it will duplicate the data blocks rather than just using references so let's use the second option and introduce a new ZFS command to manipulate clones by the same occasion: `zfs promote`. That latter command does exactly what its name suggests: it *promotes* the clone as being the parent of what is originally derives from or in other words: "What was the child becomes the parent and what was the parent becomes the child".

`root #````
zfs promote
```
`root #````
zfs list -t all
```
NAME                                                USED  AVAIL     REFER  MOUNTPOINT
zpool\_test                                         4.52G   412M      163K  /zpool\_test
zpool\_test/testfsds1                                163K   412M     4.50G  /zpool\_test/testfsds1
zpool\_test/testfsds1\_clone                         4.52G   412M     4.52G  /zpool\_test/testfsds1\_clone
zpool\_test/testfsds1\_clone@snap1                    174K      -     4.50G  -

Noticed the changes? Everything has been turned upside-down and becomes just like the `zpool_test/testfsds1_clone` had been created first, then snapshotted as `zpool_test/testfsds1_clone@snap1` then cloned as `zpool_test/testfsds1`. To illustrate things again, here is now how things are organized:

1. The clone `zpool_test/testfsds1_clone` now assumes the responsibility of `zpool_test/testfsds1` so it now has a size of 4.50GB (the original size of `zpool_test/testfsds1`) plus the additional 20MB added just here-before for a grand total of 4.52GB
2. The snapshot `zpool_test/testfsds1@snap1` has "migrated" from `zpool_test/testfsds1` to `zpool_test/testfsds1_clone` and also became the parent of `zpool_test/testfsds1`.

`root #````
 zfs get type,origin zpool_test/testfsds1_clone zpool_test/testfsds1 zpool_test/testfsds1_clone@snap1
```
NAME                              PROPERTY  VALUE                             SOURCE
zpool\_test/testfsds1              type      filesystem                        -
zpool\_test/testfsds1              origin    zpool\_test/testfsds1\_clone@snap1  -
zpool\_test/testfsds1\_clone        type      filesystem                        -
zpool\_test/testfsds1\_clone        origin    -                                 -
zpool\_test/testfsds1\_clone@snap1  type      snapshot                          -
zpool\_test/testfsds1\_clone@snap1  origin    -                                 -

That can also be illustrated by the following diagram:

![ZFS clone flow 2.png](https://wiki.gentoo.org/images/f/f4/ZFS_clone_flow_2.png)


Despite a hierarchy change due to the clone promotion, what all of those entities remains strictly the same. Both ZFS entities conserve a link in-between lifetimes and can have individual snapshots.

Just for the sake of the demonstration: what happens if /zpool\_test/testfsds1\_clone/test.dd is deleted? To find out, let's take a another snapshot of `zpool_test/testfsds1_clone` then proceed with the file deletion:

`root #````
zfs list -t all
```
NAME                                                USED  AVAIL     REFER  MOUNTPOINT
zpool\_test                                         4.52G   412M      163K  /zpool\_test
zpool\_test/testfsds1                                163K   412M     4.50G  /zpool\_test/testfsds1
zpool\_test/testfsds1\_clone                         4.52G   412M     4.52G  /zpool\_test/testfsds1\_clone
zpool\_test/testfsds1\_clone@snap1                    174K      -     4.50G  -
zpool\_test/testfsds1\_clone@snap2                      0B      -     4.52G  -

The explanation is somewhat tricky but here it is: because /zpool\_test/testfsds1\_clone/test.dd is still referenced in `zpool_test/testfsds1_clone@snap2`, its data blocks are still retained (hence the 4.52GB seen by that latter snapshot). Remember, the world was turned upside-down and in zpool\_test/testfsds1\_clone@snap1 (which is the old zpool\_test/testfsds1@snap1), /zpool\_test/testfsds1\_clone/test.dd does not exists hence the 4.50GB shown. The way the demonstration has been done makes the contents of `zpool_test/testfsds1` a subset of `zpool_test/testfsds1_clone` so ZFS only needs a couple of metadata to establish the contents of each entity.

Another 100pts question: *"is it possible to revert to the original state?"* (or reworded: *"can `zpool_test/testfsds1` be promoted?"*). Let's find out, but as a first step, rollback and destroy `zpool_test/testfsds1_clone@snap2`:

`root #````
zfs rollback zpool_test/testfsds1_clone@snap2
```
`root #````
zfs destroy zpool_test/testfsds1_clone@snap2
```
`root #````
zfs promote zpool_test/testfsds1
```
`root #````
zfs list -t all
```
NAME                                                USED  AVAIL     REFER  MOUNTPOINT
zpool\_test                                         4.52G   412M      163K  /zpool\_test
zpool\_test/testfsds1                               4.50G   412M     4.50G  /zpool\_test/testfsds1
zpool\_test/testfsds1@snap1                          163K      -     4.50G  -
zpool\_test/testfsds1\_clone                         20.2M   412M     4.52G  /zpool\_test/testfsds1\_clone

*Et voila!* Back to the original state! Notice who is the parent of who:

`root #````
zfs get type,origin zpool_test/testfsds1_clone zpool_test/testfsds1 zpool_test/testfsds1@snap1
```
`root #````
zfs list -t all
```
NAME                        PROPERTY  VALUE                       SOURCE
zpool\_test/testfsds1        type      filesystem                  -
zpool\_test/testfsds1        origin    -                           -
zpool\_test/testfsds1@snap1  type      snapshot                    -
zpool\_test/testfsds1@snap1  origin    -                           -
zpool\_test/testfsds1\_clone  type      filesystem                  -
zpool\_test/testfsds1\_clone  origin    zpool\_test/testfsds1@snap1  -

A typical usage of snapshots in a Gentoo environment is to experiment *"What if this happens?"*: just clone the production environment filesystem dataset, then mount what is required to be (as if the clone was an uncompressed Gentoo stage 3 tarball) then *chroot* into it to have a breakable playground. Cloning also avoids copying back and forth hundreds of gigabytes of data and filling up the local disk space (ZFS, so far, will duplicate all data blocks if a simple copy rather than a clone would have been made). ZFS deduplication being not of common use due to induced performance penalties.

As being a filesystem dataset, the clone can be destroyed with a now known command:

`root #````
zfs destroy zpool_test/testfsds1_clone
```
To start a scrub for the zpool *zfs\_test*:

`root #``zpool scrub zfs_test`
Monitor I/O activity on all zpools (refreshes every 6 seconds):

`root #``zpool iostat 6`
OpenZFS uses [ARC](https://en.wikipedia.org/wiki/Adaptive_replacement_cache) page replacement algorithm instead of the Last Recently Used page replacement algorithm used by other filesystems. This has a better hit rate, therefore providing better performance. The implementation of ARC in ZFS differs from the original paper in that the amount of memory used as cache can vary. This permits memory used by ARC to be reclaimed when the system is under memory pressure (via the kernel's shrinker mechanism) and grow when the system has memory to spare. The minimum and maximum amount of memory allocated to ARC varies based on system memory. The default minimum is 1/32 of all memory, or 64MB, whichever is more. The default maximum is the larger of 1/2 of system memory or 64MB.

The manner in which Linux accounts for memory used by ARC differs from memory used by the page cache. Specifically, memory used by ARC is included under "used" rather than "cached" in the output used by the \`free\` program. This in no way prevents the memory from being released when the system is low on memory. However, it can give the impression that ARC (and by extension ZFS) will use all of system memory if given the opportunity.

The minimum and maximum memory usage of ARC is tunable via zfs\_arc\_min and zfs\_arc\_max respectively. These properties can be set any of three ways. The first is at runtime (new in 0.6.2):

`root #``echo 536870912 >> /sys/module/zfs/parameters/zfs_arc_max`
The second is via /etc/modprobe.d/zfs.conf:

`root #``echo "options zfs zfs_arc_max=536870912" >> /etc/modprobe.d/zfs.conf`
The third is on the kernel command line by specifying "zfs.zfs\_arc\_max=536870912" (for 512MB). Similarly, the same can be done to adjust zfs\_arc\_min.

- Encryption: Native encryption (i.e. not using LUKS) has a history of show-stopper bugs in ZFS. There is (as of July 2023) no active maintainer for the encryption portions of ZFS and it's unofficially discouraged by the community. Some example bugs are:

- Swap: On systems with extremely high memory pressure, using a zvol for swap can result in lockup, regardless of how much swap is still available. This issue is currently being investigated in [\[1\]](https://github.com/openzfs/zfs/issues/7734). Please check the current [OpenZFS documentation on swap](https://openzfs.github.io/openzfs-docs/Project%20and%20Community/FAQ.html#using-a-zvol-for-a-swap-device-on-linux)"

- [ZFS/rootfs](https://wiki.gentoo.org/wiki/ZFS/rootfs) — installing Gentoo to a ZFS rootfs
- [Btrfs](https://wiki.gentoo.org/wiki/Btrfs) — a copy-on-write (CoW) [filesystem](https://wiki.gentoo.org/wiki/Filesystem) for Linux aimed at implementing advanced features while focusing on fault tolerance, repair, and easy administration.
