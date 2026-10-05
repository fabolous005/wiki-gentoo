<!-- source: https://wiki.gentoo.org/wiki/Old_Fashioned_Gentoo_Install | group: Gentoo Wiki (Main) | wiki-title: Old Fashioned Gentoo Install -->
---
title: Old Fashioned Gentoo Install
url: https://wiki.gentoo.org/wiki/Old_Fashioned_Gentoo_Install
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-07-27"
fingerprint: "8225315ebaaeb960"
license: CC BY-SA 4.0
---

# Old Fashioned Gentoo Install

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This document describes how to install Gentoo without the hand holding automation features that users have come to take for granted over the last 10 years. It gives you an install as it was before devfs was added to the kernel.

## Overview

### Synopsis

Its possible to start with the official stage3 files. This has only been tested on an amd64/no-multilib install.

As this document is aimed at users with at least one Gentoo install to their credit, it is not a keystroke by keystroke guide, unlike the Handbook. The handbook steps are not repeated here, there is just some general references to it from time to time.

### Overview

The install will use root in Logical Volume Manager (LVM) on NVMe, with separate /usr and /var. /home, $DISTFILES and $PACKAGES will be in LVM on rotating rust RAID as Conventional Magnetic Recording (CMR) drives are quite good at sequential access of large files. Thus we need an initrd to get started.

Its unlikely that grub2 will be used as the boot loader.

The steps include:

- Partition the target drive following the handbook.
- Install the stage3 tarball.
- Install the portage snapshot.
- Set up package.mask to keep out unwanted junk.
- Set up global USE flags to be consistent with package.mask.
- Replace udev with [sys-fs/static-dev](https://packages.gentoo.org/packages/sys-fs/static-dev).
- Follow the handbook to install cron, a logger and a bootloader of choice.
- Install a kernel.
- Configure the grub bootloader.
- Review and edit configuration settings.
- Reboot to test.

### Introduction

This document describes how to install Gentoo with a static /dev using the packages from a stage3 tarball.

**What You Get**

A modern Gentoo base system but without all the bells and whistles added in recent years. Olde Fashioned Gentooee is more about what you don't get. You do *not* get:

- udev - instead a static dev is used
- systemd - why would you want it anyway
- pulseaudio - I've not known this to actually add anything
- hotplug support
- auto mounting of any sort - use mount by label
- auto module loading
- device detection in Xorg

Separate /usr should just work as there is no udev to require that /usr is mounted before udev starts. If udev starts on your box you have done something wrong. Separate /usr is not tested as I'm using root in lvm, so while my /usr is separate, bad habits have made me mount it in the initrd.

Access to the [Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:Main_Page) is required as this guide makes frequent references to it, there is no point in repeating the handbook here.

## Getting started

### Partitioning and filesystem creation

**Making the filesystem tree**

Follow the [Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:Main_Page) up to and including making the filesystems and mounting all the bits at the /mnt/gentoo directory.

I will be using Logical Volumes on top of raid5 because its easier to recycle the volumes than it is with real partitions and if the logical volumes are the wrong size, they can be resized. I happen to have lvm on raid5 space free. This means that I will also describe the initramfs to get the raid assembled and Logical Volumes active. Users installing to real partitions should not need the initramfs.

## Chrooting

### Making the chroot

Mount /proc and but not /dev inside the chroot. We will be using a static /dev, so we have to emerge dev-static. With /dev bind mounted in the normal way, our static dev would go into the parents devtmpfs which is in RAM. If you are very very lucky, the static /dev provided by the stage3 may be enough to get you started.

The stage3 tarball is provided with a static /dev that includes sda ... sdd inclusive. If you need more that that, use mknod to make the extra /dev entries. Likewise, there ale no md entries for raid or dm enteries for LVM.

Mount the special filesystems

`root #``mount -t proc proc /mnt/gentoo/proc`
### Entering the chroot

Enter the chroot:

`root #``chroot /mnt/gentoo /bin/bash`

Set the chroot environment:

`root #````
env-update
```
`root #````
source /etc/profile
```
`root #````
export PS1="(chroot) $PS1"
```
### Setting up package.mask

This is important. Enter the package atoms that you do not want to be installed ever.

**`/etc/portage/package.mask`**

**Content of package.mask**

Add in anything else you can think of they you really don't want. Always use `-av` with emerge and add more things as they come to mind. mdev might need to be there too.

### Setting up package.use

This section is only required if you use raid, or lvm. You will need some packages built with the static USE flag.

**`/etc/portage/package.use`**

**Content of package.use**

Removing udev and Friends

There is a bug in the static-dev ebuild [bug #469620](https://bugs.gentoo.org/show_bug.cgi?id=469620) that prevents it installing if /proc/mounts reports that a dynamic /dev manager is in use.
Either patch static-dev in the overlay or unmount /proc from /mnt/gentoo/proc while static-dev is emerged.

Replacing udev with static-dev:

`root #``emerge --ask --unmerge sys-fs/udev`


`root #``emerge --ask sys-fs/static-dev`
...see [bug #469620](https://bugs.gentoo.org/show_bug.cgi?id=469620).

emerge sys-fs/static-dev will report some file collisions. That is expected as some elements of a static /dev are provided by the stage3.

`root #``emerge --ask --depclean`
The last command should offer to remove the following packages:

Let it run, they all depend on udev, which is no longer installed.

### Setting USE in make.conf

Some of my USE flags are AMD specific. The flags that are set off here are for avoidance of optional support for packages we have already masked. Optional support being on would attempt to pull those packages in and emerge would complain about masked packages.

**`/etc/portage/make.conf`**

**USE flags**

```
USE="X alsa device-mapper apng mp3 python jpeg lock session startup-notification thunar curl ffmpeg odf pdf raw gtk cairo -consolekit -dso -firmware-loader -gbm -kmod  -ldap -networkmanager -nss -oss -qt4 -systemd -tools -udev -zeroconf"
```
The `-zeroconfig` flag is a special case. Zeroconfig wasn't around 10 years ago so really should be excluded here.

### Adding to /dev

static-dev is a good start but its not moved on in a very long time. Add some of the newer entries required. See /usr/src/linux/Documentation/devices.txt for a list

- mknod all of the /dev/sd\* entries you need
- mknod any /dev/md\* kernel multipe device entries required
- mknod any /dev/dm-X device mapper entries required
- mknod any /dev/srX devices for your optical drive(s)
- mknod any other /dev nodes you might want. They can be added at any time

Do not forget nodes for removable storage devices.

DRI users, that's almost everyone except those who use nvidia-drivers for Xorg will need to make /dev/dri/\*. What is needed here is driver dependent.

### Populating /etc/conf.d/modules

Only you know what you need here. When you reboot, its a good idea to have keyboard support and udev isn't going to load it for you any more.

### Setting up a static overlay

A number of packages that are required for a modern Gentoo system require udev. In some the dependency can be avoided by careful use of USE flags. Others like lvm2 and Xorg have udev included in IUSE. Make a local overlay called static\_dev, copy these ebuilds there and remove all references to udev.

## Getting ready to reboot

### Making the kernel

Follow the instructions at [http://www.kernel-seeds.org](http://www.kernel-seeds.org) which is mirrored at [http://kernel-seeds.grytpype-thynne.org](http://kernel-seeds.grytpype-thynne.org) ( [http://kernel-seeds.bloodnoc.org/](http://kernel-seeds.bloodnoc.org/) ) with the following changes

**Key kernel options**

We can leave off the hair shirts. Unix98PTY support does work but the permissions on /dev/ptmx need to be set correctly and /dev/ptmx needs to be mounted crw-rw---- 1 root tty 5, 2 Mar 18 21:47 /dev/ptmx

Genkernel users are on their own here.

Provided your kernel can boot unaided, no initrd is required

## Making the initrd

### Preparing for usr/gen\_init\_cpio

To make everything robust and independent of what filesystem gets attached to which /dev node, we will use the filesystem UUIDs everywhere.

There are several ways to make an initramfs, we will use the kernel provided usr/gen\_init\_cpio script.

The script needs two things, a list of files to include in the initramfs and an init sctipt to execute. The use of usr/gen\_init\_cpio is well documented in the kernel.

Make a directory to hold the two files. I like /root/initrd. The two files that follow go there.

**`/root/initrd/initramfs_list`**

I'm sure there is a sh one liner to feed to busybox mknod as a part of the init script, so I don't need the huge list of nod statements but I don't know it.

If you use files systems other than extX on /usr and / or /var, which the initrd checks and mounts, you need your filesystem tools listed here. Feel free to add other things you find useful when booting fails too.

**`/root/initrd/init`**

```
#!/bin/busybox sh
rescue_shell() {
    echo "$@"
    echo "Something went wrong. Dropping you to a shell."
    /bin/busybox --install -s
    exec /bin/sh
}
# allow the use of UUIDs or filesystem lables
uuidlabel_root() {
    for cmd in $(cat /proc/cmdline) ; do
        case $cmd in
        root=*)
            type=$(echo $cmd | cut -d= -f2)
            echo "Mounting rootfs"
            if [ $type == "LABEL" ] || [ $type == "UUID" ] ; then
                uuid=$(echo $cmd | cut -d= -f3)
                mount -o ro $(findfs "$type"="$uuid") /mnt/root
            else
                mount -o ro $(echo $cmd | cut -d= -f2) /mnt/root
            fi
            ;;
        esac
    done
}
check_filesystem() {
    # most of code coming from /etc/init.d/fsck
    local fsck_opts= check_extra= RC_UNAME=$(uname -s)
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
    /sbin/fsck -p $fsck_opts $1
    ret_val=$?
    case $ret_val in
        0)      return 0;;
        1)      echo "Filesystem repaired"; return 0;;
        2|3)    if [ "$RC_UNAME" = Linux ]; then
                        echo "Filesystem repaired, but reboot needed"
                        reboot -f
                else
                        rescue_shell "Filesystem still have errors; manual fsck required"
                fi;;
        4)      if [ "$RC_UNAME" = Linux ]; then
                        rescue_shell "Fileystem errors left uncorrected, aborting"
                else
                        echo "Filesystem repaired, but reboot needed"
                        reboot
                fi;;
        8)      echo "Operational error"; return 0;;
        16)     echo "Use or Syntax Error"; return 16;;
        32)     echo "fsck interrupted";;
        127)    echo "Shared Library Error"; sleep 20; return 0;;
        *)      echo $ret_val; echo "Some random fsck error - continuing anyway"; sleep 20; return 0;;
    esac
# rescue_shell can't find tty so its broken
    rescue_shell
}
# start for real here
# temporarily mount proc and sys
mount -t proc none /proc
mount -t sysfs none /sys
# assemble the raid set(s) - they got renumbered from md1, md5 and md6
# not needed on SSD but we may want to maintain it
# /boot
/sbin/mdadm --assemble /dev/md125 /dev/sda1 /dev/sdb1 /dev/sdc1 /dev/sdd1
# don't care if /boot fails to assemble
# not needed on SSD
# /  (root)  I wimped out of root on lvm for this box
/sbin/mdadm --assemble /dev/md126 /dev/sda5 /dev/sdb5 /dev/sdc5 /dev/sdd5 || rescue_shell
# if root won't assemble, we are stuck
# LVM for everything else
# /home and everything portge related
/sbin/mdadm --assemble /dev/md127 /dev/sda6 /dev/sdb6 /dev/sdc6 /dev/sdd6 || rescue_shell
# and if the LVM space won't assemble there is no /usr or /var so we are really in a mess
# TODO could auto cope with degraded raid operation
# lvm runs as whatever its called as
ln -s /sbin/lvm.static /sbin/vgchange
# everything on the SDD
/sbin/vgchange -ay ssd | rescue_shell
# start the vg volume group - /home and everything for portage - need not die here
/sbin/vgchange -ay vg || rescue_shell
# get here with raid sets assembled and logical volumes available
# mounting rootfs on /mnt/root
uuidlabel_root || rescue_shell "Error with uuidlabel_root"
# space separated list of mountpoints that ...
mountpoints="/usr /var"
# ... we want to find in /etc/fstab ...
ln -s /mnt/root/etc/fstab /etc/fstab
# ... to check filesystems and mount our devices.
for m in $mountpoints ; do
#echo $m
    check_filesystem $m
    echo "Mounting $m"
    # mount the device and ...
    mount $m || rescue_shell "Error while mounting $m"
    # ... move the tree to its final location
    mount --move $m "/mnt/root"$m || rescue_shell "Error while moving $m"
done
echo "All done. Switching to real root."
# clean up. The init process will remount proc sys and dev later
umount /proc
umount /sys
# switch to the real root and execute init
exec switch_root /mnt/root /sbin/init
```
Now to feed the /root/initrd/initramfs\_list file to usr/gen\_init\_cpio. Make sure /boot is mounted.

Running usr/gen\_init\_cpio:

`root #````
cd /usr/src/linux
```
`root #````
usr/gen_init_cpio /root/initrd/initramfs_list > /boot/initramfs_static
```
This what the kernel build system does if you choose to build the initramfs into the kernel binary but if you don't get it right first time, you can fix your kernel without rebuilding your initramfs and vice versa.

### Populating /etc/fstab

Run blkid to discover the UUIDs of all your block devices. Paste the output into /etc/fstab, so its easy to refer to in the future. Delete lines that provide the UUIDS of block devices that are not filesystesms, e.g. lvm members, md devices. Comment out the other entries, so they can stay in the file.

Populating /etc/fstab as normal, but use UUIDs:

**`/etc/fstab`**

As there is no auto mounting, do not forget entries for optical drives.

Floppy disk users need to remember /dev/fdX and friends. Users who have not formatted a floppy with a static /dev are in for a treat.

### Configuring the system

Follow [Configuring the system](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/System) in the **amd64** handbook.

### Installing necessary system tools

Follow [Installing system tools](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Tools) in the **amd64** handbook.

### Setting up the boot loader

The Grand Unified Bootloader (GRUB) has already been installed to /boot.

Follow [Configuring the bootloader](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Bootloader) to install GRUB to the Master Boot Record (MBR) or to configure properly when using a GUID Partition Table (GPT). Associated GRUB configuration file(s) will also be needed.

## Hints for Xorg

### xorg.conf

You need a whole xorg.conf, just like in the good/bad old days. evdev depends on udev auto detecting devices, so that is out.

## Hints for a desktop environment

### GNOME or KDE Plasma

Gnome is not an option. I suspect that KDE Plasma is out too.

### Xfce 4

Xfce almost works out of the box. You need a patched mesa ebuild to build without udev.

### The gentoo-static overlay

hwids works but the ebuild needs to be patched to remove the dependency on udev

mesa

I need to get round to publishing the overlay.

## External resources

- [http://swift.siphos.be/linux\_sea/](http://swift.siphos.be/linux_sea/) - An ebook that offers a gentle yet technical (from end-user perspective) introduction to the Linux operating system, using Gentoo Linux as the example Linux distribution. (Link included with permission from the author.)
