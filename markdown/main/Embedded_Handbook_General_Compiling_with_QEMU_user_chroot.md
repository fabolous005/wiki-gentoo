<!-- source: https://wiki.gentoo.org/wiki/Embedded_Handbook/General/Compiling_with_QEMU_user_chroot | group: Gentoo Wiki (Main) | wiki-title: Embedded Handbook/General/Compiling with QEMU user chroot -->
---
title: Embedded Handbook/General/Compiling with QEMU user chroot
url: https://wiki.gentoo.org/wiki/Embedded_Handbook/General/Compiling_with_QEMU_user_chroot
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-13"
fingerprint: de02391c49c63984
license: CC BY-SA 4.0
---

# Embedded Handbook/General/Compiling with QEMU user chroot

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


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
