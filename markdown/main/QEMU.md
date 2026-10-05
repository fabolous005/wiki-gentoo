<!-- source: https://wiki.gentoo.org/wiki/QEMU | group: Gentoo Wiki (Main) | wiki-title: QEMU -->
---
title: QEMU
url: https://wiki.gentoo.org/wiki/QEMU
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-02"
fingerprint: de019b1d458239e4
license: CC BY-SA 4.0
---

# QEMU

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



**Resources**

**QEMU** (**Q**uick **EMU**lator) is a generic, open-source hardware emulator and virtualization suite.

QEMU is a [Type-2 hypervisor](https://en.wikipedia.org/wiki/Hypervisor#Classification), which runs within user namespace on a host platform and performs virtual hardware emulation. Inside a virtual machine, QEMU can emulate multiple operating systems; it can also emulate embedded systems.

QEMU supports more than 32 CPU architectures. It emulates nearly all the opcodes of these CPUs, and can execute multiple virtual CPUs in parallel.

QEMU can be paired with [KVM](https://wiki.gentoo.org/wiki/KVM) to run VMs at near-native speed. This is accomplished by using hardware extensions such as Intel VT-x or AMD-V. It can then emulate user-level processes, which allow applications, compiled for one architecture, to run on a different one.

When used in conjunction with an accelerator plugin, QEMU becomes a [Type-1 hypervisor](https://en.wikipedia.org/wiki/Hypervisor#Classification), which runs in kernel namespace. This allows a user namespace program access to the hardware virtualization features of various processors. Such an accelerator can be [KVM](https://wiki.gentoo.org/wiki/KVM) (**K**ernel-based **V**irtual **M**achine) or [Xen](https://wiki.gentoo.org/wiki/Xen).

If no accelerator is used, QEMU will run entirely in user namespace, using its built-in binary translator, TCG (Tiny Code Generator). Using QEMU without an accelerator is relatively inefficient and slow. The table at the end of this section lists available accelerators.

QEMU has different operating modes. **System mode** emulates a full system, including processors and peripherals, which allows running different operating systems and configure hardware configurations. **User mode** emulates a specific CPU architecture, which allows running Linux binaries that have been compiled for a different instruction set architecture.

QEMU virtual machines (VMs) can interface with many types of physical host hardware, including CD-ROM drives, [USB](https://wiki.gentoo.org/wiki/USB) devices, audio interfaces, hard disks and network cards.

By default, QEMU defaults to using the qcow2 virtual disk image format. This format only uses as much host disk space as the guest OS grows to use. Using the snapshot method, the guest OS can revert back to its desired state in time.

QEMU can save and restore the state of VMs of all its running programs.

QEMU does **not** depend on graphical output methods on the host system. Instead, it makes use of an integrated [VNC](https://en.wikipedia.org/wiki/VNC) server to access the screen of the guest OS.

A number of plugins are available for QEMU, including several accelerator plug-ins:

| Accelerator | Virtualization type | Description | Gentoo package name | 
|---|---|---|---|
| tcg | full/software emulation | QEMU's own Tiny Code Generator. This is the default. More frequently denoted as qemu and not qemu/tcg so often. | [app-emulation/qemu](https://packages.gentoo.org/packages/app-emulation/qemu) | 
| hvf | paravirtualization | Apple's Hypervisor framework based on Intel VT. |  | 
| whpx <sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> | hybrid | Microsoft's Windows Hypervisor Platform based on Intel VT or AMD-V. |  | 
| kvm | paravirtualization | Linux Type-1 Hypervisor. This is the common choice for hosts using **amd64**, **arm64**, or **mips**<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>. Supports Microsoft Windows. | [app-emulation/qemu](https://packages.gentoo.org/packages/app-emulation/qemu) | 
| haxm <sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup> | paravirtualization | Intel VT, by Intel Corporation. |  | 

The following pages provide detailed instructions related to QEMU configuration and options:

- [QEMU/Linux guest](https://wiki.gentoo.org/wiki/QEMU/Linux_guest) — describes the setup of a Gentoo Linux guest in [QEMU] using Gentoo bootable media.
- [QEMU/Networking/Bridge with Wifi Routing](https://wiki.gentoo.org/wiki/QEMU/Networking/Bridge_with_Wifi_Routing)
- [QEMU/Networking/KVM IPv6 Support](https://wiki.gentoo.org/wiki/QEMU/Networking/KVM_IPv6_Support) — describes IPv6 support in QEMU/KVM.
- [QEMU/Networking/Open vSwitch network](https://wiki.gentoo.org/wiki/QEMU/Networking/Open_vSwitch_network)
- [QEMU/Options](https://wiki.gentoo.org/wiki/QEMU/Options) — describes some of the options useful for configuring [QEMU] virtual machines (VMs).
- [QEMU/OS2WarpV3 guest](https://wiki.gentoo.org/wiki/QEMU/OS2WarpV3_guest)
- [QEMU/Windows guest](https://wiki.gentoo.org/wiki/QEMU/Windows_guest) — setup of a Windows guest using [QEMU]

- [Virtiofs](https://wiki.gentoo.org/wiki/Virtiofs) — a shared file system that lets virtual machines access a directory tree on the host

In order to utilize KVM, either Intel's Vt-x (`vmx`) or AMD's AMD-V (`svm`) must be supported by the processor. These technologies permit multiple operating systems to concurrently execute operations on processors.

To inspect hardware for virtualization support, run:

`user $``grep --color --extended-regexp "vmx|svm" "/proc/cpuinfo"`
If KVM support is available, there should be a `kvm` device at /dev/kvm. This will take effect *after* the system has booted to a KVM-enabled kernel.

Described below are the basic requirements for KVM kernel configuration for the host OS. A more complete and up-to-date list can be found at the [KVM Tuning Kernel](http://www.linux-kvm.org/page/Tuning_Kernel) page.

```
General setup  --->
  Timers subsystem  --->
    [*] High Resolution Timer Support 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_HIGH_RES_TIMERS</code> to find this item.
If KVM support is not available, insert `CONFIG_KVM=y` into the /usr/src/linux/.config and rebuild/reinstall the kernel (and its initramfs image). Come back here after the host is rebooted.

\[\*\] Virtualization [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_VIRTUALIZATION\</code> to find this item.  --->
  \<\*>   Kernel-based Virtual Machine (KVM) support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_KVM\</code> to find this item.

\[\*\] Virtualization [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_VIRTUALIZATION\</code> to find this item.  --->
  \<M> KVM for Intel processors support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_KVM\_INTEL\</code> to find this item.

\[\*\] Virtualization [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_VIRTUALIZATION\</code> to find this item.  --->
  \<M> KVM for AMD processors support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_KVM\_AMD\</code> to find this item.

To set the various kernel configuration settings from the command lines, the linux/scripts/kconfig/merge\_config.sh shall be used here:

**Mandatory kernel configuration options to set**:

**`/usr/src/kernel-kconfig-qemu-host.config`**

`root #````
cd "/usr/src/linux"
```
`root #````
"./scripts/kconfig/merge_config.sh" ".config" "/usr/src/kernel-kconfig-qemu-host.config"
```
*Useful* kernel configuration options to use:

**`/usr/src/kernel-kconfig-qemu-host-optional.config`**

`root #````
"./scripts/kconfig/merge_config.sh" ".config" "/usr/src/kernel-kconfig-qemu-host-optional.config"
```
Accelerated networking, **required** for `vhost-net` USE flag (recommended):

```
Device Drivers  --->
  [*] VHOST drivers  --->
    <*> Host kernel accelerator for virtio net 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_VHOST_NET</code> to find this item.
Device Drivers  --->
  \[\*\] Network device support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NETDEVICES\</code> to find this item.  --->
    \[\*\] Network core driver support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NET\_CORE\</code> to find this item.
      \<\*> Universal TUN/TAP device driver support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_TUN\</code> to find this item.

Needed for 802.1d Ethernet bridging:

\[\*\] Networking support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NET\</code> to find this item.  --->
  Networking options  --->
    \<\*> The IPv6 protocol [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_IPV6\</code> to find this item.
    \<\*> 802.1d Ethernet Bridging [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_BRIDGE\</code> to find this item.

Mediated device passthrough for Intel GPUs (Broadwell to Comet Lake)[\[4\]](https://wiki.gentoo.org#cite_note-4)

**Intel VT-g (`CONFIG_VFIO_MDEV`, `CONFIG_DRM_I915_GVT`, `CONFIG_DRM_I915_GVT_KVMGT`)**

Device Drivers  --->
  \<\*> VFIO Non-Privileged userspace driver framework [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_VFIO\</code> to find this item.
    \<\*>VFIO support for any PCI device [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_VFIO\_PCI\</code> to find this item.
      \<\*> Generic VFIO PCI extensions for Intel graphics (GVT-d) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_VFIO\_PCI\_IGD\</code> to find this item.

Graphics Support  --->
  \<\*> Direct Rendering Manager (XFree 4.1.0 and higher DRI support) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_DRM\</code> to find this item.
    \<\*> Intel 8xx/9xx/G3x/G4x/HD Graphics [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_DRM\_I915\</code> to find this item.
      \<\*> Enable KVM host support Intel GVT-g graphics virtualization [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_DRM\_I915\_GVT\_KVMGT\</code> to find this item.

Some packages have a [qemu](https://packages.gentoo.org/useflags/qemu) [USE flag](https://wiki.gentoo.org/wiki/USE_flag), to enable QEMU support.

The USE flags for QEMU itself are:


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

Additional ebuild configuration is provided by the [USE\_EXPAND](https://wiki.gentoo.org/wiki/Make.conf#USE_EXPAND) variables `QEMU_USER_TARGETS` and `QEMU_SOFTMMU_TARGETS`. The USE flags for [app-emulation/qemu](https://packages.gentoo.org/packages/app-emulation/qemu) (as shown by e.g. equery from [app-portage/gentoolkit](https://packages.gentoo.org/packages/app-portage/gentoolkit))include all the available targets. Most are very obscure and may be ignored; leaving these variables at their default values will disable almost everything, which is probably fine for most users.

For each target specified, a `qemu` executable will be built. A `softmmu` target is the standard QEMU use-case of emulating an entire system, like [VirtualBox](https://wiki.gentoo.org/wiki/VirtualBox) or [VMware](https://wiki.gentoo.org/wiki/VMware), but with optional support for emulating CPU hardware along with peripherals. `user` targets execute user-mode code only; the (somewhat ambitious) purpose of these targets is to "magically" allow importing user namespace Linux ELF binaries from a different architecture into the native system (like [multilib](https://wiki.gentoo.org/wiki/Multilib), without the need for a software stack or a CPU capable of running it).

In order to enable `QEMU_USER_TARGETS` and `QEMU_SOFTMMU_TARGETS`, add the following to [/etc/portage/package.use](https://wiki.gentoo.org/wiki//etc/portage/package.use):

**`/etc/portage/package.use/qemu`**

After reviewing and adding any desired USE flags, emerge [app-emulation/qemu](https://packages.gentoo.org/packages/app-emulation/qemu):

`root #``emerge --ask app-emulation/qemu`
To connect to the SPICE server of QEMU, a GUI client like [net-misc/spice-gtk](https://packages.gentoo.org/packages/net-misc/spice-gtk) is required.

For QEMU ecosystem/management,


For the guest VM:

The following sub-articles provide detailed instructions on QEMU configurations and options:

- [QEMU/Options](https://wiki.gentoo.org/wiki/QEMU/Options) — describes some of the options useful for configuring [QEMU] virtual machines (VMs).
- [QEMU/Linux guest](https://wiki.gentoo.org/wiki/QEMU/Linux_guest) — describes the setup of a Gentoo Linux guest in [QEMU] using Gentoo bootable media.
- [QEMU/Windows guest](https://wiki.gentoo.org/wiki/QEMU/Windows_guest) — setup of a Windows guest using [QEMU]
- [QEMU/OS2WarpV3 guest](https://wiki.gentoo.org/wiki/QEMU/OS2WarpV3_guest)

For normal QEMU usage, no environment variables are required to be user-defined. List below is included for completeness.

| name | description | 
|---|---|
| `G_MESSAGES_DEBUG` | Enables debug messages for components using [GLib](https://docs.gtk.org/glib/)'s logging system.  If set to `all`, logs are output to `stderr`.  Other options are `QEMU`, `libvirt`, `gtk`, and `pulse`. Use commas to separate options. | 
| `LISTEN_FDS` | Number of file descriptors passed. Used with QEMU activated by [systemd](https://wiki.gentoo.org/wiki/Systemd). | 
| `LISTEN_PID` | PID of the receiving process (usually set to the current PID). Used with QEMU activated by [systemd](https://wiki.gentoo.org/wiki/Systemd). | 
| `QEMU_AUDIO_DRV` | Specifies the audio backend driver to use. Options are `alsa`, `pa`, `oss`, and `none`. | 
| `XDG_RUNTIME_DIR` | Specifies where user-specific runtime files and sockets should be stored. On Gentoo, typically set to /run/user/$(id -u)/, where the output of id -u is the UID of the user. Used by a variety of software, including [Wayland](https://wiki.gentoo.org/wiki/Wayland), [PulseAudio](https://wiki.gentoo.org/wiki/PulseAudio), and [virt-manager](https://wiki.gentoo.org/wiki/Virt-manager). Refer to [XDG/Base Directories](https://wiki.gentoo.org/wiki/XDG/Base_Directories) for further information. | 

QEMU uses the following files and directories for Gentoo configuration:

- /etc/libvirt/qemu.conf - QEMU configuration file.


For a complete list of files used by QEMU, see [QEMU files](https://wiki.gentoo.org/wiki/QEMU/Files).

To start up a VM of Gentoo ISO image:

`root #``qemu-system-x86_64 -cdrom install-amd64-minimal.iso -name my_gentoo_vm`
A new window appears showing Gentoo CD's VM console directly on host; no VNC, no SSH.

For information about available front-ends, refer to [QEMU/Front-ends](https://wiki.gentoo.org/wiki/QEMU/Front-ends).

In order to run a KVM-accelerated virtual machine without root privileges, one can add normal users to the `kvm` group:

`root #``gpasswd -a larry kvm`
QEMU supports the following disk image formats:

| Description | Filetype | 
|---|---|
| QEMU copy-on-write | .qcow2, .qed, .qcow, .cow | 
| VirtualBox Virtual Disk Image | .vdi | 
| CD/DVD (ISO-9660) images | .iso | 
| Raw images, that guest OS can control | .img | 
| VFAT-16 |  | 
| VMware Virtual Machine Disk | .vmdk | 
| Virtual PC Virtual Hard Disk | .vhd | 
| Parallels disk image (read-only) | .hdd, .hds | 
| Apple macOS Universal Disk Image Format (read-only | .dmg | 
| Bochs (read-only) |  | 
| Hyper-V Virtual Hard Disk | .vhdx | 
| Linux cloop (read-only) |  | 
| LUKS disk images |  | 

See [qemu-img](https://wiki.gentoo.org/wiki/Qemu-img) for more disk image information.

To create a 4 GiB raw disk image:

`user $``qemu-img create -f raw "/home/larry/qemu/my-systems-disk-image.img" 4G`
Formatting 'my-systems-disk-image.img', fmt=raw size=4294967296

`user $``ls -lh`
total 4
-rw-r--r-- 1 larry larry 4.0G Apr 12 11:23 my-systems-disk-image.img

To create a raw disk image with copy-on-write (COW) disabled:

`user $``qemu-img create -f raw "/home/larry/qemu/my-systems-disk-image.img" -o nocow=on 4G`
Formatting 'my-systems-disk-image.img', fmt=raw size=4294967296 nocow=on

`user $``ls -lh`
total 4
-rw-r--r-- 1 larry larry 4.0G Apr 12 11:23 my-systems-disk-image.img

The `nocow` option is also a file attribute, which can be determined via the command [lsattr(1)](https://man.archlinux.org/man/lsattr.1.en)<sup>[\[5\]](https://wiki.gentoo.org#cite_note-5)</sup>.

The following will create a qcow2 disk image (useful if the host filesystem does not support sparse files):

`user $``qemu-img create -f qcow2 "/home/larry/qemu/my-systems-disk-image.qcow2" 4G`
Formatting 'my-systems-disk-image.qcow2', fmt=qcow2 cluster\_size=65536 extended\_l2=off compression\_type=zlib size=4294967296 lazy\_refcounts=off refcount\_bits=16

`user $``ls -l`
total 196K
-rw-r--r-- 1 larry larry 193K Apr 12 11:30 my-systems-disk-image.qcow2

A system can be copied onto a disk image without using a CD-ROM installation medium.

By default, QEMU uses BIOS firmware to boot the system.

The disk image can be prepared with an `msdos` disk label and a gap between the end of the 512 byte MBR (Master Boot Record) and the start of the first partition. The gap is needed for boot loaders like [GRUB](https://wiki.gentoo.org/wiki/GRUB), which place boot code within this gap.

The following example uses  [the raw disk image created above](https://wiki.gentoo.org/wiki/QEMU#Creating_a_disk_image).

A raw disk image can be prepared by attaching it as a loop device:

`root #``losetup --find --partscan --show "/home/larry/qemu/my-systems-disk-image.img"`
/dev/loop0

- The `--find` option finds the first unused loop device.
- The `--partscan` option forces the Linux kernel to scan the partition table on the newly created loop device, where a default sector size of 512 bytes is assumed.
- The `--show` option displays the name of the assigned loop device, when the `--find` option is used.

Attached loop devices can be listed with the following command:

`root #``losetup --list`
NAME       SIZELIMIT OFFSET AUTOCLEAR RO BACK-FILE                                  DIO LOG-SEC
/dev/loop0         0      0         0  0 /home/larry/qemu/my-systems-disk-image.img   0     512

The loop device can then be formatted like a normal disk.

To print the partition table, use [parted(8)](https://man.archlinux.org/man/parted.8.en)[:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`root #``parted "/dev/loop0" "unit mib print"`
Error: /dev/loop0: unrecognised disk label
Model: Loopback device (loopback)
Disk /dev/loop0: 4096MiB
Sector size (logical/physical): 512B/512B
Partition Table: unknown
...

Next, create a new partition table with an `msdos` disk label:

`root #``parted "/dev/loop0" "mklabel msdos"`
Information: You may need to update /etc/fstab.

The returned information can be ignored, since an entry in the configuration file /etc/fstab is not needed.

parted now indicates that the partition table is `msdos`:

`root #``parted "/dev/loop0" "unit mib print"`
Model: Loopback device (loopback)
Disk /dev/loop0: 4096MiB
Sector size (logical/physical): 512B/512B
Partition Table: msdos
...

Next, create an [ext4](https://wiki.gentoo.org/wiki/Ext4) partition with an offset of 2 MiB:

`root #``parted "/dev/loop0" "mkpart primary ext4 2MiB -1"`
The value of `-1` represents the last sector of the partition.

To check that the first partition has been successfully created:

`root #``parted "/dev/loop0" "unit mib print"`
Model: Loopback device (loopback)
Disk /dev/loop0: 4096MiB
Sector size (logical/physical): 512B/512B
Partition Table: msdos
Disk Flags:
 
Number  Start    End      Size     Type     File system  Flags
 1      2.00MiB  4095MiB  4093MiB  primary

This will also attach a new loop device at /dev/loop0p1:

`root #``ls -l "/dev/loop0"*`
brw-rw---- 1 root disk   7, 0 Apr 12 12:33 /dev/loop0
brw-rw---- 1 root disk 259, 0 Apr 12 12:33 /dev/loop0p1

Set the boot flag:

`root #``parted "/dev/loop0" set 1 boot on`
All partition flags can be found in the "Flags" column:

`root #``parted "/dev/loop0" "unit mib print"`
Model: Loopback device (loopback)
Disk /dev/loop0: 4096MiB
Sector size (logical/physical): 512B/512B
Partition Table: msdos
Disk Flags:
 
Number  Start    End      Size     Type     File system  Flags
 1      2.00MiB  4095MiB  4093MiB  primary               boot

Create the ext4 filesystem declared via parted earlier:

`root #``mkfs.ext4 "/dev/loop0p1"````
mke2fs 1.47.2 (1-Jan-2025)
Discarding device blocks: done
Creating filesystem with 1047808 4k blocks and 262144 inodes
Filesystem UUID: 0e344af7-6f7b-4d27-8238-89d46a5920d6
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912, 819200, 884736
 
Allocating group tables: done
Writing inode tables: done
Creating journal (16384 blocks): done
Writing superblocks and filesystem accounting information: done
```
Mount it at /mnt:

`root #``mount /dev/loop0p1 /mnt``root #``df --human-readable --print-type "/mnt/"`
Filesystem     Type  Size  Used Avail Use% Mounted on
/dev/loop0p1   ext4  3.9G   24K  3.7G   1% /mnt

Create the /mnt/boot/grub directory, which will be used by GRUB later:

`root #``mkdir --parents --verbose "/mnt/boot/grub"`
mkdir: created directory '/mnt/boot'
mkdir: created directory '/mnt/boot/grub'

Install GRUB on the loop device, instructing it to install its files to /mnt/boot/grub/:

`root #``grub-install --target="i386-pc" --boot-directory="/mnt/boot/" "/dev/loop0"``root #``tree -F "/mnt/boot/grub/"`
/mnt/boot/grub/
├── fonts/
│   └── unicode.pf2
├── grubenv
├── i386-pc/
│   ├── acpi.mod
│   ├── adler32.mod
│   ├── affs.mod
│   ├── afs.mod
│   ├── afsplitter.mod
│   ├── ahci.mod
│   ├── all\_video.mod
│   ├── aout.mod
│   ├── archelp.mod
│   ├── ata.mod
\[...\]

Unmount the filesystem and detach the loop device:

`root #````
umount "/mnt/"
```
`root #````
losetup --detach "/dev/loop0"
```
If the loop device is still busy - for example, processes are still accessing /mnt/ - no error will be returned. This can be verified and solved with the following commands:

`root #``losetup --list`
NAME       SIZELIMIT OFFSET AUTOCLEAR RO BACK-FILE                                  DIO LOG-SEC
/dev/loop0         0      0         0  0 /home/larry/qemu/my-systems-disk-image.img   0     512

`root #``lsof | grep "/mnt"`
sleep     31813                       root cwd       DIR              259,0      4096              131074 /mnt/boot/grub

`root #``kill -SIGTERM 31813`
This is sufficient to boot into a GRUB boot prompt.

This setup can be used as the basis for a bootable system.

QEMU supports around 34 different CPU architectures. To list those available:

`user $``ls "/usr/bin/qemu-system-"*`
/usr/bin/qemu-system-aarch64       /usr/bin/qemu-system-mips      /usr/bin/qemu-system-rx
/usr/bin/qemu-system-alpha         /usr/bin/qemu-system-mips64    /usr/bin/qemu-system-s390x
/usr/bin/qemu-system-arm           /usr/bin/qemu-system-mips64el  /usr/bin/qemu-system-sh4
/usr/bin/qemu-system-avr           /usr/bin/qemu-system-mipsel    /usr/bin/qemu-system-sh4eb
/usr/bin/qemu-system-cris          /usr/bin/qemu-system-nios2     /usr/bin/qemu-system-sparc
/usr/bin/qemu-system-hppa          /usr/bin/qemu-system-or1k      /usr/bin/qemu-system-sparc64
/usr/bin/qemu-system-i386          /usr/bin/qemu-system-ppc       /usr/bin/qemu-system-tricore
/usr/bin/qemu-system-loongarch64   /usr/bin/qemu-system-ppc64     /usr/bin/qemu-system-x86\_64
/usr/bin/qemu-system-m68k          /usr/bin/qemu-system-ppc64le   /usr/bin/qemu-system-x86\_64-microvm
/usr/bin/qemu-system-microblaze    /usr/bin/qemu-system-riscv32   /usr/bin/qemu-system-xtensa
/usr/bin/qemu-system-microblazeel  /usr/bin/qemu-system-riscv64   /usr/bin/qemu-system-xtensaeb

To get a list of CPUs for a specific architecture, use the `-cpu help` option with the binary for that architecture, e.g.:

`user $``qemu-system-x86_64 -cpu help`
Available CPUs:
  486                   (alias configured by machine type)
  486-v1                
  Broadwell             (alias configured by machine type)
  Broadwell-IBRS        (alias of Broadwell-v3)
  Broadwell-noTSX       (alias of Broadwell-v2)
...

As noted in the introduction, QEMU CPUs can have additional support for accelerators. An accelerator can usually only accelerate the features available on the host CPU, so the selection of CPU affects performance.

To list available accelerators, pass the `-accel help` option to the relevant binary, e.g.:

`user $``qemu-system-x86_64 -accel help`
Accelerators supported in QEMU binary:
tcg
mshv
kvm

To start QEMU with a VNC server listening on a local UNIX socket:

`user $``qemu-system-x86_64 -vnc "unix:/run/user/$(id -u)/qemu-vnc.sock" -enable-kvm -cpu host -drive "file=/home/larry/qemu/my-systems-disk-image.img,format=raw" -m 2G`
A CD-ROM can be added by using the `-cdrom` option, e.g. `-cdrom <image>`, where `<image>` should be replaced with the name of an ISO image.

Any VNC viewer can be used to connect to the VNC server, e.g. vncviewer, provided by [net-misc/tigervnc](https://packages.gentoo.org/packages/net-misc/tigervnc):

`user $``vncviewer "/run/user/$(id -u)/qemu-vnc.sock"`
TigerVNC viewer v1.15.0
Built on: 2025-05-13 12:30
Copyright (C) 1999-2025 TigerVNC team and many others (see README.rst)
See https://www.tigervnc.org for information on TigerVNC.
 
Tue May 13 14:44:36 2025
 DecodeManager: Detected 4 CPU core(s)
 DecodeManager: Creating 4 decoder thread(s)
 CConn:       Connected to socket /run/user/1000/qemu-vnc.sock
 CConnection: Server supports RFB protocol version 3.8
 CConnection: Using RFB protocol version 3.8
 CConnection: Choosing security type None(1)
 CConn:       Using pixel format depth 24 (32bpp) little-endian rgb888
 CConn:       SetDesktopSize failed: 3

This will open a separate window with the display output of the QEMU VM:

![Qemu minimal vm with grub2.png](https://wiki.gentoo.org/images/thumb/7/77/Qemu_minimal_vm_with_grub2.png/500px-Qemu_minimal_vm_with_grub2.png)


Refer to [QEMU/troubleshooting](https://wiki.gentoo.org/wiki/QEMU/troubleshooting).

`root #``emerge --ask --depclean --verbose app-emulation/qemu`
- [qemu-img](https://wiki.gentoo.org/wiki/Qemu-img) — a QEMU disk image utility
- [QEMU/Front-ends](https://wiki.gentoo.org/wiki/QEMU/Front-ends) — provide graphical, terminal, web-based, or command-line interfaces for configuring, managing, or accessing QEMU virtual machines.
- [QEMU/Guest/Gentoo Linux](https://wiki.gentoo.org/wiki/QEMU/Guest/Gentoo_Linux) — supplemental to Gentoo [Handbook](https://wiki.gentoo.org/wiki/Handbook:Main_Page) reference guide
- [QEMU/Files](https://wiki.gentoo.org/wiki/QEMU/Files) — complete list of files used by QEMU command
- [QEMU/Networking/Bridge with Wifi Routing](https://wiki.gentoo.org/wiki/QEMU/Networking/Bridge_with_Wifi_Routing)
- [QEMU/Networking/KVM IPv6 Support](https://wiki.gentoo.org/wiki/QEMU/Networking/KVM_IPv6_Support) — describes IPv6 support in QEMU/KVM.
- [QEMU/Networking/Open vSwitch network](https://wiki.gentoo.org/wiki/QEMU/Networking/Open_vSwitch_network)
- [Category:QEMU Guests](https://wiki.gentoo.org/wiki/Category:QEMU_Guests)

- [libvirt](https://wiki.gentoo.org/wiki/Libvirt) — a virtualization management toolkit
- [libvirt/QEMU guest](https://wiki.gentoo.org/wiki/Libvirt/QEMU_guest) — creation of a guest domain (virtual machine, VM), running inside a QEMU hypervisor, using tools found in [libvirt](https://packages.gentoo.org/packages/libvirt) package.
- [libvirt/QEMU networking](https://wiki.gentoo.org/wiki/Libvirt/QEMU_networking) — details the setup of Gentoo networking by [Libvirt](https://wiki.gentoo.org/wiki/Libvirt) for use by guest containers and [QEMU]-based virtual machines.
- [virt-manager](https://wiki.gentoo.org/wiki/Virt-manager) — lightweight GUI application designed for managing virtual machines and containers via the [libvirt](https://wiki.gentoo.org/wiki/Libvirt) API.
- [virt-manager/QEMU guest](https://wiki.gentoo.org/wiki/Virt-manager/QEMU_guest) — creation of a guest virtual machine (VM) running inside a QEMU hypervisor using just the virt-manager GUI tool.
- [GPU passthrough with virt-manager, QEMU, and KVM](https://wiki.gentoo.org/wiki/GPU_passthrough_with_virt-manager,_QEMU,_and_KVM) — directly present an internal PCI GPU as-is for direct use by a virtual machine

- [Comparison of virtual machines](https://wiki.gentoo.org/wiki/Comparison_of_virtual_machines) — compares the features of several platform virtual machines.
- [Fast Virtio VM](https://wiki.gentoo.org/wiki/Fast_Virtio_VM) — explains a way to build a blazing fast Gentoo VM under [KVM] using Virtio and mdev.
- [Remote desktop](https://wiki.gentoo.org/wiki/Remote_desktop) — a guide to **[remote desktop](https://en.wikipedia.org/wiki/Remote_desktop_software)** software on Gentoo
- [Virtualization](https://wiki.gentoo.org/wiki/Virtualization) — the concept and technique that permits running software in an environment separate from a computer operating system.

- [https://www.linux-kvm.org/page/KvmOnGentoo](https://www.linux-kvm.org/page/KvmOnGentoo) - The Gentoo article on the KVM wiki
- [https://wiki.qemu.org/Main\_Page](https://wiki.qemu.org/Main_Page) - The Official QEMU wiki
