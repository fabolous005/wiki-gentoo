<!-- source: https://wiki.gentoo.org/wiki/Pkg-config | group: Gentoo Wiki (Main) | wiki-title: Pkg-config -->
---
title: pkg-config
url: https://wiki.gentoo.org/wiki/Pkg-config
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-05-04"
fingerprint: "27a659e977824d11"
license: CC BY-SA 4.0
---

# pkg-config

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**pkg-config** is a helper tool which is used to obtain compiler & linker flags when building a package depending on a library.

## Installation

### USE flags


| [+native-symlinks](https://packages.gentoo.org/useflags/+native-symlinks) | Install generic symlinks like pkgconf and pkg-config. If this flag is disabled, only CHOST-prefixed pkg-config executables will be available to end users and ebuilds. | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

`root #``emerge --ask dev-util/pkgconf`
## Usage

### Get build flags

pkg-config can be used to get the build flags for libraries.

Use `--cflags` to get compile flags and `--libs` to get link flags:

`user $``pkg-config --cflags glib-2.0`
-I/usr/include/glib-2.0 -I/usr/lib64/glib-2.0/include -I/usr/include/sysprof-4 -pthread

`user $``pkg-config --libs glib-2.0`
-lglib-2.0

Example of using both options to compile a program:

`user $``gcc example.c $(pkg-config --cflags --libs glib-2.0)`
### Query library versions

Use `--modversion` to query library versions:

`user $``pkg-config openssl ncurses --modversion`
3.4.4
3.0.13

## Differences from other solutions

### libtool archives (.la files)

libtool uses specific archive file format (so-called .la files) to provide additional compiler and linker flags when linking against a library.

#### Implicit vs explicit use

libtool works implicitly whenever it is used to build a library or a program. In the former case, it creates the .la file with used flags; it latter, it reads those files and uses the flags stored in them.

pkg-config needs to be used explicitly. A library has to install .pc files explicitly, and the program built against it needs to query pkg-config explicitly in its build system.

#### Static vs dynamic linking

libtool provides no distinction between dynamic and static linking. In both cases, the complete list of dependant libraries is passed to linker.

pkg-config explicitly distinguishes between public and private dependencies. This way, when using dynamic linking only actually necessary libraries are passed to linker; and when using static linking, the complete list is used.

#### Build system-agnostic

libtool archives are usually useful only when libtool is used both to build the library and the final executable. pkg-config is designed to be build system-agnostic instead.

#### No absolute paths

For a long time Gentoo suffered an issue that libtool archives contained absolute paths to dependent library archives. Effectively, whenever libraries were moved to another library directory, all libraries depending on them became broken and required rebuild. Although the issue is currently worked-around in Gentoo by replacing absolute paths with relative library names, it is still the upstream behavior of libtool.

pkg-config expresses dependencies through package names, and thus is free by design of similar issues.

### Custom *-config* applications

Many libraries provide custom applications of similar function, for example *pcap-config*. Those applications are compiled along with the library, and often have the relevant compiler & linker flags compiled into itself.

#### Cross-compilation support

Usually, custom `-config` applications are built for the specific platform the library is built for. Effectively, they are of no use when cross-compiling if the host is unable to execute code for the target platform.

pkg-config uses simple text files which are platform-independent. Thus, for cross-compilation to work it is only necessary to install pkg-config on the host system and set appropriate `PKG_CONFIG_PATH`.

## Available implementations

In the past, various implementations of pkg-config could be obtained via [virtual/pkgconfig](https://packages.gentoo.org/packages/virtual/pkgconfig). Now only [dev-util/pkgconf](https://packages.gentoo.org/packages/dev-util/pkgconf), a reimplementation in ANSI C, is available.

Previously [dev-util/pkgconfig](https://packages.gentoo.org/packages/dev-util/pkgconfig) (the official implementation, required [dev-libs/glib](https://packages.gentoo.org/packages/dev-libs/glib) causing a circular dependency), and [dev-util/pkgconfig-openbsd](https://packages.gentoo.org/packages/dev-util/pkgconfig-openbsd) (OpenBSD implementation, written in Perl) were available but were removed.[\[1\]](https://wiki.gentoo.org#cite_note-1)<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>.
