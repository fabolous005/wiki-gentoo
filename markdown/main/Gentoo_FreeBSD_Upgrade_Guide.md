<!-- source: https://wiki.gentoo.org/wiki/Gentoo_FreeBSD/Upgrade_Guide | group: Gentoo Wiki (Main) | wiki-title: Gentoo FreeBSD/Upgrade Guide -->
---
title: Gentoo FreeBSD/Upgrade Guide
url: https://wiki.gentoo.org/wiki/Gentoo_FreeBSD/Upgrade_Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2020-02-14"
fingerprint: "3e1557669c8ffd8f"
license: CC BY-SA 4.0
---

# Gentoo FreeBSD/Upgrade Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Archived article**

This article is **archived (obsolete)**. Contents are surely incorrect for current usage, and are intended for historical reference only.

TLDR:

**Do not use this article!**

This guide will show you how to upgrade Gentoo/FreeBSD 11.1 from previous version. If you are using Gentoo/FreeBSD on a daily basis, please check [Gentoo FreeBSD/Upgrade Guide/11.0](https://wiki.gentoo.org/wiki/Gentoo_FreeBSD/Upgrade_Guide/11.0) instead of this guide.

## Preparation

### Changing to the latest profile

It is necessary to change the [profile](https://wiki.gentoo.org/wiki/Profile) to emerge packages related to FreeBSD 11.1.

Get a list of available profiles:

`root #``eselect profile list`
Available profile symlink targets:
  \[1\]   default/bsd/fbsd/amd64/9.1 \*
  \[2\]   default/bsd/fbsd/amd64/11.1
  \[3\]   default/bsd/fbsd/amd64/9.1/clang
  \[4\]   default/bsd/fbsd/amd64/11.1/clang

Set the profile to 11.1:

`root #``eselect profile set 2`
### Updating sys-apps/portage with a manual method.

If you are using the too old version of portage, [sys-apps/portage](https://packages.gentoo.org/packages/sys-apps/portage) should be updated. Let's check:

`root #``emerge -pu sys-apps/portage --exclude sys-freebsd/*`
\<snip>
The current version of portage supports EAPI '5'. You must upgrade to a
newer version of portage before EAPI masked packages can be installed.
\<snip>

If you got the above messages, please update it with a manual method:

`root #````
cd /tmp
```
`root #````
tar xjf portage-2.3.6.tar.bz2
```
`root #````
PYTHON_TARGETS="python2_7" portage-2.3.6/bin/emerge --nodeps dev-lang/python-exec
```
`root #````
eselect python set python2.7
```
`root #````
PYTHON_TARGETS="python2_7" portage-2.3.6/bin/emerge --nodeps sys-apps/portage
```
### Moving /etc/make.conf

Currently, /etc/make.conf has been moved to /etc/portage/make.conf. If it exists in /etc, please move it to the /etc/portage/ location.

In addition, don't forget to remove LDFLAGS from make.conf. If it is set, the upgrade may fail.

`root #````
mkdir -p /etc/portage
```
`root #````
if [ -e /etc/make.conf ] && [ ! -e /etc/portage/make.conf ] ; then
```
`root #````
mv /etc/make.conf /etc/portage/make.conf
```
`root #````
fi
```
`root #``gsed -i '/LDFLAGS=/d' /etc/portage/make.conf`
### Updating some of the packages to continue the upgrade

You need to update some of the packages to continue the upgrade:

`root #````
emerge -u sys-apps/findutils --exclude sys-freebsd/*
```
`root #````
emerge sys-devel/libtool --exclude sys-freebsd/*
```
`root #````
USE="-acl" emerge -u app-arch/libarchive --exclude sys-freebsd/*
```
`root #````
emerge -u sys-devel/flex sys-devel/patch sys-devel/m4 net-libs/libpcap sys-devel/gettext dev-util/dialog sys-libs/zlib --exclude sys-freebsd/*
```
## Updating toolchain

A newer version of binutils and gcc are required:

`root #````
[[ ! -e /etc/portage/package.unmask ]] && mkdir -p /etc/portage/package.unmask
```
`root #````
echo "sys-devel/binutils" > /etc/portage/package.unmask/toolchain-oldversion
```
`root #````
echo "<sys-devel/gcc-7.0" >> /etc/portage/package.unmask/toolchain-oldversion
```
`root #````
emerge -u '<sys-devel/binutils-2.28' --exclude sys-freebsd/*
```
`root #````
emerge -u sys-devel/gcc-config --exclude sys-freebsd/*
```
`root #````
emerge '<sys-devel/gcc-5.0' --exclude sys-freebsd/*
```
Set the newer version of gcc:

`root #````
gcc-config -l
```
\[1\] x86\_64-gentoo-freebsd9.1-4.6.4 \*
 \[2\] x86\_64-gentoo-freebsd9.1-4.9.4

`root #````
gcc-config 2
```
`root #````
. /etc/profile
```
## Updating config and make

Latest version of config and make are required to continue upgrading:

`root #````
cd /var/db/repos/gentoo/sys-freebsd/freebsd-ubin && ebuild freebsd-ubin-11.1.ebuild prepare
```
`root #````
cd /var/tmp/portage/sys-freebsd/freebsd-ubin-11.1/work/usr.bin/bmake
```
`root #````
make
```
`root #````
cp -a make /usr/local/bin/
```
`root #````
cd /var/db/repos/gentoo/sys-freebsd/freebsd-ubin && ebuild freebsd-ubin-11.1.ebuild clean
```
`root #````
env-update
```
`root #````
source /etc/profile
```
`root #````
emerge -u sys-freebsd/freebsd-mk-defs
```
`root #````
cd /var/db/repos/gentoo/sys-freebsd/freebsd-usbin && ebuild freebsd-usbin-11.1.ebuild prepare
```
`root #````
cd /var/tmp/portage/sys-freebsd/freebsd-usbin-11.1/work/usr.sbin/config
```
`root #````
make -m /usr/share/mk/system
```
`root #````
cp -a config /usr/local/sbin/
```
`root #````
cd /var/db/repos/gentoo/sys-freebsd/freebsd-usbin && ebuild freebsd-usbin-11.1.ebuild clean
```
## Updating kernel

First of all, you need to update your kernel. That is because some userland packages may require functions of the new kernel.

Please be sure to update the kernel first:

`root #````
emerge -C sys-freebsd/freebsd-sources
```
`root #````
emerge -u sys-freebsd/freebsd-sources
```
### Failed to compile the kernel

If it fails to compile, please get the GENERIC kernel of FreeBSD instead. If you are using x86-fbsd, please replace the amd64 to i386 in URL.

`root #````
mv /boot /boot.orig
```
`root #````
cd /
```
`root #````
tar xvJf /tmp/base.txz ./boot
```
`root #````
tar xvJf /tmp/kernel.txz
```
### Reboot

Don't have any problem? Let's restart to actually use the new kernel:

`root #``shutdown -r now`
After rebooting your machine, please check if the upgrade was successful.

`root #``uname -a`
FreeBSD daemon 11.1\_p2-Gentoo FreeBSD Gentoo 11.1\_p2 #0: Sat Dec 30 15:00:01 Local time zone must be set--see zic manual page 2017     root@daemon:/var/tmp/portage/sys-freebsd/freebsd-sources-11.1\_p2/work/sys/amd64/compile/GENTOO  amd64

## Updating FreeBSD userland

If you are upgrading from 9.x, the portage may fail to check dependencies. Let's check:

`root #``USE=build emerge -pvu freebsd-bin freebsd-lib freebsd-mk-defs freebsd-pam-modules freebsd-sbin freebsd-share freebsd-sources freebsd-ubin freebsd-usbin`
If it fails to check, some files should be removed to pass checking dependencies. Please note that this step directly changes the package information:

`root #````
echo "" > /var/db/pkg/sys-freebsd/freebsd-lib-*/RDEPEND
```
`root #````
rm /var/db/pkg/sys-freebsd/freebsd-lib-*/*.ebuild
```
`root #````
echo "" > /var/db/pkg/dev-libs/glib-*/RDEPEND
```
`root #````
echo "" > /var/db/pkg/dev-libs/glib-*/DEPEND
```
`root #````
rm /var/db/pkg/dev-libs/glib-*/*.ebuild
```
`root #````
rm /var/db/pkg/sys-freebsd/freebsd-libexec-*/*.ebuild
```
sys-freebsd packages can be updated:

`root #````
[[ -e /usr/share/misc/windrv_stub.c ]] && rm -rf /usr/share/misc/windrv_stub.c
```
`root #````
USE=build emerge -u freebsd-bin freebsd-lib freebsd-mk-defs freebsd-pam-modules freebsd-sbin freebsd-share freebsd-sources freebsd-ubin freebsd-usbin
```
`root #````
emerge freebsd-share # Don't forget to do this step to install iconv modules.
```
Please emerge the sys-freebsd packages again. Some of the packages are in need of include files of 11.1, which they couldn't use during the previous upgrade.

`root #``emerge boot0 freebsd-bin freebsd-lib freebsd-libexec freebsd-mk-defs freebsd-pam-modules freebsd-sbin freebsd-share freebsd-ubin freebsd-usbin`
/usr/local/sbin/config, /usr/local/bin/make, and utmp related files are no longer needed so please remove them:

`root #````
rm /usr/local/sbin/config /usr/local/bin/make /var/run/utmp /var/log/lastlog /var/log/wtmp*
```
`root #````
env-update && source /etc/profile
```
## Changing the CHOST variable and rebuilding the toolchain

Change the CHOST variable, and emerge binutils&gcc. (FYI, [Changing the CHOST variable](https://wiki.gentoo.org/wiki/Changing_the_CHOST_variable))

x86-fbsd users should issue:

`root #``gsed -i 's:CHOST=.*:CHOST="i686-gentoo-freebsd11.1":g' /etc/portage/make.conf`
amd64-fbsd users should issue:

`root #``gsed -i 's:CHOST=.*:CHOST="x86_64-gentoo-freebsd11.1":g' /etc/portage/make.conf`
Emerge binutils and gcc:

`root #``emerge --oneshot sys-devel/binutils '<sys-devel/gcc-7.0'`
Check the old version number of gcc:

`root #``gcc-config -l`
\[1\] x86\_64-gentoo-freebsd11.1-6.4.0 \*
 \[2\] x86\_64-gentoo-freebsd9.1-4.6.4
 \[3\] x86\_64-gentoo-freebsd9.1-4.9.4 \*

And, remove them:

`root #``emerge -C '<gcc-5.0'``root #``gcc-config -c`
Also check the old version number of binutils:

`root #``binutils-config -l`
\[1\] x86\_64-gentoo-freebsd11.1-2.29.1 \*
 \[2\] x86\_64-gentoo-freebsd9.1-2.25.1
 \[3\] x86\_64-gentoo-freebsd9.1-2.27 \*

And, remove them:

`root #``emerge -C '<binutils-2.29'``root #``binutils-config -c`
Remove the FreeBSD 9.x env file:

`root #````
cd /etc/env.d/
```
`root #``grep gentoo-freebsd9. *`
04gcc-x86\_64-gentoo-freebsd9.1:PATH="/usr/x86\_64-gentoo-freebsd9.1/gcc-bin/4.9.4"
04gcc-x86\_64-gentoo-freebsd9.1:ROOTPATH="/usr/x86\_64-gentoo-freebsd9.1/gcc-bin/4.9.4"
04gcc-x86\_64-gentoo-freebsd9.1:MANPATH="/usr/share/gcc-data/x86\_64-gentoo-freebsd9.1/4.9.4/man"
04gcc-x86\_64-gentoo-freebsd9.1:INFOPATH="/usr/share/gcc-data/x86\_64-gentoo-freebsd9.1/4.9.4/info"
04gcc-x86\_64-gentoo-freebsd9.1:LDPATH="/usr/lib/gcc/x86\_64-gentoo-freebsd9.1/4.9.4
04gcc-x86\_64-gentoo-freebsd9.1:/usr/lib/gcc/x86\_64-gentoo-freebsd9.1/4.9.4
04gcc-x86\_64-gentoo-freebsd9.1:/usr/lib/gcc/x86\_64-gentoo-freebsd9.1/4.6.4
04gcc-x86\_64-gentoo-freebsd9.1:/usr/lib/gcc/x86\_64-gentoo-freebsd9.1/4.6.4"

The target file name is 04gcc-x86\_64-gentoo-freebsd9.1 in this case.

`root #````
rm 04gcc-x86_64-gentoo-freebsd9.1
```
`root #````
env-update && source /etc/profile
```
## Removing and re-emerging dev-lang/perl (recommended)

If you are upgrading directly from from the too old version of perl, removing and re-emeging [dev-lang/perl](https://packages.gentoo.org/packages/dev-lang/perl) are recommended to upgrade easily.

Removing perl:

`root #````
emerge -C dev-lang/perl
```
`root #````
emerge -C dev-perl/* perl-core/* virtual/perl*
```
Re-emerging perl:

`root #````
emerge dev-lang/perl
```
`root #````
emerge dev-perl/Text-Unidecode dev-perl/Unicode-EastAsianWidth dev-perl/XML-Parser
```
[dev-perl/libintl-perl](https://packages.gentoo.org/packages/dev-perl/libintl-perl) is required by some packages but it cannot be compiled correctly at this time. After rebuilding all packages, you will be able to compile it. A workaround is here:

`root #````
cd /var/db/repos/gentoo/dev-perl/libintl-perl
```
`root #````
ebuild libintl-perl-1.240.0-r2.ebuild clean prepare
```
`root #````
gsed -i 's:-liconv::g' /var/tmp/portage/dev-perl/libintl-perl-1.240.0-r2/work/libintl-perl-1.24/gettext_xs/Makefile.PL
```
`root #````
ebuild libintl-perl-1.240.0-r2.ebuild merge clean
```
## Rebuilding all packages

`root #````
emerge -aC dev-lang/python:3.3
```
`root #````
gsed -i '/python3.3/d' /etc/python-exec/python-exec.conf
```
`root #````
USE=internal-glib emerge dev-util/pkgconfig
```
`root #````
emerge libtool 
```
`root #````
emerge -ae @world --exclude dev-perl/libintl-perl
```
`root #````
emerge -aC dev-lang/python:3.2
```
`root #````
emerge dev-perl/libintl-perl
```
`root #````
emerge sys-apps/portage
```
`root #````
eselect python set 1
```
If one of the packages fails to compile you can issue 'emerge --resume --skipfirst' to continue emerging the remaining packages. Also, consider filing a bug report of the problem.

## Cleaning

Let's remove the backup files when you have finished all the steps:

`root #````
emerge sys-devel/libtool app-admin/eselect
```
`root #````
emerge @preserved-rebuild
```
`root #````
dispatch-conf
```
## Re-select profile

Please re-select the profile if make.profile exists in /etc.

`root #``ls -l /etc/make.profile`
lrwxr-xr-x  1 root  portage  48 Dec  1 00:00 /etc/make.profile -> ../var/db/repos/gentoo/profiles/default/bsd/fbsd/x86/9.1

`root #````
rm /etc/make.profile
```
`root #````
eselect profile list
```
Available profile symlink targets:
  \[1\]   default/bsd/fbsd/x86/9.0
  \[2\]   default/bsd/fbsd/x86/9.1

`root #``eselect profile set 2`
## Sample script to update automatically

You can get the sample script for easy updates.

[https://gitweb.gentoo.org/proj/gentoo-bsd.git/plain/scripts/automatic\_updater.sh](https://gitweb.gentoo.org/proj/gentoo-bsd.git/plain/scripts/automatic_updater.sh)

| Argument 1 | Target Version (e.g. 11.1) | 
| Argument 2 | kernel, freebsd\_userland, world | 

### How to use script

`root #````
bash automatic_updater.sh 11.1 kernel
```
`root #````
shutdown -r now
```
`root #````
bash automatic_updater.sh 11.1 freebsd_userland
```
`root #````
REMOVEPERL=1 bash automatic_updater.sh 11.1 world
```
