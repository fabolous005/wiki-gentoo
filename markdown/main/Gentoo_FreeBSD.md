<!-- source: https://wiki.gentoo.org/wiki/Gentoo_FreeBSD | group: Gentoo Wiki (Main) | wiki-title: Gentoo FreeBSD -->
---
title: Gentoo FreeBSD
url: https://wiki.gentoo.org/wiki/Gentoo_FreeBSD
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-11-25"
fingerprint: dcbcbc7b7cc6d285
license: CC BY-SA 4.0
---

# Gentoo FreeBSD

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **archived (obsolete)**. Contents are surely incorrect for current usage, and are intended for historical reference only.

TLDR:

**Do not use this article!**

Gentoo FreeBSD was a Gentoo system based on the FreeBSD kernel. Gentoo FreeBSD is no longer maintained by any active Gentoo developers.

Gentoo FreeBSD was FreeBSD with the following changes:

- Portage replaces ports.
- The base system is managed by Portage.
- Software in ./contrib and ./crypto is replaced with Gentoo versions.
- The rc system is replaced with [OpenRC](https://wiki.gentoo.org/wiki/OpenRC).
- python, wget, and bash are included in the base system to support Portage.
- FreeBSD's bootloader is removed from the base system (useful for jails).

## Introduction to FreeBSD

### What is FreeBSD?

[FreeBSD](https://www.freebsd.org/) is a free ([license](https://www.freebsd.org/copyright/freebsd-license.html)) Unix-like operating system. Back in 1993 when development of [386BSD](https://www.386bsd.org/) stopped, two projects were born: [NetBSD](https://www.netbsd.org/), commonly known to run on a huge number of architectures, and FreeBSD which supports the x86, amd64, ia64, sparc64, and alpha platforms. FreeBSD is renowned for its stability, performance and security, thus being used from small to huge companies all over the world.

FreeBSD's current production release is version 11.0. Gentoo/FreeBSD is based on version 11.0 and older versions of Gentoo/FreeBSD are discontinued and no longer supported.

### What is Gentoo/FreeBSD?

Gentoo FreeBSD project was a sub-project of the [Prefix project](https://wiki.gentoo.org/wiki/Project:Prefix), with the goal of providing a fully-capable FreeBSD operating system featuring design sensibilities taken from Gentoo Linux, such as the init system and the Portage package management system.

### FreeBSD and Linux

Users migrating from Linux to FreeBSD commonly consider the two operating systems "almost the same". In fact, FreeBSD really shares a lot of similarities with Linux distributions in general. Nevertheless, it has some key differences that are worth noting:

- Contrary to Linux, which actually only refers to the kernel, FreeBSD is a complete operating system, consisting of a C library, userland tools, and much more. This development approach makes the overall system very consistent.
- Contrary to the Linux kernel, FreeBSD development is not led by one person, but instead managed by a small group of people called the [Core Team](https://www.freebsd.org/doc/en_US.ISO8859-1/articles/contributors/staff-committers.html).

Besides, FreeBSD also has some technical differences which set it apart from Linux. Some of them are very important to know, even for those who do not plan on joining the Gentoo FreeBSD development effort:

- To get run-time dynamic linking functions like `dlopen()`, programs do not need to be linked against libdl like on GNU/Linux. Instead they are linked against libc.
- FreeBSD doesn't have an official tool for kernel compilation, thus you'll have to resolve feature dependencies by hand.
- FreeBSD supports [UFS](https://wiki.gentoo.org/wiki/UFS)/[UFS2](https://wiki.gentoo.org/wiki/UFS2) and [ZFS](https://wiki.gentoo.org/wiki/ZFS) as native file systems.
- FreeBSD also supports Linux filesystems, but you cannot install your system on them. ext2/ext3 supports read/write access. ReiserFS and [XFS](https://wiki.gentoo.org/wiki/XFS) are read-only support. For details see FreeBSD's handbook [21.3. Linux® Filesystems](https://www.freebsd.org/doc/handbook/filesystems-linux.html)

## Installation

### Caution

This guide provides minimal information about Gentoo FreeBSD. You need specialized knowledge of Gentoo Linux and FreeBSD. The [Gentoo Linux Handbook](https://wiki.gentoo.org/wiki/Handbook:Main_Page) and [FreeBSD Handbook](https://www.freebsd.org/handbook/) should be close friends.

### Preparation

Gentoo/FreeBSD doesn't have the installation media. Instead, you can use vanilla FreeBSD 9.0 or later version one.
Please get [i386](http://ftp-archive.freebsd.org/pub/FreeBSD-Archive/old-releases/i386/ISO-IMAGES/9.0/) or [amd64](http://ftp-archive.freebsd.org/pub/FreeBSD-Archive/old-releases/amd64/ISO-IMAGES/9.1/) from ftp-archive.freebsd.org. Some ISO images are available from the ftp server, but please get -bootonly.iso image. Because you can install Gentoo/FreeBSD with the smallest media. If you want to use a memory stick, please get -memstick.img. If necessary, burn it to a CD or write it to a memory stick.

Boot your PC with the installation media. Wait for a Welcome dialog display. Please choose Shell.

#### Optional: Setting keymap

If the keyboard is not an English 101 key keyboard, set the keymap as appropriate:

`root #``kbdcontrol -l uk.iso`
A list of the keymap is available with the following command:

`root #``ls -1 /usr/share/syscons/keymaps | less`
#### Network setting

First, create a directory /tmp/bsdinstall\_etc to rewrite the resolv.conf file:

`root #``mkdir -p /tmp/bsdinstall_etc`
Identify the system's network devices. In this example the network interface is `em0`:

`root #``ifconfig````
em0: flags=8843<UP,BROADCAST,RUNNING,SIMPLEX,MULTICAST> metric 0 mtu 1500
        options=209b<RXCSUM,TXCSUM,VLAN_MTU,VLAN_HWTAGGING,VLAN_HWCSUM,WOL_MAGIC>
        ether 00:00:00:00:00:00
        nd6 options=29<PERFORMNUD,IFDISABLED,AUTO_LINKLOCAL>
        media: Ethernet autoselect (1000baseT <full-duplex>)
        status: active
lo0: flags=8049<UP,LOOPBACK,RUNNING,MULTICAST> metric 0 mtu 16384
        options=3<RXCSUM,TXCSUM>
        inet6 ::1 prefixlen 128
        inet6 fe80::1%lo0 prefixlen 64 scopeid 0x3
        inet 127.0.0.1 netmask 0xff000000
        nd6 options=21<PERFORMNUD,AUTO_LINKLOCAL>
```
Set the default gateway and IP address.

When setting a static address:

`root #````
ifconfig em0 192.168.0.100
```
`root #````
route add default 192.168.0.1
```
`root #````
echo "nameserver 8.8.8.8" > /etc/resolv.conf
```
When using DHCP:

`root #``dhclient em0`
#### Optional: Installing via ssh

`root #````
mkdir -p /tmp/etc
```
`root #````
mount -t tmpfs tmpfs /tmp/etc
```
`root #````
mount -t unionfs /tmp/etc /etc
```
`root #````
passwd
```
`root #````
echo "PermitRootLogin yes" >> /etc/ssh/sshd_config
```
`root #````
/etc/rc.d/sshd onestart
```
Please connect with any ssh client. The username is root.

#### Setting the partition

You need to find out the device name to install. You can get it with `ls /dev/*da*` or `dmesg | grep 'da[0-9]'`. In this case, it's ada0:

`root #``dmesg | grep 'da[0-9]'`
ada0 at ata0 bus 0 scbus0 target 0 lun 0
ada0: \<QEMU HARDDISK 0.15.> ATA-7 device
ada0: 16.700MB/s transfers (WDMA2, PIO 8192bytes)
ada0: 12288MB (25165824 512 byte sectors: 16H 63S/T 16383C)
ada0: Previously was known as ad0

##### Using the UFS2 file system (GPT)

If you follow this example, you will create two simple partitions. If necessary, you can split /, /usr, /var, /tmp, and /home. Please set these with gpart.

| Partition | Filesystem | Size | Description | 
|---|---|---|---|
| /dev/ada0p1 | none | just a bit | Reserved for bootloader | 
| /dev/ada0p2 | UFS2 + SU | entire disk - swap space | Root partition | 
| /dev/ada0p3 | swap | Rest of the disk | Swap partition | 

###### Using FreeBSD's default bootloader (recommended)

`root #````
gpart create -s GPT ada0
```
`root #````
gpart add -s 64k -t freebsd-boot ada0
```
`root #````
gpart add -s 10G -t freebsd-ufs -l root ada0
```
`root #````
gpart add -t freebsd-swap -l swap ada0
```
Please run gpart show in order to confirm results.

`root #``gpart show````
=>      34  25165757  ada0  GPT  (12G)
        34       128     1  freebsd-boot  (64k)
       162  20971520     2  freebsd-ufs  (10G)
  20971682   4194109     3  freebsd-swap  (2G)
```
Results when you create a partition with option -a 4k.

`root #``gpart show````
=>      34  25165757  ada0  GPT  (12G)
        34         6        - free -  (3.0k)
        40       128     1  freebsd-boot  (64k)
       168  20971520     2  freebsd-ufs  (10G)
  20971688   4194096     3  freebsd-swap  (2G)
  25165784         7        - free -  (3.5k)
```
###### Using grub2. \[Experimental\]

`root #````
gpart create -s GPT ada0
```
`root #````
gpart add -s 1024k -t bios-boot ada0 
```
`root #````
gpart add -s 10G -t freebsd-ufs -l root ada0
```
`root #````
gpart add -t freebsd-swap -l swap ada0
```
##### Using the UFS2 file system (MBR)

| Partition | Filesystem | Size | Description | 
|---|---|---|---|
| /dev/ada0s1a | UFS2 + SU | entire disk - swap space | Root partition | 
| /dev/ada0s1b | swap | Rest of the disk | Swap partition | 

###### Using FreeBSD's default bootloader (recommended)

`root #````
gpart create -s mbr ada0
```
`root #````
gpart add -t freebsd ada0
```
`root #````
gpart set -a active -i 1 ada0
```
`root #````
gpart create -s bsd ada0s1
```
`root #````
gpart add -s 10G -t freebsd-ufs ada0s1
```
`root #````
gpart add -t freebsd-swap ada0s1
```
###### Using GRUB2 (experimental)

`root #````
gpart create -s mbr ada0
```
`root #````
gpart add -b 1024k -t freebsd ada0
```
`root #````
gpart set -a active -i 1 ada0
```
`root #````
gpart create -s bsd ada0s1
```
`root #````
gpart add -s 10G -t freebsd-ufs ada0s1
```
`root #````
gpart add -t freebsd-swap ada0s1
```
##### Using the ZFS file system (experimental) / (GPT)

###### Using FreeBSD's default bootloader (recommended)

`root #````
gpart create -s GPT ada0
```
`root #````
gpart add -s 64k -t freebsd-boot ada0
```
`root #````
gpart add -t freebsd-zfs -l zfs-system ada0
```
###### Using GRUB2 (experimental)

`root #````
gpart create -s GPT ada0
```
`root #````
gpart add -s 1024k -t bios-boot ada0
```
`root #````
gpart add -t freebsd-zfs -l zfs-system ada0
```
#### Creating and mounting a file system

##### Using the UFS2 file system

Please don't forget newfs `-L` option to label.

Your choice GPT:

`root #``newfs -L gfbsdroot -U /dev/ada0p2`
Your choice MBR:

`root #``newfs -L gfbsdroot -U /dev/ada0s1a`
And mount it:

`root #````
mount /dev/ufs/gfbsdroot /mnt
```
`root #````
swapon /dev/gpt/swap (GPT)
```
`root #````
swapon /dev/ada0s1b (MBR)
```
##### Using the ZFS file system (experimental)

Load the kernel module in order to enable ZFS:

`root #``kldload zfs`
Prepare the RAM disk to write /boot/zfs/zpool.cache:

`root #``mount -t tmpfs tmpfs /boot/zfs`
Please use gnop if you have 4KB/sector HDD. You can get the best performance:

`root #``gnop create -S 4096 /dev/gpt/zfs-system`
Creates a new storage pool. When using gnop, please specify /dev/gpt/zfs-system.nop instead of /dev/gpt/zfs-system.

`root #````
zpool create zsys /dev/gpt/zfs-system
```
`root #``zfs set mountpoint=none zsys`
Creates a new ZFS file system:

`root #````
zfs create zsys/root
```
`root #````
zfs set mountpoint=/mnt zsys/root
```
`root #````
zfs mount -a
```
`root #````
zfs create zsys/root/usr
```
`root #``zfs create zsys/root/var``root #````
zfs create zsys/root/tmp
```
`root #````
zfs create zsys/home
```
`root #````
zfs create -V 8gb zsys/swap
```
`root #````
zfs set org.freebsd:swap=on zsys/swap
```
`root #````
zfs set checksum=off zsys/swap
```
`root #````
mkdir /mnt/home
```
`root #````
mount -t zfs zsys/home /mnt/home
```
#### Downloading the stage 3 file and Portage snapshot

Now, Gentoo/FreeBSD 9.0 [stage 3](https://wiki.gentoo.org/index.php?title=Stage_3&action=edit&redlink=1) is available for x86-fbsd. And Gentoo FreeBSD 9.1 [stage 3](https://wiki.gentoo.org/index.php?title=Stage_3&action=edit&redlink=1) is available for amd64-fbsd.

`root #````
cd /mnt
```
`root #``fetch` [http://dev.gentoo.org/~aballier/fbsd9.0/x86/stage3-i686-freebsd-9.0.tar.bz2](http://dev.gentoo.org/~aballier/fbsd9.0/x86/stage3-i686-freebsd-9.0.tar.bz2) (for x86-fbsd users)
#### Unpacking stage3 and Portage snapshot

`root #````
cd /mnt
```
`root #````
setenv LANG "en_US.UTF-8"
```
`root #````
tar xjpf stage3-*.tar.bz2 -C /mnt
```
`root #````
tar xjf portage-latest.tar.bz2 -C /mnt/usr
```
#### Entering the new environment

`root #````
mount -t devfs devfs /mnt/dev
```
`root #````
cp /etc/resolv.conf /mnt/etc/
```
`root #``chroot /mnt /bin/bash`
### Settings and install more in chroot environment

#### The first settings and time zone settings

env-update and time zone settings. Of course, you can modify /etc/portage/make.conf.

`(chroot) root #````
env-update && source /etc/profile
```
`(chroot) root #````
cp /usr/share/zoneinfo/GMT /etc/localtime (please select your time zone)
```
`(chroot) root #````
emerge -u sys-apps/portage ; emerge sys-devel/libtool app-admin/eselect
```
`(chroot) root #````
emerge app-editors/nano
```
`(chroot) root #````
mv /etc/make.conf /etc/portage/ (if /etc/make.conf is exist.)
```
`(chroot) root #````
nano -w /etc/portage/make.conf (if necessary)
```
#### Bug fixes (for x86-fbsd users, 9.0 stage 3 only)

Bugs have been fixed in several packages. Please upgrade them. For details, see [bug #324445](https://bugs.gentoo.org/show_bug.cgi?id=324445), [bug #412319](https://bugs.gentoo.org/show_bug.cgi?id=412319), [bug #417031](https://bugs.gentoo.org/show_bug.cgi?id=417031), and [bug #421191](https://bugs.gentoo.org/show_bug.cgi?id=421191).

`(chroot) root #````
emerge sys-devel/gcc-config
```
`(chroot) root #````
emerge sys-freebsd/freebsd-mk-defs sys-freebsd/freebsd-lib app-arch/libarchive dev-libs/libedit
```
#### Special settings for ZFS

If you select ZFS file system, the following additional work is required.

`(chroot) root #````
emerge flaggie
```
`(chroot) root #````
flaggie +zfs
```
`(chroot) root #````
emerge sys-devel/flex
```
`(chroot) root #````
emerge sys-freebsd/freebsd-cddl
```
`(chroot) root #````
emerge sys-freebsd/freebsd-lib sys-freebsd/freebsd-ubin sys-freebsd/boot0
```
#### Installing a kernel source

Unlike Gentoo/Linux, freebsd-sources package is the only sources.

`(chroot) root #````
USE=symlink emerge freebsd-sources
```
#### Compiling and installing kernel

Now let's compile the kernel.
For more information see the [FreeBSD Handbook](https://www.freebsd.org/doc/handbook/kernelconfig.html).

`(chroot) root #````
emerge -u sys-devel/flex
```
`(chroot) root #````
cd /usr/src/sys/i386/conf (case of x86-fbsd)
```
`(chroot) root #````
cd /usr/src/sys/amd64/conf (case of amd64-fbsd)
```
`(chroot) root #````
cp GENERIC.hints /boot/device.hints
```
`(chroot) root #````
config GENERIC
```
`(chroot) root #````
cd ../compile/GENERIC
```
`(chroot) root #````
make cleandepend && make depend && make -j3 && make install
```
#### System settings

Familiar settings. Please refer to [Gentoo Handbook](https://wiki.gentoo.org/wiki/Gentoo_Handbook) for more information.

When selecting UFS2:

`(chroot) root #``nano -w /etc/fstab`
\# Device	Mountpoint	FStype	Options	Dump	Pass#
/dev/ufs/gfbsdroot	/		ufs	rw	1	1
#your choice GPT (don't forget comment out)
#/dev/gpt/swap	none		swap	sw	0	0
#your choice MBR (don't forget comment out)
#/dev/ada0s1b	none		swap	sw	0	0

If you select ZFS. Note that the Pass# column should be zero for ZFS, because it cannot be checked by fsck.

`(chroot) root #``nano -w /etc/fstab`
\# Device	Mountpoint	FStype	Options	Dump	Pass#
#/dev/ad0s1a	/		ufs	rw	1	1
#/dev/ad0s1b	none		swap	sw	0	0
zsys/root	/		zfs	rw	0	0

Setting the host name

`(chroot) root #``nano -w /etc/conf.d/hostname`
\# (Set the hostname variable to your host name.  On BSD this is expected to be the FQDN.)
hostname="daemon.homenetwork"

Editing network settings

`(chroot) root #``nano -w /etc/conf.d/net`
\# (Set the dns\_domain variable to your domain name)
dns\_domain\_lo="homenetwork"
# Manually setting)
config\_em0="192.168.0.100 netmask 255.255.255.0"
routes\_em0="default via 192.168.0.1"
# Using DHCP)
config\_em0="dhcp"

Adding net.em0 to the default runlevel

`(chroot) root #````
cd /etc/init.d
```
`(chroot) root #````
ln -s net.lo0 net.em0
```
`(chroot) root #````
rc-update add net.em0 default
```
Setting keymap

Please edit /etc/conf.d/syscons if you want to change the keymap.

`(chroot) root #````
nano -w /etc/conf.d/syscons
```
A list of the keymap is available with the following command.

`(chroot) root #````
ls -1 /usr/share/syscons/keymaps | less
```
Setting the root password

`(chroot) root #````
passwd
```
Adding a user for daily use (sample)

`(chroot) root #````
adduser
```
Username: yournickname
Full name:
Uid (Leave empty for default):
Login group \[yournickname\]:
Login group is yournickname. Invite yournickname into other groups? \[\]: wheel
Login class \[default\]:
Shell (bash csh esh fish ksh sash sh tcsh zsh nologin) \[sh\]: bash
Home directory \[/home/yournickname\]:
Home directory permissions (Leave empty for default):
Use password-based authentication? \[yes\]:
Use an empty password? (yes/no) \[no\]:
Use a random password? (yes/no) \[no\]:
Enter password:
Enter password again:
Lock out the account after creation? \[no\]:
Username   : yournickname
Password   : \*\*\*\*\*
Full Name  :
Uid        : 1001
Class      :
Groups     : yournickname wheel
Home       : /home/yournickname
Home Mode  :
Shell      : /bin/bash
Locked     : no
OK? (yes/no): yes
adduser: INFO: Successfully added (yournickname) to the user database.
Add another user? (yes/no): no
Goodbye!

You can run at startup sshd:

`(chroot) root #````
emerge -u net-misc/openssh
```
`(chroot) root #````
rc-update add sshd default
```
##### Special settings for ZFS

When using the ZFS file system, the following additional work is required:

`(chroot) root #````
rc-update add zfs boot
```
`(chroot) root #````
rc-update add zvol default
```
`(chroot) root #````
echo 'zfs_load="YES"' >> /boot/loader.conf
```
`(chroot) root #````
echo 'vfs.root.mountfrom="zfs:zsys/root"' >> /boot/loader.conf
```
##### Special settings for virtual machine

When installing on a virtual machine, the following additional configuration is required:

`(chroot) root #````
echo 'kern.hz=100' >> /boot/loader.conf
```
#### Installing the required packages

To use DHCP, [net-misc/dhcpcd](https://packages.gentoo.org/packages/net-misc/dhcpcd) is required:

`(chroot) root #````
emerge net-misc/dhcpcd
```
#### Installing the boot loader

Install a boot loader to boot the Gentoo FreeBSD:

`(chroot) root #````
emerge sys-freebsd/boot0
```
Please see [this section](https://wiki.gentoo.org/wiki/Gentoo_FreeBSD/Experimental#How_to_use_sys-boot.2Fgrub2) if you want to use GRUB2. But it is experimental!

#### Setting boot loader

Let's prepare for using [sys-freebsd/boot0](https://packages.gentoo.org/packages/sys-freebsd/boot0).

If you select UFS2 (GPT):

`(chroot) root #````
gpart bootcode -b /boot/pmbr ada0
```
`(chroot) root #````
gpart bootcode -p /boot/gptboot -i 1 ada0
```
If you select UFS2 (MBR):

`(chroot) root #````
gpart bootcode -b /boot/boot0 ada0
```
`(chroot) root #````
gpart bootcode -b /boot/boot ada0s1
```
If you select ZFS (GPT):

`(chroot) root #````
gpart bootcode -b /boot/pmbr ada0
```
`(chroot) root #````
gpart bootcode -p /boot/gptzfsboot -i 1 ada0
```
#### Exiting the chroot

Exit from the chroot environment:

`(chroot) root #````
exit
```
### Cleaning up and setting the remaining

Do you want to reboot now? Just a minute. ZFS users must make few more setting changes.

#### Special settings for ZFS

If you select ZFS file system, the following additional work is required.

`root #````
umount /mnt/dev
```
`root #````
cd /
```
`root #````
zpool set bootfs=zsys/root zsys
```
`root #````
zpool export zsys
```
`root #``gnop destroy /dev/gpt/zfs-system.nop (please run if you are using gnop)``root #````
zpool import zsys
```
`root #````
mkdir /mnt/boot/zfs
```
`root #````
cp /boot/zfs/zpool.cache /mnt/boot/zfs/
```
`root #``zfs unmount -a``root #````
zfs set mountpoint=none zsys
```
`root #````
zfs set mountpoint=/usr zsys/root/usr
```
`root #````
zfs set mountpoint=/var zsys/root/var
```
`root #````
zfs set mountpoint=/tmp zsys/root/tmp
```
`root #````
zfs set mountpoint=/home zsys/home
```
`root #``zfs set mountpoint=legacy zsys/root`
#### Reboot!

No problems? You can now reboot. Don't forget to remove the installation media.

`root #``shutdown -r now`
## Running Gentoo FBSD in vanilla FreeBSD's jail

Details of jail, please see the [FreeBSD Handbook](https://www.freebsd.org/handbook/jails.html).

### Preparing directory

`root #````
mkdir -p /usr/jail/gfbsd
```
`root #``cd /usr/jail/gfbsd`
### Downloading and unpacking the stage 3 file and Portage snapshot

`root #``fetch` [http://dev.gentoo.org/~aballier/fbsd9.0/x86/stage3-i686-freebsd-9.0.tar.bz2](http://dev.gentoo.org/~aballier/fbsd9.0/x86/stage3-i686-freebsd-9.0.tar.bz2) (for x86-fbsd users)
`root #``fetch` [http://distfiles.gentoo.org/experimental/bsd/freebsd/stages/amd64-fbsd-9.1/stage3-amd64-freebsd-9.1.tar.bz2](http://distfiles.gentoo.org/experimental/bsd/freebsd/stages/amd64-fbsd-9.1/stage3-amd64-freebsd-9.1.tar.bz2) (for amd64-fbsd users)
`root #````
tar xjpf stage3-*.tar.bz2
```
`root #````
tar xjf portage-latest.tar.bz2 -C /usr/jail/gfbsd/usr
```
### Setting

Configure the jail to start with FreeBSD:

**`/etc/rc.conf`**

**Example for starting the jail at boot time**

```
jail_enable="YES"
jail_list="gfbsd"
jail_gfbsd_rootdir="/usr/jail/gfbsd"
jail_gfbsd_hostname="gfbsd.example.org"
jail_gfbsd_devfs_enable="YES"
# You will need to add the following line if you want network access.
# Please specify your network devices instead of em0.
jail_gfbsd_interface="em0"
jail_gfbsd_ip="192.168.0.10"
```
Setting to operate Gentoo/FreeBSD in the jail:

`root #````
echo 'rc_sys="jail"' >> /usr/jail/gfbsd/etc/rc.conf
```
`root #````
echo 'hostname="gfbsd"' > /usr/jail/gfbsd/etc/conf.d/hostname
```
`root #``echo 'syslogd_args="-ss"' >> /usr/jail/gfbsd/etc/conf.d/syslogd``root #``chroot /usr/jail/gfbsd /bin/bash``(chroot) root #````
rc-update add vixie-cron default
```
`(chroot) root #````
exit
```
### Run jail

Run jail:

`root #``/etc/rc.d/jail start`
You can check that it worked:

`root #``jls````
   JID  IP Address      Hostname                      Path
     1  192.168.0.10    gfbsd                         /usr/jail/gfbsd
```
Enter the jail environment with the jexec command:

`root #``jexec 1 /bin/bash`
## Developing for Gentoo/FreeBSD

### How to help

There are many things you could help with, depending on one's skill level and spare time:

- Working on current ebuilds: this means working closely with ebuild maintainers in order to create patches or modify ebuilds in a way that can be accepted into the Gentoo ebuild repository.
- Security: if you are into security, we need you! Although security advisories from the FreeBSD project are tracked and fixed, we can always use help in this area.
- Contacts: we need people who can get in touch with FreeBSD developers to maintain contacts between us and the original project to exchange patches and discuss various problems and their solutions. Note that this should never involve any kind of spamming of mailing lists or IRC channels.
- Testing: the more people are actively using Gentoo FreeBSD, the more bugs will be discovered, which helps us improving the quality of the port. If you are good at describing bugs or problems, we definitely want to hear from you.
- Other areas where we need help include: system ebuilds, creation of installation CDs, documentation, kernel hacking.

## Troubleshooting

### Architecture independent issues

#### Emerge fails app-misc/screen

For details see [bug #393039](https://bugs.gentoo.org/show_bug.cgi?id=393039) and [bug #409819](https://bugs.gentoo.org/show_bug.cgi?id=409819).

Please use [app-misc/tmux](https://packages.gentoo.org/packages/app-misc/tmux) instead of [app-misc/screen](https://packages.gentoo.org/packages/app-misc/screen).

#### lex: not found

`root #``emerge --ask sys-devel/flex`
#### cc: not found

For details see [bug #412319](https://bugs.gentoo.org/show_bug.cgi?id=412319):

`root #``emerge '>=sys-devel/gcc-config-1.7.1'`
## Contacts

- gentoo-bsd mailing list ([Gentoo Mailing Lists](https://archives.gentoo.org/gentoo-bsd/))
- IRC channel [#gentoo-bsd](ircs://irc.libera.chat/#gentoo-bsd) ([webchat](https://web.libera.chat/#gentoo-bsd)) on ~~Freenode~~ Libera Chat ([Gentoo Linux IRC channels list](https://gentoo.org/get-involved/irc-channels/all-channels.html))

## See also

- [Gentoo FreeBSD/Upgrade Guide](https://wiki.gentoo.org/wiki/Gentoo_FreeBSD/Upgrade_Guide) - A sub-article on upgrading a G/FBSD system.
- [Gentoo FreeBSD/Experimental](https://wiki.gentoo.org/wiki/Gentoo_FreeBSD/Experimental) - A sub-article that contains attempts to implement some uncommon features.
- [Gentoo FreeBSD/Developers Note](https://wiki.gentoo.org/wiki/Gentoo_FreeBSD/Developers_Note) - A sub-article of developers note.
- [Gentoo Alt/Contributor's Guide](https://wiki.gentoo.org/wiki/Gentoo_Alt/Contributor%27s_Guide) - A Gentoo Alt contributor's guide (deprecated).
- [Project:Gentoo/Staffing\_Needs](https://wiki.gentoo.org/wiki/Project:Gentoo/Staffing_Needs) - An article that lists the ways users can help.
- [Gentoo Linux Handbook](https://wiki.gentoo.org/wiki/Handbook:Main_Page)

## External resources
