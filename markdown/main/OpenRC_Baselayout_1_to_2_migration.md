<!-- source: https://wiki.gentoo.org/wiki/OpenRC/Baselayout_1_to_2_migration | group: Gentoo Wiki (Main) | wiki-title: OpenRC/Baselayout 1 to 2 migration -->
---
title: OpenRC/Baselayout 1 to 2 migration
url: https://wiki.gentoo.org/wiki/OpenRC/Baselayout_1_to_2_migration
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-01-16"
fingerprint: "953139f94f238b80"
license: CC BY-SA 4.0
---

# OpenRC/Baselayout 1 to 2 migration

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **deprecated (obsolete)**. Contents are <u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**


This guide provides instructions on migrating from baselayout-1 to baselayout-2 using OpenRC.

## Background

### What is baselayout?

Baselayout provides a basic set of files that are necessary for all systems to function properly, such as /etc/hosts. It also provides the basic filesystem layout used by Gentoo (i.e. /etc, /var, /usr, /home directories).

### What is OpenRC?

OpenRC is a dependency-based rc (run command) system that works with the init program that is provided by the system, normally /sbin/init. OpenRC is *not* a replacement for /sbin/init. The default init system used by Gentoo Linux is [sys-apps/sysvinit](https://packages.gentoo.org/packages/sys-apps/sysvinit), while Gentoo/FreeBSD uses the FreeBSD init provided by sys-freebsd/freebsd-sbin.

### Why migrate?

Originally Gentoo's rc system was built into baselayout 1 and written entirely in [bash](https://wiki.gentoo.org/wiki/Bash). This led to several limitations. For example, certain system calls need to be accessed during boot and this required C-based callouts to be added. These callouts were each statically linked, causing the rc system to bloat over time.

Additionally, as Gentoo expanded to other platforms like Gentoo/FreeBSD and Gentoo Embedded, it became impossible to require a bash-based rc system. This led to a development of baselayout 2, which is written in C and only requires a POSIX-compliant shell. During the development of baselayout 2, it was determined that it was a better fit if baselayout merely provided the base files and filesystem layout for Gentoo and the rc system was broken off into its own package. Thus OpenRC was born.

OpenRC was initially developed by [Roy Marples](http://roy.marples.name/openrc) until 2010, and is now maintained by the [Gentoo OpenRC Project](https://wiki.gentoo.org/wiki/Project:OpenRC). OpenRC supports all current Gentoo variations (i.e. Gentoo Linux, Gentoo/FreeBSD, Gentoo Embedded, and Gentoo Vserver) and other platforms such as FreeBSD and NetBSD.

## Migration to OpenRC

Migration to OpenRC is fairly straightforward; it will be pulled in as part of the regular upgrade process by the package manager. The most important step comes after `>=sys-apps/baselayout-2` and [sys-apps/openrc](https://packages.gentoo.org/packages/sys-apps/openrc) packages have been installed. It is *critical* to ensure the files in /etc are up to date by running dispatch-conf or a similar tool *before* rebooting the system. **Failure to do so will result in an unbootable system** and will require the use of a LiveCD to perform the steps below to repair the system.

Once the configuration files have been updated there are a few things to verify prior to rebooting.

### /etc/conf.d/rc

/etc/conf.d/rc has been deprecated. Any settings contained therein will need to be migrated to the appropriate settings in /etc/rc.conf. Please read through /etc/rc.conf and /etc/conf.d/rc and migrate the settings. Once complete, manually remove the /etc/conf.d/rc file.

### Kernel modules

Normally, when certain kernel modules are automatically loaded at boot they are placed into /etc/modules.autoload.d/kernel-2.6 along with any parameters to be passed. In baselayout-2, this file is not used anymore. Instead, autoloaded modules and module parameters are placed in the /etc/conf.d/modules file, no matter what the kernel version.

An example old style configuration would be:

**`/etc/modules.autoload.d/kernel-2.6`**

Converting the above example would result in the following:

**`/etc/conf.d/modules`**

In the above examples, the modules and their parameters would only be passed to 2.6.x series kernels. The new configuration allows for fine grained control over the modules and parameters based on kernel version.

An in-depth example would be:

**`/etc/conf.d/modules`**

### Boot runlevel

The `boot` runlevel performs several important steps for every machine. For example, making sure the root filesystem is mounted read/write, the filesystems are checked for errors, the mountpoints are available, and the /proc pseudo-filesystem is started at boot.

With OpenRC, volume management services for block storage devices are no longer run automatically at boot. This includes [LVM](https://wiki.gentoo.org/wiki/LVM), RAID, swap, device-mapper (dm), dm-crypt, and the like. System administrators must ensure the appropriate initscript for these services has been added to the `boot` runlevel, otherwise it is possible the system will not be able to boot.

While the OpenRC ebuild will attempt to do this migration, administrators should verify the volume management services have been migrated properly:

`root #``ls -l /etc/runlevels/boot/`
If root, procfs, mtab, swap, and fsck are missing in the above listing, perform the following commands to add them to the `boot` runlevel:

`root #````
rc-update add root boot
```
`root #````
rc-update add procfs boot
```
`root #````
rc-update add mtab boot
```
`root #````
rc-update add fsck boot
```
`root #````
rc-update add swap boot
```
If the system uses mdraid and [LVM](https://wiki.gentoo.org/wiki/LVM) and they are not mentioned in the list above, then the following initscripts should be added to the `boot` runlevel:

`root #````
rc-update add mdraid boot
```
`root #````
rc-update add lvm boot
```
### Udev

OpenRC no longer starts udev by default; it does need to be present in the `sysinit` runlevel to be started. The OpenRC ebuild should detect if udev was previously enabled and add it to the `sysinit` runlevel. However, to be safe, check if udev is present:

`root #``ls -l /etc/runlevels/sysinit`
lrwxrwxrwx 1 root root 14 2009-01-29 08:00 /etc/runlevels/sysinit/udev -> \
/etc/init.d/udev

If udev is not listed, add it to the correct runlevel:

`root #``rc-update add udev sysinit`
### Network

Due to baselayout and OpenRC being broken into two different packages, the net.eth0 initscript may disappear during the upgrade process. To replace this initscript and re-add it to the default runlevel, please perform the following:

`root #````
cd /etc/init.d
```
`root #````
ln -s net.lo net.eth0
```
`root #````
rc-update add net.eth0 default
```
When missing any other network initscripts, follow the instructions above to re-add them. Simply replace `eth0` with the name of the missing network device.

Also, /etc/conf.d/net (oldnet) no longer uses bash-style arrays for configuration. Please review /usr/share/doc/openrc-\<version>/net.example for configuration instructions. Conversion should be relatively straight-forward, converting to newlines for separate entries, for example a static IP assignment would change as follows:

**`/etc/conf.d/net`**

**Old style**

**`/etc/conf.d/net`**

**New style**

### Clock

Clock settings have been renamed from /etc/conf.d/clock to the system's native tool for adjusting the clock. On Linux it will be /etc/conf.d/hwclock and on FreeBSD it will be /etc/conf.d/adjkerntz. Systems without a working real time clock (RTC) chip should use /etc/init.d/swclock, which sets the system time based on the mtime of a file which is created at system shutdown. The initscripts in /etc/init.d/ have also been renamed accordingly, so make sure the appropriate script for the system has been added to the boot runlevel.

Additionally, the `TIMEZONE` variable is no longer in this file. Its contents are instead found in the /etc/timezone file. If it does not exist, it will need to be created with the appropriate timezone. Please review both of these files to ensure their contents.

The proper value for this file is the path relative to the timezone from the /usr/share/zoneinfo directory. For example, for users living on the east coast of the United States, the following would be a correct setting:

**`/etc/timezone`**

### XSESSION

The `XSESSION` variable is no longer found in /etc/rc.conf. Instead, the `XSESSION` variable can be set on a per-user basis in the \~/.bashrc file (or equivalent), or system-wide in /etc/env.d/.

Here is an example of setting the default `XSESSION` for the whole system:

`root #``echo 'XSESSION="Xfce4"' > /etc/env.d/90xsession`
### EDITOR and PAGER

The `EDITOR` variable is no longer found in /etc/rc.conf. Both `EDITOR` and `PAGER` are set by default in /etc/profile. Adjust as needed in the \~/.bashrc file (or equivalent) or create /etc/env.d/99editor and set the system default there.

### Boot log

Previously, the boot process could be logged using the [app-admin/showconsole](https://packages.gentoo.org/packages/app-admin/showconsole) package. However, OpenRC now handles all logging internally, so there is no need for the hacks that showconsole employed. showconsole can now be safely unmerged. To continue logging boot messages, set the appropriate variable (`rc_logger`) in the /etc/rc.conf file. Logs will appear in /var/log/rc.log.

**`/etc/rc.conf`**

**Enable logging**

### local.start and local.stop

With OpenRC, /etc/conf.d/local.start and local.stop have been deprecated. During the migration to OpenRC, the files are moved to /etc/local.d and gain the suffix .start or .stop. OpenRC then executes those in alpha-numeric order.

### System sub-types: Virtualization special cases

Earlier versions of OpenRC detected multiple types of virtualization, which was used to note when certain init scripts should be skipped, using the `keyword` call in the `depend` functions.

However, as of the 0.7.0 release, administers are required to explicitly configure the sub-type using the `rc_sys` variable in /etc/rc.conf. The sub-type should be set to match the virtualization environment that the given root is in. In general, the non-empty `rc_sys` value should be within the virtual containers; The host node will have `rc_sys=""`.

**`/etc/rc.conf`**

**Setting system sub-type to none**

The detection algorithm had to be replaced with manual configuration due to the introduction of new sub-types and changes to the kernel that made prior detection unreliable.

| Subtype | Description | Notes | 
|---|---|---|
|  | Default, no sub-type | Not the same as unset; FreeBSD, Linux and NetBSD | 
| jail | FreeBSD jails |  | 
| lxc | Linux Containers | Not auto-detected | 
| openvz | Linux OpenVZ |  | 
| prefix | Prefix | Not auto-detected; FreeBSD, Linux & NetBSD | 
| vserver | Linux vserver |  | 
| xen0 | Xen0 Domain | Linux & NetBSD | 
| xenU | XenU Domain | Linux & NetBSD | 

### Cleaning up stale configuration files

After migration, there will be files left on the file system that are not cleaned up by Portage. Those are configuration files which are protected by Portage' configuration file protection feature.

The most notable example would be /etc/conf.d/net.example which is superseded by /usr/share/doc/openrc-\*/net.example.bz2.

### Finishing up

Once finished updating the config files and initscripts, the last thing to do is to type `reboot` with root privileges at a terminal. This is necessary because system state information is not preserved during the upgrade, so a fresh boot is needed.

## Changed functionality

### The pause action

Previously it was possible to temporarily stop a service without taking down all the depending services by using /etc/init.d/service pause. In OpenRC, the `pause` action was removed; this functionality is supported by the /etc/init.d/service --nodeps stop, which also works in the old baselayout.

### rootfs entry in /etc/mtab

Previously, the initial `rootfs` entry was removed from /etc/mtab, and only the real root / entry was present. The duplicate rootfs item was actually added back during shutdown. In OpenRC, both entries must be present for full support of initramfs and tmpfs-on-root. This also means that less writing is required during shutdown.
