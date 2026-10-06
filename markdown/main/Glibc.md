<!-- source: https://wiki.gentoo.org/wiki/Glibc | group: Gentoo Wiki (Main) | wiki-title: Glibc -->
---
title: Glibc
url: https://wiki.gentoo.org/wiki/Glibc
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-06-25"
fingerprint: ce54787e5e823d66
license: CC BY-SA 4.0
---

# Glibc

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The *GNU C library*, aka **glibc**,  is the default [C library](https://wiki.gentoo.org/wiki/Libc) included with Gentoo Linux.

## Installation

### USE flags


### USE flags for
            [sys-libs/glibc](https://packages.gentoo.org/packages/sys-libs/glibc)
            
            GNU libc C library

| [+clone3](https://packages.gentoo.org/useflags/+clone3) | Enable the new clone3 syscall within glibc. Can be disabled to allow compatibility with older Electron applications. | 
| [+crypt](https://packages.gentoo.org/useflags/+crypt) | build and install libcrypt and crypt.h | 
| [+multiarch](https://packages.gentoo.org/useflags/+multiarch) | enable optimizations for multiple CPU architectures (detected at runtime) | 
| [+ssp](https://packages.gentoo.org/useflags/+ssp) | protect stack of glibc internals | 
| [+static-libs](https://packages.gentoo.org/useflags/+static-libs) | Build static versions of dynamic libraries as well | 
| [audit](https://packages.gentoo.org/useflags/audit) | Enable support for Linux audit subsystem using sys-process/audit | 
| [caps](https://packages.gentoo.org/useflags/caps) | Use Linux capabilities library to control privilege | 
| [cet](https://packages.gentoo.org/useflags/cet) | Enable Intel Control-flow Enforcement Technology (needs binutils 2.29 and gcc 8) | 
| [clang](https://packages.gentoo.org/useflags/clang) | Allow building with clang (if proper environment is set). Highly experimental. Disable to auto-force gcc usage. | 
| [compile-locales](https://packages.gentoo.org/useflags/compile-locales) | build \*all\* locales in src\_install; this is generally meant for stage building only as it ignores /etc/locale.gen file and can be pretty slow | 
| [custom-cflags](https://packages.gentoo.org/useflags/custom-cflags) | Build with user-specified CFLAGS (unsupported) | 
| [debug](https://packages.gentoo.org/useflags/debug) | When USE=hardened, allow fortify/stack violations to dump core (SIGABRT) and not kill self (SIGKILL) | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [experimental-loong](https://packages.gentoo.org/useflags/experimental-loong) | Add experimental LoongArch patchset | 
| [gd](https://packages.gentoo.org/useflags/gd) | build memusage and memusagestat tools | 
| [hash-sysv-compat](https://packages.gentoo.org/useflags/hash-sysv-compat) | enable sysv linker hashes in glibc for compatibility with binary software (EAC via wine/proton) | 
| [headers-only](https://packages.gentoo.org/useflags/headers-only) | Install only C headers instead of whole package. Mainly used by sys-devel/crossdev for toolchain bootstrap. | 
| [multilib](https://packages.gentoo.org/useflags/multilib) | On 64bit systems, if you want to be able to compile 32bit and 64bit binaries | 
| [multilib-bootstrap](https://packages.gentoo.org/useflags/multilib-bootstrap) | Provide prebuilt libgcc.a and crt files if missing. Only needed for ABI switch. | 
| [nscd](https://packages.gentoo.org/useflags/nscd) | Build, and enable support for, the Name Service Cache Daemon | 
| [old-kernel](https://packages.gentoo.org/useflags/old-kernel) | Support the oldest kernel also supported by upstream glibc, otherwise the oldest in the Gentoo tree | 
| [perl](https://packages.gentoo.org/useflags/perl) | Install additional scripts written in Perl | 
| [profile](https://packages.gentoo.org/useflags/profile) | Add support for software performance analysis (will likely vary from ebuild to ebuild) | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [sframe](https://packages.gentoo.org/useflags/sframe) | enable building with sframe backtrace support | 
| [stack-realign](https://packages.gentoo.org/useflags/stack-realign) | Realign the stack in the 32-bit build for compatibility with older binaries at some performance cost | 
| [static-pie](https://packages.gentoo.org/useflags/static-pie) | Enable static PIE support (runtime files for -static-pie gcc option). | 
| [suid](https://packages.gentoo.org/useflags/suid) | Make internal pt\_chown helper setuid -- not needed if using Linux and have /dev/pts mounted with gid=5 | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [systemtap](https://packages.gentoo.org/useflags/systemtap) | Enable enhanced debugging hooks/interface via SystemTap static probe points. Note that this isn't exclusive to SystemTap, despite the name. This provides an interface which dev-debug/gdb optionally uses, see https://sourceware.org/gdb/wiki/LinkerInterface. | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [vanilla](https://packages.gentoo.org/useflags/vanilla) | Do not add extra patches which change default behaviour; DO NOT USE THIS ON A GLOBAL SCALE as the severity of the meaning changes drastically | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

### Upgrades

When glibc is upgraded it is wise to reload the running init system.

#### OpenRC

`root #``telinit u`
#### systemd

`root #``systemctl daemon-reload`
