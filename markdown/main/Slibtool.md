<!-- source: https://wiki.gentoo.org/wiki/Slibtool | group: Gentoo Wiki (Main) | wiki-title: Slibtool -->
---
title: Slibtool
url: https://wiki.gentoo.org/wiki/Slibtool
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-05-29"
fingerprint: e7883e6f53e54b01
license: CC BY-SA 4.0
---

# Slibtool

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Slibtool** is a surrogate libtool implementation, written in C. It aims at being a replacement for GNU libtool.

## Installation

### Emerge

Install slibtool:

`root #``emerge --ask dev-build/slibtool`
## Usage

In order to make portage prefer slibtool over GNU libtool, add the following lines:

**`/etc/portage/make.conf`**

```
LIBTOOLIZE="slibtoolize"
LIBTOOL="rclibtool"
MAKEFLAGS="LIBTOOL=${LIBTOOL}"
MAKE="make LIBTOOL=${LIBTOOL}"
```
Alternatively, tell portage to use slibtool only for specific packages via env files, such as:

**`/etc/portage/env/slibtool.conf`**

```
LIBTOOLIZE="slibtoolize"
LIBTOOL="rclibtool"
MAKEFLAGS="LIBTOOL=${LIBTOOL}"
MAKE="make LIBTOOL=${LIBTOOL}"
```
**`/etc/portage/package.env/slibtool`**

```
       slibtool.conf
```
Or to disable slibtoolize while still using slibtool, tell portage to use GNU libtoolize only for specific packages via env files, such as:

**`/etc/portage/env/libtoolize.conf`**

```
LIBTOOLIZE=""
```
**`/etc/portage/package.env/slibtool`**

```
       libtoolize.conf
```
Or if slibtool is preferred, tell portage to use GNU libtool only for specifics packages via env files, such as:

**`/etc/portage/env/libtool.conf`**

```
LIBTOOLIZE=""
LIBTOOL=""
MAKEFLAGS=""
MAKE="make"
```
**`/etc/portage/package.env/slibtool`**

```
       libtool.conf
```
### Symlinks

Several different modes of operation can be used by using the corresponding installed symlink, by default either ***rclibtool*** or ***rdclibtool*** should be used.

- **slibtool**: Builds both with shared and static.
- **slibtoolize**: Prepare a package to use slibtool.
- **slibtool-ar**: Developer tools for common archive information and manipulation.
- **slibtool-shared**: Builds only shared.
- **slibtool-static**: Builds only static.
- **dlibtool**: Builds both with shared and static and extra debug output.
- **dlibtool-shared**: Builds only shared with extra debug output.
- **dlibtool-static**: Builds only static with extra debug output.
- **clibtool**: Builds both with shared and static and installs the libtool archive files (***.la***).
- **clibtool-shared**: Builds only shared and installs the ***.la*** files.
- **clibtool-static**: Builds only static and installs the ***.la*** files.
- **rlibtool**: Automatically determines whether to build shared or static with the build's configure process.
- **rdlibtool**: Like ***rlibtool*** with extra debug output.
- **rclibtool**: Like ***rlibtool*** and installs the ***.la*** files.
- **rdclibtool**: Like ***rlibtool*** with extra debug output and installs the ***.la*** files.

## Troubleshooting

### Undefined references

Using slibtool often exposes undefined references in projects that GNU libtool hides. The can happen because GNU libtool will silently ignore ***-no-undefined***, although even in other cases slibtool will expose undefined references that were previously hidden often with common linker flags or when doing a static build.

### Unknown arguments

Using slibtool will expose typos, bogus arguments and other similar mistakes in build systems, GNU libtool often will silently ignore many unknown arguments it receives.

### ld cannot find linker flag

When using slibtool ***ld(1)*** can fail when its unable to find a linker flag such as ***-lfoo*** which represents an internal dependency. This often happens when the build either fails to create the library with libtool meaning the libtool archive file (***.la***) is entirely missing or that the ***.la*** file exists and just needs to be added to the corresponding ***LDADD*** or ***LIBADD*** variable in the build.

In the event the library cannot be created with libtool as a workaround the build can link directly with the static library (***.a***) or less preferably pass the linker path directly to ***ld(1)*** with ***-Wl,-Lpath/to/foo*** which will prevent slibtool from transforming the linker path to ***-Lpath/to/foo/.libs***.

### flow error: unexpected condition or other

When using ***rlibtool*** slibtool will try to automatically determine if the build should be shared or static based on the corresponding configure arguments. However some builds that use ***$(LIBTOOL)*** do not necessarily use autotools or any kind of configure system and ***rlibtool*** is not able to determine what to build. In these cases ***slibtool-shared***, ***slibtool-static*** or ***slibtool*** should be used instead which build shared, static and both respectively.

### No libtool archive (.la) files installed

By default slibtool does not install the libtool archive (***.la***) files. build systems, ebuilds, programs and other tools usually should not expect them to exist on the user's system or in the ***DESTDIR*** during the installation process, but there are some cases where it does matter. They can still be installed by slibtool in those cases by using ***rclibtool***, ***rdclibtool*** or one of the ***clibtool*** symlinks.

In some cases, such as sys-devel/gcc, neglecting the ***.la*** files even at build time, will result in a badly broken installation that is missing critical support files (not ***.la*** files, the runtime support files!) so getting this right is quite important.

It is the responsibility of an ebuild to remove them after the fact in src\_install where they aren't wanted (as it must also handle traditional libtool), so there are no downsides to having slibtool do the same and allowing the ebuild to handle deletion. Just use the slibtool symlinks with a ***c*** in them, it is infinitely safer.

### path not found: ../.libs/.libs

The libtool implementation creates the ***.libs*** directories during compilation for its own internal use. The build system and downstream projects should not use this directory directly rather than indirectly through the use of ***$(LIBTOOL)***. Since GNU libtool is far more permissive it  often lets projects getting away with using either, but slibtool expects the paths without ***.libs***.

### Installing or using binaries created by libtool manually

One difference between GNU libtool and slibtool is that the former will conditionally place the compiled executable or a shell wrapper script in the build directory, depending on whether or not the executable depends on a build-local libtool library (e.g. ***libfoo.la***). Where slibtool will always place a compatible wrapper script in the build directory where GNU libtool would have conditionally placed the executable. When the wrapper script is created both GNU libtool and slibtool will place the executable in the ***.libs*** directory within the build directory. Consequently build systems, ebuilds, and other users should take care to avoid scenarios like installing the wrapper script to the system instead of the executable. In these cases ideally the executable would be installed by the same libtool implementation that compiled it.

## External resources

- [bug #765709](https://bugs.gentoo.org/show_bug.cgi?id=765709) - \[tracker\] Packages failing to build with sys-devel/slibtool (Gentoo Bugzilla)
- [Slibtool issue tracker](https://dev.midipix.org/cross/slibtool/issues)
