<!-- source: https://wiki.gentoo.org/wiki/Kpatch | group: Gentoo Wiki (Main) | wiki-title: Kpatch -->
---
title: Kpatch
url: https://wiki.gentoo.org/wiki/Kpatch
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-06-08"
fingerprint: de51395ec4c63920
license: CC BY-SA 4.0
---

# Kpatch

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

kpatch is a GPLv2 licensed dynamic kernel patching tool developed by RedHat.

## Installation

### Kernel

The Linux kernel must be version 4.0 or higher in order to have `LIVEPATCH` support.[\[1\]](https://wiki.gentoo.org#cite_note-1)

KERNEL **Enable `CONFIG_LIVEPATCH` support**

### USE flags


| [+kpatch](https://packages.gentoo.org/useflags/+kpatch) | Enable a command-line tool which allows a user to manage a collection of patch modules. | 
| [+kpatch-build](https://packages.gentoo.org/useflags/+kpatch-build) | Enable tools which convert a source diff patch to a patch module. | 
| [+strip](https://packages.gentoo.org/useflags/+strip) | Allow symbol stripping to be performed by the ebuild for special files | 
| [contrib](https://packages.gentoo.org/useflags/contrib) | Enable contrib kpatch services files. | 
| [dist-kernel](https://packages.gentoo.org/useflags/dist-kernel) | Enable subslot rebuilds on Distribution Kernel upgrades | 
| [kmod](https://packages.gentoo.org/useflags/kmod) | Enable a kernel module (.ko file) which provides an interface for the patch modules to register new functions for replacement. | 
| [modules-compress](https://packages.gentoo.org/useflags/modules-compress) | Install compressed kernel modules (if kernel config enables module compression) | 
| [modules-sign](https://packages.gentoo.org/useflags/modules-sign) | Cryptographically sign installed kernel modules (requires CONFIG\_MODULE\_SIG=y in the kernel) | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

`root #``emerge --ask sys-kernel/kpatch`
## Usage

`user $``kpatch --help`
### Workflow

`root #````
Kpatch-build foo.patch
```
`root #````
insmod kpatch-foo.ko
```
