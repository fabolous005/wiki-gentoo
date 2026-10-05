<!-- source: https://wiki.gentoo.org/wiki/Embedded_Handbook/General/Cross-compiling_the_kernel | group: Gentoo Wiki (Main) | wiki-title: Embedded Handbook/General/Cross-compiling the kernel -->
---
title: Embedded Handbook/General/Cross-compiling the kernel
url: https://wiki.gentoo.org/wiki/Embedded_Handbook/General/Cross-compiling_the_kernel
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-03-21"
fingerprint: "56931b3284e83f26"
license: CC BY-SA 4.0
---

# Embedded Handbook/General/Cross-compiling the kernel

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


Cross-compile a kernel for a system with flair!

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
