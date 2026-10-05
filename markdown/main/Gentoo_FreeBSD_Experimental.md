<!-- source: https://wiki.gentoo.org/wiki/Gentoo_FreeBSD/Experimental | group: Gentoo Wiki (Main) | wiki-title: Gentoo FreeBSD/Experimental -->
---
title: Gentoo FreeBSD/Experimental
url: https://wiki.gentoo.org/wiki/Gentoo_FreeBSD/Experimental
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-04-13"
fingerprint: b4051443d98be3af
license: CC BY-SA 4.0
---

# Gentoo FreeBSD/Experimental

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **archived (obsolete)**. Contents are surely incorrect for current usage, and are intended for historical reference only.

TLDR:

**Do not use this article!**

The various sections in this article are a bit experimental. Maintain an expectation they might not work as intended on Gentoo/FreeBSD systems. Handle with care!

## How to use X Window System with vesa video card

`root #``emerge --ask portage libtool app-admin/eselect``root #````
mv /etc/make.conf /etc/portage/make.conf # (be careful !)
```
`root #````
emerge -u flaggie
```
`root #````
flaggie +python +X
```
`root #````
echo 'INPUT_DEVICES="keyboard mouse"' >> /etc/portage/make.conf
```
`root #``echo 'VIDEO_CARDS="vesa"' >> /etc/portage/make.conf``root #````
emerge -p xorg-x11
```
`root #````
emerge xorg-x11
```
`root #````
emerge xterm twm xclock
```
If you are using a USB mouse, please load kernel module first:

`root #````
kldload ums
```
`root #````
echo 'ums_load="YES"' >> /boot/loader.conf
```
Configure:

`root #````
Xorg -configure
```
`root #````
mv /root/xorg.conf.new /etc/X11/xorg.conf
```
Check the device for mouse:

`root #``grep /dev /etc/X11/xorg.conf`
Option      "Device" "/dev/sysmouse"

OK, In this case /dev/sysmouse has been detected:

`root #````
/etc/init.d/moused start
```
`root #````
rc-update add moused default
```
`root #````
startx
```
### Other video cards

xf86-video-vmware) emerge [x11-drivers/xf86-video-vmware](https://packages.gentoo.org/packages/x11-drivers/xf86-video-vmware).

xf86-video-intel) Please see [bug #493436](https://bugs.gentoo.org/show_bug.cgi?id=493436).

### Q&A

**Question:** Why does the mouse not work?

**Answer:** Please specify the correct value in xorg.conf. `moused` must use /dev/sysmouse. Run `/etc/init.d/moused start`.

- PS/2 mouse is /dev/psm0.
- USB mouse is /dev/ums0.
- Another one is /dev/sysmouse.

## How to use a Portage overlay

- [Gentoo/BSD Project Overlay](http://git.overlays.gentoo.org/gitweb/?p=proj/gentoo-bsd.git)
- It is the official overlay. Good quality, it contains bugfixes for e.g. system packages. You can add using:

`root #``eselect repository enable gentoo-bsd`
## How to use a clang stage file

It is available from the following URL:

Please see bug list and page [Clang](https://wiki.gentoo.org/wiki/LLVM/Clang). In addition, please report to Gentoo Bugzilla if you found a bug.

### Bug list

- [bug #417789](https://bugs.gentoo.org/show_bug.cgi?id=417789): \[TRACKER\] Packages that fail to build with LLVM/Clang on Gentoo FreeBSD

## How to use sys-boot/grub2

### Install grub2

`root #````
mkdir -p /etc/portage/package.keywords /etc/portage/patches/sys-boot/grub
```
`root #````
echo "=sys-boot/grub-2.00_p5107* amd64" >> /etc/portage/package.keywords/grub
```
`root #``wget -P /etc/portage/patches/sys-boot/grub` [https://raw.github.com/nigoro/amd64-fbsd/master/etc/portage/patches/sys-boot/grub/grub-2.00_p5107-fbsd.patch](https://raw.github.com/nigoro/amd64-fbsd/master/etc/portage/patches/sys-boot/grub/grub-2.00_p5107-fbsd.patch)
`root #````
emerge sys-boot/grub
```
### Configure grub2

If you select UFS2 (GPT):

`root #````
grub2-install --modules="part_gpt fat ufs2" --no-floppy /dev/ada0
```
`root #````
grub2-mkconfig -o /boot/grub/grub.cfg
```
If you select UFS2 (MBR):

`root #````
grub2-install --modules="biosdisk part_msdos bsd search_fs_uuid ufs2 msdospart" --no-floppy /dev/ada0
```
`root #````
grub2-mkconfig -o /boot/grub/grub.cfg
```
If you select ZFS (GPT):

`root #````
grub2-install --modules="part_gpt fat zfs" --no-floppy /dev/ada0
```
`root #````
grub2-mkconfig -o /boot/grub/grub.cfg
```
