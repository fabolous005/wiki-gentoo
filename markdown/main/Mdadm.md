<!-- source: https://wiki.gentoo.org/wiki/Mdadm | group: Gentoo Wiki (Main) | wiki-title: Mdadm -->
---
title: mdadm
url: https://wiki.gentoo.org/wiki/Mdadm
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-07-27"
fingerprint: fe23145a240f2939
license: CC BY-SA 4.0
---

# mdadm

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**mdamd** is a tool for running software RAID in Linux systems.

## Introduction to Software RAID

[Redundant Array of Inexpensive Disks (RAID)](https://en.wikipedia.org/wiki/RAID) is a technology to combine multiple disks in order to improve their reliability and/or performance. There is *hardware RAID*, implemented by the controller on your motherboard or specific extension cards, and there is *software RAID*, implemented by the kernel.

### Advantages of RAID

The main advantages of RAID are reliability and/or performance improvements.

There are vast differences between soft- and hardware RAID. Differences include: cost (monetarily), performance, overhead and flexibility. Of course software RAID comes cheapest. Another advantage of soft- over hardware RAID is that you can easily move you RAID set to another (Linux) computer. With hardware RAID you will depend on the vendor.

### Disadvantages of RAID

RAID usually takes more disk space than it delivers. E.g. a RAID-1 set of two 3TB disks delivers a 3TB RAID disk (a reduction of 50%).

The time it takes to synchronize you RAID disk (initially and when they need to rebuild).

The extra effort to manage and monitor your RAID disks. To make full use of software RAID, you need to learn about disk failures so you don't end up loosing two disks from your 3 disk RAID-5 just because you don't notice failure of the first one.

## Setting up Software RAID

### Installing the Tools

Start with the kernel configuration. After all, the kernel does most of the work:

Get the proper RAID admin tool:

`root #``emerge --ask mdadm`
Make sure your RAID disks are started and stopped:

`root #``rc-update add mdraid boot`
### Using Software RAID

To create a simple mirrored disk (RAID-1), make sure you have two partitions that have the same size and bring them together in your first RAID-1 disk:

`root #``mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sdd1 /dev/sde1`
The disks in the mirror will now be synchronized (also when there is no data or file system yet). you can check the status like this:

`user $``cat /proc/mdstat````
Personalities : [raid1] 
md1 : active raid1 sde2[1] sdd2[0]
      1900953472 blocks super 1.2 [2/2] [UU]
      [======>..............]  resync = 31.6% (600768512/1900953472) finish=819.4min speed=26444K/sec
      
md0 : active raid1 sde1[1] sdd1[0]
      52395904 blocks super 1.2 [2/2] [UU]
      
unused devices: <none>
```
Create a filesystem of choice:

`root #``mkfs.ext4 /dev/md0`
Start using the RAID!

### Software RAID for Root File System

#### Boot Using GRUB and genkernel's initramfs

When the root file system is located on a software RAID, an initramfs is necessary for automatic assembly. [Genkernel](https://wiki.gentoo.org/wiki/Genkernel)'s initramfs can be used for that purpose. Generate the initramfs with `--mdadm` option, or have `MDADM="yes"` in /etc/genkernel.conf before generation.

**`/etc/genkernel.conf`**

Then, add the `domdadm` parameter to the kernel command line. The script in initramfs interprets this parameter, not the kernel itself. Also, be aware that the auto assembly takes into account the hostname, which will probably be set to `(none)` in the initramfs environment. Using [GRUB](https://wiki.gentoo.org/wiki/GRUB), the parameter can be added to /etc/default/grub:

**`/etc/default/grub`**

Remember to regenerate the /boot/grub/grub.cfg configuration file with the following command:

`root #``grub-mkconfig -o /boot/grub/grub.cfg`
## Managing Software RAID

### Adding Disks

To add a new disk to the software RAID, issue this command:

`root #``mdadm /dev/md0 --add /dev/sdd1`
If the RAID has healthy members, the new disk will be added as spare disk `(S)`.

### Removing Disks

First determine the disk to be removed.

`root #``cat /proc/mdstat````
md0 : active raid1 sdd1[1](F) sde1[0]
      32000 blocks [2/1] [U_]
      bitmap: 0/4 pages [0KB], 4KB chunk
```
As you can see, there sdd1 marked as failed `(F)` and RAID missing one of disks `[U_]`. To remove the failed drive execute following command:

`root #``mdadm /dev/md0 --remove /dev/sdd1`
Physically replace the (sdd) disk or add blank space from another attached location.

Sometimes you may need to remove an healthy member. For this you need to mark drive as failed then remove it from the software RAID set. Perform this action all in one command:

`root #``mdadm /dev/md0 --fail /dev/sdd1 --remove /dev/sdd1`
### Changing RAID Layout

In some cases it is necessary to change the RAID layout. An example case would be when adding disks to a RAID1 or converting a RAID1 to a RAID5.
