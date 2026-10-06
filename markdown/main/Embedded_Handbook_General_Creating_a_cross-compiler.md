<!-- source: https://wiki.gentoo.org/wiki/Embedded_Handbook/General/Creating_a_cross-compiler | group: Gentoo Wiki (Main) | wiki-title: Embedded Handbook/General/Creating a cross-compiler -->
---
title: Embedded Handbook/General/Creating a cross-compiler
url: https://wiki.gentoo.org/wiki/Embedded_Handbook/General/Creating_a_cross-compiler
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-03-19"
fingerprint: "2e8c1e3b7f843880"
license: CC BY-SA 4.0
---

# Embedded Handbook/General/Creating a cross-compiler

[Embedded Handbook](https://wiki.gentoo.org/wiki/Special:MyLanguage/Embedded_Handbook) |

[General](https://wiki.gentoo.org/wiki/Special:MyLanguage/Embedded_Handbook/General)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)




The first thing users should know about building a toolchain is that some versions of toolchain components refuse to work together. Exactly which combinations are problematic is a matter that's constantly in flux as the Gentoo ebuild repository evolves. The only reliable way to determine what works is to run crossdev, adjusting individual component versions as necessary, until crossdev completes the toolchain build successfully. Even then, the cross toolchain may build binaries which break on the target system. Only through trial, error, and patience will one arrive at a favorable combination of all factors.

Users do not have to worry about the cross-compiler interfering with the native build system. All of the toolchain packages are designed such that they are isolated from each other based on the target. This way cross-compilers can be installed for desired architecture(s) without breaking the rest of the system.

### crossdev

#### Intro

Generating a cross-compiler by hand is a long and painful process. This is why it has been fully integrated into Gentoo! A command-line front-end called [crossdev](https://wiki.gentoo.org/wiki/Crossdev) will run emerge with all of the proper environment variables and install all the right packages to generate arbitrary cross-compilers based on the need of the user.

First, install [Crossdev](https://wiki.gentoo.org/wiki/Crossdev):

`root #``emerge --ask sys-devel/crossdev`
Consider installing the unstable version of crossdev to get all the latest fixes.

Only basic usage of crossdev is covered here, but crossdev can customize the process fairly well for most needs. Run crossdev --help to get some ideas on how to use crossdev. Here are some common usage options:

`crossdev --g [gcc version] --l [(g)libc version] --b [binutils version] --k [kernel headers version] -P -v -t [tuple]``crossdev -S -P -v -t [tuple]`
#### Installing

First, go set up an overlay as described on the [crossdev page](https://wiki.gentoo.org/wiki/Crossdev#Crossdev_overlay).

Then you must select the proper tuple for the target. Here, it will be assumed that a cross-compiler for the SH4 (SuperH) processor with glibc running on Linux is desired to be built by the user. This action will be performed on a PowerPC machine. Generate a SH4 cross-compiler:

`root #``crossdev --target sh4-unknown-linux-gnu````
-----------------------------------------------------------------------------------------------------
 * Host Portage ARCH:     ppc
 * Target Portage ARCH:   sh
 * Target System:         sh4-unknown-linux-gnu
 * Stage:                 4 (C/C++ compiler)
 * binutils:              binutils-[latest]
 * gcc:                   gcc-[latest]
 * headers:               linux-headers-[latest]
 * libc:                  glibc-[latest]
 * PORTDIR_OVERLAY:       /var/db/repos/local
 * PORT_LOGDIR:           /var/log/portage
 * PKGDIR:                /usr/portage/packages/powerpc-unknown-linux-gnu/cross/sh4-unknown-linux-gnu
 * PORTAGE_TMPDIR:        /var/tmp/cross/sh4-unknown-linux-gnu
  _  -  ~  -  _  -  ~  -  _  -  ~  -  _  -  ~  -  _  -  ~  -  _  -  ~  -  _  -  ~  -  _  -  ~  -  _  
 * Forcing the latest versions of {binutils,gcc}-config/gnuconfig ...                          [ ok ]
 * Log: /var/log/portage/cross-sh4-unknown-linux-gnu-binutils.log
 * Emerging cross-binutils ...                                                                 [ ok ]
 * Log: /var/log/portage/cross-sh4-unknown-linux-gnu-gcc-stage1.log
 * Emerging cross-gcc-stage1 ...                                                               [ ok ]
 * Log: /var/log/portage/cross-sh4-unknown-linux-gnu-linux-headers.log
 * Emerging cross-linux-headers ...                                                            [ ok ]
 * Log: /var/log/portage/cross-sh4-unknown-linux-gnu-glibc.log
 * Emerging cross-glibc ...                                                                    [ ok ]
 * Log: /var/log/portage/cross-sh4-unknown-linux-gnu-gcc-stage2.log
 * Emerging cross-gcc-stage2 ...                                                               [ ok ]
```
#### Quick test

If everything goes as planned, a shiny new compiler should now be present on the machine. Give it a spin!

Use the SH4 cross-compiler:

`user $``sh4-unknown-linux-gnu-gcc --version`
sh4-unknown-linux-gnu-gcc (GCC) 4.2.0 (Gentoo 4.2.0 p1.4)
Copyright (C) 2007 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

`user $````
echo 'int main(){return 0;}' > sh4-test.c
```
`user $````
sh4-unknown-linux-gnu-gcc -Wall sh4-test.c -o sh4-test
```
`user $``file sh4-test`
sh4-test: ELF 32-bit LSB executable, Renesas SH, version 1 (SYSV), for GNU/Linux 2.6.9, dynamically linked (uses shared libs), not stripped

If the crossdev command failed, the log file may be reviewed to see if the problem is local. If unable to fix the issue, a bug may be [filed in Bugzilla](https://wiki.gentoo.org/wiki/Bugzilla/Bug_report_guide).

#### Tuples

To find out which tuple should be used, look over the output from the following command:

`root #``crossdev -t help`
There should now be a newly compiled cross-compiler in the sysroot at /usr/${CTARGET}/. It's a good idea to create pre-built binary packages so as to not end up waiting another two to three hours every time this toolchain should be reinstalled.

#### Create binpkgs

`root #````
quickpkg --include-unmodified-config=y cross-sh4-unknown-linux-gnu/gcc
```
`root #````
quickpkg --include-unmodified-config=y cross-sh4-unknown-linux-gnu/glibc
```
`root #````
quickpkg --include-unmodified-config=y cross-sh4-unknown-linux-gnu/binutils
```
`root #````
quickpkg --include-unmodified-config=y cross-sh4-unknown-linux-gnu/linux-headers
```
If the quickpkg command warns about excluded files, please follow its prompts to include all files.

In the future the sysroot can be reinstalled by executing the following simple Portage command:

`root #``emerge -k cross-sh4-unknown-linux-gnu/gcc cross-sh4-unknown-linux-gnu/glibc cross-sh4-unknown-linux-gnu/binutils cross-sh4-unknown-linux-gnu/linux-headers`
### Uninstalling

To uninstall a toolchain, simply use the `--clean` option. If the sysroot was modified by hand, there will be a prompt to delete every file inside, so it is possible to prepend yes |  to this command *if* there is no doubt about what can be deleted:

Uninstall the SH4 cross-compiler:

`root #``crossdev --clean sh4-unknown-linux-gnu`
Deleting any and all files in the /usr/${CTARGET}/ directory should be completely safe.

### Cross-compiler internals

#### Overview

There are generally two ways to build a cross-compiler. The "accepted" way, and the cheater's shortcut.

The current "accepted" way is:

1. binutils
2. kernel headers
3. libc headers
4. gcc stage1 (c-only)
5. libc
6. gcc stage2 (c/c++/etc...)

The cheater's shortcut is:

1. binutils
2. kernel headers
3. gcc stage1 (c-only)
4. libc
5. gcc stage2 (c/c++/etc...)

The reason people are keen on the shortcut is that the libc headers step tends to take quite a while, especially on slower machines. It can also be kind of a pain to setup kernel/libc headers without a usable cross compiler. Note, though, that if help with cross-compilers is sought, upstream projects will not want to help if the shortcut was taken.

Also note that the shortcut requires the gcc stage1 to be "crippled". Since building without headers, the sysroot option cannot be enabled nor proper gcc libs can be built. This is okay if the only thing that is being used in the stage1 is building the C library and a kernel, but beyond that, a nice sysroot-based compiler is needed.

The "accepted" way is described below as the steps are pretty much the same. Some extra patches are needed for gcc in order to take the shortcut.

#### Sysroot

The cross-compiling will be done using the sysroot method. But what does the sysroot do?

The sysroot tells GCC to consider dir as the root of a tree that contains (a subset of) the root filesystem of the target operating system. Target system headers, libraries and run-time object files will be searched in there.

The top level directory is commonly rooted in /usr/$CTARGET

As can be seen, it's just like the directory setup in / but in /usr/$CTARGET. This setup is of course not an accident but designed on purpose so applications/libraries can be easily migrated out of /usr/$CTARGET and into / on the target board. If desired, /usr/$CTARGET could be used as a quick NFS root!

#### Binutils

Grab the latest binutils tarball and unpack it.

The `--disable-werror` option is to prevent binutils from aborting the compile due to warnings. Great feature for developers, but a pain for users. Configure and build binutils:

`root #````
make
```
`root #````
make install DESTDIR=$PWD/install-root
```
The reason of install into the localdir is the crap that doesn't belong can be removed. For example, a normal install will give /usr/lib/libiberty.a which doesn't belong in the host /usr/lib. So clean out stuff first:

`root #``rm -rf install-root/usr/{info,lib,man,share}` And install what's left:

`root #``cp -a install-root/* /`
#### Kernel headers

Grab the latest Linux tarball and unpack it. There are two ways of installing the kernel headers: sanitized and unsanitized. The former option is generally better, but requires a recent version of the Linux kernel. Both options will be covered here.

Build and install the unsanitized headers, replacing `$ARCH` with the target architecture as found in the arch folder:

`root #``yes "" | make ARCH=$ARCH oldconfig prepare``root #````
mkdir -p /usr/$CTARGET/usr/include
```
`root #````
cp -a include/linux include/asm-generic /usr/$CTARGET/usr/include/
```
`root #````
cp -a include/asm-$ARCH /usr/$CTARGET/usr/include/asm
```
Build and install the sanitized headers:

`root #``make ARCH=$ARCH headers_install INSTALL_HDR_PATH=/usr/$CTARGET/usr`
#### System libc headers

Grab the latest glibc tarball and unpack it. Glibc is picky, so compilation have to be done in a directory separate from the source code. Build and install the glibc headers:

`root #````
mkdir build
```
`root #````
cd build
```
`root #````
../configure --host=$CTARGET --prefix=/usr --with-headers=/usr/$CTARGET/usr/include --without-cvs --disable-sanity-checks
```
`root #``make -k install-headers install_root=/usr/$CTARGET`
glibc can be awkward sometimes, so have to do a few things by hand:

`root #````
mkdir -p /usr/$CTARGET/usr/include/gnu
```
`root #````
touch /usr/$CTARGET/usr/include/gnu/stubs.h
```
`root #````
cp bits/stdio_lim.h /usr/$CTARGET/usr/include/bits/
```
#### GCC stage 1 (C only)

At first, help gcc find the current libc headers:

`root #``ln -s usr/include /usr/$CTARGET/sys-include`
Then grab the latest gcc tarball and unpack it:

`root #````
mkdir build
```
`root #````
cd build
```
`root #````
make
```
`root #````
make install DESTDIR=$PWD/install-root
```
Same as binutils, gcc leaves some stuff behind that doesn't wanted. Clean the gcc stage 1:

`user $``rm -rf install-root/usr/{info,include,lib/libiberty.a,man,share}` #### System libc

Remove the old glibc build directory and recreate it:

`root #````
rm -rf build
```
`root #````
mkdir build
```
`root #````
cd build
```
`root #````
../configure --host=$CTARGET --prefix=/usr --without-cvs
```
`root #````
make
```
`root #``make install install_root=/usr/$CTARGET`
#### GCC stage 2 (all frontends)

A full GCC can be built up now. Whichever compiler frontends are preferred can be selected; C/C++ will just be done for simplicity. Build and install the gcc stage 2:

`root #````
./configure --target=$CTARGET --prefix=/usr --with-sysroot=/usr/$CTARGET --enable-languages=c,c++ --enable-shared --disable-checking --disable-werror
```
`root #````
make
```
`root #``make install` #### Core runtime files

There are many random core runtime files that people wonder what they may be for. Let's explain:

| Files provided by [sys-libs/glibc](https://packages.gentoo.org/packages/sys-libs/glibc) |  | 
|---|---|
| File | Purpose | 
| crt0.o | Older style of the initial runtime code. No one generates this anymore. | 
| crt1.o | Newer style of the initial runtime code. Contains the \_start symbol which sets up the env with argc/argv/libc \_init/libc \_fini before jumping to the libc main. glibc calls this file 'start.S'. | 
| crti.o | Defines the function prolog; \_init in the .init section and \_fini in the .fini section. glibc calls this 'initfini.c'. | 
| crtn.o | Defines the function epilog. glibc calls this 'initfini.c'. | 
| Scrt1.o | Used in place of crt1.o when generating PIEs. | 
| gcrt1.o | Used in place of crt1.o when generating code with profiling information. Compile with -pg. Produces output suitable for the gprof util. | 
| Mcrt1.o | Like gcrt1.o, but is used with the prof utility. glibc installs this as a dummy file as it's useless on linux systems. | 

| Files provided by [sys-devel/gcc](https://packages.gentoo.org/packages/sys-devel/gcc) |  | 
|---|---|
| File | Purpose | 
| crtbegin.o | GCC uses this to find the start of the constructors. | 
| crtbeginS.o | Used in place of crtbegin.o when generating shared objects/PIEs. | 
| crtbeginT.o | Used in place of crtbegin.o when generating static executables. | 
| crtend.o | GCC uses this to find the start of the destructors. | 
| crtendS.o | Used in place of crtend.o when generating shared objects/PIEs. | 

The general linking order:

`crt1.o crti.o crtbegin.o [-L paths] [user objects] [gcc libs] [C libs] [gcc libs] crtend.o crtn.o`

## External resources
