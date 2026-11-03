<!-- source: https://wiki.gentoo.org/wiki/OpenRC/Advanced | group: Gentoo Wiki (Main) | wiki-title: OpenRC/Advanced -->
---
title: OpenRC/Advanced
url: https://wiki.gentoo.org/wiki/OpenRC/Advanced
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-11-02"
fingerprint: "9c6fbfd92a2d2a8f"
license: CC BY-SA 4.0
---

# OpenRC/Advanced

[OpenRC](https://wiki.gentoo.org/wiki/OpenRC)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article covers more advanced topic about configuring and using [OpenRC](https://wiki.gentoo.org/wiki/OpenRC).

## BusyBox

Please note that there are currently many BusyBox applets that are incompatible with OpenRC, see [bug #529086](https://bugs.gentoo.org/show_bug.cgi?id=529086) for details. **Be warned that using OpenRC with BusyBox may require some work to set up.** BusyBox is more adapted to embedded use, see previous section about mdev.

[BusyBox](https://wiki.gentoo.org/wiki/Busybox) can be used to replace most of the userspace utilities needed by OpenRC (init, shell, awk, and other POSIX tools), by using a complete BusyBox as the shell for OpenRC [\[1\]](https://github.com/OpenRC/openrc/blob/master/BUSYBOX.md), all the calls that normally would cause a fork/exec would be spared, improving the overall speed.

The SysV-init /etc/inittab file provided by Gentoo is not compatible with the BusyBox init. Here is an example inittab compatible with BusyBox:

**`/etc/inittab`**

**Example inittab compatible with BusyBox init**

```
::sysinit:/sbin/openrc sysinit
::wait:/sbin/openrc boot
::wait:/sbin/openrc
```
BusyBox provides a number of applets that could be used to replace third party software like acpid or dhcp/dhcpcd.

See the OpenRC documentation on [using BusyBox with OpenRC](https://github.com/OpenRC/openrc/blob/master/BUSYBOX.md).

### udev and mdev

[udev](https://wiki.gentoo.org/wiki/Udev) and [mdev](https://wiki.gentoo.org/wiki/Mdev) are systems available on Gentoo to manage /dev. [eudev](https://wiki.gentoo.org/wiki/Eudev) used to be available as well, but has been removed. udev is available for OpenRC via the [sys-apps/systemd-utils\[udev\]](https://packages.gentoo.org/packages/sys-apps/systemd-utils) package, however should be able to work with both on Gentoo.

Older Gentoo installs used udev as the main [virtual/udev](https://packages.gentoo.org/packages/virtual/udev) provider. Based on [bug #575718](https://bugs.gentoo.org/show_bug.cgi?id=575718) this was changed to eudev, but it was changed back to udev in [bug #807193](https://bugs.gentoo.org/show_bug.cgi?id=807193). However, the rc service is still /etc/init.d/udev.

See [mdev](https://wiki.gentoo.org/wiki/Mdev), for possible use, e.g. for embedded systems.
