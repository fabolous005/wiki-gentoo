<!-- source: https://wiki.gentoo.org/wiki/Hardened_Gentoo | group: Gentoo Wiki (Main) | wiki-title: Hardened Gentoo -->
---
title: Hardened Gentoo
url: https://wiki.gentoo.org/wiki/Hardened_Gentoo
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-04"
fingerprint: "16cbfb7a1583bba6"
license: CC BY-SA 4.0
---

# Hardened Gentoo

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Gentoo Hardened** is a Gentoo project that offers multiple additional security services on top of the well-known Gentoo Linux installation.

Whether running an Internet-facing server or a flexible workstation, when dealing with multiple threats it can be advantageous to harden the system further than just automatically applying the latest security patches. *Hardening* a system means taking additional countermeasures against attacks and other risks and is usually a combined set of activities performed on the system.

The base of Gentoo Hardened is a hardened toolchain by enabling specific options in the toolchain (compiler, linker ...) such as forcing position-independent executables (PIE), stack smashing protection and compile-time buffer checks. See the [table](https://wiki.gentoo.org/wiki/Hardened/Toolchain#Changes).

Within Gentoo Hardened, several *additional* projects are active that help further harden a Gentoo system through:

- Enabling [SELinux](https://wiki.gentoo.org/wiki/Hardened_Gentoo/SELinux) extensions in the Linux kernel, which offers a Mandatory Access Control system enhancing the standard Linux permission restrictions.
- Enabling [Integrity](https://wiki.gentoo.org/wiki/Integrity) related technologies, such as Integrity Measurement Architecture, for making systems resilient against tampering.

Of course, this includes the necessary userspace utilities to manage these extensions.

Select a hardened [profile](https://wiki.gentoo.org/wiki/Portage/Profiles), so that *package management* will be done in a hardened way.

`root #``eselect profile list``root #``eselect profile set [number of hardened profile]``root #``source /etc/profile`
By choosing the hardened profile, certain package management settings (masks, USE flags, etc) become default for the system. This applies to many packages, including the toolchain. The toolchain is used for building/compiling programs, and includes: the [gcc](https://wiki.gentoo.org/wiki/Gcc) (GNU Compiler Collection), [binutils](https://wiki.gentoo.org/wiki/Binutils) (linker, etc.), and the [glibc](https://wiki.gentoo.org/wiki/Glibc) (GNU C library). By re-emerging the toolchain, these new default settings will apply to the toolchain, which will allow all future *package compiling* to be done in a hardened way.

`root #``emerge --oneshot sys-devel/gcc``root #``emerge --oneshot sys-devel/binutils sys-libs/glibc`
The above commands rebuilt GCC, which can now be used to compile hardened software. Make sure that the compiler selected is the version just built:

`root #````
gcc-config -l
```
\[1\] x86\_64-pc-linux-gnu-9.3.0 \*
\[2\] x86\_64-pc-linux-gnu-8.5.0

Finally source the new profile settings:

`root #``source /etc/profile`
Now reinstall all packages with the new hardened toolchain:

`root #``emerge --emptytree --verbose @world`
If not using the [distribution kernel](https://wiki.gentoo.org/wiki/Project:Distribution_Kernel), reinstall the kernel sources:

`root #``emerge --ask gentoo-sources`
Now configure/compile the sources and add the new kernel to the boot manager (e.g. GRUB).

For more information, check out the following resources:
