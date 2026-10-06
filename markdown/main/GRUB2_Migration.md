<!-- source: https://wiki.gentoo.org/wiki/GRUB2_Migration | group: Gentoo Wiki (Main) | wiki-title: GRUB2 Migration -->
---
title: GRUB2 Migration
url: https://wiki.gentoo.org/wiki/GRUB2_Migration
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-08-16"
fingerprint: "9ea2e16f08acad95"
license: CC BY-SA 4.0
---

# GRUB2 Migration

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Archived article**

This article is

**archived (obsolete)**. Contents are surely incorrect for current usage, and are intended for historical reference only.

[GRUB2](https://wiki.gentoo.org/wiki/GRUB2)has been out for a very long time, these instructions will probably rarely be needed nowadays.

TLDR:

**Do not use this article!**

This guide provides instructions for smooth migration from [GRUB Legacy](https://wiki.gentoo.org/wiki/GRUB_Legacy) to [GRUB](https://wiki.gentoo.org/wiki/GRUB).

## Background

### What's GRUB?

GRUB is one of the most commonly found boot loaders in use on non-embedded Linux machines. The role of GRUB is to facilitate the Linux kernel being loaded from the disk into memory and start executing the Linux kernel.

### Why migrate?

Firstly, GRUB Legacy is no longer maintained and as such no longer receives updates. GRUB Legacy was created at a time when the developers felt safe in making several assumptions which no longer hold true today. For example, GRUB Legacy is unable to boot from disks larger than 2 TB and assumes that newer filesystems wouldn't come to replace /boot.

GRUB2 aims to be more robust, more portable, more powerful and is maintained with a cleaner code base. GRUB2 supports more hardware configurations, more filesystems and more drive layouts than its predecessor.

## Migration to GRUB2

Migration to GRUB2 is fairly straightforward: it will be pulled in as part of the regular upgrade process by the package manager. If it is not pulled in automatically, it can always be merged via the `sys-boot/grub:2` package atom:

`root #``emerge --ask sys-boot/grub:2`
### Boot drive

The first important part is to understand which drive is bootable. For those who followed the Gentoo Handbook it should be /dev/sda. For those who are uncertain, the easiest way to find out is to look at the existing GRUB Legacy configuration. Viewing the /boot/grub/grub.conf file is the main place to check.

The grub.conf file will look something like this:

**`/boot/grub/grub.conf`**

```
default 0
timeout 30
splashimage=(hd0,0)/boot/grub/splash.xpm.gz
  
title Gentoo Linux 3.2.12
root (hd0,0)
kernel /boot/kernel-3.2.12-gentoo root=/dev/sda3 quiet dolvm
initrd /boot/initramfs-genkernel-x86_64-3.2.12-gentoo
```
Based on the above file it is possible to know that `(hd0)` is the boot drive but we must map this to a real device. To know this, look at the /boot/grub/device.map file. An example one is provided below.

**`/boot/grub/device.map`**

```
(fd0) /dev/fd0
(hd0) /dev/sda
(hd1) /dev/sdb
```
Based on the above file we know that /dev/sda is the boot drive.

### Installing and configuring GRUB2

The next step is to install and configure GRUB2 for the /boot partition without removing GRUB Legacy from the drive's Master Boot Record (MBR). The example below uses /dev/sda — replace it with the correct boot drive path.

First install the necessary GRUB2 files to /boot/grub.

`root #``grub-install --grub-setup=/bin/true /dev/sda`
Installation finished. No error reported.

Now we can scan the available kernels and generate a suitable config file to /boot/grub/grub.cfg. Skip this step when using a [Manual Configuration](https://wiki.gentoo.org/wiki/GRUB2_Quick_Start#Manual_configuration).

`root #``grub-mkconfig -o /boot/grub/grub.cfg`
Generating grub.cfg ...
Found linux image: /boot/kernel-3.2.12-gentoo
Found initrd image: /boot/initramfs-genkernel-x86\_64-3.2.12-gentoo
done

### Chainloading GRUB2 from GRUB Legacy to test the setup

Because a broken GRUB configuration could mean an unbootable system, we want to test our GRUB2 configuration before making it permanent. To do this we will chainload GRUB2 from GRUB Legacy. This is done by adding a new section into /boot/grub/grub.conf. An example is shown below.

**`/boot/grub/grub.conf`**

```
default 0
timeout 30
splashimage=(hd0,0)/boot/grub/splash.xpm.gz
  
title GRUB2 Chainload
root (hd0,0)
kernel /boot/grub/i386-pc/core.img
boot
  
title Gentoo Linux 3.2.12
root (hd0,0)
kernel /boot/kernel-3.2.12-gentoo root=/dev/sda3 quiet dolvm
initrd /boot/initramfs-genkernel-x86_64-3.2.12-gentoo
```
At this point the machine should be rebooted, and `GRUB2 Chainload` selected from the GRUB menu when the machine begins to boot. Another GRUB menu will be presented which should advertise itself as GRUB 2.0.0 or higher at the top and show the available kernel(s) to boot. Should this not work, simply reboot the system and pick the normal boot option instead of `GRUB2 Chainload`.

### Replacing and removing GRUB Legacy

If everything worked successfully, replace GRUB Legacy and remove it from the system.

`root #``grub-install /dev/sda`
Installation finished. No error reported.

At this point use the package manager to remove `sys-boot/grub:0`.

`root #``emerge --ask -vc sys-boot/grub:0`
The migration is now complete.

## Maintaining GRUB2

Whenever a new kernel is installed, perform the next step so that the GRUB2 configuration recognizes the new kernel (except when using a [manual configuration](https://wiki.gentoo.org/wiki/GRUB2_Quick_Start#Manual_configuration)).

`root #``grub-mkconfig -o /boot/grub/grub.cfg`
Generating grub.cfg ...
Found linux image: /boot/kernel-3.3.8-gentoo
Found initrd image: /boot/initramfs-genkernel-x86\_64-3.3.8-gentoo
Found linux image: /boot/kernel-3.2.12-gentoo
Found initrd image: /boot/initramfs-genkernel-x86\_64-3.2.12-gentoo
done
