<!-- source: https://wiki.gentoo.org/wiki/Gentoo_FreeBSD/Install_Guide | group: Gentoo Wiki (Main) | wiki-title: Gentoo FreeBSD/Install Guide -->
---
title: Gentoo FreeBSD/Install Guide
url: https://wiki.gentoo.org/wiki/Gentoo_FreeBSD/Install_Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-11-22"
fingerprint: "948df85b8fe78bed"
license: CC BY-SA 4.0
---

# Gentoo FreeBSD/Install Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Archived article**

This article is **archived (obsolete)**. Contents are surely incorrect for current usage, and are intended for historical reference only.

TLDR:

**Do not use this article!**


This document provides general information on FreeBSD, as well as installation instructions for FreeBSD. It also includes some reference for people interested in helping out with development.

## Introduction

### What is FreeBSD?

[FreeBSD](http://www.freebsd.org/) is a free ([license](http://www.freebsd.org/copyright/freebsd-license.html)) Unix-like operating system. Back in 1993 when development of [386BSD](http://www.386bsd.org/) stopped, two projects were born: [NetBSD](http://www.netbsd.org/), commonly known to run on a huge number of architectures, and FreeBSD which supports the **x86**, **amd64**, **ia64**, **sparc64** and **alpha** platforms. FreeBSD is renowned for its stability, performance and security, thus being used from small to huge companies all over the world.

FreeBSD's current production release is version 6.2. Gentoo/FreeBSD is based on this version and older versions of Gentoo/FreeBSD are discontinued and no longer supported.

### What is Gentoo/FreeBSD?

[Gentoo FreeBSD](https://wiki.gentoo.org/wiki/Gentoo_FreeBSD) is a sub-project of the [Prefix project](https://wiki.gentoo.org/wiki/Project:Prefix), with the goal of providing a fully-capable FreeBSD operating system featuring design sensibilities taken from Gentoo Linux, such as the init system and the Portage package management system.

### FreeBSD and Linux

Users migrating from Linux to FreeBSD commonly consider the two operating systems "almost the same". In fact, FreeBSD really shares a lot of similarities with Linux distributions in general. Nevertheless, it has some key differences that are worth noting:

- Contrary to Linux, which actually only refers to the kernel, FreeBSD is a complete operating system, consisting of a C library, userland tools and much more. This development approach makes the overall system very consistent.
- Contrary to the Linux kernel, FreeBSD development is not led by one person, but instead managed by a small group of people called the Core Team.

Besides, FreeBSD also has some technical differences which set it apart from Linux. Some of them are very important to know, even if you don't plan on joining the Gentoo/FreeBSD development effort:

- To get run-time dynamic linking functions like `dlopen()`, programs do not need to be linked against libdl like on GNU/Linux. Instead they are linked against libc.
- FreeBSD doesn't have an official tool for kernel compilation, thus you'll have to resolve feature dependencies on your own.
- FreeBSD uses UFS/UFS-2 as its filesystems and has no official support for e.g. XFS. However, there are projects for adding read-only support for these filesystems. Accessing ext2/ext3 partitions is already possible, but you cannot install your system on them.

## Installation

### Booting the CD

After this short introduction, it's about time to finally install Gentoo/FreeBSD. Unfortunately, we currently lack our own installation media, however there are two alternative installation methods from which to choose. The first would be to use an existing FreeBSD installation to partition your hard drive and use it as a base for installing Gentoo/FreeBSD. This guide will describe how to use the [FreeSBIE LiveCD](http://www.freesbie.org/) as an installation medium for Gentoo/FreeBSD.

First, boot the CD in order to begin the installation process. You'll be presented with a login screen. The username is freesbie, and there is no password. Next, run `sudo su` to become root, and optionally setup a password. If you want to pass time during the installation process, you can run `startx` to enter into an Xfce environment, suitable for web browsing, AIM, and other things. Unlike Linux, FreeBSD bases the name of your interface on the driver for the interface. For example, the Intel EtherExpress driver (fxp) appears as fxp0 (driver fxp, first network card). To see what your interface is, use `ifconfig`:

`root #``ifconfig````
fxp0: flags=8843<UP,BROADCAST,RUNNING,SIMPLEX,MULTICAST> mtu 1500
        options=8<VLAN_MTU>
        inet6 fe80::2d0::b7ff:febc:4fe3%fxp0 prefixlen 64 scopeid 0x1
        inet 192.168.0.106 netmask 0xffffff00 broadcast 192.168.0.255
        ether 00:d0:b7:bc:4f:e3
        media: Ethernet autoselect (100baseTX <full-duplex>)
        status: active
lo0: flags=8007<LOOPBACK,MULTICAST> mtu 16384
```
If the original DHCP request during the CD bootup failed, you can use the `dhclient` command to obtain an IP address:

`root #``dhclient fxp0`
DHCPDISCOVER on fxp0 to 255.255.255.255 port 67 interval 9
DHCPOFFER from 192.168.0.1
DHCPREQUEST on fxp0 to 255.255.255.255 port 67
DHCPACK from 192.168.0.1
bound to 192.168.0.106 -- renewal in 302400 seconds

### Partitioning the drive

Now that we have a mount point, it's time to partition the drive. This is done with the `sysinstall` command:

`root #``sysinstall diskPartitionEditor diskPartitionWrite`
We recommend that you use the default layout. Press enter at the dialog, then press a followed by q to accept the default layout. The next screen will present you with the option of a bootloader. For this option, choose "None" as we'll be installing the bootloader later on. Next comes the actual partition sizing and mount points.

The next step also uses `sysinstall`, but with different arguments:

`root #``sysinstall diskLabelEditor diskLabelCommit`
Here, we'll refrain from using the automatic layout, and create one giant root partition, followed by a swap partition. Hit c to create a new partition. A dialog prompts you to enter a size. Go ahead and do so, using MB/GB for setting different sizes, or C for cylinders. For root, choose FS as the partition type, and set the mount point as /mnt/. If you do not adjust the mount point, it will overwrite the FreeSBIE environment! As /boot is not a separate partition, you'll need to disable soft-updates, or your system will not boot! To do so, use the arrow keys to navigate to your newly created partition, then hit the s key, until "Newfs" contains no +S. Now navigate the arrow keys until the "Disk" line is highlighted, and hit c again to create a swap partition. Generally, we recommend a swap space that is twice the size of your RAM. Choose SWAP as the partition type, and don't worry about soft-updates, as it does not apply to swap. Now we're finished, so hit q to finish the process.

When choosing a different mountpoint than / for your partition, `sysinstall` will actually create a 'd' slice, which the bootloader will not boot from. To fix this, run the following:

`root #``disklabel ad0s1` `root #` `sed 's/^  d:/  a:/' {{:}} disklabel -R ad0s1 /dev/stdin`
This will finalize the partitioning process, and format the drive in UFS for FreeBSD to utilize. This will also mount the drive for you at the mount point specified earlier (/mnt/). You can verify this worked by running mount:

`root #``mount`
/dev/ad0s1a on /mnt (ufs, local)

Now that you have mounted the target partition, it is time to start on the Gentoo setup.

## Gentoo setup

First, we need to download a stage3 tarball and unpack it into the chroot. Point a browser to [http://gentoo.osuosl.org/experimental/x86/freebsd/stages/](http://gentoo.osuosl.org/experimental/x86/freebsd/stages/), grab the latest snapshot, and unpack it into the mountpoint:

`root #````
cd /mnt/
```
`root #``tar -jxvpf stage3-x86-freebsd-6.2.tar.bz2`
If desired delete the tarball with the following command to save space:

`root #``rm stage3-x86-freebsd-6.2.tar.bz2`
In order for the install to work, the /dev filesystem must be mounted from the currently running system into the Gentoo/FreeBSD mount point before proceeding with the chroot.

`root #````
mount -t devfs none /mnt/dev/
```
`root #````
cp /etc/resolv.conf /mnt/etc/
```
`root #````
chroot /mnt/ /bin/bash
```
`root #````
env-update && source /etc/profile
```
After you obtain the Gentoo/FreeBSD overlay, it is time to link /etc/make.profile to the correct profile and get the /etc/portage/make.conf ready for Gentoo/FreeBSD.

Now obtain a copy of the main Gentoo repository, which, depending on the connection speed, could quite a while:

`root #``emerge --sync`
Symlink the profile:

`root #``ln -sf /usr/portage/profiles/default-bsd/fbsd/6.2/x86/ /etc/portage/make.profile`
Then edit make.conf:

**`/etc/portage/make.conf`**

```
CHOST="i686-gentoo-freebsd6.2"
FEATURES="collision-protect"
```
Now rebuild the system's core packages (optional):

`root #``emerge --ask --emptytree @system`
## Booting

### Set a time zone

First make sure the date and time is set correctly using the `date` utility. The proper format is: `yyyymmddHHMM`, be sure to use UTC time.

Next, set the time zone information by using the correct listing in /usr/share/zoneinfo:

`root #``cp /usr/share/zoneinfo/Europe/Brussels /etc/localtime`
Edit /etc/conf.d/clock to define the time zone used previously:

**`/etc/conf.d/clock`**

```
TIMEZONE="Europe/Brussels"
```
### Install the kernel

If `emerge --emptytree @system`, the sources for the FreeBSD kernel were installed to /usr/src/sys. If that step was skipped it is possible to install the the sources in the following way:

`root #``emerge --ask` [sys-freebsd/freebsd-sources](https://packages.gentoo.org/packages/sys-freebsd/freebsd-sources)
Configuring and compiling a custom kernel is quite different from compiling Linux, so if you are not familiar with the process we encourage you to have a look at [chapter 8](http://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/kernelconfig.html) of the FreeBSD handbook. For now, you can do an installation of the GENERIC kernel, which works on most systems. To begin, enter the source directory for the kernel.

Enter kernel source directory:

`root #``cd /usr/src/sys/`
To begin the installation head into the i386/conf/ directory. Look over the layout to see see various architectures and sub-directories for various parts of the kernel.:

`root #````
cd i386/conf/
```
`root #``ls`
.cvsignore      GENERIC         Makefile        PAE
DEFAULTS        GENERIC.hints   NOTES           SMP

The main files to note are GENERIC and GENERIC.hints. As it will be needed by the installation of the kernel copy GENERIC.hints file to /boot/device.hints:

`root #``cp GENERIC.hints /boot/device.hints`
This file is used by the kernel drivers for basic configuration information such as IRQ settings. It is time to configure the kernel. FreeBSD uses the `config` command for this purpose. `config` uses the given file (in this instance GENERIC) to copy over the required build files to a compile directory in the parent directory. GENERIC is similar to the Linux kernel's .config file. Run `config` to produce the build directory:

`root #``config GENERIC`
Kernel build directory is ../compile/GENERIC
Don't forget to ''make cleandepend; make depend''

`config` has now created a GENERIC build directory in the parent directory. Move to into the GENERIC directory, then run the following commands to do a complete build:

`root #````
cd ../compile/GENERIC
```
`root #````
make cleandepend && make depend && make && make install
```
After the complication has finished a complete kernel will be made available. The next step is to configure the bootloader to boot the newly generated kernel. The next chapter will discuss two methods of setting up the bootloader: `boot0` and `grub`.

### Bootloader configuration

#### boot0

To install and configure `boot0`, run the following. Remember to replace adXsY with the actual number (`X`) and partition (`Y`) of the disk:

`root #``emerge --ask sys-freebsd/boot0`
Type `exit` to leave the chroot environment:

`root #``exit`
Issue the following commands from outside the chroot:

`root #````
fdisk -B -b /mnt/boot/boot0 /dev/adX
```
`root #````
chroot /mnt/ /bin/bash
```
`root #````
disklabel -B adXsY
```
If additional information on setting up boot0 is required, please consult [chapter 13](http://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/boot.html) of the FreeBSD handbook. Now it is time to do some basic system configuration and settings.

Skip the next section on `grub` if `boot0` has been selected as the system's bootloader.

#### grub

As of grub 0.97-r1, UFS slices are readable to `grub`. This lets us use `grub` as a bootloader, the preferred method for those coming from a Linux background. To begin, `emerge grub` and setup the label as bootable. Remember to replace adXsY with the actual number and slice of the disk:

`root #``emerge --ask {{{1}}}``root #``disklabel -B adXsY`
Now run `grub` to bring up the command prompt, and set up the partition as shown:

`root #````
sysctl kern.geom.debugflags=16
```
`root #``grub`
When inside the `grub` prompt issue:

`grub>``root (hd0,0,d)`
Filesystem type is ufs2, partition type 0xa5

`grub>``setup (hd0)`
Checking if "/boot/grub/stage1" exists... yes
 Checking if "/boot/grub/stage2" exists... yes
 Checking if "/boot/grub/ufs2\_stage1\_5" exists... yes
 Running "embed /boot/grub/ufs2\_stage1\_5 (hd0)"... 14 sectors are embedded.
succeeded
 Running "install /boot/grub/stage1 (hd0) (hd0)1+14 p (hd0,0,d)/boot/grub/stage
2 /boot/grub/menu.lst"... succeeded
Done.

`grub>``quit`
To make the loader find the kernel on a specific slice (the default is 'a'), add a vfs.root.mountfrom line to the /boot/loader.conf file:

`root #``echo 'vfs.root.mountfrom="ufs:ad0s1d"' >> /boot/loader.conf`
When you first boot, you may not receive a `grub` menu. If so, run this at the prompt:

`grub>``find /boot/grub/stage1`
(hd0,0,d)

`grub>``kernel (hd0,0,d)/boot/loader`
\[FreeBSD-a.out, loadaddr=0x200000, text=0x1000, data=0x3a000, bss=0x0, entry=0x200000\]

`grub>``boot`
### System configuration

First, we are going to setup the filesystem mounting points in /etc/fstab:

`root #``nano /etc/fstab`
#Device         Mountpoint      Fstype          Options         Dump    Pass
/dev/adXsYb     none            swap            sw              0       0
/dev/adXsYa     /               ufs             rw              1       1
/dev/adXsYe     /usr/home       ufs             rw              2       2
/dev/adXsYd     /tmp            ufs             rw              2       2
/dev/acdX       /cdrom          cd9660          ro,noauto       0       0

Now would also be a good time to set up your network connection before the final reboot. You can find all the information necessary to configure your network in the Gentoo Handbook. To have your network interface activated at boot time, you have to add it to the default runlevel:

`root #``rc-update add net.fxp0 default`
The system's hostname can be changed in /etc/conf.d/hostname:

`root #``nano /etc/conf.d/hostname`
HOSTNAME="tux"

The domain name should be set for the system. This is done in the /etc/conf.d/domainname file:

`root #``nano /etc/conf.d/domainname`
dns\_domain\_lo="homenetwork"

If a NIS domain is needed, it should be defined in the /etc/conf.d/domainname file:

`root #``nano /etc/conf.d/domainname`
nis\_domain\_lo="my-nisdomain"

In case another keyboard layout is required for the locale, set the correct value in /etc/conf.d/syscons. The following example uses the Spanish layout, adjust it to fit the needed language:

`root #``nano /etc/conf.d/syscons`
KEYMAP="spanish.iso.acc"

Change the root password:

`root #``passwd`
Add a user:

`root #``adduser`
Congratulations, you have just finished your Gentoo/FreeBSD installation which you can start exploring after the final reboot. Have fun!

Reboot the system:

`root #````
exit
```
`root #````
reboot
```
## Contributing

### How to help

There are many things you could help with, depending on your skill level and spare time:

- Working on current ebuilds: this means working closely with ebuild maintainers in order to create patches or modify ebuilds in a way that can be accepted into the main tree.
- Security: if you are into security, we need you! Although security advisories from the FreeBSD project are tracked and fixed, we can always use help in this area.
- Contacts: we need people who can get in touch with FreeBSD developers to maintain contacts between us and the original project to exchange patches and discuss various problems and their solutions. Note that this should never involve any kind of spamming of mailing lists or IRC channels.
- Testing: the more people are actively using Gentoo/FreeBSD, the more bugs will be discovered, which helps us improving the quality of the port. If you are good at describing bugs or problems, we definitely want to hear from you.
- Other areas where we need help include: system ebuilds, creation of installation CDs, documentation, kernel hacking.

### Known issues

At the moment, there are still quite a lot of known issues. Here are the ones really worth noting:

- Some init scripts depend on the clock service which we don't provide right now. You can just remove it from the dependencies of the script and report that on our Bugzilla. Please remember to use the "Gentoo/Prefix" product for your submission.

## Contact

A list of Gentoo/FreeBSD developers can be found at the project page. Other ways to contact FreeBSD developers include the [#gentoo-bsd](ircs://irc.libera.chat/#gentoo-bsd) ([webchat](https://web.libera.chat/#gentoo-bsd)) IRC channel on Freenode, as well as the [gentoo-bsd@gentoo.org](mailto:gentoo-bsd@gentoo.org) mailing list.
