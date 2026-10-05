<!-- source: https://wiki.gentoo.org/wiki/QEMU/User | group: Gentoo Wiki (Main) | wiki-title: QEMU/User -->
---
title: QEMU/User
url: https://wiki.gentoo.org/wiki/QEMU/User
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-01"
fingerprint: af9594112767ba96
license: CC BY-SA 4.0
---

# QEMU/User

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

QEMU User space emulator is a CPU emulator for Linux and BSD binaries.


## Introduction

QEMU User loads a binary compiled for a CPU architecture other than that of the host, and executes it by emulating the CPU architecture the binary expects.

Nothing else is modified, i.e. the binary will run in the context of where it was started and interact with host resources (kernel, file system, etc.).


## Installation

By default, [app-emulation/qemu](https://packages.gentoo.org/packages/app-emulation/qemu) does not define any `QEMU_USER_TARGETS`. To add all of them:

**`/etc/portage/package.use/qemu`**

**Configure QEMU User to build all targets**

To use QEMU User to run foreign architecture rootfs images or containers, it must be built as static binary by enabling the [static-user](https://packages.gentoo.org/useflags/static-user)[) and](https://wiki.gentoo.org/wiki/USE_flag) [static-libs](https://packages.gentoo.org/useflags/static-libs) [USE flags on its supporting libraries:](https://wiki.gentoo.org/wiki/USE_flag)

**`/etc/portage/package.use/qemu`**

**Configure QEMU User to be static**


### Register as binfmt\_misc interpreter

Although QEMU User can be used directly by providing a binary as an argument,  [binfmt\_misc](https://wiki.gentoo.org/wiki/Binfmt_misc), kernel support for miscellaneous binary formats, allows invoking a binary simply by typing its name. For this to work, each qemu-${ARCH} binary must be registered with the kernel as a binfmt\_misc interpreter for its corresponding architecture. The kernel then automatically invokes it when a foreign architecture binary is to be executed.


#### OpenRC

To start the `qemu-binfmt` service:

`root #``rc-service qemu-binfmt start`
To start the service on boot:

`root #``rc-update add qemu-binfmt default`

#### systemd

Modern versions of QEMU ship a binfmt configuration file that supports all binary formats. Simply add a link to it in /etc/binfmt.d/.

`root #``ln -s /usr/share/qemu/binfmt.d/qemu.conf /etc/binfmt.d/qemu.conf`

## Usage

To use QEMU directly, using the target binary as the argument:

`user $``qemu-${ARCH} [options] program [arguments...]` IF QEMU has been registered as binfmt\_misc interpreter, it will be used indirectly when running the relevant binary via its name alone.

Options can be passed as arguments or set through environment variables. When running as a binfmt\_misc interpreter, options must be present in the environment of the binary. For a full list of parameters, refer to the output of qemu-${ARCH} --help.

The available options and associated environment variables are:

| Option | Variable | Description | 
|---|---|---|
| `-h` | - | print this help | 
| `-help` | - | print this help | 
| `-g port` | `QEMU_GDB` | wait gdb connection to 'port' | 
| `-L path` | `QEMU_LD_PREFIX` | set the elf interpreter prefix to 'path' | 
| `-s size` | `QEMU_STACK_SIZE` | set the stack size to 'size' bytes | 
| `-cpu model` | `QEMU_CPU` | select CPU ( `-cpu help` for list) | 
| `-E var=value` | `QEMU_SET_ENV` | sets targets environment variable (see below) | 
| `-U var` | `QEMU_UNSET_ENV` | unsets targets environment variable (see below) | 
| `-0 argv0` | `QEMU_ARGV0` | forces target process argv\[0\] to be 'argv0' | 
| `-r uname` | `QEMU_UNAME` | set qemu uname release string to 'uname' | 
| `-B address` | `QEMU_GUEST_BASE` | set guest\_base address to 'address' | 
| `-R size` | `QEMU_RESERVED_VA` | reserve 'size' bytes for guest virtual address space | 
| `-t tsig hsig n[,...]` | `QEMU_RTSIG_MAP` | map target rt signals \[tsig,tsig+n) to \[hsig,hsig+n\] | 
| `-d item[,...]` | `QEMU_LOG` | enable logging of specified items (use '-d help' for a list of items) | 
| `-dfilter range[,...]` | `QEMU_DFILTER` | filter logging based on address range | 
| `-D logfile` | `QEMU_LOG_FILENAME` | write logs to 'logfile' (default stderr) | 
| `-one-insn-per-tb` | `QEMU_ONE_INSN_PER_TB` | run with one guest instruction per emulated TB | 
| `-tb-size size` | `QEMU_TB_SIZE` | TCG translation block cache size | 
| `-strace` | `QEMU_STRACE` | log system calls | 
| `-seed` | `QEMU_RAND_SEED` | Seed for pseudo-random number generator | 
| `-trace` | `QEMU_TRACE` | \[\[enable=\]\<pattern>\]\[,events=\<file>\]\[,file=\<file>\] | 
| `-version` | `QEMU_VERSION` | display version information and exit | 
| `-perfmap` | `QEMU_PERFMAP` | Generate a /tmp/perf-${pid}.map file for perf | 
| `-jitdump` | `QEMU_JITDUMP` | Generate a jit-${pid}.dump file for perf | 

The default for `QEMU_LD_PREFIX` is `/usr/gnemul/qemu-${ARCH}`; the default for `QEMU_STACK_SIZE` is `xxx byte`.

Use the `-E` and `-U` options or the `QEMU_SET_ENV` and `QEMU_UNSET_ENV` environment variables to set and unset environment variables for the target process.

It is possible to provide several variables by separating them with commas in [getsubopt(3)](https://man.archlinux.org/man/getsubopt.3.en) [style. It's also possible to provide the](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) `-E` and `-U options` multiple times. The following lines are equivalent:

-E var1=val2 -E var2=val2 -U LD\_PRELOAD -U LD\_DEBUG
   -E var1=val2,var2=val2 -U LD\_PRELOAD,LD\_DEBUG
   QEMU\_SET\_ENV=var1=val2,var2=val2 QEMU\_UNSET\_ENV=LD\_PRELOAD,LD\_DEBUG

Note that if several changes are made to a variable, the last change will be the one in effect.


### QEMU\_CPU

QEMU supports a variety of CPUs per architecture. QEMU User disables most CPU extensions by default.

`user $``/usr/${CHOST}/bin/uname -m`
qemu: uncaught target signal 4 (Illegal instruction) - core dumped
\[1\]    1015619 illegal hardware instruction

To enable CPU extensions, pass a specific CPU via `-cpu model` or `QEMU_CPU`.

`user $``QEMU_CPU=${CPU} /usr/${CHOST}/bin/uname -m``${ARCH}`
To print a list of supported CPUs per architecture:

`user $``qemu-$ARCH -cpu help`

### QEMU\_LD\_PREFIX

A foreign architecture binary can interact with native binaries, but cannot use native libraries. When not in a container or chroot, running such a binary will fail because it cannot find its runtime dependencies (unless it is statically linked).

For example, when calling a binary in a [crossdev](https://wiki.gentoo.org/wiki/Crossdev) environment (/usr/${CHOST):

`user $``/usr/${CHOST}/bin/uname -m``qemu-${ARCH}: Could not open '/lib/ld-musl-powerpc.so.1': No such file or directory`
For this to work, QEMU needs to be told the location of the dynamic libraries:

`user $``QEMU_LD_PREFIX=/usr/${CHOST} /usr/${CHOST}/bin/uname -m``${ARCH}`

#### /usr/gnemul

The default for `QEMU_LD_PREFIX` is /usr/gnemul/qemu-${ARCH}, as indicated by qemu-${ARCH} --help.

By placing a rootfs in that path or creating a symlink to one, foreign binaries can run without explicitly providing `QEMU_LD_PREFIX`:

`root #``ln -s /usr/${CHOST} /usr/gnemul/qemu-${ARCH}` `user $``/usr/${CHOST}/bin/uname -m``${ARCH}`

## Troubleshooting


### Crossdev/QEMU\_CPU

When using [crossdev](https://wiki.gentoo.org/wiki/Crossdev) with certain features, e.g. `altivec` for [PowerPC G4](https://wiki.gentoo.org/wiki/Safe_CFLAGS#G4_.28PPC_74xx.29), `QEMU_CPU` needs to be passed to the crossdev environment:

COMMON\_FLAGS="-mcpu=7450 -O2 -maltivec -mabi=altivec -pipe"
 export QEMU\_CPU=7450 crossdev ...


## See also

- [QEMU](https://wiki.gentoo.org/wiki/QEMU) — a generic, open-source hardware emulator and virtualization suite.
