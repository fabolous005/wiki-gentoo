<!-- source: https://wiki.gentoo.org/wiki/Gentoo_FreeBSD/Upgrade_Guide/11.0 | group: Gentoo Wiki (Main) | wiki-title: Gentoo FreeBSD/Upgrade Guide/11.0 -->
---
title: Gentoo FreeBSD/Upgrade Guide/11.0
url: https://wiki.gentoo.org/wiki/Gentoo_FreeBSD/Upgrade_Guide/11.0
hostname: gentoo.org
sitename: Gentoo FreeBSD/Upgrade Guide/11.0
date: "2022-03-07"
fingerprint: "61552728c2ef78f"
license: CC BY-SA 4.0
---

# Gentoo FreeBSD/Upgrade Guide/11.0

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Archived article**

This article is **archived (obsolete)**. Contents are surely incorrect for current usage, and are intended for historical reference only.

TLDR:

**Do not use this article!**


This guide will show you how to update the latest version. If you run into any trouble, please check the same section of [Gentoo FreeBSD/Upgrade Guide](https://wiki.gentoo.org/wiki/Gentoo_FreeBSD/Upgrade_Guide).

## Preparation

### Updating all packages

`root #``emerge -uDN @world`
### Changing to the latest profile

It is necessary to change the [profile](https://wiki.gentoo.org/wiki/Profile) to emerge packages related to FreeBSD 11.1.

Get a list of available profiles:

`root #``eselect profile list`
Available profile symlink targets:
  \[1\]   default/bsd/fbsd/amd64/9.1
  \[2\]   default/bsd/fbsd/amd64/11.1
  \[3\]   default/bsd/fbsd/amd64/9.1/clang
  \[4\]   default/bsd/fbsd/amd64/11.1/clang

Set the profile to 11.1:

`root #``eselect profile set 2`
## Updating kernel

First of all, you need to update your kernel. That is because some userland packages may require functions of the new kernel.

Please be sure to update the kernel first:

`root #``emerge -u sys-freebsd/freebsd-sources`
### Reboot

Don't have any problem? Let's restart to actually use the new kernel:

`root #``shutdown -r now`
After rebooting your machine, please check if the upgrade was successful.

`root #``uname -a`
FreeBSD daemon 11.1\_p2-Gentoo FreeBSD Gentoo 11.1\_p2 #0: Sat Dec 30 15:00:01 Local time zone must be set--see zic manual page 2017     root@daemon:/var/tmp/portage/sys-freebsd/freebsd-sources-11.1\_p2/work/sys/amd64/compile/GENTOO  amd64

## Updating FreeBSD userland

First, please update sys-freebsd packages with USE=build:

`root #``USE=build emerge -u freebsd-bin freebsd-lib freebsd-mk-defs freebsd-pam-modules freebsd-sbin freebsd-share freebsd-sources freebsd-ubin freebsd-usbin`
Please emerge the sys-freebsd packages again. Some of the packages are in need of include files of 11.1, which they couldn't use during the previous upgrade.

`root #``emerge boot0 freebsd-bin freebsd-lib freebsd-libexec freebsd-mk-defs freebsd-pam-modules freebsd-sbin freebsd-share freebsd-ubin freebsd-usbin`
## Changing the CHOST variable and rebuilding the toolchain

Change the CHOST variable, and emerge binutils&gcc. (FYI, [Changing the CHOST variable](https://wiki.gentoo.org/wiki/Changing_the_CHOST_variable))

x86-fbsd users should issue:

`root #``gsed -i 's:CHOST=.*:CHOST="i686-gentoo-freebsd11.1":g' /etc/portage/make.conf`
amd64-fbsd users should issue:

`root #``gsed -i 's:CHOST=.*:CHOST="x86_64-gentoo-freebsd11.1":g' /etc/portage/make.conf`
Emerge binutils and gcc:

`root #``emerge --oneshot sys-devel/binutils '<sys-devel/gcc-7.0'`
## Rebuilding all packages

`root #````
emerge --oneshot sys-devel/libtool
```
`root #````
emerge -ae @world
```
If one of the packages fails to compile you can issue 'emerge --resume --skipfirst' to continue emerging the remaining packages. Also, consider filing a bug report of the problem.

## Cleaning

Let's remove the backup files when you have finished all the steps:

`root #````
emerge @preserved-rebuild
```
`root #````
etc-update
```
