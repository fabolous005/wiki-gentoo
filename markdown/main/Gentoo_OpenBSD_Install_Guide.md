<!-- source: https://wiki.gentoo.org/wiki/Gentoo_OpenBSD/Install_Guide | group: Gentoo Wiki (Main) | wiki-title: Gentoo OpenBSD/Install Guide -->
---
title: Gentoo OpenBSD/Install Guide
url: https://wiki.gentoo.org/wiki/Gentoo_OpenBSD/Install_Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-01-05"
fingerprint: "941408101fca6fec"
license: CC BY-SA 4.0
---

# Gentoo OpenBSD/Install Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

As of **April 20, 2017**, this article is **deprecated (obsolete)**. Contents are <u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

This guide contains old installation instructions for Gentoo OpenBSD.

## Introduction to OpenBSD

### What is OpenBSD?

The OpenBSD project produces a [freely available](http://www.openbsd.org/faq/faq1.html), multi-platform 4.4BSD-based UNIX-like operating system. Our goals place emphasis on correctness, security, standardization, and portability. OpenBSD supports binary emulation of most binaries from SVR4 (Solaris), FreeBSD, Linux, BSDI, SunOS, and HPUX.

It was forked from [NetBSD](http://www.netbsd.org/), a previous open source operating system based on BSD, by project leader Theo de Raadt in 1994, and is widely known for the developers' insistence on open source and documentation, uncompromising position on software licensing, and focus on security and code correctness.

## Installation

The Gentoo OpenBSD project currently has official installation media, so you can download an ISO image from here.

Burn this image to a CD and use it boot your computer. Please log in as user 'root', using blank password. Once logged in, you have to create and format partitions for your Gentoo OpenBSD installation. If you're unsure how to do this, please consult the section "Setting up disks" of the [OpenBSD FAQ](http://www.openbsd.org/faq/faq4.html).

Partitioning the disk:

`root #``fdisk -e wdX`
Substitute `X` to reflect the setup:

`root #``disklabel -E wdX`
Substitute `X` and `Y` to reflect the correct disk/partition:

`root #``newfs /dev/wdXY`
When done partitioning the disk, create a mount point to the previously created partition(s). Replace `X` and `Y` with the correct values for the hard disk(s):

`root #````
mkdir /var/gentoo
```
`root #````
mount /dev/wdXY /var/gentoo
```
After mounting the target partition, it is time to fetch and unpack a stage3 tarball and sync with the main Gentoo repository:

`root #````
cd /var/gentoo
```
`root #````
tar xjpfv gentoo-openbsd-stage3-211105.tar.bz2 
```
Congratulations, it should now be possible to update the Gentoo OpenBSD installation using Portage! In order to be able to boot the new system later on, be sure to install a bootloader or add Gentoo OpenBSD to the current boot loader's configuration. Additionally remember to populate the /dev directory with the necessary device nodes. Finally edit /etc/fstab to reflect the partition layout.

`root #````
 cd /var/gentoo/dev
```
`root #````
./MAKEDEV all
```
`root #````
chroot /var/gentoo /bin/bash
```
`root #````
cd /usr/mdec; ./installboot boot biosboot wdX 
```
`root #````
vim /etc/fstab
```
