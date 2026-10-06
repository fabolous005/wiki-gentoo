<!-- source: https://wiki.gentoo.org/wiki/Btrfs/System_Root_Guide | group: Gentoo Wiki (Main) | wiki-title: Btrfs/System Root Guide -->
---
title: Btrfs/System Root Guide
url: https://wiki.gentoo.org/wiki/Btrfs/System_Root_Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-09-18"
fingerprint: abf911da5e16b61
license: CC BY-SA 4.0
---

# Btrfs/System Root Guide

[Btrfs](https://wiki.gentoo.org/wiki/Btrfs)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This exercise is one example for re-basing a Gentoo installation's root filesystem to use btrfs. In this case, the existing system is an `mdadm` based mirror set using two 2TB drives located at /dev/sda and /dev/sdb. Two fresh 2TB drives have been added at /dev/sdc and /dev/sdd.

## Converting to a btrfs Based System

### Existing layout

Simple two way `mdadm` mirror (RAID1):

- 250MB /boot partition as ext3 with metadata=0.90
- 75GB / partition as ext4 with metadata=0.90
  - 750GB /home partition as ext4 with metadata=1.2
  - 1TB+ /vm partition as ext4 with metadata=1.2

The use of the older metadata format for /boot and / partitions allows grub-0.97 to find and boot the system without needing to resort to an initial ram device.

**`/boot/grub/grub.conf`**

**Existing grub.conf**

```
default 0
timeout 6
splashimage=(hd0,0)/boot/grub/splash.xpm.gz
title Gentoo Linux 3.8.13-gentoo
root (hd0,0)
kernel /boot/kernel-3.8.13-gentoo md=2,/dev/sda2,/dev/sdb2 root=/dev/md2
```
**`/etc/fstab`**

**Existing fstab**

```
/dev/md1                /boot           ext3            defaults,noatime        1 2
/dev/md2                /               ext4            defaults,noatime        0 1
/dev/md3                /home           ext4            defaults,noatime        0 1
/dev/md4                /vm             ext4            defaults,noatime        0 1
```
### New layout

- 250MB /boot partition as ext3 with metadata=0.90
- 1.9TB+ btrfs partition with RAID1 metadata and data

We are keeping the /boot as a simple software mirror in order to stay with grub-0.97 but will now need to use an initial ram filesystem. The kernel will otherwise panic when it attempts to mount btrfs filesystems that need to have a btrfs device scan done first. We will be following the write up for [Early Userspace Mounting](https://wiki.gentoo.org/wiki/Early_Userspace_Mounting) to build that filesystem. See also [Custom Initramfs](https://wiki.gentoo.org/wiki/Custom_Initramfs) for more details on various ways to prepare the filesystem.

Btrfs has been built into the kernel (not a module) along with lzo compression/decompression as that will be used to optimize space utilization and read performance on the system volumes.  Likewise, the raid modules used by `mdadm` are also built-in.

**Kernel configuration for btrfs**

```
General setup  --->
  [*] Initial RAM filesystem and RAM disk (initramfs/initrd) support
Device Drivers  --->
  [*] Multiple devices driver support (RAID and LVM)  --->
    {*}   RAID support
    [*]     Autodetect RAID arrays during kernel boot
    <*>     Linear (append) mode
    <*>     RAID-0 (striping) mode
    {*}     RAID-1 (mirroring) mode
    {*}     RAID-10 (mirrored striping) mode
    {*}     RAID-4/RAID-5/RAID-6 mode
File systems  --->
  <*> Btrfs filesystem support
  [*]   Btrfs POSIX Access Control Lists
```
There are a number of places where lzo is part of a module name in the kernel .config.

`root #``cd /usr/src/linux``root #``grep -i lzo .config`
CONFIG\_HAVE\_KERNEL\_LZO=y
# CONFIG\_KERNEL\_LZO is not set
CONFIG\_RD\_LZO=y
CONFIG\_SQUASHFS\_LZO=y
CONFIG\_CRYPTO\_LZO=m
CONFIG\_LZO\_COMPRESS=y
CONFIG\_LZO\_DECOMPRESS=y
CONFIG\_DECOMPRESS\_LZO=y

It is unclear what kernel options are pickable to make sure that the lzo module btrfs relies on will be enabled. The likely suspect is `CONFIG_HAVE_KERNEL_LZO` which comes in to play for the compression of the kernel image itself:

```
General setup  --->
  Kernel compression mode (Gzip)  --->
    (X) Gzip
    ( ) Bzip2
    ( ) LZMA
    ( ) XZ
    ( ) LZO
```
The default selection of gzip causes CONFIG\_KERNEL\_LZO to not be set as shown above. There doesn't appear to be a way to control the setting of CONFIG\_HAVE\_KERNEL\_LZO short of editing .config directly.

#### Partitioning

`cfdisk` was used to partition /dev/sdc with the 250MB and remainder partitions by hand. `sfdisk` then can be used to apply the same scheme to /dev/sdd.

`root #``sfdisk -d /dev/sdc >/tmp/sdc.txt``root #``cat /tmp/sdc.txt`
\# partition table of /dev/sdc
unit: sectors
/dev/sdc1 : start=       63, size=   481887, Id=83, bootable
/dev/sdc2 : start=   481950, size=3906547218, Id=83
/dev/sdc3 : start=        0, size=        0, Id= 0
/dev/sdc4 : start=        0, size=        0, Id= 0

`root #``# sfdisk /dev/sdd </tmp/sdc.txt`
#### Setting up boot

`root #``mdadm --create --verbose /dev/md5 --level=mirror --raid-devices=2 --metadata=0.90 /dev/sdc1 /dev/sdd1`
mdadm: size set to 240832K
mdadm: array /dev/md5 started.

`root #``cat /proc/mdstat | grep md5`
md5 : active raid1 sdd1\[1\] sdc1\[0\]

`root #``mkfs -t ext3 /dev/md5````
mke2fs 1.42.7 (21-Jan-2013)
/dev/md5 alignment is offset by 512 bytes.
This may result in very poor performance, (re)-partitioning suggested.
Filesystem label=
OS type: Linux
Block size=1024 (log=0)
Fragment size=1024 (log=0)
Stride=4 blocks, Stripe width=4 blocks
60240 inodes, 240832 blocks
12041 blocks (5.00%) reserved for the super user
First data block=1
Maximum filesystem blocks=67371008
30 block groups
8192 blocks per group, 8192 fragments per group
2008 inodes per group
Superblock backups stored on blocks: 
        8193, 24577, 40961, 57345, 73729, 204801, 221185
Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (4096 blocks): done
Writing superblocks and filesystem accounting information: done 
```
The warning about /boot partition alignment might have been avoided with some more care with cfdisk.

#### /boot transfer

`root #``mkdir /mnt/altboot``root #``mount -t ext3 /dev/md5 /mnt/altboot``root #``cd /boot``root #``tar --xattrs --xattrs-include='*' cpf - . | (cd /mnt/altboot; tar --xattrs xpf -)`
Grub must be installed on each of the new mirrors by hand as shown. That way if the first drive fails, the second drive can be moved down to /dev/sda, and the GRUB MBR code will have already been set up to make it look the first drive in the device chain.

`root #``grub`
grub> device (hd0) /dev/sdc
grub> root (hd0,0)
 Filesystem type is ext2fs, partition type 0x83
grub> setup (hd0)
 Checking if "/boot/grub/stage1" exists... yes
 Checking if "/boot/grub/stage2" exists... yes
 Checking if "/boot/grub/e2fs\_stage1\_5" exists... yes
 Running "embed /boot/grub/e2fs\_stage1\_5 (hd0)"...  22 sectors are embedded.
succeeded
 Running "install /boot/grub/stage1 (hd0) (hd0)1+22 p (hd0,0)/boot/grub/stage2 /boot/grub/menu.
lst"... succeeded
Done.
grub> device (hd0) /dev/sdd
grub> root (hd0,0)
 Filesystem type is ext2fs, partition type 0x83
grub> setup (hd0)
 Checking if "/boot/grub/stage1" exists... yes
 Checking if "/boot/grub/stage2" exists... yes
 Checking if "/boot/grub/e2fs\_stage1\_5" exists... yes
 Running "embed /boot/grub/e2fs\_stage1\_5 (hd0)"...  22 sectors are embedded.
succeeded
 Running "install /boot/grub/stage1 (hd0) (hd0)1+22 p (hd0,0)/boot/grub/stage2 /boot/grub/menu.
lst"... succeeded
Done.
grub> quit

#### / transfer

We assume that a "hot" transfer of the system will be okay and thus do a remount of / to /mnt/rawroot to grab the basics without pulling in any additional baggage from /proc, udev and other mounts. If the system is running any database servers such as mysql, postgres or an ldap back-end, it is better to turn those services off first before attempting this. The new btrfs will have the rootfs as a subvolume.

Other existing filesystem such as /home and /vm will become other subvolumes. We edit /etc/fstab on an interim basis to provide mountpoints for the new filesystem and its subvolumes. The compression and auto defragmentation features of btrfs may or may not be applicable for the underlying data. The lzo compressor has been turned off for /mnt/newdistfiles since it will be getting the contents of /var/cache/distfiles where files are already compressed. The /mnt/newvm filesystem leaves out autodefrag as an option since it interferes with the performance of virtual machines and copy on write.

**`/etc/fstab`**

**additions to existing version**

```
LABEL=BTRFSMIRROR	/mnt/btrfsmirror	btrfs	defaults,noatime	0 0
LABEL=BTRFSMIRROR	/mnt/newroot		btrfs	defaults,noatime,compress=lzo,autodefrag,subvol=root	0 0
LABEL=BTRFSMIRROR	/mnt/newhome		btrfs	defaults,noatime,compress=lzo,autodefrag,subvol=home	0 0
LABEL=BTRFSMIRROR	/mnt/newdistfiles	btrfs	defaults,noatime,autodefrag,subvol=distfiles	0 0
LABEL=BTRFSMIRROR	/mnt/newvm		btrfs	defaults,noatime,compress=lzo,subvol=vm	0 0
```
`root #``mkfs.btrfs -L BTRFSMIRROR -d raid1 -m raid1 /dev/sdc2 /dev/sdd2````
WARNING! - Btrfs Btrfs v0.19 IS EXPERIMENTAL
WARNING! - see http://btrfs.wiki.kernel.org before using
adding device /dev/sdd2 id 2
fs created label BTRFSMIRROR on /dev/sdc2
        nodesize 4096 leafsize 4096 sectorsize 4096 size 3.64TB
Btrfs Btrfs v0.19
```
`root #``mkdir /mnt/btrfsmirror``root #``mount /mnt/btrfsmirror``root #``mkdir /mnt/newroot``root #``btrfs subvolume create /mnt/btrfsmirror/root`
Create subvolume '/mnt/btrfsmirror/root'

`root #``mount /mnt/newroot``root #``mkdir /mnt/newhome``root #``btrfs subvolume create /mnt/btrfsmirror/home`
Create subvolume '/mnt/btrfsmirror/home'

`root #``mount /mnt/newhome``root #``mkdir /mnt/newdistfiles``root #``btrfs subvolume create /mnt/btrfsmirror/distfiles`
Create subvolume '/mnt/btrfsmirror/distfiles'

`root #``mount /mnt/newdistfiles``root #``mkdir /mnt/newvm``root #``btrfs subvolume create /mnt/btrfsmirror/vm`
Create subvolume '/mnt/btrfsmirror/vm'

`root #``mount /mnt/newvm``root #``df`
Filesystem      1K-blocks      Used  Available Use% Mounted on
rootfs           71959568  25451008   42846548  38% /
/dev/md2         71959568  25451008   42846548  38% /
devtmpfs         12332984         4   12332980   1% /dev
tmpfs            12367228      1056   12366172   1% /run
shm              12367228        84   12367144   1% /dev/shm
cgroup\_root         10240         0      10240   0% /sys/fs/cgroup
cachedir             4096         4       4092   1% /lib64/splash/cache
/dev/md1           186555     51950     124973  30% /boot
/dev/md3        720795012 298068320  386105780  44% /home
/dev/md4       1129522164 978970760   93168360  92% /vm
/dev/md5           233225     52460     168724  24% /mnt/altboot
/dev/sdd2      3906547216      1128 3904391680   1% /mnt/btrfsmirror
/dev/sdd2      3906547216      1128 3904391680   1% /mnt/newroot
/dev/sdd2      3906547216      1128 3904391680   1% /mnt/newhome
/dev/sdd2      3906547216      1128 3904391680   1% /mnt/newdistfiles
/dev/sdd2      3906547216      1128 3904391680   1% /mnt/newvm

We kick off the root fs transfer and come back after a cup of whatever. Roughly speaking, the existing 2TB drive set is using almost 1.9TB, with about 40GB available on the / and /home filesystems and about 90GB available on the old /vm. The copy of the root filesystem is roughly 30 minutes. The other copies are left for overnight.

`root #``mkdir /mnt/rawroot``root #``mount --bind / /mnt/rawroot``root #``cd /mnt/rawroot``root #``tar --xattrs --xattrs-include='*' cvpf - . | (cd /mnt/newroot; tar --xattrs xpf -)`
In this particular install, /var/cache/distfiles had been a softlink to a directory on the old /home filesystem. If it was to have been split off from a physical directory instead, the transfer from the new root subvolume's /var/cache/distfiles to the newdistfiles subvolume is effectively a move between filesystems that would involve a copy and then a delete. It would be more efficient to make judicious use of the -exclude switch on tar when doing the initial copy to the new mirror set.

#### Edit configuration on the new mirror

We edit the /etc/fstab on the new mirror set to reflect the way things should look when the new mirror set becomes the boot set. It is also probably cleaner to do a mountpoint for /distfiles as shown and then have /var/cache/distfiles be a softlink to it.

**`/etc/fstab`**

**as it appears on /mnt/newroot mirror**

```
/dev/md1                /boot           ext3    defaults,noatime                1 2
#/dev/md2		/		ext4		defaults,noatime	0 1
#/dev/md3		/home		ext4		defaults,noatime	0 1
#/dev/md4		/vm		ext4		defaults,noatime	0 1
LABEL=BTRFSMIRROR	/mnt/btrfsmirror	btrfs	defaults,noatime	0 0
LABEL=BTRFSMIRROR	/		btrfs	defaults,noatime,compress=lzo,autodefrag,subvol=root	0 0
LABEL=BTRFSMIRROR	/home		btrfs	defaults,noatime,compress=lzo,autodefrag,subvol=home	0 0
LABEL=BTRFSMIRROR	/distfiles	btrfs	defaults,noatime,autodefrag,subvol=distfiles	0 0
LABEL=BTRFSMIRROR	/vm		btrfs	defaults,noatime,compress=lzo,subvol=vm	0 0
```
We generate a new mdadm.conf file to include the new /dev/md5 /boot mirror but then edit it to rename that to /dev/md1. The other existing arrays are stubbed out, but the information is there in case the old mirror set is put back on again.

`root #``cd /mnt/newroot/etc``root #``mdadm --detail --scan >mdadm.conf`
**`/etc/mdadm.conf`**

**on /mnt/newroot edited as noted**

```
#ARRAY /dev/md1 metadata=0.90 UUID=a4e5e7b8:b2ce2666:78a9b883:5f5f67d5
#ARRAY /dev/md2 metadata=0.90 UUID=881a4c3b:3cc63489:78a9b883:5f5f67d5
#ARRAY /dev/md3 metadata=1.2 name=whatever:3 UUID=9be86c4f:2b0a9e82:b70f0ccc:1eb9b458
#ARRAY /dev/md4 metadata=1.2 name=whatever:4 UUID=3f51c3cd:64b9aefa:957dde49:e1130638
ARRAY /dev/md1 metadata=0.90 UUID=651f4200:d2f48834:c68863d8:e9942ea0
```
#### Creating the Initial Ram Filesystem

We will have to use an initial ram filesystem and an embedded init to mount the mirror set. Following the wiki entry for [Early Userspace Mounting](https://wiki.gentoo.org/wiki/Early_Userspace_Mounting), we create the following files in /usr/src/linux/initramfs.

**`initramfs_list`**

**files to go into the initial ram fs**

```
# directory structure with files required for sys-fs/btrfs-progs-4.3.1 and newer
#
# basic root directories
dir /proc       755 0 0
dir /usr        755 0 0
dir /usr/lib64  755 0 0
dir /bin        755 0 0
dir /sys        755 0 0
dir /var        755 0 0
dir /lib64      755 0 0
dir /sbin       755 0 0
dir /mnt        755 0 0
dir /mnt/root   755 0 0
dir /etc        755 0 0
dir /root       700 0 0
dir /dev        755 0 0
# busybox
file /bin/busybox		/bin/busybox		755 0 0
#
# fsck deps
#
file /sbin/fsck			/sbin/fsck		755 0 0
file /lib64/libmount.so.1	/lib64/libmount.so.1	755 0 0
file /lib64/libblkid.so.1	/lib64/libblkid.so.1	755 0 0
file /lib64/libc.so.6		/lib64/libc.so.6	755 0 0
file /lib64/libuuid.so.1	/lib64/libuuid.so.1	755 0 0
file /lib64/ld-linux-x86-64.so.2  /lib64/ld-linux-x86-64.so.2 755 0 0
#
#  fsck.ext4 and added deps
#
file /sbin/fsck.ext4		/sbin/fsck.ext4		755 0 0
file /lib64/libext2fs.so.2	/lib64/libext2fs.so.2	755 0 0
file /lib64/libcom_err.so.2	/lib64/libcom_err.so.2	755 0 0
file /lib64/libe2p.so.2		/lib64/libe2p.so.2	755 0 0
file /lib64/libpthread.so.0	/lib64/libpthread.so.0	755 0 0
#
# btrfs utils and added deps
#
file /sbin/btrfs		/sbin/btrfs		755 0 0
file /sbin/btrfs-convert	/sbin/btrfs-convert	755 0 0
file /sbin/btrfs-find-root	/sbin/btrfs-find-root	755 0 0
file /sbin/btrfs-image		/sbin/btrfs-image	755 0 0
file /sbin/btrfs-map-logical	/sbin/btrfs-map-logical	755 0 0
file /sbin/btrfsck		/sbin/btrfsck		755 0 0
file /sbin/btrfstune		/sbin/btrfstune		755 0 0
file /sbin/mkfs.btrfs		/sbin/mkfs.btrfs	755 0 0
file /lib64/libz.so.1		/lib64/libz.so.1	755 0 0
file /lib64/liblzo2.so.2	/lib64/liblzo2.so.2	755 0 0
# if btrfs-progs are compiled with USE=zstd
file /usr/lib64/libzstd.so.1	/usr/lib64/libzstd.so.1	755 0 0
#
#  init script
#
file    /init                   /usr/src/linux/initramfs/init             755 0 0
#
#  fstab
#
file	/etc/fstab		/usr/src/linux/initramfs/fstab	644 0 0
```
There's a bit more than minimally necessary there to mount and pivot the root, but it allows us to use the rescue shell in busybox to fsck /boot as necessary and to do btrfs scrub and balance on a cold filesystem if we feel it is necessary. The following minimalist fstab is the key reason for this initial ram filesystem. It allows btrfs to locate the root volume by label name and to enable compression and autodefrag on the initial mount.

**`fstab`**

**minimal fstab for /mnt/root**

```
/dev/md1		/boot		ext3		defaults,noatime	1 2
LABEL=BTRFSMIRROR	/mnt/root	btrfs	defaults,noatime,compress=lzo,autodefrag,subvol=root	0 0
```
**`init`**

**initramfs init script**

```
#!/bin/busybox sh
export PATH="/sbin:/bin:/usr/sbin:/usr/bin:${PATH}"
rescue_shell() {
    echo "$@"
    echo "Something went wrong. Dropping you to a shell."
    busybox --install -s
    exec /bin/sh
}
mount_root() {
    echo "scanning for btrfs filesystems.... will take about 5-10 seconds"
#
#  earlier versions of btrfs required devices on scan.  As of around 0.19.11 or so this now is a syntax error
#
#    btrfs device scan /dev/sda /dev/sdb
    btrfs device scan
    echo "mounting /mnt/root"
    busybox mount /mnt/root
}
check_filesystem() {
    # most of code coming from /etc/init.d/fsck
    local fsck_opts= check_extra= RC_UNAME=$(busybox uname -s)
    # FIXME : get_bootparam forcefsck
    if [ -e /forcefsck ]; then
        fsck_opts="$fsck_opts -f"
        check_extra="(check forced)"
    fi
    echo "Checking local filesystem $check_extra : $1"
    if [ "$RC_UNAME" = Linux ]; then
        fsck_opts="$fsck_opts -C0 -T"
    fi
    trap : INT QUIT
    # using our own fsck, not the builtin one from busybox
    fsck -p $fsck_opts $1
    case $? in
        0)      return 0;;
        1)      echo "Filesystem repaired"; return 0;;
        2|3)    if [ "$RC_UNAME" = Linux ]; then
                        echo "Filesystem repaired, but reboot needed"
                        busybox reboot -f
                else
                        rescue_shell "Filesystem still have errors; manual fsck required"
                fi;;
        4)      if [ "$RC_UNAME" = Linux ]; then
                        rescue_shell "Fileystem errors left uncorrected, aborting"
                else
                        echo "Filesystem repaired, but reboot needed"
                        busybox reboot
                fi;;
        8)      echo "Operational error"; return 0;;
        12)     echo "fsck interrupted";;
        *)      echo "Filesystem couldn't be fixed";;
    esac
    rescue_shell
}
# temporarily mount proc and sys
busybox mount -t proc none /proc
busybox mount -t sysfs none /sys
busybox mount -t devtmpfs none /dev
# disable kernel messages from popping onto the screen
echo 0 > /proc/sys/kernel/printk
# clear the screen
busybox clear
# mounting rootfs on /mnt/root
mount_root || rescue_shell "Error with uuidlabel_root"
echo "All done. Switching to real root."
# clean up. The init process will remount proc sys and dev later
busybox umount /proc
busybox umount /sys
busybox umount /dev
# switch to the real root and execute init
exec /bin/busybox switch_root /mnt/root /sbin/init
```
The init script here was essentially stolen from the early usermount page and has been hacked up a bit. It could probably stand some more cleaning and maybe additional smarts. One might notice that the root and rootflags parameters in the grub.conf appearing below are superfluous because they will be ignored by the init script. However it is useful to show what mount options are being used and how the mount happens at a glance. The actual initramfs creation is done as follows.

`root #``cd /usr/src/linux``root #``scripts/gen_initramfs_list.sh -o /mnt/altboot/initrd_btrfs.cpio.gz /usr/src//linux/initramfs/initramfs_list`
**`grub.conf`**

**updated grub.conf on /mnt/altboot**

```
default 0
timeout 6
splashimage=(hd0,0)/boot/grub/splash.xpm.gz
title Gentoo Linux 3.8.13-gentoo
root (hd0,0)
kernel /boot/kernel-3.8.13-gentoo root=LABEL=BTRFSMIRROR rootflags=defaults,noatime,compress=lzo,autodefrag,subvol=root selinux=0
initrd /boot/initrd_btrfs.cpio.gz
```
It would be a good idea to balance the new btrfs filesystems before booting into the new mirror set. It is not crucial to do it now, but it will speed up performance of the initial boot. The balance can just as easily be done on the live volumes after the new mirror set is booted. Balancing everything on the new 2tb set will probably take a good bit of an overnight depending on the amount of space used.

`root #``btrfs fi balance /mnt/newroot``root #``btrfs fi balance /mnt/newhome``root #``btrfs fi balance /mnt/newdistfiles``root #``btrfs fi balance /mnt/newvm`
#### Booting the new system

We take out the old mirror set after poweroff and put them into safe storage. The new mirror set is moved to become /dev/sda and /dev/sdb.

Once GRUB has booted into the kernel, the initial set of system messages display for detecting disk drives, etc. The screen will clear and appear to hang for about 5 to 10 seconds as btrfs scans for system devices. If the system has a cdrom drive, ignore any warnings about media not being present in /dev/sr0. Then it will continue to boot after switching in to the real root as noted in the early userspace mounting entry.

If the new btrfs filesystems has not been balanced, performance may be a bit sluggish initially, but things will speed up. For this example exercise, the most significant thing to notice is the amount of space now available for use. It is also all part of the same pool shared by the subvolumed filesystems:

`root #``df`
Filesystem      1K-blocks       Used  Available Use% Mounted on
rootfs         3906547216 2506997920 1397527976  65% /
/dev/sda2      3906547216 2506997920 1397527976  65% /
tmpfs            12367228       1028   12366200   1% /run
udev                10240          4      10236   1% /dev
shm              12367228        608   12366620   1% /dev/shm
cgroup\_root         10240          0      10240   0% /sys/fs/cgroup
cachedir             4096          4       4092   1% /lib64/splash/cache
/dev/md1           233225      55809     165375  26% /boot
/dev/sda2      3906547216 2506997920 1397527976  65% /mnt/btrfsmirror
/dev/sda2      3906547216 2506997920 1397527976  65% /home
/dev/sda2      3906547216 2506997920 1397527976  65% /vm
/dev/sda2      3906547216 2506997920 1397527976  65% /distfiles

Most of the space savings probably came from compression of the kvm guest images that were in /vm, but /home and the system root also contribute significant savings.

`root #``grep btrfs /etc/mtab`
/dev/sda2 / btrfs rw,noatime,compress=lzo,space\_cache,autodefrag 0 0
/dev/sda2 /mnt/btrfsmirror btrfs rw,noatime 0 0
/dev/sda2 /home btrfs rw,noatime,compress=lzo,autodefrag,subvol=home 0 0
/dev/sda2 /vm btrfs rw,noatime,compress=lzo,subvol=vm 0 0
/dev/sda2 /distfiles btrfs rw,noatime,autodefrag,subvol=distfiles 0 0

## Troubleshooting

### Mounting rootfs fails when using btrfs RAID

In some cases the booting of a btrfs root volume that has a mirroring(btrfs raid-1) and lzo compression activated, the initramfs mount of the root volume fails with:

cannot mount /dev/sda3:
"Invalid argument".

if you drop to the shell and look to the dmesg output it shows

open\_ctree failed

I was able to correct the boot with adding appropriate **rootflags=device=/dev/sda3,device=/dev/sdb3** and **rootfstype=btrfs** to the kernel cmdline:

**`grub.conf`**

**updated grub.conf on /mnt/altboot**

```
default 0
timeout 6
splashimage=(hd0,0)/boot/grub/splash.xpm.gz
title Gentoo Linux 3.18.4
root (hd0,0)
kernel /boot/kernel-3.18.4 root=LABEL=root rootflags=device=/dev/sda3,device=/dev/sdb3,defaults,noatime,compress=lzo rootfstype=btrfs
initrd /boot/initrd_btrfs.cpio.gz
```
