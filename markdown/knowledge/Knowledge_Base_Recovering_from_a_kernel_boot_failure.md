<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:Recovering_from_a_kernel_boot_failure | group: Gentoo Knowledge | wiki-title: Knowledge_Base:Recovering_from_a_kernel_boot_failure -->
---
title: Knowledge Base:Recovering from a kernel boot failure
url: https://wiki.gentoo.org/wiki/Knowledge_Base:Recovering_from_a_kernel_boot_failure
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-11-28"
fingerprint: "9a3e789c5666534c"
license: CC BY-SA 4.0
---

# Knowledge Base:Recovering from a kernel boot failure

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Synopsis

Booting a Gentoo Linux system fails to boot up properly. No recovery mode is presented (such as a single-user login) but the boot loader does work properly.

## Environment

Any Gentoo Linux installation.

## Analysis

Boot failures can have several reasons. This article will focus on generic approaches to recover from such failures.

The approaches are two-fold:

1. Try to get a root prompt from the (malfunctioning) system, or
2. Boot from a LiveCD or other environment, mount the system and chroot to perform recovery steps.

## Resolution

There are several possible resolutions to fix a boot failure. The first one, which covers how to get a root prompt, is especially useful if partitions are reachable. The second one works in almost all circumstances as it is based on the installation of Gentoo.

### Getting a root prompt

To get a root prompt, reboot the system until the Linux boot loader (such as GRUB) appears. In the boot loader, edit the boot entry. For instance, with GRUB, this is done by pressing the `E` key (for **e**dit). On the kernel line, add `init=/bin/sh`:

kernel /kernel root=/dev/sda3 init=/bin/sh

If the boot failure is because of a wrong `root=` setting, then correct it using the kernel line as well. When finished editing, boot the entry (for GRUB, this means pressing the `B` key (for **b**oot).

These changes (which are *not* persistent) will boot the system, but instead of calling the init binary, it immediately drops the init process into a root shell. From this shell recovery steps can be performed. If persistent changes need to be made the root file system, as well as all other partitions that needs changes, should be mounted as read-write:

`root #``mount -o remount,rw /``root #``mount -a`
### Booting back from a LiveCD or other environment

Boot from the LiveCD (or a different environment) until the environment grants shell access. Create the /mnt/gentoo mount point (if one does not exist) and mount the root file system on it:

`root #``mkdir /mnt/gentoo``root #``mount /dev/sda3 /mnt/gentoo`
Next, bindmount /proc, /dev and /sys and /run on top the /mnt/gentoo mountpoint:

`root #````
mount --types proc /proc /mnt/gentoo/proc 
```
`root #````
mount --rbind /sys /mnt/gentoo/sys 
```
`root #````
mount --rbind /dev /mnt/gentoo/dev 
```
`root #````
mount --bind /run /mnt/gentoo/run 
```
`root #````
mount --make-rslave /mnt/gentoo/sys 
```
`root #````
mount --make-rslave /mnt/gentoo/dev 
```
`root #````
mount --make-slave /mnt/gentoo/run 
```
Finally, `chroot` into the environment, mount the remaining partitions, and start recovering the system:

`root #``chroot /mnt/gentoo /bin/sh``root #``source /etc/profile``root #``mount -a`
Once finished with the recovery steps, umount all partitions, exit the chroot, umount the remaining partitions, and reboot back into the system to see if the recovery has succeeded:

`root #````
umount -a
```
`root #````
exit
```
`root #````
umount -l /mnt/gentoo/{dev,proc,sys,run,}
```
`root #````
sync
```
`root #``reboot`
## See also

- [Project:Portage/Fixing broken portage](https://wiki.gentoo.org/wiki/Project:Portage/Fixing_broken_portage) — provides guidance on how to manually update or fix a broken Portage installation - particularly in the event emerge -v1 sys-apps/portage cannot be run.
- [Fix my Gentoo](https://wiki.gentoo.org/wiki/Fix_my_Gentoo) — rescuing an installation when a chroot is not possible
- [Support](https://wiki.gentoo.org/wiki/Support) — provide **support** for technical issues encountered when installing or using Gentoo Linux
- [Troubleshooting](https://wiki.gentoo.org/wiki/Troubleshooting) — provide users with a set of techniques and tools to troubleshoot and fix problems with their Gentoo setups.
