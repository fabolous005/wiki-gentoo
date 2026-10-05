<!-- source: https://wiki.gentoo.org/wiki/Kernel/Building_from_userspace | group: Gentoo Wiki (Main) | wiki-title: Kernel/Building from userspace -->
---
title: Kernel/Building from userspace
url: https://wiki.gentoo.org/wiki/Kernel/Building_from_userspace
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-10-16"
fingerprint: "5783113894a61366"
license: CC BY-SA 4.0
---

# Kernel/Building from userspace

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article discusses an alternative to the normal Gentoo way of building the Linux kernel from root. It allows managing several kernels at once and managing kernels for different machines and architectures in a uniform way from a non-root user. The approach described uses facilities within the Linux kernel build system.

## Rationale

Reasons to work this way include:

- you may dislike doing kernel configuration and build from root for security and safety; building as a user prevents damage your system (at least, until you install your new kernel; that must be done as root or a user with write access to /boot and /lib/modules);
- the kernel developers recommend not building the kernel as root (one once joked it would be worth including rm -r / in the kernel scripts to teach people not to do it); and
- the normal Gentoo way to build a kernels is to have the configuration in /usr/src/linux, which means there is only one configuration at a time; a second configuration requires a copy of the /usr/src/linux-\<v>-\<r>-\<m>-gentoo kernel source directory tree.

Working as described in the following article allows one or more kernel build subdirectories of a user's home directory (or of course of some admin user set up for the purpose). Each directory has its own configuration stored in its own .config file and optionally a set of Environment variables to tailor how the kernel is built. For example:

- a directory on a powerful desktop machine to build the kernel for a feeble laptop;
- with the necessary cross-development environment in place, a directory for building the kernel for a single-board micro such as the Raspberry Pi; or
- a directory for building the kernel using Clang LLVM and LTO. instead of gcc.

## Set up

Create the following shell script and store it somewhere convenient, such as /usr/local/bin/kmake:

**`/usr/local/bin/kmake`**

**Userspace kernel make command**

```
#! /bin/sh
### Run kernel make from user directory
#   Set environment variables such as ARCH, KCFLAGS etc. from "kmake.env" in the current directory
#   Usage: the same as "make" in a kernel directory.
#   If the the specified target is "source <directory>", sets the kernel source link to the named directory (default /usr/src/linux)
#   Paul Gover 2021
set -e
defaultsrc="/usr/src/linux"
me="${0##*/}"
profile="$PWD/$me.env"
if [ -r "$profile" ]
then    # Run the profile, exporting all variables to the environment
        set -a
        . "$profile"
        set +a
fi
if [ "$1" = "source" ]
then
        [ -h "source" ] && was="(was $(realpath source))"
        # The (undocumented) "outputmakefile" make target creates the symlink, the Makefile, and some other stuff.
        make -s -C "${2:-$defaultsrc}" O="$PWD" "outputmakefile"
        printf 'Kernel source now set to %s %s\n' "$(realpath source)" "$was"
elif [ -h "source" ]
then
        if [ -e "source" ]
        then    make O="$PWD" "$@"
        else
                printf "The kernel source directory %s is no longer valid. Run %s\n" "$(realpath source)" "$me source <directory>" >&2
                exit 1
        fi
else
        printf 'The current directory %s does not contain a link to the kernel source\n' "$PWD" >&2
        printf 'Run "%s source <directory>" first.  (Default directory %s)\n' "$me" "$defaultsrc" >&2
        exit 1
fi
```
Use this script kmake instead of the make command when working in a kernel build directory.

For each kernel configuration, make a directory, and if necessary create a kmake.env script within that directory containing environment settings for the kernel.

Specify or update the chosen kernel source directory (default the current target of the /usr/src/linux symlink) by running kmake source \<directory> within the kernel build directory.

### Example kmake.env files

- cross-compile for a Raspberry Pi:

**`kmake.env`**

- compile a version 5.12 or later kernel using Clang, LLVM and LTO:

**`kmake.env`**

- an (unsupported) full-rice kernel might have:

**`kmake.env`**

## Operation

Run kmake source, then kmake clean, and setup an initial configuration .config file. That be the result of kmake default, an old .config from a previous kernel build, or a kernel seed from the Internet. If necessary, set up a kmake.env environment settings file. Thus:

`user $````
mkdir kernel-pi
```
`user $````
cd kernel-pi
```
`user $````
nano kmake.env
```
`user $````
kmake source
```
`user $````
kmake clean
```
`user $````
# Obtain a starter configuration - for example:
```
`user $````
kmake default
```
`user $````
kmake nconfig
```
`user $````
kmake -j25
```
`user $``su -`
When finished, enter root mode with su - and enter the commands to install the kernel, modules and build any initramfs as normal, or use sudo for each command. For example:

`root #````
kmake install
```
`root #````
kmake modules_install
```
When building several different kernels, it's worth setting a unique `Local version id` in each kernel configuration (use a leading hyphen to separate it from the rest of the release string).  For example, the following might cause the kernel to be named vmlinuz-5.12.1-gentoo-test:

## Kernel updates

The source from which the kernel is built is determined by the target of the /usr/src/linux symbolic link, in the usual way.  Manage this either through eselect kernel, or by enabling the `symlink` USE variable in [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources) to automatically update the link after installing updated source.

After changing the link target,

`user $``kmake source``user $``kmake clean``user $``kmake oldconfig`
to update the chosen source and configuration.

### Reverting to an earlier kernel version

kmake install will normally create versioned kernel image vmlinux-\<v>-\<m>-\<r>-... and configuration config-\<v>-\<m>-\<r>-... files in /boot. Provided the configuration file exists or is recoverable from backup, and providing the kernel source tree also exists or can be recovered or reinstalled, then it's trivial to restore the build directory by copying the configuration as .config and setting the correct source link with kmake source /usr/src/linux-\<v>-\<r>-\<m>-... .

## Packaging a kernel for use on another host

Create a compressed tarball of your compiled kernel using kmake tarxz-pkg or other packaging target. See kmake help (which is the same thing as make help - there's nothing special from the kmake script) for a list of that and other packaging targets.

Install the tarball by:

1. transferring it to the intended host;
2. changing to the root of the file system;
3. using tar to extract the /boot and /lib/modules directories into the system.

An alternative is to use the kmake dir-pkg command, which builds tar-install/boot and tar-install/lib/modules/\<v>-\<r>-\<m>-gentoo directories. This enables adding symbolic links for vmlinuz, config and Symbol.map to the tar-install/boot directory to support boot managers which respect these links. Then manually create the tarball:

`user $``kmake dir-pkg``user $``cd tar-install/boot``user $``for f in vmlinuz-* config-* System.map-* ; do ln -r -s "$f" "${f%%-*}"; done``user $``cd ../..``user $``tar cvf mytarball.tar -C tar-install`
This can of course be incorporated into a script.

To install, **on the host** continue as before, taking a moment to modify the previous symbolic links:

`root #``mount /boot``root #``cd /boot``root #``for f in vmlinuz config System.map; do mv $f $f.old; done``root #``cd /``root #``tar -xvf mytarball.tar`
