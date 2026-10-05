<!-- source: https://wiki.gentoo.org/wiki/OpenVZ | group: Gentoo Wiki (Main) | wiki-title: OpenVZ -->
---
title: OpenVZ
url: https://wiki.gentoo.org/wiki/OpenVZ
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2017-03-13"
fingerprint: "1e05122d01dd6bd3"
license: CC BY-SA 4.0
---

# OpenVZ

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **deprecated (obsolete)**. Contents are <u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

In this guide provides instructions on setting up a basic virtual server using the OpenVZ Technology.

## Installation

### Emerge

Install the [sys-kernel/openvz-sources](https://packages.gentoo.org/packages/sys-kernel/openvz-sources) package:

`root #``emerge --ask sys-kernel/openvz-sources`
### Kernel

**Configure openvz-sources**

After you've built and installed the kernel (steps not covered in this article), update the boot loader and reboot to see if the kernel boots correctly.

### Host environment

To maintain your virtual servers you need the [sys-cluster/vzctl](https://packages.gentoo.org/packages/sys-cluster/vzctl) package which contains all necessary programs and many useful features:

`root #``emerge --ask sys-cluster/vzctl`
## Configuration

The vzctl packages has installed an init script called vz. It will help you to start/stop virtual servers on boot/shutdown:

`root #````
rc-update add vz default
```
`root #````
/etc/init.d/vz start
```
### Creating a guest template

#### Template tarball

Since many hardware related commands are not available inside a virtual server, there has been a patched version of baselayout known as baselayout-vserver. However all required changes have been integrated into normal baselayout-2, eliminating the need for seperate vserver stages, profiles and baselayout. The only (temporary) drawback is that baselayout-2 is still considered to be in alpha stage and there are no stages with baselayout-2 available on the mirrors yet.

As soon as baselayout-2 is stable you can use a precompiled stage3/4 from one of our mirrors. In the meantime please download a stage3/4 from here.

Convert the stage tarball:

`root #````
cd /vz/template/cache
```
`root #````
bunzip2 stage4-<arch>-<version>.tar.bz2
```
`root #````
mv stage4-tarball.tar gentoo-<arch>-<version>.tar
```
`root #````
gzip gentoo-<arch>-<version>.tar
```
`root #````
cd -
```
Create VPS

`root #``vzctl create <vpsid> --ostemplate gentoo-<arch>-<version>`
## Usage

### Test the virtual server

You should be able to start and enter the vserver by using the commands below:

`root #````
vzctl start <vpsid>
```
`root #````
vzctl enter <vpsid>
```
`root #``ps ax````
PID   TTY      STAT   TIME COMMAND
    1 ?        S      0:00 init [3]
20496 pts/0    S      0:00 /bin/bash -i
20508 pts/0    R+     0:00 ps ax
```
`root #``logout`
