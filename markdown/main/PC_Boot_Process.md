<!-- source: https://wiki.gentoo.org/wiki/PC_Boot_Process | group: Gentoo Wiki (Main) | wiki-title: PC Boot Process -->
---
title: PC Boot Process
url: https://wiki.gentoo.org/wiki/PC_Boot_Process
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-03-31"
fingerprint: "1f991b1a10f61f21"
license: CC BY-SA 4.0
---

# PC Boot Process

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page is aimed at readers configuring their own [kernel](https://wiki.gentoo.org/wiki/Kernel), it aims to explain why some kernel settings must be set in a certain way, and what makes up the general boot process. This understanding will help with the kernel configuration. Individual kernel settings for particular hardware are not addressed.

The Personal Computer (PC), aka the [IBM PC](https://en.wikipedia.org/wiki/IBM_Personal_Computer) and compatibles, all follow the same boot process, from the very first 8086 up to the present day.

Other architectures do things differently but have the same problems to overcome.

## Bootstrapping

At power up, the only code available to the CPU is the [firmware](https://wiki.gentoo.org/wiki/Category:Firmware). It doesn't matter at this level if it's [BIOS](https://wiki.gentoo.org/wiki/BIOS) or [EFI](https://wiki.gentoo.org/wiki/Category:UEFI). It does its thing, then loads the [boot loader](https://wiki.gentoo.org/wiki/Category:Bootloaders), which then usually shows a boot menu. Lets say that the boot loader is [GRUB](https://wiki.gentoo.org/wiki/GRUB), but it could be any other boot loader.

All that is in RAM now is GRUB. GRUB can make calls to the firmware but that's it. There is no kernel yet, no initrd, and no kernel [filesystem](https://wiki.gentoo.org/wiki/Filesystem) tree. They all come later.

From the bootloader menu, the kernel to boot is chosen, which may include a matching initrd. GRUB loads the kernel, optionally loads the initrd, and leaves the kernel command line where the kernel will find it. GRUB exits by jumping to the kernel start address.

Now the kernel and optionally an initrd are loaded into RAM, with the kernel command line. Without any outside help, the kernel must be able to [mount](https://wiki.gentoo.org/wiki/Mount) the root filesystem and start the [init](https://wiki.gentoo.org/wiki/Init_system) process.

The kernel actually cannot find the root [filesystem](https://wiki.gentoo.org/wiki/Filesystem) on its own as root is not mounted, so /etc/fstab cannot be read, which constitutes a circular dependency. The kernel parameter root=/dev/... is used to break the circular dependency.

As the kernel is alone until root is mounted, all the required code to mount root must be [configured](https://wiki.gentoo.org/wiki/Kernel/Configuration) as "\<\*>" for *built-in*. Loadable modules, "\<M>", are stored in /lib/modules/\`uname -r\`, which is on the root filesystem, so cannot be read until root is mounted. Its a very bad thing to need to read /lib/modules/ to load a module required to mount root to read /lib/modules/. Making things built-in avoids that circular dependency.

## Initrd

The [initrd](https://en.wikipedia.org/wiki/Initial_ramdisk) (initial ramdisk) can help with the need to have modules available to the kernel in the first stages of boot. An initrd is a small root filesystem in a file. When the kernel is configured to support an initrd, it knows how to mount and read the initrd.

If there is an initrd, the kernel will mount it and use it as a temporary root filesystem. The initrd can be configured to hold whatever is needed: kernel modules, user space tools, etc. If it contains kernel modules, the initrd is tied to the kernel build because the kernel will check for bits of itself.

The use of an initrd is only compulsory if user space tools are required to make the root filesystem visible to the kernel. e.g. root on [RAID](https://wiki.gentoo.org/wiki/Device-mapper) where mdadm is required to start the RAID. Root in a [logical volume](https://wiki.gentoo.org/wiki/LVM), root in [LUKS](https://wiki.gentoo.org/wiki/Dm-crypt_full_disk_encryption) ... and all the combinations.

Mounting root by filesystem UUID requires the user space mount command.

## Kernel configuration guidelines

- If an option is needed to mount root, it must be configured as built-in.
- If an option is always used but not required to mount root, it can be either built-in or modular.
- If an option is never required, set it to off.

### Options needed to mount root

- The partition table drivers
- The Block layer
- The PCIe layer
- The SCSI layer
- The hardware driver for the storage device holding the root filesystem
- The root filesystem driver

### Console drivers

#### Console output drivers

- EFI Framebuffer, VESA Framebuffer and Simple Framebuffer. All other Framebuffer drivers must be off.
- DRM
- The kernel driver for the video card

Its not an error to set these options off, but the console will be blank.

#### Console input drivers

It is usually required to be able to log in at the console, so these will be needed:

- evdev
- The keyboard hardware driver

### Other things

The list above is not intended be be exhaustive. Other options must also be enabled, but they are usually enabled by default in the PC kernel.

## Configuring the kernel

All of the kernel-provided configuration tools, except one, provide a search function.

In menuconfig, press / and enter a search term. That's on every page of menuconfig.

However, there is a wart here. To make configuring the kernel more manageable, by default, options that cannot be selected are hidden. That is, they do not appear in menus or the search output. This is usually because something they depend on is off.

When this happens, it's possible to turn on the display of all the hidden symbols. The 'z' key is a toggle. That works everywhere that 'z' is not a shortcut. Now it's possible to go to the symbol (it still cannot be selected) and read it's help. The "Depends on:" boolean expression must be true before the dependent option can be selected.

It's possible that the above method will need to be used recursively to set the options to satisfy the "Depends on:", so that the required option can be selected.

While reading the help, notice the "Selects:". When using a text editor on the .config file, its unlikely that the "Selects:" would have been chosen, resulting in an illegal .config file.
