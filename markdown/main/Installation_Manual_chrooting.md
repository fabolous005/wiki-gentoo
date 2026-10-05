<!-- source: https://wiki.gentoo.org/wiki/Installation/Manual_chrooting | group: Gentoo Wiki (Main) | wiki-title: Installation/Manual chrooting -->
---
title: Installation/Manual chrooting
url: https://wiki.gentoo.org/wiki/Installation/Manual_chrooting
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-21"
fingerprint: "9c0e5a9ab587d204"
license: CC BY-SA 4.0
---

# Installation/Manual chrooting

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

To ensure that the installation can proceed without issue, and that files and changes are recorded with correct timestamps, the first thing to do is to set the system time.

Stage archives are generally obtained using HTTPS, which requires relatively accurate system time. Also, adjusting the system time by any considerable amount after installation can cause unpredictable errors, so it really is important to set it correctly now.

The current date and time can be verified with date:

`root #``date`
Wed Jul 26 13:16:22 UTC 2000

If the displayed date and time is more than few minutes off, set it to the correct time use the [Handbook provided steps](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Stage#Setting_the_date_and_time).

One thing still remains to be done before entering the new environment and that is copying over the DNS information in /etc/resolv.conf. This needs to be done to ensure that networking still works even after entering the new environment. /etc/resolv.conf contains the name servers for the network.

To copy this information, it is recommended to pass the `--dereference` option to the cp command. This ensures that, if /etc/resolv.conf is a symbolic link, that the link's target file is copied instead of the symbolic link itself. Otherwise in the new environment the symbolic link would point to a non-existing file (as the link's target is most likely not available inside the new environment).

`root #``cp --dereference /etc/resolv.conf /etc/`
In a few moments, the Linux root will be changed towards the new location.

The filesystems that need to be made available are:

- /proc/ is a pseudo-filesystem. It looks like regular files, but is generated on-the-fly by the Linux kernel
- /sys/ is a pseudo-filesystem, like /proc/ which it was once meant to replace, and is more structured than /proc/
- /dev/ is a regular file system which contains all device. It is partially managed by the Linux device manager (usually udev)
- /run/ is a temporary file system used for files generated at runtime, such as PID files or locks

The /proc/ location will be mounted on /mnt/gentoo/proc/ whereas the others are bind-mounted. The latter means that, for instance, /mnt/gentoo/sys/ will actually *be* /sys/ (it is just a second entry point to the same filesystem) whereas /mnt/gentoo/proc/ is a new mount (instance so to speak) of the filesystem.

`root #````
mount --types proc /proc /mnt/gentoo/proc
```
`root #````
mount --rbind /sys /mnt/gentoo/sys
```
`root #````
mount --make-rslave /mnt/gentoo/sys
```
`root #````
mount --rbind /dev /mnt/gentoo/dev
```
`root #````
mount --make-rslave /mnt/gentoo/dev
```
`root #````
mount --bind /run /mnt/gentoo/run
```
`root #````
mount --make-slave /mnt/gentoo/run
```
Now that all partitions are initialized and the base environment installed, it is time to enter the new installation environment by chrooting into it. This means that the session will change its root (most top-level location that can be accessed) from the current installation environment (installation CD or other installation medium) to the installation system (namely the initialized partitions). Hence the name, *change root* or *chroot*.

This chrooting is done in three steps:

1. The root location is changed from / (on the installation medium) to /mnt/gentoo/ (on the partitions) using chroot or arch-chroot, if available.
2. Some settings (those in /etc/profile) are reloaded in memory using the source command
3. The primary prompt is changed to help us remember that this session is inside a chroot environment.

`root #````
chroot /mnt/gentoo /bin/bash
```
`root #````
source /etc/profile
```
`root #``export PS1="(chroot) ${PS1}"`
From this point, all actions performed are immediately on the new Gentoo Linux environment.
