<!-- source: https://wiki.gentoo.org/wiki/Kernel/Command-line_parameters | group: Gentoo Wiki (Main) | wiki-title: Kernel/Command-line parameters -->
---
title: Kernel/Command-line parameters
url: https://wiki.gentoo.org/wiki/Kernel/Command-line_parameters
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-30"
fingerprint: "9692101f11e3bbaf"
license: CC BY-SA 4.0
---

# Kernel/Command-line parameters

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article mentions some of more commonly useful tuning knobs which can be passed to the Linux kernel at boot time. These are defined by upstream as "command-line parameters".

A full list of parameters for the latest Linux kernel is provided at: [https://www.kernel.org/doc/html/latest/admin-guide/kernel-parameters.html](https://www.kernel.org/doc/html/latest/admin-guide/kernel-parameters.html)

| Linux kernel commandline options |  |  | 
|---|---|---|
| Parameter | Options | Notes | 
|---|---|---|
| `debug=` | N/A | Enable kernel debug events. | 
| `mitigations` | `off`, `auto` (default option), `auto,nosmt` | Disables or adjusts protections against known CPU vulnerabilities, but can provide speed improvements for certain CPUs or systems when trading security for speed is desired. | 
| `loglevel=` | `0` (lowest output), `1`, `2`, `3`, `4`, `5`, `6`, `7`, `8` (highest output) | Useful for adjusting the kernel's ring buffer output verbosity. The higher the number, the greater the verbosity. Be careful, high levels of verbosity can quickly consume high amounts of disk space! | 
| `root=` | `PARTUUID=<UUID>`, `PARTLABEL="<label_name>"`, `/dev/sda`, `/dev/sda1`, etc. | A block device specifier can be passed via this parameter such as a UUID of a device partition label ( `PARTUUID=<UUID>`), a partition label (`PARTLABEL="<label_name>"`), a device number of a disk (`/dev/<disk_name>`), a device number of a partition (`/dev/<disk_name><decimal>`), etc. See the [full list here](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/block/early-lookup.c#n217). | 
| `rootdelay=` | `N` (where N is an integer number) | A delay in seconds the kernel should wait before attempting to mount the rootfs. Adding a delay here can be very useful for an uncommonly slow drive, like when running the rootfs off a USB drive. | 
| `earlyprintk=` | `vga`, `sclp`, `serial[,ttySn[,baudrate]]`, | Provides an alternate output location for the kernel's printk messages. Useful in the event at the main display crashes before any kernel messages can be read from the output. | 
| `module_blacklist=` | `<module_name>`, `<module_name_2>`, etc. | A comma separated list of module names to block from loading during the kernel boot process. Useful if a certain module is causing a problem; such as accidentally loading a debug kernel module with spews millions of messages into the printk output, therefore making information difficult to find. | 
| `nomodule` | N/A | An option to prevent *all* modules from loading during the kernel boot process. | 
| `fbcon=` | `font:<name>`, `map:<0123>`, `vc:<n1>-<n2>`, `rotate:<n>`, `margin:<color>`, `nodefer`, `logo-pos:center`, `logo-count:<n>` | Sets [various kernel.org framebuffer console options](https://www.kernel.org/doc/html/v6.1/fb/fbcon.html) when booting the system. For `CONFIG_FRAMEBUFFER_CONSOLE_DEFERRED_TAKEOVER=y` and `CONFIG_LOGO=y` kernels like [sys-kernel/gentoo-kernel-bin](https://packages.gentoo.org/packages/sys-kernel/gentoo-kernel-bin), `fbcon=nodefer` restores Tux for every online CPU. | 

There are different ways to pass parameters to the kernel. The *Kconfig* option is suitable for those who manually configure and compile the kernel. The UEFI variant can be used with a kernel distributed in binary form, but is only suitable for UEFI systems. The Bootloaders option is non-deterministic and depends on the bootloader.

For the x86 architecture, the parameters can be set via *menuconfig* as follows:

**AMD64**

Or by directly specifying the corresponding settings:

**`.config`**

For the ARM architecture, the parameters can be set via *menuconfig* as follows:

**ARM64**

Or by directly specifying the corresponding setting:

**`.config`**

The kernel parameters can be written into UEFI entries, for example by using the `--unicode` argument of the *efibootmgr* program. See [this article](https://wiki.gentoo.org/wiki/EFI_stub#Root_partition_configuration) for more information.

Each bootloader has its own way of passing parameters to the kernel. The most popular bootloaders and their options are presented below.

The kernel parameters can be set via the GRUB setting `GRUB_CMDLINE_LINUX`. See [this article](https://wiki.gentoo.org/wiki/GRUB#Setting_configuration_parameters) for more information.

See [this article](https://wiki.gentoo.org/wiki/LILO#Adding_kernel_parameters) to pass the kernel parameters via LILO.

The kernel parameters can be set via the `options` setting. See [this article](https://wiki.gentoo.org/wiki/Systemd/systemd-boot#Menu_entry_files) for more information.

Parameters passed to the kernel at boot are known as the kernel command line. The command line passed to the currently running kernel can be displayed by using the [proc(5)](https://man.archlinux.org/man/proc.5.en) [filesystem path /proc/cmdline:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`user $``cat /proc/cmdline`
BOOT\_IMAGE=/boot/vmlinuz-6.3.3-gentoo-x86\_64 ro init=/usr/lib/systemd/systemd pcie\_aspm=off pcie\_port\_pm=off pcie\_pme=nomsi iommu=1 amd\_iommu=on

For further information, refer to [bootparam(7)](https://man.archlinux.org/man/bootparam.7.en) [and, if using](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [systemd](https://wiki.gentoo.org/wiki/Systemd), [kernel-command-line(7)](https://man.archlinux.org/man/kernel-command-line.7.en)[.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

- [Kernel](https://wiki.gentoo.org/wiki/Kernel) — a central part of the Gentoo [operating system (OS)](https://en.wikipedia.org/wiki/operating_system)
