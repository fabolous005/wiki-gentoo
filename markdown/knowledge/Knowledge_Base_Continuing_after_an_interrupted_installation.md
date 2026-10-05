<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:Continuing_after_an_interrupted_installation | group: Gentoo Knowledge | wiki-title: Knowledge_Base:Continuing_after_an_interrupted_installation -->
---
title: Knowledge Base:Continuing after an interrupted installation
url: https://wiki.gentoo.org/wiki/Knowledge_Base:Continuing_after_an_interrupted_installation
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-02-28"
fingerprint: "942c325e17e74201"
license: CC BY-SA 4.0
---

# Knowledge Base:Continuing after an interrupted installation

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Synopsis

The Gentoo Linux installation method is a manual approach which can take some time to perform, especially if the user performs it for the first time. However it is possible to interrupt the installation and continue later, but a few vital steps are required.

## Environment

This article is applicable to Gentoo Linux installations that have not been finished.

## Analysis

The Gentoo Linux installation uses a phased approach (roughly the chapters of the installation section in the [Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64)) which allows users to interrupt an installation and continue later. Of course, persistent changes that were previously made do not need to be reiterated again.

## Resolution

### Getting the file systems ready

#### If "Preparing the disks" was not finished

[Chapter 4](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Disks) ("Preparing the disks") must be completed before it is possible for changes to persist on the drives. If the disks have been partitioned but do on to have any file system on them, then continue from that step. There is no need to re-partition the disks if the work has been done.

Make sure that the file systems are mounted before continuing:

`root #````
swapon /dev/sda2
```
`root #````
mount /dev/sda3 /mnt/gentoo
```
`root #````
mkdir /mnt/gentoo/boot
```
`root #``mount /dev/sda1 /mnt/gentoo/boot`
#### If "Preparing the disks" was finished

If the chapter on preparing the disks was finished, then simply remount the partitions:

`root #````
swapon /dev/sda2
```
`root #````
mount /dev/sda3 /mnt/gentoo
```
`root #````
mkdir /mnt/gentoo/boot
```
`root #``mount /dev/sda1 /mnt/gentoo/boot`
### Getting the chroot ready

The next steps in the Gentoo Installation is to prepare the chroot by downloading and extracting the proper files on the file systems. If the middle of this process (i.e. [chapter 5](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Stage) ("Installing the Gentoo installation files") was interrupted then simply continue at the step that was last performed.

If this section was finished as well as [chapter 6](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base) ("Installing the Gentoo base system"), section [Chrooting](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Chrooting), then what is needed is a remounting of the /dev, /proc, and /sys locations and chroot:

`root #````
mount --types proc /proc /mnt/gentoo/proc
```
`root #````
mount --rbind /sys /mnt/gentoo/sys
```
`root #````
mount --make-rslave /mnt/gentoo/sys
```
`root #````
mount --rbind /dev /mnt/gentoo/dev
```
`root #````
mount --make-rslave /mnt/gentoo/dev
```
`root #````
chroot /mnt/gentoo /bin/bash
```
`root #````
env-update
```
`root #````
source /etc/profile
```
### Continuing after the chroot

From the moment after the chroot, continue from the last step performed; all changes made in a chrooted environment will be persistent changes.

## See also

- [Recovering from a kernel boot failure](https://wiki.gentoo.org/wiki/Knowledge_Base:Recovering_from_a_kernel_boot_failure) uses a similar approach (Knowledge Base).
