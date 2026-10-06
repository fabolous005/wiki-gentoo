<!-- source: https://wiki.gentoo.org/wiki/Crossdev_qemu-static-user-chroot | group: Gentoo Wiki (Main) | wiki-title: Crossdev qemu-static-user-chroot -->
---
title: Crossdev qemu-static-user-chroot
url: https://wiki.gentoo.org/wiki/Crossdev_qemu-static-user-chroot
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-05-26"
fingerprint: "6cc987f7d47bf27"
license: CC BY-SA 4.0
---

# Crossdev qemu-static-user-chroot

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



**Deprecated article**

This article is **deprecated (obsolete)**. Contents are <u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

This page started as a clone of [Cross Container Support Project](https://wiki.gentoo.org/wiki/Cross_Container_Support_Project) by [User:Jing huang](https://wiki.gentoo.org/wiki/User:Jing_huang), so I wouldn't have to play with formatting so much..I merely edited and updated it to match my experiences.

I suddenly found myself with a handfull of omap4430 devices (samsung galaxy tab2, nexus4, etc) and wish to experement with a full glibc userland, and eventually a custom hybrid bionic/glibc userland running a full linux and glx xserver likely running plasma active or some such.

I have been a gentoo user for almost 10 years, but I have little experience cross compiling beyond distcc for a x86 host so I will document the progress of my project here, so that I can remember later.. please excuse my blatant plagerism from all the sites I read as reference while I concocted a solution that worked. while I hope that this guide will prove useful, [ymmv](http://www.urbandictionary.com/define.php?term=YMMV).

## My hardware

My host system is an aging i7 860 with 8 gigs of ddr3 (and a cheap 1tb hdd...) and I was \_utterly\_ unable to build a static qemu (or qemu-user for that matter).

I will attach my configs for reference, and that would have helped me a bit..

## Isn't a chroot good enough?

Short answer, no. As soon as you try to build python, GCC, or any number of others you begin to see. The issue is that many of these programs perform tests to verify function, or use helpers to compile bytecode etc. Seems like not an issue, until you think about whats happening here: You are generating binaries for arm that the build system then tries to execute to verify they work. Our solution is to use qemu to run them.

With QEMU user emulation you can run non-native executables. I.e. with QEMU configured for arm-linux-user you can run arm binaries. Unfortunately, without any further configuration, you can run only static executables. For dynamic executables you need to have all the libraries the executable depends on, built for the same architecture as the main executable (ARM here). This at first sight poses a problem -- you need pretty much the same standard libraries that you already have in your system (glibc, ...), but for a different architecture.

Such situation (wanting a different set of libraries) can be solved by a chroot. It would also nicely isolate your new ARM linux install. But ordinary chroot must keep the same architecture. Without one more ingredient to the mix you would only get:

`user $``chroot`
failed to run command \`/bin/bash': Exec format error

The last magic ingredient is binfmt\_misc. Binfmt\_misc generalizes the classical shebang, allowing you to associate custom interpreters to specific file types. Yes, you guessed it -- it allows us to associate ARM (or some other arch) ELF executables with our qemu. After setting up the association the executables can be run the same way as native executables, without specifying QEMU on the command line.

So, to recap: We use

qemu user emulation to run the ARM executables
   chroot as a place to keep the installed distro including the required dynamic libraries
   binfmt\_misc to tell the kernel to run ARM ELF executables with the help of qemu

There's one last thing to resolve: We need a static build of QEMU, so that QEMU itself doesn't need any libraries. -- With dynamic QEMU, we'd find ourselves with the need of native libraries inside the chroot, which would defeat the purpouse.

Ready? Lets get started..

### Setting the required kernel options

In order for binfmt\_misc support to work, we need to enable the appropriate kernel drivers. As well, there is some VIRTIO and KVM stuff that will be useful. You all know how to config a kernel, i assume. If not, have a look at the Gentoo Handbook, and come on back when you're ready.

Here is the relevant kernel option:

**3.2.1-gentoo-r2**

```
Executable file formats / Emulations  --->
 [*] Kernel support for MISC binaries
```
get that building if needed, and well turn to qemu-user.

### install static qemu-user

I tried for a couple days to get this to compile with static libs. Turns out there are come issues with glib that prevent it. However, debian provides one that'll do nicely. (If this link goes bad, you can browse their current versions \[[here](http://packages.debian.org/sid/qemu-user-static)\].)

Grab Alien if you dont have it, this will let us easily strip the package off our binary.

`root #``emerge --ask alien`
unpack it with

`user $``alien -t qemu-user-static_2.0.0+dfsg-7_amd64.deb` which will spit out a nice tarball. The only file I cared about was qemu-arm-static which is the arm interpreter. Extract and copy qemu-arm-static to /usr/bin/qemu-arm

`user $````
mkdir qemu-user-static
```
`user $````
tar xvzpf qemu-user-static-2.0.0+dfsg.tgz -C qemu-user-static
```
`user $````
cp qemu-user-static/usr/bin/qemu-arm-static /usr/bin/qemu-arm
```
`user $````
echo rm -rf qemu-user-static*
```
#### Binfmt\_misc Configuration

Now mount the binfmt\_misc handler if it's not already, then we need to register our format with the kernel via the procfs.

`root #````
[ -d /proc/sys/fs/binfmt_misc ] || modprobe binfmt_misc
```
`root #````
[ -f /proc/sys/fs/binfmt_misc/register ] || mount binfmt_misc -t binfmt_misc /proc/sys/fs/binfmt_misc
```
Next, register qemu-user-arch to the binfmt\_misc module. You don't need to add them one by one and Luca has provided a initscript to get the bin formats registered. You can use it:

`root #``/etc/init.d/qemu-binfmt start`
or start the service on bootstrap:

`root #``rc-update add qemu-binfmt default`
You could learn more detail of registering bin formats through /usr/portage/app-emulation/qemu-user/files/qemu-binfmt.initd.

### Enter/Exit the Chroot

Firstly, you should chose and download a stage3 tarball from [installation media](http://www.gentoo.org/main/en/where.xml).
We take arm arch as an example to show how to enter/exit the chroot.

- Download and unpack arm stage tarball:

- `root #``mkdir arm-chroot && cd arm-chroot``root #``wget http:// stage3-armv7a-date.tar.bz2``root #``tar -xvf stage3-armv7a-date.tar.bz2`

- Install the static qemu-user into the chroot:

- `root #``ROOT=$PWD/ emerge -K qemu-user`

- Mount the required directories:

- `root #``mkdir -p usr/portage``root #` `mount --bind /usr/portage usr/portage` `root #` `mount --bind /proc proc` `root #` `mount --bind /sys sys`

- Chroot into the environment:

- `root #``chroot . /bin/busybox mdev -s` `root #` `chroot . /bin/bash --login`

- Unmount stuff when not in use:

- `root #``umount usr/portage` `root #` `umount sys` `root #` `umount proc`

## Setup Crossdev

The first thing that is necessary is the creation of an [ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository). If you have one, emerge the script:

`root #``emerge --ask sys-devel/crossdev`
This will provide you with the crossdev script. This script automates the steps necessary to build a toolchain. These steps are, in short:

1. binutils: Build a cross-binutils, which links and processes for the target architecture.
2. linux-headers: Install a set of C library and kernel headers for the target architecture.
3. libc-headers: Additional header files
4. gcc-stage-1: Build a basic (stage 1) gcc cross-compiler. This will be used to compile the C library. It will be unable to build anything almost else (because it can't link against the C library it doesn't have).
5. libc: Build the cross-compiled C library (using the stage 1 cross compiler).
6. gcc-stage-2: Build a full (stage 2) C cross-compiler.

All cross toolchains will be kept locally in the ebuild repository, separate from native tools.

**`/etc/portage/make.conf`**

```
source /var/lib/layman/make.conf
PORTDIR_OVERLAY="/usr/local/portage ${PORTDIR_OVERLAY}"
```
The script is used like:

`root #``crossdev -t powerpc-unknown-linux-gnu`
This will build a cross-compiling toolchain for PowerPC machines.

By default, the newest stable version of the binutils, libraries, and C compiler will be used. It is quite often the case they will not compile themselves through the entire build process. Less bleeding edge versions can be specified with additional flags:

--b 2.22      # specifies the version of binutils
--g 4.6.3     # specifies the version of gcc
--l 2.15-r2   # specifies the version of the tuple-specified libc
--k 3.5       # specifies the version of the kernel headers

It is recommended trying older versions, particularly of gcc, if the script fails.

If you want to remove a toolchain, use the clean flag:

`root #``crossdev -C powerpc-unknown-linux-gnu`
This will unmerge the packages created by crossdev.

If you got the errors about fortran, use the fellow command:

`root #``USE="-fortran nossp" crossdev -t powerpc-unknown-linux-gnu`
## Cross Container

The lxc.sh tool can download, configure and create a multi-arch gentoo guest. Using this tool, the user build a native gcc in chroot. You can download it here: [lxc.sh](https://github.com/jinghuang/cross_container_support)

Next, we will take **armv7a\_hardfloat** as a example to build the native compiler in chroot.

### Install a cross compiler

You must install the cross compiler manually:

`root #``USE="-fortran nossp" crossdev -t armv7a-hardfloat-linux-gnueabi`
### Create the chroot

You can create an arm gentoo guest:

`root #````
./lxc.sh create -i 
What is the subarch of arm? armv7a
```
*ip_address* -g *gateway* -n *guest_name* -r *rootfs* -a arm
You can also start and destroy the arm gentoo guest:

`root #``./lxc.sh start -n` *guest_name*`root #``./lxc.sh destroy -n` *guest_name* -r *rootfs*
Additional developers, bug fixes, comments, etc. are welcome.

### Switch to native compiler

In chroot, you can switch to native compiler:

`root #````
cd /root
```
`root #````
source switch.sh native
```
Then, you can get the native armv7a-hardfloat-linux-gnueabi-gcc. To use it, just specify the **CC=armv7a-hardfloat-linux-gnueabi-gcc** in the Makefile.

You also could switch to emulated gcc as well:

`root #``source switch.sh emu`
