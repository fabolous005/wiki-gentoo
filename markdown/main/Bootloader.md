<!-- source: https://wiki.gentoo.org/wiki/Bootloader | group: Gentoo Wiki (Main) | wiki-title: Bootloader -->
---
title: Bootloader
url: https://wiki.gentoo.org/wiki/Bootloader
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-14"
fingerprint: df83b01e00b0bfac
license: CC BY-SA 4.0
---

# Bootloader

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

A **bootloader** is a program that, in the Linux context, finds and runs the operating system kernel when the system is started. It typically provides a choice between multiple operating systems, and the ability to customize the arguments it will be launched with.

## Available software

Depending on the architecture of the machine, several bootloaders are available:

| Name | Package | Description | Notes | 
|---|---|---|---|
| [EFI stub](https://wiki.gentoo.org/wiki/EFI_stub) | - | Using the [(U)EFI](https://wiki.gentoo.org/wiki/UEFI) firmware to directly load a Linux kernel, skipping the use of a secondary bootloader. |  | 
| [GRUB 2](https://wiki.gentoo.org/wiki/GRUB_2) | [sys-boot/grub](https://packages.gentoo.org/packages/sys-boot/grub) | Reworked version of GRUB. Gentoo's default bootloader on **x86**, **amd64**, **ppc**, **ppc64**, **sparc** and some **mips** based devices. |  | 
| [LILO](https://wiki.gentoo.org/wiki/LILO) | [sys-boot/lilo](https://packages.gentoo.org/packages/sys-boot/lilo) | Simple boot loader with some advantages over GRUB and GRUB2. | Beneficial for low-memory devices (e.g. \< 32MiB). | 
| [Limine](https://wiki.gentoo.org/wiki/Limine) | [sys-boot/limine::guru](https://github.com/gentoo-mirror/guru/tree/master/sys-boot/limine) | Multiprotocol bootloader and boot manager. |  | 
| [rEFInd](https://wiki.gentoo.org/wiki/REFInd) | [sys-boot/refind](https://packages.gentoo.org/packages/sys-boot/refind) | Boot manager for EFI and UEFI platforms. | Does not provide native drivers to read XFS devices. | 
| [syslinux](https://wiki.gentoo.org/wiki/Syslinux) | [sys-boot/syslinux](https://packages.gentoo.org/packages/sys-boot/syslinux) | Collection of simple bootloaders for various purposes. |  | 
| [systemd-boot](https://wiki.gentoo.org/wiki/Systemd/systemd-boot) | [sys-apps/systemd](https://packages.gentoo.org/packages/sys-apps/systemd) or [sys-apps/systemd-utils](https://packages.gentoo.org/packages/sys-apps/systemd-utils) | Bootloader that is specific to UEFI firmware and the [systemd](https://wiki.gentoo.org/wiki/Systemd) init system. |  | 
| [U-Boot](https://wiki.gentoo.org/wiki/Embedded_Handbook/Bootloaders/Das_U-Boot) | [dev-embedded/u-boot-tools](https://packages.gentoo.org/packages/dev-embedded/u-boot-tools) | Bootloader popular with embedded devices. See also [U-boot-tools](https://wiki.gentoo.org/wiki/U-boot-tools). |  | 

## See also

- [Handbook:AMD64/Installation/Bootloader](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Bootloader)
- [Security Handbook/Boot Path Security](https://wiki.gentoo.org/wiki/Security_Handbook/Boot_Path_Security) — boot path security.
- [Embedded Handbook/Bootloaders](https://wiki.gentoo.org/wiki/Embedded_Handbook/Bootloaders)

## External resources

- [Comparison of boot loaders](https://en.wikipedia.org/wiki/Comparison_of_boot_loaders) (Wikipedia)
