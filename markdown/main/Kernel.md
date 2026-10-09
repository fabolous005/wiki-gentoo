<!-- source: https://wiki.gentoo.org/wiki/Kernel | group: Gentoo Wiki (Main) | wiki-title: Kernel -->
---
title: Kernel
url: https://wiki.gentoo.org/wiki/Kernel
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-05"
fingerprint: d793193e90e23f21
license: CC BY-SA 4.0
---

# Kernel

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

![](https://wiki.gentoo.org/images/thumb/a/af/Tux.png/250px-Tux.png)

The **[Linux](https://en.wikipedia.org/wiki/Linux_kernel) kernel** is a central part of the Gentoo [operating system (OS)](https://en.wikipedia.org/wiki/operating_system). It provides essential OS facilities such as [device drivers](https://en.wikipedia.org/wiki/Device_driver), [virtual consoles](https://wiki.gentoo.org/wiki/Terminal_emulator#Virtual_consoles_and_switching), [memory management](https://en.wikipedia.org/wiki/memory_management), [task scheduling](<https://en.wikipedia.org/wiki/Scheduling_(computing)>), [inter-process communication](https://en.wikipedia.org/wiki/inter-process_communication), [virtual filesystems](https://en.wikipedia.org/wiki/Virtual_file_system), etc.

Though Linux is a [monolithic kernel](https://en.wikipedia.org/wiki/monolithic_kernel), its [modular design](https://en.wikipedia.org/wiki/Loadable_kernel_module) means that code will only ever be loaded if required, allowing modules to be available without affecting performance or memory usage. Linux kernel deployments can therefore provide many device drivers and services, with little to no penalty on performance: unneeded modules will simply be ignored.

The kernel is predominantly written in [C](https://wiki.gentoo.org/wiki/C), and also uses [assembly](https://wiki.gentoo.org/wiki/Assembly_Language) and [Rust](https://wiki.gentoo.org/wiki/Rust).

## Which kernel to install?

Gentoo provides a choice of methods to get a kernel up and running, from a standard binary kernel as would be supplied by most distributions to a custom configured and compiled kernel.

### Distribution kernels

[Distribution Kernel](https://wiki.gentoo.org/wiki/Distribution_Kernel)

The [distribution kernel project](https://wiki.gentoo.org/wiki/Project:Distribution_Kernel) provides packages to install and manage kernels through [Portage](https://wiki.gentoo.org/wiki/Portage). These kernels are compiled (if needed) and installed with just an emerge command like any other package, which can lessen the administrative burden. Kernel updates are performed when [updating the system](https://wiki.gentoo.org/wiki/Upgrading_Gentoo) (e.g. emerge -avuDN @world).

These kernels come with a default configuration that should "just work" for most systems. For users not interested in configuring their own kernel from scratch, these kernels can get things up and running quicker:

#### gentoo-kernel-bin

The [sys-kernel/gentoo-kernel-bin](https://packages.gentoo.org/packages/sys-kernel/gentoo-kernel-bin) is a binary package containing a precompiled kernel, allowing for faster installation. This package is  a precompiled version of the gentoo-kernel package with a default configuration.

#### gentoo-kernel

The [sys-kernel/gentoo-kernel](https://packages.gentoo.org/packages/sys-kernel/gentoo-kernel) package provides a kernel that will be compiled and installed when the package is emerged. This comes with a default configuration that should work out of the box on most systems.

[sys-kernel/gentoo-kernel](https://packages.gentoo.org/packages/sys-kernel/gentoo-kernel) can also be configured to use a custom kernel config, which simplifies automation compared to using [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources).

Below are some examples of this:

##### Code snippet

If only a few config lines need to be changed, it is possible to apply the changes using .config files in /etc/kernel/config.d.

This could be something like applying CONFIG\_X86\_NATIVE\_CPU=y to tune performance to the build system, or setting CONFIG\_BLK\_DEV\_NVME=y to allow booting without [initramfs](https://wiki.gentoo.org/wiki/Initramfs).

This process is described in more detail in the [Distribution kernel config.d](https://wiki.gentoo.org/wiki/Distribution_Kernel#Using_.2Fetc.2Fkernel.2Fconfig.d) section

##### savedconfig

When a completely custom kernel config is required, it is usually better to use the `savedconfig` USE flag.

This allows a kernel config made with [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources) to be transferred to the distribution and kernel allows that config to applied to future updates.

Some consider this a better way to manage the kernel than [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources), because [sys-kernel/gentoo-kernel](https://packages.gentoo.org/packages/sys-kernel/gentoo-kernel) can automatically trigger portage rebuilds of out-of-tree modules such as [x11-drivers/nvidia-driver](https://packages.gentoo.org/packages/x11-drivers/nvidia-driver) with every kernel update, when the `dist-kernel` USE flag is set on those packages.

This process is described in more detail in the [Distribution kernel savedconfig](https://wiki.gentoo.org/wiki/Distribution_Kernel#Using_savedconfig) section

### gentoo-sources

When manually compiling kernel sources, Gentoo recommends the [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources) package for most users. Its stable versions follow the long term stable (LTS) kernels from upstream kernel.org.

## Installing kernel source code

To obtain a kernel, it is necessary to install the kernel source code. The recommended kernel sources for a Gentoo desktop system are [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources). These are maintained by the Gentoo developers, and patched when necessary to fix security vulnerabilities and functional problems, as well as to improve compatibility with rare system architectures.

### USE flags


### USE flags for
            [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources)
            
            Full sources including the Gentoo patchset for the 7.2 kernel tree

| [build](https://packages.gentoo.org/useflags/build) | !!internal use only!! DO NOT SET THIS FLAG YOURSELF!, used for creating build images and the first half of bootstrapping \[make stage1\] | 
| [experimental](https://packages.gentoo.org/useflags/experimental) | Apply experimental patches; for more information, see "https://wiki.gentoo.org/wiki/Project:Kernel/Experimental". | 
| [symlink](https://packages.gentoo.org/useflags/symlink) | Force kernel ebuilds to automatically update the /usr/src/linux symlink | 
| [vanilla](https://packages.gentoo.org/useflags/vanilla) | Do not add extra patches which change default behaviour; DO NOT USE THIS ON A GLOBAL SCALE as the severity of the meaning changes drastically | 

### Emerge

To install [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources):

`root #``emerge --ask sys-kernel/gentoo-sources`
#### Alternative kernels

There are many other kernel packages in the Portage tree. For details on many of these, see the [kernel packages](https://wiki.gentoo.org/wiki/Kernel/Packages) article. Further help on choosing a kernel can be found in developer Greg Kroah-Hartman's article [What Stable Kernel Should I Use?](http://kroah.com/log/blog/2018/08/24/what-stable-kernel-should-i-use/).

#### Searching all kernel packages

A full list of kernel sources with short descriptions can be found by searching with emerge:

`root #``emerge --search sources`
## Managing the kernel

### Configuration

- [Understanding manual configuration](https://wiki.gentoo.org/wiki/Kernel/Configuration)
- A guide on manual configuration offering a broader understanding of concepts.

- [Applying manual configuration](https://wiki.gentoo.org/wiki/Kernel/Gentoo_Kernel_Configuration_Guide)
- A guide on manual configuration providing the tools and steps needed to get the job done.

- [Deblobbing](https://wiki.gentoo.org/wiki/Kernel/Deblobbing)
- A guide to deblobbing the kernel

- [Security](https://wiki.gentoo.org/wiki/Security_Handbook/Kernel_security)
- Instructions for hardening the kernel.

- [Modules](https://wiki.gentoo.org/wiki/Kernel_Modules)
- Modules are object files that contain code to extend the kernel.

- [Optimization](https://wiki.gentoo.org/wiki/Kernel/Optimization)
- Descriptions of various optimizations for the kernel, including speed and hardening.

- [Command-line parameters](https://wiki.gentoo.org/wiki/Kernel/Command-line_parameters)
- Descriptions of some commonly useful command-line parameters which can be passed to the kernel at boot time for troubleshooting.

### Upgrade

- [Kernel upgrade](https://wiki.gentoo.org/wiki/Kernel/Upgrade)
- Steps to upgrade to a new kernel using an existing configuration.

### Removal

- [Kernel removal](https://wiki.gentoo.org/wiki/Kernel/Removal)
- Steps to completely remove old kernels.

## Troubleshooting

### Kernel configuration support

See the [IKCONFIG support](https://wiki.gentoo.org/wiki/Kernel/IKCONFIG_support) sub-article.

### Kernel command-line parameters

When booting from a bootloader, the Linux kernel can accept command-line parameters to change its behavior. This can help, for example, in troubleshooting the kernel at boot time, or to blacklist a certain module that should not be loading. See Gentoo's [Kernel/Command-line parameters](https://wiki.gentoo.org/wiki/Kernel/Command-line_parameters) article for more details.

Kernel.org has a nicely formatted list of available [kernel command-line parameters](https://www.kernel.org/doc/html/latest/admin-guide/kernel-parameters.html) in their admin guide.

## See also

- [fwupd](https://wiki.gentoo.org/wiki/Fwupd) — a daemon that provides a safe, reliable way of applying firmware updates on Linux.
- [Handbook:AMD64/Installation/Kernel](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Kernel) — handbook page about installing and setting up a kernel
- [Linux firmware](https://wiki.gentoo.org/wiki/Linux_firmware) — contains [binary blobs](https://en.wikipedia.org/wiki/binary_blobs) of firmware necessary for partial or full functionality of certain hardware devices on Linux systems.
- [The kernel category](https://wiki.gentoo.org/wiki/Category:Kernel) — all the kernel related articles on the wiki.
- [The hardware category](https://wiki.gentoo.org/wiki/Category:Hardware) — lists of hardware stacks with associated kernel configurations.

## External resources

- [planet.kernel.org](http://planet.kernel.org/) - Blogs related to the Linux kernel.
- [kernelnewbies.org](https://kernelnewbies.org/) - A "community of aspiring Linux kernel developers who work to improve their kernels, as well as more experienced developers willing to share their knowledge".
- [kernel.org/doc/](https://www.kernel.org/doc/) - Official comprehensive documentation for the Linux kernel.
- [What Stable Kernel Should I Use?](http://kroah.com/log/blog/2018/08/24/what-stable-kernel-should-i-use/) - An article by kernel developer Greg Kroah-Hartman.
- [Building the kernel as root can be harmful](https://forums.gentoo.org/viewtopic-t-1064076.html)
- [The Linux Kernel Module Programming Guide](https://github.com/sysprog21/lkmpg/)
