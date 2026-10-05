<!-- source: https://wiki.gentoo.org/wiki/Binary_package_guide/Building_cross | group: Gentoo Wiki (Main) | wiki-title: Binary package guide/Building cross -->
---
title: Binary package guide/Building cross
url: https://wiki.gentoo.org/wiki/Binary_package_guide/Building_cross
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-12"
fingerprint: "4e691a7e6543072a"
license: CC BY-SA 4.0
---

# Binary package guide/Building cross

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Gentoo binhost**

When building for different architectures such as AMD64 host and ARM64 client, then cross compiling is the method needed to create binpkgs.

Gentoo mainly supports two different ways to do this with ease:

- Crossdev

crossdev builds a toolchain that can compile for a different architecture then it is bring run on. It is very fast at making these binaries, but also prone to issues during build time.

- QEMU user

Using QEMU it is possible to emulate a completely different architecture on the host as if it was just a normal chroot. It can be up to ten times slower, however less compile time bugs are *usually* found.

### crossdev

[crossdev](https://wiki.gentoo.org/wiki/Crossdev) is a tool to easily build cross-compile toolchains. This is useful to create binary packages for installation on a system whose [architecture](https://packages.gentoo.org/arches) differs from that of the system used to build the packages. A common example would be building binary packages for a device like an **arm64** [Raspberry Pi](https://wiki.gentoo.org/wiki/Raspberry_Pi) from a more powerful **amd64** desktop PC.

An installation guide for [sys-devel/crossdev](https://packages.gentoo.org/packages/sys-devel/crossdev) can be found at the [crossdev](https://wiki.gentoo.org/wiki/Crossdev) page.

Using crossdev with the following command can build a toolchain for the desired system:

`root #``crossdev --stable -t <arch-vendor-os-libc>`
For the rest of this section, the example target will be for a Raspberry Pi 4:

`root #``crossdev --stable -t  aarch64-unknown-linux-gnu`
After this has built, a toolchain will have been created in /usr/aarch64-unknown-linux-gnu, and will look like a bare-bones Gentoo install where it is possible to edit [Portage](https://wiki.gentoo.org/wiki/Portage) settings as normal.

Removing the `-pam` flag from the `USE` line in /usr/aarch64-unknown-linux-gnu/etc/portage/make.conf is generally recommended in a setup like this:

**`/usr/aarch64-unknown-linux-gnu/etc/portage/make.conf`**

**Disable the pam USE flag**

```
CHOST=aarch64-unknown-linux-gnu
CBUILD=x86_64-pc-linux-gnu
 
ROOT=/usr/${CHOST}/
 
ACCEPT_KEYWORDS="${ARCH}"
 
USE="${ARCH}"
 
CFLAGS="-O2 -pipe -fomit-frame-pointer"
CXXFLAGS="${CFLAGS}"
 
FEATURES="-collision-protect sandbox buildpkg noman noinfo nodoc"
# Ensure pkgs from another repository are not overwritten
PKGDIR=${ROOT}var/cache/binpkgs/
 
#If you want to redefine PORTAGE_TMPDIR uncomment (and/or change the directory location) the following line
PORTAGE_TMPDIR=${ROOT}var/tmp/
 
PKG_CONFIG_PATH="${ROOT}usr/lib/pkgconfig/"
#PORTDIR_OVERLAY="/var/db/repos/local/"
```
List available profiles for the device by running:

`root #``PORTAGE_CONFIGROOT=/usr/aarch64-unknown-linux-gnu eselect profile list`
Next, select the profile that best suits:

`root #``PORTAGE_CONFIGROOT=/usr/aarch64-unknown-linux-gnu eselect profile set <profile number>`
To build a single binary package for use on the device, use the following:

`root #``emerge-aarch64-unknown-linux-gnu --ask foo`
To build every package in the world file, then the following command is needed:

`root #``emerge-aarch64-unknown-linux-gnu --emptytree @world`
By default, all binary packages will be stored in /usr/aarch64-unknown-linux-gnu/var/cache/binpkgs, so this is the location needed to be selected when [setting up a binary package host](https://wiki.gentoo.org/wiki/Binary_package_guide#Setting_up_a_binary_package_host).

### QEMU chroot compiling

Another method is to emulate the CPU of the other architecture using [QEMU virtualization software](https://en.wikipedia.org/wiki/QEMU) as if it was a normal chroot on the host system.

The pros of this method is that is simple to work with and are far less likely to run into cross compiling bugs which can save a lot of time. The downside is qemu can be around 10 times slower to compile and some of the more niches architectures, like HPPA are less used so there is a chance the user could be the first to discover a new QEMU bug.

To setup QEMU cross compiling, just follow the excellent guide at [Embedded Handbook/General/Compiling with QEMU user chroot](https://wiki.gentoo.org/wiki/Embedded_Handbook/General/Compiling_with_QEMU_user_chroot) and then use native chroot guide from [Binary package guide](https://wiki.gentoo.org/wiki/Binary_package_guide#Configuring_the_chroot)
