<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:Unable_to_mount_root_fs | group: Gentoo Knowledge | wiki-title: Knowledge_Base:Unable_to_mount_root_fs -->
---
title: Knowledge Base:Unable to mount root fs
url: https://wiki.gentoo.org/wiki/Knowledge_Base:Unable_to_mount_root_fs
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-11-22"
fingerprint: "5f11191e9aaaafed"
license: CC BY-SA 4.0
---

# Knowledge Base:Unable to mount root fs

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Synopsis

When booting a freshly configured kernel, the boot fails with the following message:

VFS: Cannot open root device "hda3" or unknown-block(2,0)
Please append a correct "root=" boot option; here are the available partitions:
...
Kernel panic - not syncing: VFS: Unable to mount root fs on unknown-block(2,0)

The numbers in the `unknown-block` section can be different.

## Environment

Any Gentoo Linux system on which a new kernel is being booted (or an update was made on the bootloader configuration).

## Analysis

The panic informs that the Linux kernel is unable to:

1. Detect the controller for the hard disk (a likely candidate if the message says `unknown-block(0,0)`);
2. Detect the partition because it does not have support for the partition type (less likely, but still possible);
3. Mount the partition because it does not know how to access the file system (a likely candidate if the message gives a non-zero figure in the first number set, such as `unknown-block(2,0)`);
4. Detect the partition because the wrong device was passed in the boot loader configuration.

## Resolution

Boot with the old kernel (if available), or with a LiveCD, and open the kernel configuration:

`root #``make menuconfig`
- Ensure that the controller chipset has been configured in the Linux kernel (and in-kernel, not as a module);
- Ensure that the used file systems (such as ext2, ext3, ext4, Btrfs, etc.) are configured in the Linux kernel (and built into the kernel, not as a module);
- When using `root=UUID=...`, an initramfs must be built when configuring the bootloader, see "Configuring the linux kernel" of the installation handbook
- If running Gentoo as a Qemu guest, ensure that the virtio block drivers and PCI driver for virtio devices are enabled in the kernel (that is, `CONFIG_VIRTIO_BLK=y` and `CONFIG_VIRTIO_PCI=y` in the kernel configuration file).
- When an initramfs is not used and the root filesystem specified on the kernel command line is on an MTD device (such as an NVME drive), it may be necessary to make the kernel wait for asynchronous initialization of the device by adding either `rootdelay=<time-in-seconds>` (to timeout if the device is never detected) or `rootwait` (to wait without timeout).


Also verify that the boot loader configuration (/boot/grub/grub.conf for GRUB legacy and /boot/grub/grub.cfg for GRUB2) refers to the correct partition:

**`/boot/grub/grub.conf`**

**GRUB Legacy partition example**

When using an [initramfs](https://wiki.gentoo.org/wiki/Initramfs/Guide) and upgrade to another kernel version, make sure the initramfs is recreated correctly in /boot for the new kernel version and that the boot loader configuration refers to the correct initramfs.
