<!-- source: https://wiki.gentoo.org/wiki/Security_Handbook/Kernel_security | group: Gentoo Wiki (Main) | wiki-title: Security Handbook/Kernel security -->
---
title: Security Handbook/Kernel security
url: https://wiki.gentoo.org/wiki/Security_Handbook/Kernel_security
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-03-26"
fingerprint: "16931d5e5ca63f01"
license: CC BY-SA 4.0
---

# Security Handbook/Kernel security

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This section is on securing the Linux [kernel](https://wiki.gentoo.org/wiki/Kernel).



Kerneli was a patch developed in the late 1990s/early 2000s[\[1\]](https://wiki.gentoo.org#cite_note-1)<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup> which added support for cryptographic ciphers, digest algorithms and cryptographic loop filters, as early versions of the kernel did not contain these due to export regulations. Since the introduction of the Crypto API in version 2.5.45[\[3\]](https://wiki.gentoo.org#cite_note-3)<sup>[\[4\]](https://wiki.gentoo.org#cite_note-4)</sup>, this is only of historical interest now.

Removing whatever is unneeded when configuring the kernel will minimize attack surface, create a more optimized kernel, and reduce the chance for bugs in drivers or other features to be a means of compromise.

If loadable module support is unnecessary (`CONFIG_MODULES=n`), disable it. Though it is still possible to add rootkits without this feature, removing it makes it harder for attackers to install them via kernel modules. For further information see [Kernel\_Modules#Going completely "module-less"](https://wiki.gentoo.org/wiki/Kernel_Modules#Going_completely_.22module-less.22). If modules are needed, the kernel should be set to load only digitally signed modules (see [Signed kernel module support](https://wiki.gentoo.org/wiki/Signed_kernel_module_support)).

Information on kernel lockdown modes is available at the [dedicated page](https://wiki.gentoo.org/wiki/Security_Handbook/Kernel_security/Kernel_Lockdown).

The Kernel Self-Protection Project now has its own [page](https://wiki.gentoo.org/wiki/Security_Handbook/Kernel_security/Kernel_Self-Protection_Project) that gives an overview of the project and how to enable the recommended hardening options on Gentoo.

[sysctl](https://wiki.gentoo.org/wiki/Sysctl) can be used to manipulate the /etc/sysctl.conf configuration file.

- [Kernel](https://wiki.gentoo.org/wiki/Kernel) — a central part of the Gentoo [operating system (OS)](https://en.wikipedia.org/wiki/operating_system)
- [Kernel Modules](https://wiki.gentoo.org/wiki/Kernel_Modules) — object files that contain code to extend the [kernel](https://wiki.gentoo.org/wiki/Kernel) of an operating system.
- [Signed kernel module support](https://wiki.gentoo.org/wiki/Signed_kernel_module_support) — allows further hardening of the system by disallowing unsigned kernel modules, or kernel modules signed with the wrong key, to be loaded.

- [An abridged history of Linux kernel security](https://www.youtube.com/watch?v=LdcnxIviHuk) — Russell Currey (Everything Open 2023)
- [ASLR-NG: ASLR Next Generation](https://web.archive.org/web/20230306172439/http://cybersecurity.upv.es/solutions/aslr-ng/aslr-ng.html)
- [Kernel Address Space Layout Randomization](https://web.archive.org/web/20180619154220/http://selinuxproject.org/~jmorris/lss2013_slides/cook_kaslr.pdf) — Kees Cook, Linux Security Summit 2013<sup>[\[5\]](https://wiki.gentoo.org#cite_note-5)</sup>
- [Kernel address randomization](https://lwn.net/Articles/444503/) — Jonathan Corbet, 2011
- [Kernel Security Is Cool Again](https://www.youtube.com/watch?v=GFGJ3e3oj2c) — Casey Schaufler, linux.conf.au 2019
- [Overview of the Linux Kernel Security Subsystem](https://www.youtube.com/watch?v=L7KHvKRfTzc) — James Morris, Microsoft
- [Rule Set Based Access Control](https://www.rsbac.org/) (RSBAC)
- [The OpenWall Project](https://www.openwall.com/)
