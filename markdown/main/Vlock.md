<!-- source: https://wiki.gentoo.org/wiki/Vlock | group: Gentoo Wiki (Main) | wiki-title: Vlock -->
---
title: vlock
url: https://wiki.gentoo.org/wiki/Vlock
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-11-20"
fingerprint: b79d9fbfc1c5bb98
license: CC BY-SA 4.0
---

# vlock

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**vlock** is a **V**irtual Console **lock** program.

### Concepts

Sometimes a malicious local user could cause more problems than a sophisticated remote one. vlock is a program that locks one or more sessions on the Linux console to prevent attackers from gaining physical access to the machine.

## Installation

### USE flags


| [pam](https://packages.gentoo.org/useflags/pam) | Add support for PAM (Pluggable Authentication Modules) - DANGEROUS to arbitrarily flip | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

To install [app-misc/vlock](https://packages.gentoo.org/packages/app-misc/vlock):

`root #``emerge --ask app-misc/vlock`
## Usage

When not working in a virtual console, switch to one by pressing `CTRL`+`ALT`+`F1` through `F6`. By default, vlock locks the current console session. Use the `-a` switch in order to lock all console sessions.

`user $``vlock -a`
It is also possible to use vlock from an X session. Use the `-n` option to make vlock switch to an empty virtual console.

`root #``usermod -a -G vlock larry``user $``vlock -na`
### Disable SysRq key

The magic `SysRq` key combination can unlock consoles when least expected. In order to prevent this, disable the SysRq mechanism while consoles are locked like so:

`user $``vlock -sa`
If a user does not know how to use the `SysRq` key, then it is probably not needed. Disable it when configuring the kernel:

**Disabling Magic SysRq key**
