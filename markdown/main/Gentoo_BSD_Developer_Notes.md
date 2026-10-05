<!-- source: https://wiki.gentoo.org/wiki/Gentoo_BSD/Developer_Notes | group: Gentoo Wiki (Main) | wiki-title: Gentoo BSD/Developer Notes -->
---
title: Gentoo BSD/Developer Notes
url: https://wiki.gentoo.org/wiki/Gentoo_BSD/Developer_Notes
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2019-01-09"
fingerprint: "9fc27a6b8d250fa9"
license: CC BY-SA 4.0
---

# Gentoo BSD/Developer Notes

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

As of **April 20, 2017**, this article is **deprecated (obsolete)**. Contents are <u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

This guide was meant to display some Gentoo BSD specific notes about working with packages on a Gentoo BSD environment.

## System structure

### System ebuild organization

For consistency with the way upstream packages the system's sources (at least on FreeBSD), it is suggested to follow, now and in future ports, a simple rule about the organization of the system's ebuild.

First of all, the categories should reflect the port itself, as used with the sys-freebsd category. This allows excluding with `rsync_exclude` the part of Portage that does not apply for your system.

After that divide the packages on the name of the source package. This means that they should be divided on "bin", "usr.bin", "lib", and so on. Since a dot (`.`) is not a good character for a package name, it is better to contract the "usr." part to "u", as FreeBSD's devs do when they release the sources of a new version (see 5.4 sources release as an example). We'll have bin and ubin. The name should then made unique prefixing it with the port name for example: ([sys-freebsd/freebsd-bin](https://packages.gentoo.org/packages/sys-freebsd/freebsd-bin), [sys-freebsd/freebsd-ubin](https://packages.gentoo.org/packages/sys-freebsd/freebsd-ubin), [sys-freebsd/freebsd-lib](https://packages.gentoo.org/packages/sys-freebsd/freebsd-lib)).

Currently the following package categories are being worked on:

- sys-freebsd
- sys-openbsd
- sys-dragonfly (developed unofficially by arachnist)
- sys-netbsd

### Base packages

Gentoo/\*BSD should try to be in conformance with the Gentoo layout as much as possible. At the same it should also attempt to maintain the BSD layout. There are cases where using software in Portage is preferred over using the applicable port's versions. The experience on Gentoo/FreeBSD suggest a good way to deal with this. The reasons behind this is that it takes less time to build a simpler package than to install everything to base packages, and it is quicker to fix them if needed, for example, for security patches.

What should not be installed in base system is mainly things that belongs to contrib sections, like GNU software that is installed by the system (think of `grep`, `gsed`, and so on), but it can also be something else. For example Gentoo/FreeBSD builds on its own bsdtar because that can be used by other systems, too (like Linux). The same applies for other packages that can be shared between different Gentoo/\*BSD systems.

While stated earlier that base system packages go into sys-SYSTEM category, packages like bsdtar, libbegemot and libbsnmp goes into their "classic" categories, app-arch, dev-libs and net-libs, as they are shared between systems. If they are not going to work on Linux, the best thing is to package.mask them on default-linux profile in Portage (if you're going to merge Portage or this is not going to be a problem for main tree maintainers).

When doing this kind of splitting, is important to watch at the differences that might be between different BSD flavors, as sometimes they share and mix the same library or tool between them. When there are differences that might mean different behavior, is better not to split, or try to contact the upstream devs (trying to find who is the original author of that code) to see if it's possible to merge the behavior again.

An exception can be done for packages such as gcc and binutils that can have patches that are mandatory on the porting platform and that can be difficult to split out. It's usually recommended that the patches get split out as soon as possible. In case there's no clean way to do so, the entire patchset should be applied over a vanilla gcc that is built with the same way Gentoo's GCC is built. This ensures that `gcc-config` can take it up.

## Non-system package notes

### Source tarballs structur

When preparing the source tarballs, according to the notes above, you should package separately, as is done by the original FreeBSD. To make easier this task, you can find a simple script in CVS called extract.sh. This scripts accepts two parameters, the first one is the version you're getting the packages for, and must be formatted in the Gentoo way (that is, 6.0 beta4 packages would be created asking for 6.0\_beta4 version), the script takes care of translating that to the FreeBSD naming scheme. After that, the scripts downloads all the sources in 1.44-sized files, recompose them and then translate the tarballs from gzip to bzip2.

The files should then be loaded into /space/distfiles-local so that the mirrors can fetch them.

### libtool and library naming

One of the most "black magic" problems that are faced by Gentoo/\*BSD developers is the libtool naming scheme. By default on FreeBSD and DragonFly BSD the libraries are named with a single versioning number after the .so extension. This makes the soname and the actual library name be the same.

Unfortunately this approach, while having its own reasons, makes difficult for users to updated from one version of a library to another, if that changes the first part of its version. For instance, libiconv 1.9 and libiconv 1.10, are respectively libiconv.so.2.2 and libiconv.so.2.3 on Linux, while they are libiconv.so.4 and libiconv.so.5 on native FreeBSD. This breaks everything that links against libiconv, and this is a big part of the system, as also GCC, if compiled with nls support, links against it. The same trouble goes for glib, GTK+ and Gnome libraries.

The solution to this problem is to use the Linux naming scheme on Gentoo/FreeBSD and Gentoo/DragonFly. To achieve this, it's needed to patch libtool, but as many packages are shipped with complete autotools, it's not practical to re-execute libtoolize over everyone of them. The solution is to make use of the already widely used elibtoolize function, that patches directly the already created files (in particular the configure file) to use the Linux naming when being on Gentoo systems.

## Eclasse guide

### Introduction

To make simple the work for developers working with BSD-packaged source code, the Gentoo/FreeBSD ebuilds make use of two main [eclasses](https://wiki.gentoo.org/wiki/Eclass) designed just for supporting the "ebuildification" of ports-like packages. One is generic and is shared between all the other Gentoo/\*BSD ports, while the other is more related to the handling of the system ebuilds.

The two eclasses are bsdmk.eclass and freebsd.class. Their use is not limited to Gentoo/FreeBSD itself, as many packages uses ports interfaces directly in their sources, and can then be built with those eclasses on other systems (take for example [net-fs/smbfs](https://packages.gentoo.org/packages/net-fs/smbfs) that should work on Darwin, too).

### bsdmk eclass

Starting from the most generic eclass, we have bsdmk. This eclass is used to simplify the handling of packages with a build system based on "mk" files, in ports style. The mk files are files that contains predefined rules, variables and that can be included to avoid writing every time a complete makefile.

A makefile that uses a mk-like interface is much similar to a Makefile.am, as it only defines the targets, the sources, and the extra non-standard rules. It also can specify sub-directories, to use recursive make.

It's not possible to build a mk-based package using GNU make because it uses syntax extensions used by BSD-ish makes. You can find on Gentoo/Alt maintainers notes an explanation on how to handle BSD-style make and GNU make in ebuilds.

mk-based packages support passing extra options to enable/disable features, alike to configure options. This kind of options are often called "knobs", and are simply extra variables passed on the command line.

While using autotooled packages you need to pass the options to configure only inside `src_compile` function, using mk-based packages you need to pass the same knobs to both build and install commands. To simplify this, the eclass provides two functions (`mkmake` and `mkinstall`) that passes to the make command the knobs set in ${mymakeopts} variable, that should be filled during pkg\_setup referring to the USE flag the user set (or the features that must/must not be enabled for the build).

As many packages - like the source packages for FreeBSd and probably other BSD source packages - just needs to call `mkmake` to build and `mkinstall` to install the files built, bsdmk eclass also exports bsdmk\_src\_compile and bsdmk\_src\_install functions that are a quick way to prepare an ebuild for a `mk-based` package.

Sometimes you want to exclude, from a bigger source tree, a single directory or a set of directories. This is the case of Gentoo/FreeBSD sources, where the main source tarballs contains also libraries and programs that are built outside the system ebuilds directly by Portage.

To make simpler avoiding building an entire sub-directory tree, while not removing the sources (that might be included or used directly by other targets in other directories), the simplest way is to create a fake Makefile that just includes a dummy mk file. This is what `dummy_mk` function does.

The `dummy_mk` function replace the Makefile of the given sub-directory with a Makefile that incldues bsd.lib.mk mk file (that defines shared and static libraries) but does not declare any target. This way, when make will enter the directory, it will pass on without building anything.

### The freebsd eclass

freebsd.eclass contains some extra functions and variables used by Gentoo/FreeBSD system ebuilds. Their use is and should always be limited to those ebuilds, to limit the necessity of extending too much this eclass. If new functions are needed that are common to more than one \*BSD system, it must be done inside bsdmk.eclass.

The eclass exports the basic `src_compile` and `src_install` function that are used by almost every Gentoo/FreeBSD system package. They are mainly just the functions from bsdmk eclass, but they add the support for the always present NOPROFILE= knob, that is used to avoid building profiled libraries and binaries for all the system (this allows to save space on disk, as the profiled libraries and binaries are mostly used for performance evaluation and have no use for end users).

What most differentiates freebsd.eclass from bsdmk.eclass is the `src_unpack` function (missing from bsdmk) that, on freebsd eclass, takes care of many tasks common to almost all the packages.

First of all, it extract the tarballs, as there might be more than one tarball for a FreeBSD package (they usually cross-reference files, as they are designed to be compiled from a single directory). Then it patches the sources with the patches defined in ${PATCHES} variable. After that, it uses `dummy_mk` function to disable the directories listed in ${REMOVE\_SUBDIRS} variable.

The final step in unpack process is renaming the libraries. As there are some differences between the library names in FreeBSD and upstream ones, all the libraries, such as ncurses and libfl (from flex) that have different names, are being replaced inside the Makefiles.
