<!-- source: https://wiki.gentoo.org/wiki/Gentoo_NetBSD/Install_Guide | group: Gentoo Wiki (Main) | wiki-title: Gentoo NetBSD/Install Guide -->
---
title: Gentoo NetBSD/Install Guide
url: https://wiki.gentoo.org/wiki/Gentoo_NetBSD/Install_Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2017-04-20"
fingerprint: "100ca3700caa6b64"
license: CC BY-SA 4.0
---

# Gentoo NetBSD/Install Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

As of **April 20, 2017**, this article is **deprecated (obsolete)**. Contents are <u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

This guide contains installation instructions written by the old Gentoo NetBSD project.

## Introduction

### What is NetBSD?

NetBSD is a [free](http://www.netbsd.org/about/), secure, and highly portable Unix-like Open Source operating system available for many platforms, from 64-bit Opteron machines and desktop systems to handheld and embedded devices.

Back in 1993 when development of [386BSD](http://www.386bsd.org/) stopped, two projects were born: [FreeBSD](http://www.freebsd.org/) which focuses mainly on the **x86** platform, and [NetBSD](http://www.netbsd.org/), commonly known to run on a huge number of architectures.

## Installation

Unfortunately the Gentoo NetBSD project currently has no official installation media, so you have to download an ISO image of a NetBSD LiveCD which can be found here.

Burn this image to a CD and use it boot your computer. Please log in as user 'root', using 'NetBSD' as a password. Once logged in, you have to create and format partitions for your Gentoo NetBSD installation. If you're unsure on how to do this, please consult the section "Adding a new hard disk" of the NetBSD manual.

Create a partition:

`root #``fdisk -u wdX`
Mark the partition as active:

`root #``fdisk -a wdX`
Provide the disk a label:

`root #``disklabel -i wdX`
Create a new file system on the disk:

`root #``newfs /dev/wdXY`
When disk setup is finished, create a mount point for the previously created partitions:

`root #````
mkdir /var/gentoo
```
`root #````
mount /dev/wdXY /var/gentoo
```
Now that the target partition has been mounted, it is time to fetch and unpack a stage3 tarball and sync with the main Gentoo repository:

`root #````
cd /var/gentoo 
```
`root #````
tar -xjpf gentoo-netbsd-stage3-051106.tar.bz2
```
`root #````
chroot /var/gentoo /bin/bash
```
`root #````
env-update
```
`root #````
emerge --sync
```
`root #````
echo "x86-nbsd" >> /usr/portage/profiles/arch.list
```
`root #````
emerge metadata
```
Congrats, you now should be able to update your Gentoo NetBSD installation using Portage! But in order to be able to boot your new system later on, you will have to install a boot manager or add Gentoo NetBSD to your current boot loader's configuration. Additionally you will have to populate your /dev dirctory with the necessary device nodes. Finally you'll have to edit /etc/fstab to reflect the system's partition layout.

`root #````
cd /dev
```
`root #````
./MAKEDEV all
```
`root #````
fdisk -B /dev/wdX
```
`root #``installboot -v /dev/rwdXY /usr/mdec/bootxx_ffsv1`
Adjust the following files accordingly:

- /etc/fstab
- /etc/rc.conf

We hope you will enjoy your new Gentoo NetBSD system and will have fun using it!
