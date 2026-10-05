<!-- source: https://wiki.gentoo.org/wiki/Kernel/Packages/Deprecated | group: Gentoo Wiki (Main) | wiki-title: Kernel/Packages/Deprecated -->
---
title: Kernel/Packages/Deprecated
url: https://wiki.gentoo.org/wiki/Kernel/Packages/Deprecated
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-10-26"
fingerprint: d79f191f94d22f27
license: CC BY-SA 4.0
---

# Kernel/Packages/Deprecated

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

List of **[kernel](https://wiki.gentoo.org/wiki/Kernel) packages that were once available** in the [Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository), for historical reference.

See the [kernel packages](https://wiki.gentoo.org/wiki/Kernel/Packages) article for information on kernel packages currently available in ::gentoo.

## Previously available kernel packages

### aa-sources

aa-sources was a heavily modified kernel with all kinds of patches. The upstream maintainer stopped releasing kernel patchsets and subsequently this package has been removed.

### alpha-sources

alpha-sources was a 2.4 kernel with patches applied to improve hardware compatibility for the Alpha architecture. These patches have been developed and are now included in the mainline kernel. Alpha users can run any recent kernel with no need for extra patches.

### Architecture dependent kernels

cell-sources was a 2.6 kernel designed to run on the Sony PlayStation 3 game console.

### aufs-sources

The aufs-sources package contains full kernel sources including the official genpatchset (found in gentoo-sources) for the 4.14/4.19 kernel tree and aufs4 support. This kernel is useful when attempting to utilize the aufs4 filesystem. For more information see the aufs page on [Sourceforge](http://aufs.sourceforge.net/) or the [genpatches homepage](https://dev.gentoo.org/~mpagano/genpatches/index.htm).

### ck-sources

ck-sources is Con Kolivas's kernel patch set. This patchset is primarily designed to improve system responsiveness and interactivity and is configurable for varying workloads (from servers to desktops). The patchset includes a different scheduler, MuQSS, designed to keep systems responsive and smooth even when under heavy load. Support and information is available [here](http://www.users.on.net/~ckolivas/kernel/) and in the `#ck` channel on [irc.oftc.net](https://www.oftc.net/).

### development-sources

development-sources, the official 2.6 kernel from [kernel.org](https://www.kernel.org/), can now be found under the [vanilla-sources](https://wiki.gentoo.org#vanilla-sources) package.

### gentoo-dev-sources

gentoo-dev-sources, a 2.6 kernel patched with bug, security, and stability fixes, can now be found under the [gentoo-sources](https://wiki.gentoo.org#General_purpose:_gentoo-sources) package.

### grsec-sources

The grsec-sources kernel source used to be patched with the latest grsecurity updates (grsecurity version 2.0 and up) which included, amongst other security-related patches, support for PaX. Grsecurity patches are included in the [hardened-sources](https://wiki.gentoo.org#hardened-sources) kernel, so this package is no longer available in Portage.

### hardened-sources

The [sys-kernel/hardened-sources](https://packages.gentoo.org/packages/sys-kernel/hardened-sources) kernel was based on the official Linux kernel and was targeted at users running Gentoo on server systems. It once provided patches for the various sub-projects of Gentoo Hardened (such as support for [SELinux](https://selinuxproject.org/) and [grsecurity](https://grsecurity.net)), together with stability and security-enhancements. Check out the [Hardened project here on the wiki](https://wiki.gentoo.org/wiki/Project:Hardened) for more information.

### hardened-dev-sources

hardened-dev-sources can now be found under the [hardened-sources](https://wiki.gentoo.org#hardened-sources) package.

### hppa-sources

hppa-sources was a 2.6 kernel with patches applied to improve hardware compatibility for the HPPA architecture. These patches have been developed and included in the mainline kernel. HPPA users can now run any recent kernel with no need for extra patches.

### mm-sources

The mm-sources were based on [vanilla-sources](https://wiki.gentoo.org#vanilla-sources) and contained Andrew Morton's patch set. They included the experimental and bleeding-edge features that were going to be included in the official kernel (or were going to be rejected because they set systems on fire!). They were known to be always moving at a fast pace and could change radically from one week to the other; kernel hackers often used mm-sources as a testing ground for highly experimental stuff. They have since been removed from the Portage tree.

### openvz-sources

OpenVZ is a server virtualization solution built on Linux. OpenVZ creates isolated, secure virtual private servers (VPSs) or virtual environments on a single physical server enabling better server utilization and ensuring that applications do not conflict. For more information, see [https://openvz.org/](https://openvz.org/).

### rsbac-dev-sources

The rsbac-dev-sources kernels could be found under the sys-kernel/rsbac-sources package.

### rsbac-sources

Back in the days of 2.6-based kernels sys-kernel/rsbac-sources contained patches to use Rule Set Based Access Controls ([RSBAC](http://www.rsbac.org)). It was removed due to lack of maintainers, but has has magically reappeared with the 3.10 kernel series. Use [hardened-sources](https://wiki.gentoo.org#hardened-sources) if additional security features are needed.

### selinux-sources

selinux-sources, a 2.4 kernel including lots of security enhancements, has been obsoleted by security development in the 2.6 kernel tree. SELinux functionality can be found in the [hardened-sources](https://wiki.gentoo.org#hardened-sources) package.

### sh-sources

sh-sources was a 2.6 kernel with patches applied to improve hardware compatibility for the SuperH architecture. These patches have been developed and included in the mainline kernel. SuperH users can now run any recent kernel with no need for extra patches.

### sparc-sources

sparc-sources was a 2.4 kernel with patches applied to improve hardware compatibility for the SPARC architecture. These patches have been developed and included in the mainline kernel. SPARC users can now run any recent kernel with no need for extra patches.

### tuxonice-sources

tuxonice-sources has been last-rited, see [bug #627924](https://bugs.gentoo.org/show_bug.cgi?id=627924).

The [sys-kernel/tuxonice-sources](https://packages.gentoo.org/packages/sys-kernel/tuxonice-sources) (formerly sys-kernel/suspend2-sources) are patched with both genpatches which includes the patches found in gentoo-sources, and the patches found in TuxOnIce which are an improved implementation of suspend-to-disk for the Linux kernel, formerly known as *suspend2*.

### uclinux-sources

The uclinux-sources are meant for CPUs without MMUs as well as embedded devices. For more information, see [http://www.uclinux.org](http://www.uclinux.org). Lack of security patches as well as hardware to test on were the reasons this package is no longer found in the Portage tree.

### usermode-sources

usermode-sources are the User Mode Linux kernel patches and can be found in the [sys-apps/usermode-utilities](https://packages.gentoo.org/packages/sys-apps/usermode-utilities) package. These kernel patches are designed to allow Linux to recursively run within Linux. User Mode Linux is intended for testing and virtual server support. For more information about this amazing tribute to the stability and scalability of Linux, see [http://user-mode-linux.sourceforge.net](http://user-mode-linux.sourceforge.net).

For more information on UML and Gentoo, read the [Gentoo User-mode Linux Guide](https://wiki.gentoo.org/wiki/User-mode_Linux/Guide)

### win4lin-sources

win4lin-sources were patched to support the userland win4lin tools that allowed Linux users to run many Microsoft Windows (TM) applications at almost native speeds. These kernel sources were removed due to security issues.

xbox-sources sources for the Xbox Linux kernel

### xen-sources

xen-sources was a 2.6-based kernel that allowed running multiple operating systems on a single physical system. A user could create virtual environments in which one or more guest operating systems could run on a [Xen](https://www.citrix.com/products/xenserver/)-powered host operating system.

The xen-sources patches were incorporated into the mainline Linux kernel as of version 3.0.


For more information on working with Xen and Gentoo, read the [Xen article here on the wiki](https://wiki.gentoo.org/wiki/Xen).

## See also

- [Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository)
- [Kernel](https://wiki.gentoo.org/wiki/Kernel) — a central part of the Gentoo [operating system (OS)](https://en.wikipedia.org/wiki/operating_system)
- [Kernel/Packages](https://wiki.gentoo.org/wiki/Kernel/Packages) — an overview of the different **[Linux kernel](https://wiki.gentoo.org/wiki/Kernel) packages** available in the [Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository).
