<!-- source: https://wiki.gentoo.org/wiki/Embedded_Handbook/General/Full | group: Gentoo Wiki (Main) | wiki-title: Embedded Handbook/General/Full -->
---
title: Embedded Handbook/General/Full
url: https://wiki.gentoo.org/wiki/Embedded_Handbook/General/Full
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-01-10"
fingerprint: "5e001a1e4dc719a4"
license: CC BY-SA 4.0
---

# Embedded Handbook/General/Full

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


\<translate>



## Introduction

Cross development has traditionally been a black art, requiring a lot of research, trial and error, and perseverance. Intrepid developers face a shortage of documentation and the lack of mature, comprehensive open source toolkits for multi-platform cross development. Ongoing work by the [Embedded](https://wiki.gentoo.org/wiki/Project:Embedded) or [Toolchain](https://wiki.gentoo.org/wiki/Project:Toolchain) projects, and other contributors is yielding a Gentoo-based development platform that greatly simplifies cross development.

### The toolchain

The term "toolchain" refers to the collection of packages used to build up a system (the "tools" which are used in the "chain" of events to take some input and produce some output). It is a loose definition in terms of what packages exactly are considered part of the toolchain, but for the sake of keeping things simple, we will consider the components that are needed to compile code into something fun and usable.

Your typical toolchain is therefore composed of the following:

- [sys-devel/binutils](https://packages.gentoo.org/packages/sys-devel/binutils)
- Essential utilities for handling binaries (includes assembler and linker).
- [sys-devel/gcc](https://packages.gentoo.org/packages/sys-devel/gcc)
- The GNU Compiler Collection (the C and C++ compiler).
- [sys-libs/glibc](https://packages.gentoo.org/packages/sys-libs/glibc), [sys-libs/uclibc-ng](https://packages.gentoo.org/packages/sys-libs/uclibc-ng), or [sys-libs/newlib](https://packages.gentoo.org/packages/sys-libs/newlib)
- The system C library.
- [sys-kernel/linux-headers](https://packages.gentoo.org/packages/sys-kernel/linux-headers)
- Kernel headers needed by the system C library.
- [sys-devel/gdb](https://packages.gentoo.org/packages/sys-devel/gdb)
- The GNU debugger.

All proper Gentoo systems have a toolchain installed as part of the base system. This toolchain is configured to build binaries native to its host platform.

In order to build binaries on the host system for a non-native platform you'll need a special toolchain - a so-called cross toolchain - which can target that particular platform. Gentoo provides a simple but powerful tool called crossdev for this purpose. Crossdev can build and install arbitrary GCC-supported cross toolchains on the host system, and because Gentoo installs toolchain files into target-specific directories the toolchains built by crossdev will not interfere with the host's native toolchain.

### Toolchain tuples

All toolchains have a prefix (think `CHOST`). More details on that can be found in the [system tuples article](https://wiki.gentoo.org/wiki/Embedded_Handbook/Tuples).

### Environment variables

Certain environment variables used by the Gentoo toolchain and Portage can thoroughly confuse developers inexperienced with cross development. The following table explains some tricky variables and provides sample values based on the cross development examples presented in this guide. See *[More terminology and variables](https://wiki.gentoo.org#More_terminology_and_variables)* (below) for more unusual variables and related concepts.

| Variable name | Meaning when building cross-toolchain | Meaning when building cross-binaries | 
|---|---|---|
| `CBUILD` | Platform you are building on | Platform you are building on | 
| `CHOST` | Platform the cross-toolchain will run on | Platform the binaries built by cross-toolchain will run on | 
| `CTARGET` | Platform the binaries built by cross-toolchain will run on | Platform the binaries built by cross-toolchain will run on. Redundant, but there's no harm in setting this, and a few binaries do like it. | 
| `ROOT` | Path to the virtual root (/) you are installing into |  | 
| `PORTAGE_CONFIGROOT` | Path to the virtual root (/) Portage can find its config files (like /etc/make.conf) |  | 

Say we have an **AMD64** desktop as our normal Gentoo machine and we have an **ARM** PDA we wanted to develop for, the above table would look like:

| Variable name | Value for building cross-toolchain | Value for building cross-binaries | 
|---|---|---|
| `CBUILD` | `x86_64-pc-linux-gnu` | `x86_64-pc-linux-gnu` | 
| `CHOST` | `x86_64-pc-linux-gnu` | `arm-unknown-linux-gnu` | 
| `CTARGET` | `arm-unknown-linux-gnu` | Not set. | 
| `ROOT` | Not set - defaults to `/` | `/path/where/you/install` | 
| `PORTAGE_CONFIGROOT` | Not set - defaults to `/` | `/path/where/your/portage/env/for/arm/pda/is` | 

### More terminology and variables

- canadian cross
- The process of building a cross-compiler which will run on a different machine from the one it was compiled on (CBUILD != CHOST && CHOST != CTARGET)
- sysroot
- The system root is where all the cross-compiler libraries and headers are installed. In other words, every library and header the cross-compiler needs to generate cross-compiled binaries are put into this directory. In theory, this means the toolchain is good enough. In practice, the cross-compiler often wants some of a packages's library dependencies, such as ncurses, installed into sysroot first.
- hardfloat
- The system has a hardware Floating Point Unit (FPU) to handle floating point math
- softfloat
- The system lacks a hardware FPU so all floating point operations are approximated with fixed point math
- PIE
- Position Independent Executable (`-fPIE -pie`)
- PIC
- Position Independent Code (`-fPIC`)
- CRT
- C run time


\</translate>

\<translate>

This page explains how to use [QEMU](https://wiki.gentoo.org/wiki/QEMU) to chroot into a system that targets a different architecture (e.g. **aarch64**) than the one being used (e.g. **amd64**). While [cross compiling](https://wiki.gentoo.org/wiki/Crossdev) enables building *binaries* for different architectures, some packages need to run some of those binaries as part of the build process. This can be done by using QEMU to emulate the target architecture, and [chroot](https://wiki.gentoo.org/wiki/Chroot) into it to simulate running that system natively.

## Installation

### Kernel

The build system's kernel must support miscellaneous binary formats. This can be enabled with `CONFIG_BINFMT_MISC=m` or `CONFIG_BINFMT_MISC=y` in the kernel's .config file.

**Enable CONFIG\_BINFMT\_MISC**

### USE Flags


| [+aio](https://packages.gentoo.org/useflags/+aio) | Enables support for Linux's Async IO | 
| [+curl](https://packages.gentoo.org/useflags/+curl) | Support ISOs / -cdrom directives via HTTP or HTTPS. | 
| [+doc](https://packages.gentoo.org/useflags/+doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [+fdt](https://packages.gentoo.org/useflags/+fdt) | Enables firmware device tree support | 
| [+filecaps](https://packages.gentoo.org/useflags/+filecaps) | Use Linux file capabilities to control privilege rather than set\*id (this is orthogonal to USE=caps which uses capabilities at runtime e.g. libcap) | 
| [+gnutls](https://packages.gentoo.org/useflags/+gnutls) | Enable TLS support for the VNC console server. For 1.4 and newer this also enables WebSocket support. For 2.0 through 2.3 also enables disk quorum support. | 
| [+jpeg](https://packages.gentoo.org/useflags/+jpeg) | Enable jpeg image support for the VNC console server | 
| [+oss](https://packages.gentoo.org/useflags/+oss) | Add support for OSS (Open Sound System) | 
| [+pin-upstream-blobs](https://packages.gentoo.org/useflags/+pin-upstream-blobs) | Pin the versions of BIOS firmware to the version included in the upstream release. This is needed to sanely support migration/suspend/resume/snapshotting/etc... of instances. When the blobs are different, random corruption/bugs/crashes/etc... may be observed. | 
| [+png](https://packages.gentoo.org/useflags/+png) | Enable png image support for the VNC console server | 
| [+seccomp](https://packages.gentoo.org/useflags/+seccomp) | Enable seccomp (secure computing mode) to perform system call filtering at runtime to increase security of programs | 
| [+slirp](https://packages.gentoo.org/useflags/+slirp) | Enable TCP/IP in hypervisor via net-libs/libslirp | 
| [+vhost-net](https://packages.gentoo.org/useflags/+vhost-net) | Enable accelerated networking using vhost-net, see https://www.linux-kvm.org/page/VhostNet | 
| [+vnc](https://packages.gentoo.org/useflags/+vnc) | Enable VNC (remote desktop viewer) support | 
| [X](https://packages.gentoo.org/useflags/X) | Add support for X11 | 
| [accessibility](https://packages.gentoo.org/useflags/accessibility) | Adds support for braille displays using brltty | 
| [alsa](https://packages.gentoo.org/useflags/alsa) | Enable alsa output for sound emulation | 
| [bpf](https://packages.gentoo.org/useflags/bpf) | Enable eBPF support for RSS implementation. | 
| [bzip2](https://packages.gentoo.org/useflags/bzip2) | Enable bzip2 compression support | 
| [capstone](https://packages.gentoo.org/useflags/capstone) | Enable disassembly support with dev-libs/capstone | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [fuse](https://packages.gentoo.org/useflags/fuse) | Enables FUSE block device export | 
| [glusterfs](https://packages.gentoo.org/useflags/glusterfs) | Enables GlusterFS cluster fileystem via sys-cluster/glusterfs | 
| [gtk](https://packages.gentoo.org/useflags/gtk) | Add support for x11-libs/gtk+ (The GIMP Toolkit) | 
| [infiniband](https://packages.gentoo.org/useflags/infiniband) | Enable Infiniband RDMA transport support | 
| [io-uring](https://packages.gentoo.org/useflags/io-uring) | Enable the use of io\_uring for efficient asynchronous IO and system requests | 
| [iscsi](https://packages.gentoo.org/useflags/iscsi) | Enable direct iSCSI support via net-libs/libiscsi instead of indirectly via the Linux block layer that sys-block/open-iscsi does. | 
| [jack](https://packages.gentoo.org/useflags/jack) | Add support for the JACK Audio Connection Kit | 
| [jemalloc](https://packages.gentoo.org/useflags/jemalloc) | Use dev-libs/jemalloc for memory management | 
| [keyutils](https://packages.gentoo.org/useflags/keyutils) | Support Linux keyrings via sys-apps/keyutils | 
| [lzo](https://packages.gentoo.org/useflags/lzo) | Enable support for lzo compression | 
| [multipath](https://packages.gentoo.org/useflags/multipath) | Enable multipath persistent reservation passthrough via sys-fs/multipath-tools. | 
| [ncurses](https://packages.gentoo.org/useflags/ncurses) | Enable the ncurses-based console | 
| [nfs](https://packages.gentoo.org/useflags/nfs) | Enable NFS support | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [numa](https://packages.gentoo.org/useflags/numa) | Enable NUMA support | 
| [opengl](https://packages.gentoo.org/useflags/opengl) | Add support for OpenGL (3D graphics) | 
| [pam](https://packages.gentoo.org/useflags/pam) | Add support for PAM (Pluggable Authentication Modules) - DANGEROUS to arbitrarily flip | 
| [passt](https://packages.gentoo.org/useflags/passt) | Enable TCP/IP in hypervisor via net-misc/passt | 
| [pipewire](https://packages.gentoo.org/useflags/pipewire) | Enable pipewire output for sound emulation | 
| [plugins](https://packages.gentoo.org/useflags/plugins) | Enable qemu plugin API via shared library loading. | 
| [pulseaudio](https://packages.gentoo.org/useflags/pulseaudio) | Enable pulseaudio output for sound emulation | 
| [python](https://packages.gentoo.org/useflags/python) | Add optional support/bindings for the Python language | 
| [rbd](https://packages.gentoo.org/useflags/rbd) | Enable rados block device backend support, see https://docs.ceph.com/en/mimic/rbd/qemu-rbd/ | 
| [sasl](https://packages.gentoo.org/useflags/sasl) | Add support for the Simple Authentication and Security Layer | 
| [sdl](https://packages.gentoo.org/useflags/sdl) | Enable the SDL-based console | 
| [sdl-image](https://packages.gentoo.org/useflags/sdl-image) | SDL Image support for icons | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [smartcard](https://packages.gentoo.org/useflags/smartcard) | Enable smartcard support | 
| [snappy](https://packages.gentoo.org/useflags/snappy) | Enable support for Snappy compression (as implemented in app-arch/snappy) | 
| [spice](https://packages.gentoo.org/useflags/spice) | Enable Spice protocol support via app-emulation/spice | 
| [ssh](https://packages.gentoo.org/useflags/ssh) | Enable SSH based block device support via net-libs/libssh2 | 
| [static-user](https://packages.gentoo.org/useflags/static-user) | Build the User targets as static binaries | 
| [systemtap](https://packages.gentoo.org/useflags/systemtap) | Enable SystemTap/DTrace tracing | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [udev](https://packages.gentoo.org/useflags/udev) | Enable virtual/udev integration (device discovery, power and storage device support, etc) | 
| [usb](https://packages.gentoo.org/useflags/usb) | Enable USB passthrough via dev-libs/libusb | 
| [usbredir](https://packages.gentoo.org/useflags/usbredir) | Use sys-apps/usbredir to redirect USB devices to another machine over TCP | 
| [valgrind](https://packages.gentoo.org/useflags/valgrind) | Enable annotations for accuracy. May slow down runtime slightly. Safe to use even if not currently using dev-debug/valgrind | 
| [vde](https://packages.gentoo.org/useflags/vde) | Enable VDE-based networking | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 
| [virgl](https://packages.gentoo.org/useflags/virgl) | Enable experimental Virgil 3d (virtual software GPU) | 
| [virtfs](https://packages.gentoo.org/useflags/virtfs) | Enable VirtFS via virtio-9p-pci / fsdev. See https://wiki.qemu.org/Documentation/9psetup | 
| [vte](https://packages.gentoo.org/useflags/vte) | Enable terminal support (x11-libs/vte) in the GTK+ interface | 
| [wayland](https://packages.gentoo.org/useflags/wayland) | Enable dev-libs/wayland backend | 
| [xattr](https://packages.gentoo.org/useflags/xattr) | Add support for getting and setting POSIX extended attributes, through sys-apps/attr. Requisite for the virtfs backend. | 
| [xdp](https://packages.gentoo.org/useflags/xdp) | Enable support for XDP through net-libs/xdp-tools | 
| [xen](https://packages.gentoo.org/useflags/xen) | Enables support for Xen backends | 
| [zstd](https://packages.gentoo.org/useflags/zstd) | Enable support for ZSTD compression | 

**`/etc/portage/package.use/qemu`**

**Enable`aarch64` user target and `static-libs` in supporting libraries.**

#### QEMU target configuration

By default, [app-emulation/qemu](https://packages.gentoo.org/packages/app-emulation/qemu) does not define any `QEMU_SOFTMMU_TARGETS` or `QEMU_USER_TARGETS`. The example configuration above only includes `aarch64` targets.

To build all targets:

**`/etc/portage/package.use/qemu`**

**Configure QEMU to build all targets.**

### Emerge

`root #``emerge --ask --update --newuse --deep app-emulation/qemu`
## Configuration

### Group configuration

For non-root users to use QEMU, they must be added to the **kvm** group.

To add *larry* to the **kvm** group:

`root #``gpasswd -a larry kvm`
### OpenRC

To start the qemu-binfmt service:

`root #``rc-service qemu-binfmt start`
It may be wise for the services to be started by default on boot:

`root #``rc-update add qemu-binfmt default`
### systemd

The systemd-binfmt service must be configured by adding files containing the desired handler registration strings to /etc/binfmt.d/.

Modern versions of qemu ship a binfmt configuration file that supports all binary formats. Simply link it to /etc for binary format support, then skip the following manual file creation steps.

`root #``ln -s /usr/share/qemu/binfmt.d/qemu.conf /etc/binfmt.d/qemu.conf``root #``systemctl restart systemd-binfmt`
To confirm the service is running properly after restarting:

`root #``systemctl status systemd-binfmt`
## Usage

### Preparing the chroot

To be able to chroot into a system of a different platform (e.g. **aarch64** while using an **amd64** system), mounted in e.g. /mnt/gentoo, the QEMU static-user binary must be copied into the environment.

This file is named `qemu-` under /usr/bin/ and should be copied to usr/bin/ within the build environment.
**\<architecture>**

To use an **aarch64** chroot environment at /mnt/gentoo-**aarch64**:

`user $``cp /usr/bin/qemu-aarch64 /mnt/gentoo-aarch64/usr/bin`
### Chrooting

Once the environment has been prepared, [sys-apps/arch-chroot](https://packages.gentoo.org/packages/sys-apps/arch-chroot) can be used:

`root #``arch-chroot /mnt/gentoo-aarch64`
#### Portage configuration

Currently qemu-user does not support several of Portage's sandboxes, like **pid-sandbox** ([bug #703278](https://bugs.gentoo.org/show_bug.cgi?id=703278)) and **network-sandbox** ([bug #703276](https://bugs.gentoo.org/show_bug.cgi?id=703276)) Portage *features*.

To disable these features:

**`/etc/portage/make.conf`**

```
FEATURES="${FEATURES} -ipc-sandbox -pid-sandbox -network-sandbox"
```
#### Glibc

New glibc versions tends to lead to trouble with qemu-user especially for the unstable only arches like MIPS.
Because of this it would be wise to follow [Project:RelEng](https://wiki.gentoo.org/wiki/Project:RelEng)'s advice and mask the newest version until it has been more tested. i.e. if the latest version of glibc is 2.44 then the chroot should mask any version above 2.43.

**`/etc/portage/package.mask/glibc`**

**example if glibc-2.44 is the latest version**

```
# new glibc tends to lead to trouble with sandbox (and qemu),
# especially for the unstable arches
#
>=sys-libs/glibc-2.44
```
This will need to be manually updated with every new version becoming stable, so setting up an RSS reader to check for changes on the [Project:RelEng](https://wiki.gentoo.org/wiki/Project:RelEng) file [https://github.com/gentoo/releng/commits/master/releases/portage/stages-qemu/package.mask/releng/glibc.atom](https://github.com/gentoo/releng/commits/master/releases/portage/stages-qemu/package.mask/releng/glibc.atom) or using GitHub's internal tools for email notifications, would be a simple but effective way to keep track of when it is safe to switch while keeping the system following an actively developed toolchain.

More adventurous users are welcome to ignore the above at the risk of breaking the chroot and if applicable, the system receiving the binary packages. If a suspected bug is found after ruling out user error and checking [bugs.gentoo.org (BGO)](https://bugs.gentoo.org) then it would be helpful to Gentoo to send a copy of the failed build log. the chroot's emerge --info and a brief description of the issue while mentioning the last known version that worked to [#gentoo-toolchain](ircs://irc.libera.chat/#gentoo-toolchain) ([webchat](https://web.libera.chat/#gentoo-toolchain)). Do please remember while no one is going to tell you off for making a genuine mistake. It is expected etiquette in return that you wait for the current discussion to finish, keep to the issue at hand and if possible hang around in the room as long as possible encase more information is required and bug report is required to be submitted.

## Additional Usage

- [Binfmt\_misc#Binary\_format\_handlers](https://wiki.gentoo.org/wiki/Binfmt_misc#Binary_format_handlers) - a list of possible handlers
-  [Manual setup](https://wiki.gentoo.org/wiki/Binfmt_misc#Manually) - how to manually register an interpreter for a specific handler

### Advanced Tips

Sometimes we'll need to pass additional args to QEMU (CPU model):

#### QEMU wrapper

We can create a wrapper script (in C) that'll call QEMU with it:

**`qemu-wrapper.c`**

```
/*
 * Pass arguments to qemu binary
 */
#include <string.h>
#include <unistd.h>
int main(int argc, char **argv, char **envp) {
	char *newargv[argc + 3];
	newargv[0] = argv[0];
	newargv[1] = "-cpu";
	newargv[2] = "cortex-a8"; /* here you can set the cpu you are building for */
	memcpy(&newargv[3], &argv[1], sizeof(*argv) * (argc -1));
	newargv[argc + 2] = NULL;
	return execve("/usr/bin/qemu-arm", newargv, envp);
}
```
Compile the wrapper with:

`root #``gcc -static qemu-wrapper.c -O3 -s -o qemu-wrapper`
Then copy into the chroot. Notice the first example ARM entry in the binfmt\_misc section uses this method.

#### QEMU env vars

QEMU can also be controlled trough env vars. For this to work the vars need to be present in the (sub-)environments where the commands are executed.

Example: Setting `QEMU_CPU` for [vec\_add.c](https://ftp.cvut.cz/kernel/people/geoff/cell/ps3-linux-docs/CellProgrammingTutorial/src/example2_1/vec_add.c):

`user $``./vec_add`
qemu: uncaught target signal 4 (Illegal instruction) - core dumped
Illegal instruction        ./vec\_add

`user $``QEMU_CPU=7450 ./vec_add`
c\[0\]=3, c\[1\]=7, c\[2\]=11, c\[3\]=15

When using `podman`/`docker` this can be done by via `-e QEMU_CPU=${QEMU_CPU}`.

See also [QEMU/User#QEMU\_CPU](https://wiki.gentoo.org/wiki/QEMU/User#QEMU_CPU)

## References

\</translate>

\<translate>



## Creating a cross-compiler

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


\</translate>

## Cross-compiling with Portage

For more information on the variables used in this section, please refer to the [Introduction](https://wiki.gentoo.org/wiki/Embedded_Handbook/General/Introduction).

### Filesystem setup

Cross-compilation with Portage generally involves two directory structures; the `SYSROOT` and the virtual `ROOT`.

The sysroot is the path where the cross-compiler finds its libraries. By default, /usr/${CTARGET}/ is used as the sysroot. Another directory could be used with custom `-I`/`-L` added to `CPPFLAGS` and `LDFLAGS`; however, this has historically proven to be problematic and is discouraged for practical use.

The virtual root is defined by the `ROOT` variable. This variable can be exported or set on the emerge command line using the `--root=` option. In the Embedded Handbook it is assumed that `ROOTSYSROOT` is being used as the development `ROOT`, which will facilitate the installation of packages later on.

### Emerge wrappers

In the crossdev package are included simple scripts that setup the environment variables to point to the appropriate locations, enabling cross-compilation using emerge. As is the norm, `PORTAGE_CONFIGROOT` and `ROOT` are here assumed to both point to `SYSROOT`.

`root #``emerge-wrapper --target ${CTARGET} --init`
This command will configure the environment aswell as the ${SYSROOT}/etc/ directory, and enables the use of `cross-emerge`.

For example, if an armv4tl-softfloat-linux-gnueabi toolchain has been merged via crossdev, emerge can now be invoked as:

`root #``armv4tl-softfloat-linux-gnueabi-emerge pkg0 pkg1 pkg2`
These tools can be used for both installing into the sysroot and into the virtual root. For the latter, the `--root` option should be specified.

By default these wrappers use the `--root-deps=rdeps` option to avoid the host dependencies from being pulled into the deptree. This can lead to incomplete deptrees. Therefore the `--root-deps` option can be used alone to see the full dependency graph.

### Portage configuration

By default crossdev will link to the generic embedded profile. This is done to simplify things, but the user may wish to use a more advanced targeted profile. The profile symlink can be updated in order to do that.

`root #``ln -s /var/db/repos/gentoo/profiles/default/linux/arm/17.0 ${SYSROOT}/etc/portage/make.profile`
To change settings (such as USE flags) for the target system, edit the standard Portage config files:

`root #``${EDITOR} ${SYSROOT}/etc/portage/make.conf`
This file is generated by the wrapper and should be inspected before starting the installation process. In general, the sysroot portage is configured just the same as an ordinary system.

#### Overriding package tests

Sometimes there are additional, cross-compile-incompatible tests that must be overridden for configure scripts. To make this possible, crossdev exports a few variables to force the test to get the answer it should receive. This helps prevent bloat in packages which add local functions to workaround issues it assumes the **target** system has because it could not run the test while cross-compiling (because, of course, it's not running on the target system). From time-to-time additional variables may need to be added to these files in /usr/share/crossdev/include/site/ in order to get a package to compile. To figure out the variable that needs to be added, it's often as simple as grepping the configure script for the autoconf variable (which commonly begins with `ac_`) and adding it to the appropriate target file. This becomes easier with ebuild development experience.

### Package installation

The standard procedure for installing a package to the intended `ROOT` is to first build it in the `SYSROOT`, and then emerge the binary package to `ROOT`. This method has the benefit that unneeded build-time dependencies will not clutter the `ROOT`. By default, `FEATURES`=buildpkg is enabled in $SYSROOT/etc/portage/make.conf, meaning binary packages will be built automatically for every package.

Here is an example showing an installation of postfix:

`root #``arm-unknown-linux-gnu-emerge --ask mail-mta/postfix`
These are the packages that would be merged, in order:
\[ebuild  N     \] dev-db/lmdb-0.9.35 to /usr/arm-unknown-linux-gnu/ USE="-static-libs"
\[ebuild  N     \] dev-util/pkgconf-2.5.1 to /usr/arm-unknown-linux-gnu/ USE="native-symlinks -test"
\[ebuild  N     \] virtual/pkgconfig-3 to /usr/arm-unknown-linux-gnu/ USE="native-symlinks"
\[ebuild  N     \] mail-mta/postfix-3.11.1-r1 to /usr/arm-unknown-linux-gnu/ USE="berkdb eai lmdb -cdb -dovecot-sasl -ldap -ldap-bind -mbox -memcached -mongodb -mysql -nis -pam -postgres -sasl (-selinux)
-sqlite -ssl -tlsrpt -verify-sig"

When the build is finished, the binary package is then installed to `ROOT`:

`root #``arm-unknown-linux-gnu-emerge --ask --oneshot --usepkgonly --root=/mnt/root`
These are the packages that would be merged, in order:
\[binary  N    \] postfix-3.11.1-r1 to /mnt/root

To exclude certain files/directories from being installed, the `INSTALL_MASK` portage variable can be configured; see man.5 make.conf. By default all documentation, such as man and info pages, is excluded from the install.

### Uninstall

When uninstalling the toolchain, the sysroot tree can be safely removed without affecting any native packages. Refer to the "Uninstalling" section in the [crossdev guide](https://wiki.gentoo.org/wiki/Embedded_Handbook/General/Creating_a_cross-compiler#Uninstalling).

### See also

- [Binary package guide](https://wiki.gentoo.org/wiki/Binary_package_guide) — in-depth **binary package** creation, distribution, use, and maintenance

## Cross-compiling the kernel

### Sources

First, the appropriate kernel sources must be obtained.

Run emerge [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources) or fetch them from [kernel.org](https://www.kernel.org/).

### Setup

This section describes the configuration of the build environment.

Fundamentally, the kernel build system uses two variables to determine the target architecture: `ARCH` and `CROSS_COMPILE`. Normally these are inferred from the build environment, so they will naturally need to be overridden for the purpose of cross-compilation. The default values are found in the top-level Makefile and may be set there or overridden on the command line.

The `ARCH` variable refers to the architecture that the kernel is being built for, as recognized by the kernel itself. The names of the recognized architectures are not necessarily conventional, wherefore the arch/ subdirectory should be checked to determine the appropriate target architecture.

The `CROSS_COMPILE` variable should be set to the prefix of the toolchain (`CTARGET`), including the trailing dash. For example, if the toolchain is invoked as armv6j-unknown-linux-gnu-gcc, the trailing gcc should be stripped off, so that what remains is armv6j-unknown-linux-gnu-.

**`Makefile`**

**The vanilla Makefile**

```
ARCH            ?= $(SUBARCH)
CROSS_COMPILE   ?=
```
**`Makefile`**

**Set the`ARCH` and `CROSS_COMPILE` default values**

```
ARCH            ?= arm
CROSS_COMPILE   ?= armv6j-unknown-linux-gnu-
```
Overriding on the command-line (instead of in the Makefile) can look like:

`root #``make ARCH=arm CROSS_COMPILE=armv6j-unknown-linux-gnu-`
A small helper script can be used if it is necessary to switch between different kernel trees at the same time. The script will be called xkmake:

**`xkmake`**

```
#!/bin/sh
exec make ARCH="arm" CROSS_COMPILE="armv6j-unknown-linux-gnu-" INSTALL_MOD_PATH="${SYSROOT}" "$@"
```
Now, when building a kernel or performing any other action, xkmake should be executed instead of make.

#### Installation variables

There are two additional variables to consider; `INSTALL_MOD_PATH`, which defines the prefix to the /lib/modules/ directory where the kernel modules will be installed, and `INSTALL_PATH` which determines the installation path for installkernel. It is recommended to set `INSTALL_MOD_PATH` to equal `SYSROOT` to facilitate the building of packages containing out-of-tree modules. If the user intends to install the kernel manually then `INSTALL_PATH` can be ignored; otherwise, this can be set to `ROOT`.

### Finalizing

Configure the kernel:

`root #``xkmake menuconfig`
Configuring the kernel is similar to any other kernel. This topic is covered in detail in articles such as [Kernel/Configuration](https://wiki.gentoo.org/wiki/Kernel/Configuration) and [Kernel/Gentoo Kernel Configuration Guide](https://wiki.gentoo.org/wiki/Kernel/Gentoo_Kernel_Configuration_Guide).

Compile the kernel:

`root #``xkmake -j$(nproc)`
When the build is finished the compressed and bootable kernel image will be located in arch/$ARCH/boot/\*zImage. The kernel and its modules are now ready for installation:

`root #``xkmake modules_install`
The user will most likely want to install the kernel manually to tailor to the needs of the specific platform.

## Frequently asked questions

### I get "configure: error: C compiler cannot create executables"

This is a generic error and can be caused by just about anything. The test is pretty simple: can the requested compiler create an executable? However, this relies on many things being correct: the toolchain itself being completely sane, the compiler and compiler flags being appropriate, your environment set up properly, etc... The only way to find out the real source of the problem is to open up the generated config.log file and scroll down to where this test is run and see what exactly the error message is that the toolchain is spitting out.

### "epatch" always fails in newly compiled system

The bash package does not properly cross-compile and mixes the host signal definitions with those of the target. This manifests itself differently depending on the combination of host architecture and target architecture. To resolve the issue, simply re-compile bash natively. "But bash uses epatch!" you exclaim. In that case, you will need to modify the ebuild and comment out all the calls to epatch. Once you've installed the fixed bash this way, uncomment all of the bash lines and rebuild it again.

### uClibc build segfaults/crashes while building locale

The uClibc locale support is pretty experimental at this point. Unless you really need support for it (and you're willing to help bang on the problem), simply disable support by adding `-nls -iconv -pregen -userlocales` values to the `USE` flags when building uClibc.
