<!-- source: https://wiki.gentoo.org/wiki/Orange_Pi_Plus2 | group: Gentoo Wiki (Main) | wiki-title: Orange Pi Plus2 -->
---
title: Orange Pi Plus2
url: https://wiki.gentoo.org/wiki/Orange_Pi_Plus2
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-21"
fingerprint: "52ba1e5c15c02b96"
license: CC BY-SA 4.0
---

# Orange Pi Plus2

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Intro

The Orange Pi Plus2 ([http://www.orangepi.org/orangepiplus2/](http://www.orangepi.org/orangepiplus2/)) is one of the most powerful boards of the Orange Pi line of products. It carries 2Gb RAM and an USB-SATA bridge, therefore allowing the connection of classical HDDs to it.

A variant named Orange Pi Plus 2E ([http://www.orangepi.org/orangepiplus2e/](http://www.orangepi.org/orangepiplus2e/)) is supposed to have a better performance and more stability. However, it lacks the SATA interface and carries 3 USB 2.0 ports instead of 4.

The GPIO header on both boards is much similar to the one found on Raspberry Pi but one shall take into account that the header is rotated 180 degrees on the 2E model.

Both models work relatively well under an implementation of Gentoo for a IoT/server environment (no graphics tested, no video, GPIO intensively used). The only frequent issue are hangouts during long and highly intensive use of the board, however, they are not as frequent as with the Raspberry Pi. Anyway, building a system from stage3 on these boards is NOT recommended. Package building and upgrading may not be so troublesome as with the RPi family but there is a clear exception to such large builds like gcc, glibc or llvm. So, the best way to create and support a Orange Pi Plus2 Gentoo implementation is to cross-compile, which can be quite difficult. However, there is a plus - the main difference between Plus2, the Plus2E and probably some other boards is miserable. In most cases one needs only to choose the correct Device Tree Blob (DTB) file in the boot sequence. So, if one manages to create a cross-compile environment for one Orange, probably that will fit for other models.

With some changes, the cross-compile environment can be used on Raspberry Pi 2 boards (it was successfully tested here).

## Building an image

### Initial remarks

Most data to implement a Gentoo system was taken from:

3\. [Raspberry\_Pi](https://wiki.gentoo.org/wiki/Raspberry_Pi)

However, there are several critical issues that do not allow to follow the above instructions by the letter. The most important - avoid all about kernel version 3. It will bring more trouble using such versions than to build a kernel 4.15 or higher. Even if taken from a ready build, most 3.\* kernels look far from being stable on the Orange platform.

Note that Orange is a armv7 machine which corresponds to the newer RPis.

### Build crossdev

In any case we need crossdev. So, we build it much like in the instructions in the above pages:

`root #``emerge --ask sys-devel/crossdev`
However better not use the `-S` (stable build) in the following step:

`root #``crossdev -v -t armv7a-hardfloat-linux-gnueabi`
The "stable" build is based on GCC version 5.\* and this will lead to conflicts with more recent packages.

### Build from zero?

With good serious effort, it seems possible to install Gentoo through the sequence suggested on \[3\] - crossdev and latter chroot. But to achieve it there is the need to overcome several issues with packages preceding the build of portage. A simpler approach is to load a stage3 and unpack it under the path where we will cross-compile. Gentoo has an old image in its Download page ([https://gentoo.org/downloads/](https://gentoo.org/downloads/)). You can find it by clicking the ARM tab and choosing ARMv7a|HardFP. Once downloaded check its correct name and unpack it as below:

`root #````
cp stage3-armv7a_hardfp.tar.bz2 /usr/armv7a-hardfloat-linux-gnueabi
```
`root #````
cd /usr/armv7a-hardfloat-linux-gnueabi
```
`root #````
tar -xjf stage3-armv7a_hardfp.tar.bz2
```
### Cross-compiling

If you unpacked a stage3 you can jump over this section. However, cross compiling is several times faster than chroot and besides, in certain situations, you may be forced to use directly cross-compiling.

Two things shall be remarked here:

1\. The make.conf for cross-compiling is radically different from the usual ones. This one carries several paths and options showing that you are in a cross-compiling environment. So, every time you cross-compile directly, you SHALL change the make.conf! Failure to do this will lead to ARM binaries landing on your main system and causing serious havoc!

2\. Cross-compiling is NOT based on your usual building tools but on a parallel set, specially created by crossdev. However, due the way certain tools are made or depending on how crossdev was built, there could be cases when some building step tries to call the tools on the main system! Surely this will bring serious trouble without a clear way around it.

We some changes you may try this make.conf:

**`/etc/portage/make.conf`**

```
CHOST=armv7a-hardfloat-linux-gnueabi
CBUILD=x86_64-pc-linux-gnu
ARCH=arm
HOSTCC=${CBUILD}-gcc
CXX_FOR_BUILD="armv7a-hardfloat-linux-gnueabi-g++"
ROOT=/usr/${CHOST}/
CFLAGS="-O3 -march=armv7ve -mcpu=cortex-a7 -mfpu=neon-vfpv4 -mtune=cortex-a7 -mfloat-abi=hard -pipe -U_FORTIFY_SOURCE"
#CFLAGS="-O2 -pipe -U_FORTIFY_SOURCE"
CXXFLAGS="${CFLAGS}"
ACCEPT_KEYWORDS="${ARCH} ~${ARCH}"
PORTDIR="/usr/portage"
DISTDIR="/usr/portage/distfiles"
#PORTDIR_OVERLAY="/usr/portage/local/"
PKGDIR=${ROOT}packages/
PKG_CONFIG_PATH="${ROOT}usr/lib/pkgconfig/"
USE="${ARCH} -pam -acl -ncurses -xattr -vtv -native-extensions"
FEATURES="-collision-protect sandbox buildpkg noman noinfo nodoc"
PORTAGE_TMPDIR=${ROOT}tmp/
MAKEOPTS="-j4"
ELIBC="glibc"
LC_MESSAGES=C
PYTHON_SINGLE_TARGET="python3_6"
PYTHON_TARGETS="python2_7 python3_6"
```
Note the CXX\_FOR\_BUILD. This is one of the cases mentioned above - instead of calling the correct g++ some config tools called /usr/bin/g++.

Meanwhile, note the correct way to build packages in such environment is to use the tools with a host prefix: "armv7a-hardfloat-linux-gnueabi-". For example, if you would want to compile gcc you shall call:

`root #` `armv7a-hardfloat-linux-gnueabi-emerge gcc`
### Chroot

## Partitioning

The are three variants to install Gentoo on Plus2 (two on the Plus2E): SD, MMC and SATA. In any case a SD card is needed for the first steps.
