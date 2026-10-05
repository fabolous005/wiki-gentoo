<!-- source: https://wiki.gentoo.org/wiki/Limine | group: Gentoo Wiki (Main) | wiki-title: Limine -->
---
title: Limine
url: https://wiki.gentoo.org/wiki/Limine
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-03"
fingerprint: "1fa53b1a8fa7becd"
license: CC BY-SA 4.0
---

# Limine

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Limine is a modern, advanced, portable, multiprotocol [bootloader](https://wiki.gentoo.org/wiki/Bootloader) and boot manager which also provides the reference implementation of the Limine boot protocol.

This guide is designed to be followed alongside [the installation Handbook](https://wiki.gentoo.org/wiki/Handbook:Main_Page) and [the ZFS root guide](https://wiki.gentoo.org/wiki/ZFS/rootfs) if so desired, though the config file examples are of course universal to all.

## Installation

### Unmasking the Package

Limine is now available in the main tree, but it is not currently unmasked. Unmasking the package is a relatively simple process, to do so, the allowed keywords variable needs to be edited, in which the following needs to be appended utilising a preferred editor:

**`/etc/portage/package.accept_keywords/limine`**

**Unmasking Limine for install**

### Optional: USE flags

It is possible to configure Limine for only the target system's method, it may be done so by specifying USE flags, assuming the target is an AMD64 UEFI system, the required USE flags would be:

**`/etc/portage/package.use/limine`**

**Specifying USE flags to exclusively build UEFI AMD64 files**

The other USE flags may be viewed with the following command:

`user $``emerge --pretend --verbose sys-boot/limine`
### Emerging the Package

Once all the above steps are completed, [sys-boot/limine](https://packages.gentoo.org/packages/sys-boot/limine) can be installed to the target:

`root #``emerge --ask --verbose sys-boot/limine`
## Setting up the Bootloader

Now that Limine is installed on the target system, configuring the bootloader can begin.

To configure the bootloader, differing files will be utilised depending on target architecture and whether the target is using BIOS or UEFI.

### Copying the Bootloader Files

In order for Limine to boot, Limine requires there to be a mounted vfat partition as /boot, which will be your ESP on UEFI systems, and simply a normal extra partition formatted as vfat on BIOS systems.

The bootloader files are stored in /usr/share/limine and are named after their respective architectures, excluding the BIOS file, which is universal for both x86 and amd64 BIOS systems.

It is a good idea initially to check if /boot is mounted, this can be done by simply running the lsblk command:

`user $``lsblk`
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
zram0       252:0    0     2G  0 disk \[SWAP\]
zram1       252:1    0     2G  0 disk /tmp
nvme0n1     259:0    0 238.5G  0 disk
├─nvme0n1p1 259:1    0   400M  0 part /boot
└─nvme0n1p2 259:2    0 238.1G  0 part /

After confirming /boot is mounted, the architecture files can be copied across.

#### For UEFI

For UEFI, there is no need for [sys-boot/efibootmgr](https://packages.gentoo.org/packages/sys-boot/efibootmgr) 99% of the time, using the default directory structure will get picked up by the firmware.

`root #``mkdir -p /boot/EFI/BOOT/`
After creating this directory, the respective .EFI firmware file can be copied across, assuming the target is an AMD64 UEFI system, the correct command would be:

`root #``cp -v /usr/share/limine/BOOTX64.EFI /boot/EFI/BOOT/`
If the target is not AMD64 EFI, and rather RISCV, AARCH64 or x86, their respective files can be located by listing the contents of /usr/share/limine:

`user $``ls /usr/share/limine`
##### Using efibootmgr (optional)

If the target systems UEFI firmware is unable to find the ESP, or for peace of mind, [sys-boot/efibootmgr](https://packages.gentoo.org/packages/sys-boot/efibootmgr) can be utilized in order to create an entry pointing towards Limine in the EFI Firmware.

[sys-boot/efibootmgr](https://packages.gentoo.org/packages/sys-boot/efibootmgr) can be installed with the following command:

`root #``emerge --ask --verbose sys-boot/efibootmgr`
Following it's install and assuming the target is an amd64 system, creating an entry will look like this:

`root #````
efibootmgr \
```
```
     --create \
     --disk /dev/sdX \
     --part Y \
     --label "Gentoo Linux Limine Bootloader" \
     --loader '\EFI\limine\BOOTX64.EFI' \
     --unicode \
     --verbose
```
#### For BIOS

For BIOS systems, the bootloader file can be copied to the root of the /boot partition:

`root #``cp -v /usr/share/limine/limine-bios.sys /boot`
Now the BIOS file is in place, Limine can be installed to the MBR of the drive, so that BIOS firmware can pick up and boot Limine on the next boot:

`root #``limine bios-install /dev/sdX````
Physical block size of 512 bytes.
Installing to MBR.
Stage 2 to be located at 0x200 and 0x2600.
Reminder: Remember to copy the limine-bios.sys file in either
          the root, /boot, /limine, or /boot/limine directories of
          one of the partitions on the device, or boot will fail!
Limine BIOS stages installed successfully!
```
### Writing the Config File

Limine's configuration file to find kernels, initramfs' and microcode is entirely manually written. There's a lot of customisation that can be done within the config file, but for simplicity's sake, only the basics will be covered here. Limine's config file needs to be called limine.conf, and can be /boot/limine.conf, /boot/limine/limine.conf or /boot/EFI/BOOT/limine.conf. For a more detailed explanation of Limine's config, [Limine's official CONFIG.md file can be viewed](https://github.com/Limine-Bootloader/Limine/blob/v12.x/CONFIG.md).

The config has a pretty simple syntax to follow, with boot():/ being the root of the partition all the boot files are inside of. The cmdline variable is a standard command line to be parsed to the kernel when it loads, this is required, so the kernel knows what to mount as the root, and can also be used to pre-load modules that might be needed very early on.

#### Generic Config Example

The following config example will likely fit most users, it assumes the target system is running the [sys-kernel/gentoo-kernel](https://packages.gentoo.org/packages/sys-kernel/gentoo-kernel) or [sys-kernel/gentoo-kernel-bin](https://packages.gentoo.org/packages/sys-kernel/gentoo-kernel-bin) distribution kernel, and are on BtrFS, but can be easily edited to specific needs.

**`/boot/limine.conf`**

**Generic Config Example**

#### Using Limine With ZFS Root

If [ZFS/rootfs](https://wiki.gentoo.org/wiki/ZFS/rootfs) is being followed, Limine can also be utilised by editing the config slightly, as showcased:

**`/boot/limine.conf`**

**ZFS Config Example**

In this config, the root filesystem variable has been changed to tell the kernel to look for a ZFS root partition, and the modules argument tells the kernel to preload the ZFS module, so there is not any issues with the rootfs support not being built into the kernel, as ZFS modules have to be side-loaded with the [sys-fs/zfs-kmod](https://packages.gentoo.org/packages/sys-fs/zfs-kmod) package.

#### Dual-booting with Windows in Limine (UEFI)

Unlike [sys-boot/grub](https://packages.gentoo.org/packages/sys-boot/grub), Limine does not require [sys-boot/os-prober](https://packages.gentoo.org/packages/sys-boot/os-prober) to find other operating systems, Limine can find other EFI files, and has the ability to chainload them when manually pointed to the other EFI file.

**`/boot/limine.conf`**

**Dual-Boot Config Example (EFI)**

This example shows off entries and sub-entries, where as one slash deeper identifies contents of a directory, being able to group and nest entries together for neatness's sake. It may be needed to double-check the location of the Windows' EFI file to make sure Limine is pointing to the right location.

#### Dual-booting with Windows in Limine (BIOS)

Limine can also be pointed to other partitions of OSes installed in MBR configs. The config isn't changed too much compared to EFI chainloading, as detailed below, however it may require more trial and error than EFI chainloading to ensure you have specified the right drive and partition.

**`/boot/limine.conf`**

**Dual-Boot Config Example (MBR)**

## Finalising

Now, Limine is entirely setup and ready to boot to. Extra eye-candy can be found and applied by revising [the official configuration file documentation](https://github.com/limine-bootloader/limine/blob/v8.x/CONFIG.md). Any issues can be reported on the official GitHub, and the official Limine discord, both of which can be found on their website linked [at the top of the page.](https://wiki.gentoo.org/wiki/Limine#Contents)
