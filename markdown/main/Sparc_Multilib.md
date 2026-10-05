<!-- source: https://wiki.gentoo.org/wiki/Sparc/Multilib | group: Gentoo Wiki (Main) | wiki-title: Sparc/Multilib -->
---
title: Sparc/Multilib
url: https://wiki.gentoo.org/wiki/Sparc/Multilib
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-19"
fingerprint: "3ed5737a2d813dc2"
license: CC BY-SA 4.0
---

# Sparc/Multilib

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This guide will help you to migrate your existing Gentoo Linux SPARC installation from a non-multilib profile to a multilib profile.

## Overview

This guide will help you to migrate your existing Gentoo Linux SPARC installation from a non-multilib profile to a multilib profile.

Multilib has advantages but it also has some disadvantages, these are the facts:

| Advantages |  | Drawbacks |  | 
|---|---|---|---|

## Migration

### Update the make.profile symlink

Because the profile is still in an experimental state you have to create/change the /etc/make.profile symlink manually.

The multilib profile also provides these three sub-profiles for your convenience, just like 2008.0 does:

- desktop
- developer
- server

`root #````
rm /etc/make.profile
```
`root #````
ln -s /usr/portage/profiles/default/linux/sparc/experimental/multilib /etc/make.profile
```
or

`root #``ln -s /usr/portage/profiles/default/linux/sparc/experimental/multilib/desktop /etc/make.profile`
or

`root #``ln -s /usr/portage/profiles/default/linux/sparc/experimental/multilib/developer /etc/make.profile`
or

`root #``ln -s /usr/portage/profiles/default/linux/sparc/experimental/multilib/server /etc/make.profile`
### Confirm that you read this guide

If someone would switch to the multilib profile without reading this guide, their system would break. To prevent that you need to confirm that you read this by adding `I_READ_THE_MULTILIB_MIGRATION_GUIDE` to the end of your /etc/make.conf.

**`/etc/make.conf`**

**Edit make.conf**

```
I_READ_THE_MULTILIB_MIGRATION_GUIDE="yes"
```
### Rename lib to lib32

Though it is not fully compliant with [FHS](https://wiki.gentoo.org/wiki/Filesystem_Hierarchy_Standard), common practice in Gentoo is to store 32-bit libraries in lib32, 64-bit libraries in lib64 and to have lib as a symlink to the default library directory.

The 2008.0 profile stores libraries in lib. These commands will rename all lib directories to lib32 and create a symlink lib to lib32. Additionally they will create empty lib64 directories.

### Remerge baselayout

For multilib profiles, [sys-apps/baselayout](https://packages.gentoo.org/packages/sys-apps/baselayout) installs additional files, like /etc/env.d/04multilib. To get these you need to remerge it.

`root #``emerge --ask --oneshot sys-apps/baselayout``root #``env-update && source /etc/profile`
### Install a multilib glibc

To compile a multilib glibc you need a biarch gcc but to compile a biarch gcc you need a multilib glibc. You *could* compile glibc using a cross-compiler such as [sys-devel/kgcc64](https://packages.gentoo.org/packages/sys-devel/kgcc64) but that is not something you would enjoy...

Therefore we will install a binary packages of glibc first and afterwards, once the migration is complete, remerge it with your USE- and CFLAGS.

If you get *All ebuilds that could satisfy [sys-libs/glibc](https://packages.gentoo.org/packages/sys-libs/glibc) have been masked.* try using a different version of glibc, these two are available as binary packages:

- =[sys-libs/glibc](https://packages.gentoo.org/packages/sys-libs/glibc)-2.8\_p20080602-r1
- =[sys-libs/glibc](https://packages.gentoo.org/packages/sys-libs/glibc)-2.9\_p20081201-r2

### Install a biarch GCC

With a multilib glibc installed you can simply compile a biarch GCC using portage.

`root #``emerge --ask --oneshot sys-devel/gcc`
Now you need to configure your system to use the newly installed GCC using gcc-config. Replace `4.3.2` with the version you just installed.

`root #````
gcc-config sparc-unknown-linux-gnu-4.3.2
```
`root #````
source /etc/profile
```
### Remerge glibc with your preferred settings

Remerge glibc with your system specific settings (USE-flags, CFLAGS and such).

`root #``emerge --ask --oneshot sys-libs/glibc`
If the glibc compilation finishes without any strange error messages it means that your multilib setup is working. You can use file to verify this by checking the contents of /lib64.

`user $````
file /lib64/libc-*.so
```
/lib64/libc-2.7.so: ELF 64-bit MSB shared object, SPARC V9, version 1 (SYSV), for GNU/Linux 2.6.9, stripped

### Unmerge kgcc64

[sys-devel/gcc](https://packages.gentoo.org/packages/sys-devel/gcc) can compile the kernel now, therefore you don't need [sys-devel/kgcc64](https://packages.gentoo.org/packages/sys-devel/kgcc64) anymore to do it. If you have some scripts that need it, just replace `sparc64-unknown-linux-gnu-gcc` with `sparc-unknown-linux-gnu-gcc -m64`.

`root #``emerge --ask --depclean --verbose sys-devel/kgcc64`
### Remerge world

Your system was build with lib as library directory, now it is lib32. Though you will not notice anything because there is a symlink in place but if you have a lot of spare CPU time and like your system clean...

Or in other words: If you are one of those guys who uses Portage's multilib-strict feature just for the fun of it...this is for you!

`root #``emerge --ask --emptytree world`
## Closing-off

### Usage example

Here's a simple example how to compile a hello world program in both 32 and 64-bit.

Compile it as 32-bit binary:

`user $``sparc-unknown-linux-gnu-gcc -m32 -o hello_world hello_world.c`
or

`user $``sparc-unknown-linux-gnu-gcc -o hello_world hello_world.c`
Compile it as 64-bit binary:

`user $``sparc-unknown-linux-gnu-gcc -m64 -o hello_world hello_world.c`
### Things you should not do

So now you have a multilib installation and you are thinking about adding `-m64` to `CFLAGS` in /etc/make.conf and recompiling your entire userland in 64-bit? **PLEASE DO NOT!**

While compiling everything in 64-bit may be a good idea on other 64-bit architectures, like **amd64**, on **sparc** it is not. There are good reasons why we have been using a pure 32-bit userland until now, some of these are:

- 32-bit is faster than 64-bit
- 32-bit is well tested, 64-bit is not tested at all
- 2039 is still long way off

The only reasons why it *might* be appropriate to compile an application in 64-bit are:

- It needs to access more than 4GB of memory. In the real world this only applies to huge databases.
- It needs to talk to the kernel directly. Some applications, like [net-firewall/iptables](https://packages.gentoo.org/packages/net-firewall/iptables), contain ugly hacks to support the 64-bit kernel/32-bit userland thing.
- It is a kernel.

If you would like to read more about the differences between 32 and 64-bit, there are dozens of webpages about it on the internet, one of them is [http://www.superlogic.net/downloads/pub/docs/64bit.htm](http://www.superlogic.net/downloads/pub/docs/64bit.htm).

## Acknowledgements

We would like to thank the following authors and editors for their contributions to this guide:

- bluebird
