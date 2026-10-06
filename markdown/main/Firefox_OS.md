<!-- source: https://wiki.gentoo.org/wiki/Firefox_OS | group: Gentoo Wiki (Main) | wiki-title: Firefox OS -->
---
title: Firefox OS
url: https://wiki.gentoo.org/wiki/Firefox_OS
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-06-10"
fingerprint: b74c944f6f45ebab
license: CC BY-SA 4.0
---

# Firefox OS

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Archived article**

This article is **archived (obsolete)**. Contents are surely incorrect for current usage, and are intended for historical reference only.

TLDR:

**Do not use this article!**

This page details how to build Firefox OS and flash it on your phone. Note that Firefox OS is not the [Firefox](https://wiki.gentoo.org/wiki/Firefox) web browser.

## Requirements

### Phone

- Obviously, you will need a smartphone supported by Firefox OS, see the [list here](https://developer.mozilla.org/en-US/Firefox_OS/Firefox_OS_build_prerequisites#Have_a_compatible_deviceemulator).
- fastboot must be enabled, you may need to unlock the bootloader for that. This is out of the scope of this document.

### Computer

- 64bit Gentoo with a multilib profile
- 4GB of RAM
- 30GB of disk space (sources alone take 14GB)

### Packages

Quite a few packages are required, so we might as well create a set.

**`/etc/portage/sets/firefox_os`**

**@firefox\_os set**

```
# see https://developer.mozilla.org/en-US/Firefox_OS/Firefox_OS_build_prerequisites
=sys-devel/autoconf-2.13
sys-devel/bison
app-arch/bzip2
dev-java/icedtea-bin
dev-util/ccache 
net-misc/curl
sys-devel/flex
sys-apps/gawk
dev-vcs/git
=sys-devel/make-3.82-r4
sys-devel/patch
media-libs/mesa
x11-libs/libX11
sys-libs/ncurses 
sys-libs/zlib
dev-util/android-tools
```
You also need the 32bit versions of ncurses and zlib.

**`/etc/portage/package.use/firefox_os`**

```
sys-libs/ncurses abi_x86_32
sys-libs/zlib abi_x86_32
```
`root #``emerge --ask @firefox_os`
### adb/fastboot

#### Test adb

Check if adb detects your phone

`user $``adb devices`
\* daemon not running. starting it now on port 5037 \*
\* daemon started successfully \*
List of devices attached 
roamer2 device

#### Udev rule

If your phone is not detected, you may need a new udev rule

**`/etc/udev/rules.d/firefox_os.rules`**

```
SUBSYSTEM=="usb", ATTR{idVendor}=="19d2", ATTR{idProduct}=="1351", MODE="0666", GROUP="plugdev"
```
`root #````
/etc/init.d/udev restart
```
`root #````
adb kill-server
```
Then retry to detect the phone.

## Backing up your data

`user $````
mkdir backup
```
`user $````
adb pull /system backup/system
```
`user $````
adb pull /data backup/data
```
`user $````
# Some phones also have a /vendor
```
`user $````
adb pull /vendor backup/vendor
```
This backup can be used later to build Firefox OS because some non-free files are not included in the sources and must be extracted from a backup (or the phone directly)

Alternatively you can use [this script](https://github.com/Mozilla-TWQA/B2G-flash-tool/#backup_restore_profilepy) to backup and restore user data.

## Building Firefox OS

### Find the phone's codename

See [this table](https://developer.mozilla.org/en-US/Firefox_OS/Developer_phone_guide/Phone_specs).

### Choose a Firefox OS version

The list of versions available can be seen [here](https://github.com/mozilla-b2g/b2g-manifest/branches).
As of this writing, *v2.2* is the lastest stable version but you can build any version you want.

### Python

The script fetching the sources uses python, unfortunatly it only works with python2, if your default python interpreter is python3, you will need this small hack

`user $````
mkdir /tmp/ffos_bin
```
`user $````
ln -s `which python2` /tmp/ffos_bin/python
```
`user $````
export PATH="/tmp/ffos_bin:${PATH}"
```
### GCC

Old versions of Firefox OS require GCC 4.6, the current master can be built with any modern version. See this [MDN page](https://developer.mozilla.org/en-US/Firefox_OS/Customization_with_the_.userconfig_file#Changing_the_default_host_compiler) for information on how to use older compilers if you need to build an older version.

### Non-free files

To build Firefox OS you need some non-free files only present on the phone (or backups of the phone). If you did a backup, you can use it for the build by setting ANDROIDFS\_DIR, otherwise you need to connect your phone via USB while building.

`user $``export ANDROIDFS_DIR="${PWD}/backup"`
### Fetching the sources

`user $``git clone` [git://github.com/mozilla-b2g/B2G.git](git://github.com/mozilla-b2g/B2G.git)
`user $````
cd B2G
```
`user $````
BRANCH=v1.4 ./config.sh inari
```
The last command will start fetching the sources, this will take a **very long time**, possibly hours since it basically downloads 14GB of data.

### Build

`user $``./build.sh`
Building takes quite some time and displays a *lot* of warnings.

## Flashing

`user $``./flash.sh`
If all goes well, your phone should reboot on the new version of Firefox OS
