<!-- source: https://wiki.gentoo.org/wiki/Gentoo_FreeBSD/Developers_Note | group: Gentoo Wiki (Main) | wiki-title: Gentoo FreeBSD/Developers Note -->
---
title: Gentoo FreeBSD/Developers Note
url: https://wiki.gentoo.org/wiki/Gentoo_FreeBSD/Developers_Note
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2020-02-14"
fingerprint: d83efcea7d464317
license: CC BY-SA 4.0
---

# Gentoo FreeBSD/Developers Note

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Archived article**

This article is **archived (obsolete)**. Contents are surely incorrect for current usage, and are intended for historical reference only.

TLDR:

**Do not use this article!**

This article is a guide on how to make new Gentoo FreeBSD stages with `catalyst`. The latest stage3 equivalent environment is required in order to set the source\_subpath of catalyst. Let's ready to update the old stage3.

A couple sample scripts are available: [create\_forcestage3.sh](https://wiki.gentoo.org#create_forcestage3.sh) and [stages\_builder.sh](https://wiki.gentoo.org#stages_builder.sh).

### Preparation

Extract the old stage3, chrooting, update, and compress with tar.

e.g.) Target version 10.2, amd64 architecture

`root #````
export WORKDIR=/10.2/amd64
```
`root #``mkdir -p "${WORKDIR}"`
#### Downloading stage3 tarball and unpack

`root #``wget -P /tmp` [http://distfiles.gentoo.org/experimental/bsd/freebsd/stages/amd64-fbsd-9.1/stage3-amd64-freebsd-9.1.tar.bz2](http://distfiles.gentoo.org/experimental/bsd/freebsd/stages/amd64-fbsd-9.1/stage3-amd64-freebsd-9.1.tar.bz2)
`root #``tar xjpf /tmp/stage3-*.tar.bz2 -C "${WORKDIR}"`
#### Mount main Gentoo repository

`root #````
mkdir -p "${WORKDIR}"/usr/portage
```
`root #````
mount -t nullfs /usr/portage "${WORKDIR}"/usr/portage
```
#### Entering the new environment

`root #````
mount -t devfs devfs "${WORKDIR}"/dev
```
`root #````
cp /etc/resolv.conf "${WORKDIR}"/etc/
```
`root #``chroot "${WORKDIR}" /bin/bash`
#### Update old stage3

Please see [Gentoo FreeBSD/Upgrade Guide](https://wiki.gentoo.org/wiki/Gentoo_FreeBSD/Upgrade_Guide) if you want to update the manual method.

`(chroot) root #````
[ -e /etc/make.conf ] && mv /etc/make.conf /etc/portage/make.conf
```
`(chroot) root #````
echo "MAKEOPTS=-j5" >> /etc/portage/make.conf
```
`(chroot) root #````
echo 'USE="${USE} -fortran -build-kernel"' >> /etc/portage/make.conf
```
`(chroot) root #``wget -P /tmp` [https://gitweb.gentoo.org/proj/gentoo-bsd.git/plain/scripts/automatic_updater.sh](https://gitweb.gentoo.org/proj/gentoo-bsd.git/plain/scripts/automatic_updater.sh)
`(chroot) root #````
bash /tmp/automatic_updater.sh 10.2 kernel
```
`(chroot) root #````
bash /tmp/automatic_updater.sh 10.2 freebsd_userland
```
`(chroot) root #````
emerge -C dev-vcs/git
```
`(chroot) root #````
export EMERGE_DEFAULT_OPTS="--keep-going=y"
```
`(chroot) root #````
REMOVEPERL=1 bash /tmp/automatic_updater.sh 10.2 world
```
`(chroot) root #````
unset EMERGE_DEFAULT_OPTS
```
`(chroot) root #``exit`
#### Compress with tar

`root #````
kill -9 $(ps auxw | grep ebuild-helpers/ecompressdir | grep -v grep | awk '{ print $2 }' | xargs)
```
`root #````
umount "${WORKDIR}"/usr/portage
```
`root #``umount "${WORKDIR}"/dev``root #````
mkdir -p /var/tmp/catalyst/builds/default
```
`root #````
cd "${WORKDIR}"
```
`root #``tar cjpf /var/tmp/catalyst/builds/default/stage3-amd64-fbsd-10.2-forcestage3.tar.bz2 .`
### Run stages\_builder.sh script

`root #`
OLDVER=10.2 \
TARGETVER=10.2 \
FORCESTAGE3=stage3-amd64-fbsd-10.2-forcestage3 \
NOOVERLAY=1 \
/tmp/stages_builder.sh &> ~/amd64-fbsd-10.2.log

## create\_forcestage3.sh

Download the sample script for easy creation of a forcestage3 tarball:

[https://gitweb.gentoo.org/proj/gentoo-bsd.git/plain/scripts/mkstages/create\_forcestage3.sh](https://gitweb.gentoo.org/proj/gentoo-bsd.git/plain/scripts/mkstages/create_forcestage3.sh)

On completion, it will be displayed FORCESTAGE3. Please set when you run stages\_builder.sh.

### amd64-fbsd

`root #``TARGETVER=10.2 bash create_forcestage3.sh`
### amd64-fbsd/clang

`root #`
TARGETVER=10.2 \
OLDSTAGE3=http://distfiles.gentoo.org/experimental/bsd/freebsd/stages/amd64-fbsd-9.1/clang/stage3-amd64-clangfbsd-9.1.tar.bz2 \
CLANG=1 \
bash create_forcestage3.sh

### x86-fbsd

`root #`
TARGETVER=10.2 \
OLDSTAGE3=http://dev.gentoo.org/~aballier/fbsd9.0/x86/stage3-i686-freebsd-9.0.tar.bz2 \
TARGETARCH=i686 \
bash create_forcestage3.sh

## stages\_builder.sh

Download a sample script to easily create new stages:

[https://gitweb.gentoo.org/proj/gentoo-bsd.git/plain/scripts/mkstages/stages\_builder.sh](https://gitweb.gentoo.org/proj/gentoo-bsd.git/plain/scripts/mkstages/stages_builder.sh)

### amd64-fbsd

`root #`
OLDVER=10.2 \
TARGETVER=10.2 \
FORCESTAGE3=stage3-amd64-fbsd-10.2-forcestage3 \
NOOVERLAY=1 \
bash /tmp/stages_builder.sh &> ~/amd64-fbsd-10.2.log

### amd64-fbsd/clang

`root #`
OLDVER=10.2 \
TARGETVER=10.2 \
FORCESTAGE3=stage3-amd64-fbsd-10.2-forcestage3-cl \
CLANG=1 \
NOOVERLAY=1 \
bash /tmp/stages_builder.sh &> ~/amd64-fbsd-10.2-clang.log

### x86-fbsd

`root #`
OLDVER=10.2 \
TARGETVER=10.2 \
FORCESTAGE3=stage3-i686-fbsd-10.2-forcestage3 \
NOOVERLAY=1 \
bash /tmp/stages_builder.sh x86 &> ~/x86-fbsd-10.2.log

## Run the catalyst manually

Sample spec files are available from gentoo-bsd overlay. Please use after replace the contents.

[https://gitweb.gentoo.org/proj/gentoo-bsd.git/tree/scripts/mkstages/specs/](https://gitweb.gentoo.org/proj/gentoo-bsd.git/tree/scripts/mkstages/specs/)

`root #````
catalyst -C target=snapshot version_stamp=20171201
```
`root #````
catalyst -f stage1_tmp.spec
```
`root #````
catalyst -f stage2_tmp.spec
```
`root #````
catalyst -f stage3_tmp.spec
```
`root #````
catalyst -f stage1.spec
```
`root #````
catalyst -f stage2.spec
```
`root #``catalyst -f stage3.spec`
