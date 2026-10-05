<!-- source: https://wiki.gentoo.org/wiki/VPS/VServer_Guide | group: Gentoo Wiki (Main) | wiki-title: VPS/VServer Guide -->
---
title: VPS/VServer Guide
url: https://wiki.gentoo.org/wiki/VPS/VServer_Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-09-25"
fingerprint: c920a110193ebfa
license: CC BY-SA 4.0
---

# VPS/VServer Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**March 13, 2017**, the information in this article is probably

**outdated**. You can help the Gentoo community by verifying and

[updating this article](https://wiki.gentoo.org/index.php?title=VPS/VServer_Guide&action=edit).

This guide walks through the setup of a basic virtual server using Linux-VServer Technology.

## Introduction

### The Linux-VServer Concept

The basic concept of the Linux-VServer solution is to separate the user-space environment into distinct units (sometimes called Virtual Private Servers) in such a way that each VPS looks and feels like a real server to the processes contained within.

### Terminology

| Term | Description | 
|---|---|
| Linux-VServer, VServer | Linux-VServer is the official name of the project and used in this Howto the same way | 
| virtual server, vserver, guest system | All these are interchangable and refer to one instance of a server (i.e. one virtual server) | 
| host system, host | The physical machine running your Gentoo Linux will host all virtual servers | 
| util-vserver | The util-vserver package contains all programs necessary for maintaining your virtual servers | 

## Installation

### Emerge

Install a VServer kernel sources:

`root #``emerge --ask vserver-sources`
### Kernel

After the vserver-sources are installed it's time to configure them using make menuconfig. Below is a common configuration for 2.1.1 and above. If you are using 2.0.x some configuration options may not be present.

**Configure vserver-sources**

**Configure vserver-sources**

## Configuration

### fstab

**`/etc/fstab`**

**Fstab with extended attributes**

After you've built and installed the kernel, update your boot loader and finally reboot to see if the kernel boots correctly.

### Kernel

Install the kernel:

`root #````
make
```
`root #````
make modules_install
```
`root #````
cp arch/<arch>/boot/bzImage /boot/kernel-<KERNELVERSION>-vserver-<VSERVERVERSION>
```
`root #````
reboot
```
### Setup host environment

To maintain your virtual servers you need the util-vserver package which contains all necessary programs and many useful features.

Install util-vserver:

`root #``emerge --ask >=sys-cluster/util-vserver-0.30.212`
You have to run the vprocunhide command after every reboot in order to setup /proc permissions correctly for vserver guests. Two init scripts have been installed by util-vserver which run the `vprocunhide` command for you and take care of virtual servers during shutdown of the host.

util-vserver init scripts:

`root #````
rc-update add vprocunhide default
```
`root #````
/etc/init.d/vprocunhide start
```
`root #````
rc-update add util-vserver default
```
`root #````
/etc/init.d/util-vserver start
```
## Guest creation

### Download a precompiled stage3

Since many hardware related commands are not available inside a virtual server, there has been a patched version of baselayout known as baselayout-vserver. However, since baselayout-2/openrc, all required changes have been integrated, eliminating the need for seperate vserver stages, profiles and baselayout. Stage tarballs can be downloaded from our mirrors.

Since a stage3 contains a complete root filesystem you can use the template build method of util-vserver. However, this method only works reliable since util-vserver-0.30.213\_rc5, so make sure you have the right version installed.

You have to choose a context ID for your vserver (dynamic context IDs are discouraged) as well as the necessary network device information (In this example eth0 is configured with 192.168.1.253/24 and the context ID is equivalent to the last two parts of the virtual servers IP).

### Using the template build method

For a long time now, plain init style was the only init style available for gentoo, i.e. a normal init process will be started inside the guest, just like on any common Unix system. However this approach has some drawbacks:

- No possibility to see output of init/rc scripts
- Wasted resources for idle init processes in each guest
- Annoying conflicts for /etc/inittab

Therefore, many users have requested to re-implement the Gentoo init style, which has been abandoned since it was a very hacky implementation and more or less worked by accident due to other modifications done to base layout back then. However, as of util-vserver-0.30.212 the gentoo init style has been re-implemented in a concise manner and will become the default in the future.

Start stage3 installation:

`root #``vserver myguest build --context 1253 --hostname gentoo --interface eth0:192.168.1.253/24 --initstyle gentoo -m template -- -d gentoo -t /path/to/stage3-<arch>-<version>.tar.bz2`
You should now be able to start and enter the vserver by using the commands below. Test the virtual server:

`root #``vserver myguest start`
OpenRC 0.4.3 is starting up Gentoo Linux (x86\_64) \[VSERVER\]
Press I to enter interactive boot mode
\* /proc is already mounted, skipping
\* Setting hostname to myguest...                     \[ ok \]
\* Creating user login records...                     \[ ok \]
\* Cleaning /var/run...                               \[ ok \]
\* Wiping /tmp directory...                           \[ ok \]
\* Updating /etc/mtab...                              \[ ok \]
\* Initializing random number generator...            \[ ok \]
\* Starting syslog-ng...                              \[ ok \]
\* Starting fcron...                                  \[ ok \]
\* Starting Name Service Cache Daemon...              \[ ok \]
\* Starting local...                                  \[ ok \]

`root #``vserver-stat`
CTX   PROC    VSZ    RSS  userTIME   sysTIME    UPTIME NAME
0       90   1.4G 153.4K  14m00s11   6m45s17   2h59m59 root server
1252     2     3M   286    0m00s45   0m00s42   0m02s91 myguest

`root #``vserver myguest enter``root #``ps ax````
  PID TTY      STAT   TIME COMMAND
    1 ?        Ss     0:04 init [3]
27637 ?        Ss     0:00 /usr/sbin/syslog-ng
27656 ?        Ss     0:00 /usr/sbin/fcron -c /etc/fcron/fcron.conf
27676 ?        Ssl    0:00 /usr/sbin/nscd
27713 ?        S+     0:00 login
27737 pts/15   Ss     0:00 /bin/bash
27832 pts/15   R+     0:00 ps ax
```
`root #``logout`
## Maintenance made easy

### Start guests on boot

You can start certain guests during boot. Each guest can be assigned a MARK. Now everything you have to do is configure these MARKs in the guests configuration and add the appropriate init scripts to the default runlevel. Configure MARKs for each guest:

`root #````
mkdir -p /etc/vservers/myguest/apps/init
```
`root #````
echo "default" > /etc/vservers/myguest/apps/init/mark
```
Add init script to the default runlevel:

`root #``rc-update add vservers.default default`
### Keep Portage in sync

The script vesync will help you to keep the metadata cache and overlays in sync. vemerge is a simple wrapper for emerge in guests:

`root #````
vesync myguest
```
`root #````
vesync --all
```
`root #````
vesync --all --overlay /usr/local/overlays/myoverlay --overlay-host rsync://rsync.myhost.com/myoverlay --overlay-only
```
`root #````
vemerge myguest -- app-editors/vim -va
```
### Update guests

Gentoo guests can share packages to save compilation time. In order to use shared packages, you have to create a central directory for packages on the host. We will use /var/cache/vpackages on the host and mount it to /var/cache/binpkgs in every guest.

`root #``mkdir -p /var/cache/vpackages`
**`/etc/vservers/myguest/fstab`**

**Add bind mount to guest configuration**

Now you can use vupdateworld to update every guest. The command is equivalent to something like `emerge --deep --update --newuse world` depending on command line options.

Pretend update for 'myguest':

`root #``vupdateworld myguest -- -vp`
Update 'myguest' using binary packages:

`root #``vupdateworld myguest -- -k`
Update all guests using binary packages:

`root #``vupdateworld --all -- -k`
After a successful update you can easily update all configuration files with `vdispatch-conf`. It is a simple wrapper for `dispatch-conf` and behaves exactly the same.

Update configuration files for 'myguest':

`root #``vdispatch-conf myguest`
Update configuration files for all guests:

`root #``vdispatch-conf --all`
### Contact

Please feel free to contact the author or file a bug on [Bugzilla](https://bugs.gentoo.org/) in case of any problems.
