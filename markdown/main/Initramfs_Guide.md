<!-- source: https://wiki.gentoo.org/wiki/Initramfs/Guide | group: Gentoo Wiki (Main) | wiki-title: Initramfs/Guide -->
---
title: Initramfs/Guide
url: https://wiki.gentoo.org/wiki/Initramfs/Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-26"
fingerprint: b399193a279fafc4
license: CC BY-SA 4.0
---

# Initramfs/Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


Some Linux-based computer systems require an **initramfs** to boot properly. This guide covers the concepts of the initramfs as well as how to properly create and manage initramfs instances.

## Initramfs concepts

### Introduction

Systems with exotic drivers or setups, or encrypted file systems need [initramfs](https://wiki.gentoo.org/wiki/Initramfs) so the Linux kernel is capable of handing over control to the init binary on their system.

### Linux boot process

Once the Linux kernel has control over the system (which it gets after being loaded by the boot loader), it prepares its memory structures and drivers. It then hands over control to an application (usually init) whose task is to further prepare the system and make sure that, at the end of the boot process, all necessary services are running and the user is able to log on. The init application does that by launching, among other services, the udev daemon who will further load up and prepare the system based on the detected devices. When udev is launched, all remaining file systems that have not been mounted are mounted, and the remainder of services are started.

For systems where all necessary files and tools reside on the same file system, the init application can perfectly control the further boot process. But when multiple file systems are defined (or more exotic installations are done), this might become a bit more tricky:

- When the /usr partition is on a separate file system, tools and drivers that have files stored within /usr cannot be used unless /usr is available. If those tools are needed to make /usr available, then we cannot boot up the system.

- If the root file system is encrypted, then the Linux kernel will not be able to find the init application, resulting in an unbootable system.

The solution for this problem has long since been to use an *initrd* (initial root device).

### The initial root disk

The **initrd** is an in-memory disk structure (ramdisk) that contains the necessary tools and scripts to mount the needed file systems *before* control is handed over to the init application on the root file system. The Linux kernel triggers the setup script (usually called linuxrc but that name is not mandatory) on this root disk, which prepares the system, switches to the real root file system and then calls init.

Although the initrd method is all that is needed, it had a few drawbacks:

- It is a full-fledged block device, requiring the overhead of an entire file system; it has a fixed size. Choosing an initrd that is too small and all needed scripts cannot fit. Make it too big and memory will be wasted.

- Because it is a real, static device it consumes cache memory in the Linux kernel and is prone to the memory and file management methods in use (such as paging), this makes initrd greater in memory consumption.

To resolve these issues the initramfs was created.

### The initial ram file system

An **initramfs** is an initial ram file system based on [tmpfs](https://wiki.gentoo.org/wiki/Tmpfs) (a size-flexible, in-memory lightweight file system), which also did not use a separate block device (so no caching was done and all overhead mentioned earlier disappears). Just like the initrd, it contains the tools and scripts needed to mount the file systems before the init binary on the real root file system is called. These tools can be decryption abstraction layers (for encrypted file systems), logical volume managers, software raid, bluetooth driver based file system loaders, etc.

The content of the initramfs is made by creating a [cpio](https://wiki.gentoo.org/wiki/Cpio) archive. cpio is an old (but proven) file archiver solution (and its resulting archive files are called cpio files). cpio is definitely comparable to the [tar](https://wiki.gentoo.org/wiki/Tar) archiver. The choice of cpio here was because it was easier to implement (code-wise) and supported device files which tar did not support at the time.

All files, tools, libraries, configuration settings (if applicable), etc. are put into the cpio archive. This archive is then compressed using the [gzip](https://wiki.gentoo.org/wiki/Gzip) utility and stored alongside the Linux kernel. The boot loader will then offer it to the Linux kernel at boot time so the kernel knows an initramfs is needed.

Once detected, the Linux kernel will create a tmpfs file system, extract the contents of the archive on it, and then launch the init script located in the root of the tmpfs file system. This script then mounts the real root file system (after making sure it can mount it, for instance by loading additional modules, preparing an encryption abstraction layer, etc.) as well as vital other file systems (such as /usr and /var ).

Once the root file system and the other vital file systems are mounted, the init script from the initramfs will switch the root towards the real root file system and finally call the /sbin/init binary on that system to continue the boot process.

## Creating an initramfs

### Introduction and bootloader configuration

To create an initramfs, it is important to know what additional drivers, scripts, and tools will be needed to boot the system.

In practice, this usually means `CONFIG_SATA_AHCI` or `CONFIG_BLK_DEV_NVME` (for the disk), and `CONFIG_EXT4_FS` (the filesystem) should ideally be built-in \[=y\] to the kernel; otherwise having these as modules \[=m\] will *require* these modules to be loaded by the initramfs first so booting can continue.

If additional utilities are required to mount the root partition (like [LVM](https://wiki.gentoo.org/wiki/LVM) for volume management, [mdadm](https://wiki.gentoo.org/wiki/Mdadm) for software RAID, [ZFS](https://wiki.gentoo.org/wiki/ZFS) or [btrfs](https://wiki.gentoo.org/wiki/Btrfs) for advanced filesystems), they should be added to the initramfs.

Several automated tools exist that help users create initramfs (compressed cpio archives) for their system. Most commonly used are [Dracut](https://wiki.gentoo.org/wiki/Dracut) and [Genkernel](https://wiki.gentoo.org/wiki/Genkernel). Those who want total control can easily create personal, custom initramfs images as well manually.

**Once created**, the **bootloader** configuration will need to be adjusted to inform it an initramfs is to be used.

Usually this is done by re-generating the config file grub-mkconfig -o /boot/grub/grub.cfg

For instance, if the initramfs file is stored as /boot/initramfs-5.15.32-gentoo-r1.img, then afterwards, the **GRUB** configuration in /boot/grub/grub.cfg would look somewhat like the following:

**`/boot/grub/grub.cfg`**

**Minimalistic theoretical entry in grub.cfg for booting with an initramfs**

### Using genkernel

Gentoo's kernel building utility, [Genkernel](https://wiki.gentoo.org/wiki/Genkernel), can be used to generate an initramfs. genkernel expects that both the kernel and initramfs are built by it, although in some cases, an initramfs built by genkernel with the kernel built by another method *may* work (but is discouraged).

To use genkernel for generating an initramfs, it is recommended all necessary drivers and code that is needed to mount the / **root** and /usr file systems be **included as built-in** in the kernel (not as modules). If these are built-in, genkernel can be called with the  `--no-ramdisk-modules` option.

If modules are required in the initramfs, call genkernel as follows:

`root #``genkernel --install initramfs`
Depending on the system, one or more of the following options may be needed:

| Option | Description | 
|---|---|
| `--disklabel` | Add support for `LABEL=` settings in /etc/fstab | 
| `--dmraid` | Add support for fake hardware RAID. | 
| `--firmware` | Add in firmware code found on the system. | 
| `--gpg` | Add in GnuPG support. | 
| `--iscsi` | Add support for iSCSI. | 
| `--luks` | Add support for LUKS encryption containers. | 
| `--lvm` | Add support for LVM. | 
| `--mdadm` | Add support for software RAID. | 
| `--multipath` | Add support for multiple I/O access towards a SAN. | 
| `--zfs` | Add support for ZFS. | 

When finished, the resulting initramfs file will be stored in /boot.

### Using dracut

The [Dracut](https://wiki.gentoo.org/wiki/Dracut) utility is created for the sole purpose of managing initramfs files.

To install the Dracut utility, run:

`root #``emerge --ask sys-kernel/dracut`
The next step is to configure dracut by editing /etc/dracut.conf.

Once configured, create an initramfs by calling dracut as follows:

`root #``dracut`
The resulting image supports generic system boots based on the configuration in /etc/dracut.conf.

For systems with full disk encryption, [Dracut](https://wiki.gentoo.org/wiki/Full_Disk_Encryption_From_Scratch#Dracut) may be followed.

It is also possible to generate an initramfs specifically tailored to an individual system (which dracut tries to detect the needed tools, drivers, etc. from the existing system). If the modules and drivers are built into the kernel (not as separate modules and references to the firmware), then the `--no-kernel` option can be added:

`root #``dracut --host-only --no-kernel`
For more information, check out the dracut and dracut.cmdline manual pages:

`user $````
man dracut
```
`user $````
man dracut.cmdline
```
### Using µgRD

µgRD ([sys-kernel/ugrd](https://packages.gentoo.org/packages/sys-kernel/ugrd)) was designed to create a minimal initramfs suitable for booting the system which built it. It will automatically configure LUKS, LVM, and subvolume usage.

Install it before use:

`root #``emerge --ask sys-kernel/ugrd`
To make an image for the current kernel:

`root #``ugrd /boot/ugrd.cpio.xz`
To make an image for a specific kernel version:

`root #``ugrd --kver gentoo-6.6.41-dist-hardened /boot/ugrd.cpio.xz`
## See also

- [Full Disk Encryption From Scratch](https://wiki.gentoo.org/wiki/Full_Disk_Encryption_From_Scratch) — a guide which covers the process of configuring a drive to be encrypted using LUKS and btrfs.
- [Initramfs](https://wiki.gentoo.org/wiki/Initramfs) — is used to prepare Linux systems during boot before the **init** process starts.
- [Initramfs - make your own](https://wiki.gentoo.org/wiki/Initramfs_-_make_your_own) — build an initramfs which does not contain kernel modules.
- [Custom Initramfs](https://wiki.gentoo.org/wiki/Custom_Initramfs) — the successor of *initrd*. It provides early userspace which can do things the kernel can't easily do by itself during the boot process.
- [Early Userspace Mounting](https://wiki.gentoo.org/wiki/Early_Userspace_Mounting) — how to build a custom minimal [initramfs](https://wiki.gentoo.org/wiki/Initramfs) that checks the /usr filesystem and pre-mounts /usr.

## External resources

- The [ramfs-rootfs-initramfs.txt](https://www.kernel.org/doc/Documentation/filesystems/ramfs-rootfs-initramfs.txt) file within the Linux kernel documentation.
