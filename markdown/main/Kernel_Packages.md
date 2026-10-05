<!-- source: https://wiki.gentoo.org/wiki/Kernel/Packages | group: Gentoo Wiki (Main) | wiki-title: Kernel/Packages -->
---
title: Kernel/Packages
url: https://wiki.gentoo.org/wiki/Kernel/Packages
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-26"
categories: ['sys-kernel']
fingerprint: "1793395e90b23f25"
license: CC BY-SA 4.0
---

# Kernel/Packages

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


This document is an overview of the different **[Linux kernel](https://wiki.gentoo.org/wiki/Kernel) packages** available in the [Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository).

The [kernel team](https://wiki.gentoo.org/wiki/Project:Kernel) aims to give users a choice of Linux kernel variants. Kernel packages can be found in the [sys-kernel](https://packages.gentoo.org/categories/sys-kernel) package category in the Gentoo ebuild repository.

There is an article listing [previously available kernel packages](https://wiki.gentoo.org/wiki/Kernel/Packages/Deprecated), for historical reference.

## Distribution Kernel packages

The [Distribution Kernel](https://wiki.gentoo.org/wiki/Distribution_Kernel) packages provide a convenient solution to install and manage kernels on Gentoo via the [Portage](https://wiki.gentoo.org/wiki/Portage) package manager.

### gentoo-kernel-bin

[sys-kernel/gentoo-kernel-bin](https://packages.gentoo.org/packages/sys-kernel/gentoo-kernel-bin) is prebuilt and preconfigured to support most systems.

### gentoo-kernel

[sys-kernel/gentoo-kernel](https://packages.gentoo.org/packages/sys-kernel/gentoo-kernel) is preconfigured to support most systems, but provides integrated facilities for configuration customization.

### vanilla-kernel

[sys-kernel/vanilla-kernel](https://packages.gentoo.org/packages/sys-kernel/vanilla-kernel) is a vanilla (unmodified) upstream kernel with the convenience of Distribution Kernels. This package's secondary goal is to allow building universal binary packages that can be installed on a variety of systems with different hardware, /boot layouts, and bootloaders. For details, see [Michał Górny - A distribution kernel for Gentoo](https://blogs.gentoo.org/mgorny/2019/12/19/a-distribution-kernel-for-gentoo/).

## Kernel-source packages

### Supported

#### gentoo-sources

[sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources) is a kernel based on Linux 6.x, lightly patched to fix security problems, kernel bugs, and to increase compatibility with the more uncommon system architectures.

The [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources) package absorbs most of the resources of the Gentoo kernel team. They are brought to the user by a group of talented developers, which can count on the expertise of popular kernel hacker Greg Kroah-Hartman, maintainer of udev and responsible for the USB and PCI subsystems of the official Linux kernel.

#### git-sources

The [sys-kernel/git-sources](https://packages.gentoo.org/packages/sys-kernel/git-sources) package tracks daily snapshots of the upstream development kernel tree. These kernels are good for users interested in kernel development or testing. Bug reports should go to the [Linux Kernel Bug Tracker](https://bugzilla.kernel.org/) or [LKML](https://lkml.org/) (Linux Kernel Mailing List).

#### Architecture dependent kernels

[sys-kernel/mips-sources](https://packages.gentoo.org/packages/sys-kernel/mips-sources) is patched to run best on MIPS-based machines and has some of the patches for hardware and feature support from other patch sets.

### Unsupported

These kernels are provided as a courtesy only, and are not supported by the Gentoo kernel team.

#### pf-sources

The [sys-kernel/pf-sources](https://packages.gentoo.org/packages/sys-kernel/pf-sources) kernel brings together parts of several different kernel patches. It includes the BFS patchset from [sys-kernel/ck-sources](https://packages.gentoo.org/packages/sys-kernel/ck-sources), the [sys-kernel/tuxonice-sources](https://packages.gentoo.org/packages/sys-kernel/tuxonice-sources) patches, [LinuxIMQ](http://www.linuximq.net), and the [BFQ](http://algo.ing.unimo.it/people/paolo/disk_sched/patches/) I/O scheduler.

#### rt-sources

The [sys-kernel/rt-sources](https://packages.gentoo.org/packages/sys-kernel/rt-sources) kernel is based on [sys-kernel/vanilla-sources](https://packages.gentoo.org/packages/sys-kernel/vanilla-sources) and includes the PREEMPT\_RT patch. That patch turns the Linux kernel into a real-time operating system (RTOS). Use this if your system requires real-time guarantees. For more information, see [https://wiki.linuxfoundation.org/realtime/start](https://wiki.linuxfoundation.org/realtime/start).

#### vanilla-sources

Many Linux users will probably be familiar with the [sys-kernel/vanilla-sources](https://packages.gentoo.org/packages/sys-kernel/vanilla-sources) package. These kernels are copies of the official kernel sources released on [https://www.kernel.org/](https://www.kernel.org/). Please note that the Gentoo kernel team does not patch vanilla-sources at all; they are for people who wish to run a completely unmodified Linux kernel. The Gentoo kernel team recommends [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources) instead.

#### zen-sources

The [sys-kernel/zen-sources](https://packages.gentoo.org/packages/sys-kernel/zen-sources) package is designed for desktop systems. It includes code not found in the mainline kernel. The Zen kernel has patches that add new features, support additional hardware, and contains various tweaks for desktops. For more information on the Zen kernel please visit [Zen Kernel GitHub repository](https://github.com/zen-kernel/zen-kernel).

## See also

- [Genkernel](https://wiki.gentoo.org/wiki/Genkernel) — a tool created by Gentoo used to automate the build process of the [kernel](https://wiki.gentoo.org/wiki/Kernel) and [initramfs](https://wiki.gentoo.org/wiki/Initramfs).
- [Handbook:AMD64/Installation/Kernel](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Kernel)
- [Kernel](https://wiki.gentoo.org/wiki/Kernel) — a central part of the Gentoo [operating system (OS)](https://en.wikipedia.org/wiki/operating_system)
- [Kernel/Packages/Deprecated](https://wiki.gentoo.org/wiki/Kernel/Packages/Deprecated) — **[kernel](https://wiki.gentoo.org/wiki/Kernel) packages that were once available** in the [Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository)
