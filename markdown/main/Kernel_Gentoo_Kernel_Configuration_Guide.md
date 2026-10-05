<!-- source: https://wiki.gentoo.org/wiki/Kernel/Gentoo_Kernel_Configuration_Guide | group: Gentoo Wiki (Main) | wiki-title: Kernel/Gentoo Kernel Configuration Guide -->
---
title: Kernel/Gentoo Kernel Configuration Guide
url: https://wiki.gentoo.org/wiki/Kernel/Gentoo_Kernel_Configuration_Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-26"
fingerprint: "96931b1a10a23f04"
license: CC BY-SA 4.0
---

# Kernel/Gentoo Kernel Configuration Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


This document aims to introduce the concepts of **manual kernel configuration** and details some of the most common configuration pitfalls.

## Introduction

Gentoo provides two ways for users to handle kernel configuration, installation, and upgrades: *automatic* (such as genkernel or Dracut) and *manual*. Although the automatic method can be regarded as easier for most users, there are a number of reasons why a large proportion of Gentoo users choose to configure their kernels manually:

- Greater flexibility and control over configuration
- Smaller (kernel) sizes
- Shorter compilation times
- The learning experience

This guide does not attempt to document the manual configuration process from start to finish — the configuration process relies upon a large degree of common sense and a relatively high level of technical knowledge about the system being used. Instead it will introduce the concepts of manual configuration and detail the most common pitfalls which users face.

At this point, the user is presumed to have Linux kernel sources unpacked on the hard disk (usually somewhere under /usr/src), and is expected to know how to enter the menuconfig or nconfig configuration utility with knowledge to navigate through the ncurses-based menu system. If the user is not at this stage, other documentation is available to help. Read the following articles, then return to this guide:

- The [kernel packages article](https://wiki.gentoo.org/wiki/Kernel/Packages) contains information on the various kernel source packages available in the Portage tree.
- The [Kernel upgrade article](https://wiki.gentoo.org/wiki/Kernel/Upgrade) explains how to upgrade a kernel or switch from one kernel to another.

## Configuration concepts

### The basics

The general process is actually rather simple: a series of options, categorized into individual menus and sub-menus, are presented and the desired hardware support and kernel features relevant to the system are selected.

The kernel includes a *default configuration*, which is presented the first time menuconfig is run on a particular set of sources. The defaults are generally broad and sensible, which means that the majority of users will only have to make a small number of changes to the base configuration. When deciding to disable an option that was enabled from kernel's default configuration, make sure a good understanding has been obtained of exactly what that option does, and the consequences of disabling it.

During a first time Linux kernel configuring, aim to be conservative; do not be too adventurous, and try to make as few modifications to the default settings as possible. At the same time, keep in mind that there are certain parts to a system's setup that must be customized to actually allow for the system to boot.

### Built-in vs modular

Most configuration options are *tristate*: they can be either not built at all `(N)`, built directly into the kernel `(Y)`, or built as a module `(M)`. Modules are stored externally on the filesystem, whereas built-in items are built directly into the kernel image itself.

There is an important difference between [built-in and modular](https://wiki.gentoo.org/wiki/Kernel_Modules): with a few exceptions, the kernel makes no attempt whatsoever to load any external modules when the system might need them; it is left up to the user to decide when, or when to not, load a module. While certain other parts of the system may have load-on-demand facilities, and there are some automatic module loading utilities available, it is recommended to build hardware support and kernel features directly into the kernel. The kernel can then ensure the functionality and hardware support is available whenever needed. This is done by setting each kernel feature to `(Y)`. For this setup to be coherent it is also necessary to include firmware support in the kernel. For details see the [Linux firmware](https://wiki.gentoo.org/wiki/Linux_firmware) article.

For other parts of the configuration, built-in is an absolute requirement. For example, if the root partition is a btrfs filesystem the system can't boot if btrfs was built as a module. The system would have to look on the root partition to find the btrfs module (since modules are stored in the root partition), but it cannot look on the root partition unless it already has btrfs support loaded! If btrfs has not been built-in then the init process will fail to find the root device.

See the article on [kernel Modules](https://wiki.gentoo.org/wiki/Kernel_Modules) for more information, and the instructions on [using menuconfig](https://wiki.gentoo.org/wiki/Kernel/Configuration#Usage) to configure the kernel.

### Hardware support

Beyond detecting the *architecture type* of the system, the configuration utility makes no attempt to identify what hardware is actually present in the system. While there are default settings for *some* hardware support, users almost certainly need to find and select the configuration options relevant to each system's hardware configuration.

Selecting the proper configuration options requires a knowledge of the components inside and connected to the computer. Most of the time these components can be identified without taking the lid off the system. For most internal components, users need to identify the *chipset* used on each device, rather than the retailed product name. Many expansion cards are retailed with a certain brand name, but use another manufacturer's chipset.

There are some utilities available to help users determine what kernel configuration options to use. lspci (part of the [sys-apps/pciutils](https://packages.gentoo.org/packages/sys-apps/pciutils) package) will identify PCI-based and AGP-based hardware, this includes components built onto the motherboard itself. lsusb (from the [sys-apps/usbutils](https://packages.gentoo.org/packages/sys-apps/usbutils) package) will identify various devices connected to the system's USB ports.

The situation is somewhat confused by varying degrees of standardization in the hardware world. Unless the user selects extreme deviation from the default configuration settings, the IDE hard disks should "just work", as will the PS/2 or USB keyboard and mouse. Basic VGA display support is also included. However, some devices such as Ethernet adapters are hardly standardized at all; for these devices users will have to identify the Ethernet chipset and select the appropriate hardware support for the specific card to get network access.

In addition, while some things just-about-work with the default settings, more specialized options may need to be selected to get the full potential from the system. For example, if support for the appropriate IDE chipset has not been enabled, the IDE hard disks will run *very* slowly.

It is recommended for drivers that require firmware to be configured as modules to more easily load the firmware from disk.  Common groups that should be included here are GPU and networking drivers (when not using an [NFS](https://wiki.gentoo.org/wiki/NFS), or similar networked based, rootfs).

### Kernel features

In addition to hardware support, users need to consider the software features that will be required in the kernel. One important example of such a feature is filesystem support: users must select support for the filesystems in use on their hard disks, as well as any filesystems they might use on external storage devices (e.g. VFAT on USB drives).

Another common software feature example is advanced network functionality. In order to do some kind of routing or firewalling the relevant configuration items must be included in the kernel configuration.

### Ready?

Now that the concepts have been introduced, it should be easy to start identifying the system hardware, browsing through the menuconfig interface, and selecting the required kernel options for the system.

The rest of this guide should clear up common areas of confusion, and provide advice for how to avoid common problems which users often run into. Best wishes!

## Configuring the kernel

### Enabling required options

When using [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources), it is strongly recommend the Gentoo-specific configuration options be enabled. These ensure that a minimum of kernel features required for proper functioning is available:

**Enabling Gentoo-specific options**

Naturally the choice in the last two lines depends on the selected init system ([OpenRC](https://wiki.gentoo.org/wiki/OpenRC) vs. [systemd](https://wiki.gentoo.org/wiki/Systemd)). It does not hurt to have support for both init systems enabled.

When using [sys-kernel/vanilla-sources](https://packages.gentoo.org/packages/sys-kernel/vanilla-sources), the additional selections for init systems will be unavailable. Enabling support is possible.

### Enabling support for typical system components

Make sure that every driver that is vital to the booting of the system (such as SATA controllers, NVMe block device support, filesystem support, etc.) is compiled in the kernel and not as a module, otherwise the system may not be able to boot completely.

Next select the exact processor type. It is also recommended to enable MCE features (if available) so that users are able to be notified of any hardware problems. On some architectures (such as x86\_64), these errors are not printed to dmesg, but to /dev/mcelog. This requires the [app-admin/mcelog](https://packages.gentoo.org/packages/app-admin/mcelog) package.

Also select *Maintain a devtmpfs file system to mount at /dev* so that critical device files are already available early in the boot process (`CONFIG_DEVTMPFS` and `CONFIG_DEVTMPFS_MOUNT`):

**Enabling devtmpfs support (`CONFIG_DEVTMPFS`)**

### SATA and NVMe disks

Most modern desktop systems ship with storage devices (hard disk and CD/DVD drives) on a [Serial ATA](https://en.wikipedia.org/wiki/SATA) bus, rather than the older [Parallel ATA IDE](https://en.wikipedia.org/wiki/Parallel_ATA) (ribbon cable) bus type.

SATA support in Linux is implemented in a layer referred to as *libata*, which sits below the SCSI subsystem. For this reason, SATA drivers are found in the SCSI driver section of the configuration. Additionally, the system's storage devices will be treated as SCSI devices, which means SCSI disk/cdrom support will also be required. The first SATA hard disk will be named /dev/sda and the first SATA CD/DVD drive will be named /dev/sr0.

Although the majority of these drivers are for SATA controllers, libata was not designed to be SATA-specific. All common IDE drivers were also ported to libata, and at this point, the above considerations will also apply for IDE users.

**Configuration options for libata**

**Enable basic NVMe support for Linux 5.x.x (`CONFIG_BLK_DEV_NVME`)**

It does not hurt to enable the following additional NVMe support:

**Enabling additional NVMe support (`CONFIG_NVME_MULTIPATH`, `CONFIG_NVME_MULTIPATH`, `CONFIG_NVME_HWMON`, `CONFIG_NVME_FC`, `CONFIG_NVME_TCP`, `CONFIG_NVME_TARGET`, `CONFIG_NVME_TARGET_PASSTHRU`, `CONFIG_NVME_TARGET_LOOP`, `CONFIG_NVME_TARGET_FC`, `CONFIG_NVME_TARGET_FCLOOP`, `CONFIG_NVME_TARGET_TCP`)**

Now go to File Systems and select support for the filesystems that will be used by the system. Do not compile the file system that is used for the root filesystem as module, otherwise the system may not be able to mount the partition. Also select *Virtual memory* and */proc file system*. Select one or more of the following options as needed by the system:

### File system support

**Enable file system support (`CONFIG_EXT2_FS`, `CONFIG_EXT3_FS`, `CONFIG_EXT4_FS`, `CONFIG_BTRFS_FS`, `CONFIG_XFS_FS`, `CONFIG_MSDOS_FS`, `CONFIG_VFAT_FS`, `CONFIG_PROC_FS`, and `CONFIG_TMPFS`)**

Note that ext2 and ext3 are redundant since the [ext4](https://wiki.gentoo.org/wiki/Ext4) driver already supports all ext versions.

### USB host controllers

[USB](https://wiki.gentoo.org/wiki/USB) is a widely adopted bus for connecting external peripherals to a computer. One of the reasons behind the success of USB is that it is a standardized protocol, however the USB *host controller devices (HCDs)* implemented on the host computer do vary a little. There are 4 main types:

1. `UHCI` is the Universal Host Controller Interface. It supports USB 1.1, and is usually found on motherboards based on a VIA or Intel chipset.
2. `OHCI` is the Open Host Controller Interface. It supports USB 1.1 and is usually found on motherboards based on an NVIDIA or SiS chipset.
3. `EHCI` is the Extended Host Controller Interface. It is the only common host controller to support USB 2.0, and can typically be found on any computer that supports USB 2.0.
4. `XHCI` is the eXtensible Host Controller Interface. It is the host controller for USB 3.0 and is compatible with USB 1.0, 1.1, 2.0, 3.0 and future speeds. Enable this feature when the board supports USB 3.0.

Most systems come with two of the above interface types: XHCI (USB 3.0) *and* EHCI (USB 2.0). To use USB devices, it is no longer necessary to select both options since XHCI is compatible with slower USB-controllers. Users can also enable EHCI to be "extra" safe; it does no harm if USB 2.0 controllers are unavailable.

If the relevant options corresponding to the USB HCD types present on the system are not selected, then 'dead' USB ports may be experienced. This case can be determined if a working USB device is plugged in, but it does not get power or respond in any way.

A neat lspci trick (from the [sys-apps/pciutils](https://packages.gentoo.org/packages/sys-apps/pciutils) package) makes it relatively easy to detect which HCDs are present on system. Ignoring the SATA controller which was also matched, it is easy to spot that this system requires EHCI and XHCI support:

`root #``lspci -v | grep HCI`
00:14.0 USB controller: Intel Corporation 8 Series/C220 Series Chipset Family USB xHCI (rev 04) (prog-if 30 \[XHCI\])
00:1a.0 USB controller: Intel Corporation 8 Series/C220 Series Chipset Family USB EHCI #2 (rev 04) (prog-if 20 \[EHCI\])
00:1d.0 USB controller: Intel Corporation 8 Series/C220 Series Chipset Family USB EHCI #1 (rev 04) (prog-if 20 \[EHCI\])
00:1f.2 SATA controller: Intel Corporation 8 Series/C220 Series Chipset Family 6-port SATA Controller 1 \[AHCI mode\] (rev 04) (prog-if 01 \[AHCI 1.0\])

Select the HCDs present on the system. In general select all three options for maximum support, or if the correct option is uncertain:

In Linux kernel 3.12.13 and later, `OHCI support for PCI-bus USB controllers` (`USB_OHCI_HCD_PCI`) has to be enabled if the USB controller is OHCI and a USB keyboard or mouse is used.

### Multiprocessor, hyper-threading, and multi-core systems

Many computer systems are based on multiple processors, but not always in an immediately obvious way.

- Nearly every recent Intel/AMD/IBM CPU supports the [simultaneous multithreading](https://en.wikipedia.org/wiki/simultaneous_multithreading) (SMT) technology which is also known as [hyper-threading](https://en.wikipedia.org/wiki/hyper-threading). This technology enables a single CPU to be viewed by the system as two or more *logical* processors.
- Most recent CPUs actually consist of multiple physical processors inside a single package, these processors are known as [multi-core](https://en.wikipedia.org/wiki/multi-core_processor) processors.
- Some high-end computer systems actually have multiple physical processors installed on specialized motherboards to provide a significant performance increase over a *uniprocessor* system. System users will probably know if they have such a system, since they are not cheap.

In all of these cases, the appropriate kernel options must be selected to obtain optimum performance from these setups:

**Configuration for multi-processing support**

The next option not only enables power management features, but might also be a requirement for making all CPUs available to the system:

**Power management for multi-processor systems**

### Compressed kernel modules

From kernel version 3.18.x (and up) compression of kernel modules has been possible. It is important to emerge [sys-apps/kmod](https://packages.gentoo.org/packages/sys-apps/kmod) with the proper USE flags *before* compiling a kernel with compressed modules:

**`/etc/portage/package.use/kmod`**

**Enabling compression support for kmod**

Re-emerge [sys-apps/kmod](https://packages.gentoo.org/packages/sys-apps/kmod):

`root #``emerge --ask --oneshot --changed-use sys-apps/kmod`
Enable module compression and select a preferred compression method:

**Enable module compression**

`[*] Enable loadable module support` [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_MODULES</code> to find this item. --->
  Module compression mode () --->
    ( ) None
    (X) GZIP
    ( ) XZ
    ( ) ZSTD
### PPPoE

If PPPoE is used to connect to the Internet, or a dial-up modem, then enable the following options (`CONFIG_PPP`, `CONFIG_PPP_ASYNC`, and `CONFIG_PPP_SYNC_TTY`):

**Enabling PPPoE support (`PPPoE`, `CONFIG_PPPOE`, `CONFIG_PPP_ASYNC`, `CONFIG_PPP_SYNC_TTY`)**

Usually make modules\_install runs depmod. If [sys-apps/kmod](https://packages.gentoo.org/packages/sys-apps/kmod) did not have the proper USE flags set (see the package.use step above) the first time it was run, then the dependency list will be empty. The system will therefore be unable to load any modules that were built compressed.

After kmod has been recompiled, re-run depmod as a solution to this problem:

`root #````
depmod -a
```
`root #````
modprobe <module_name>
```
### Signed kernel modules and SecureBoot

To automatically sign the kernel modules enable `CONFIG_MODULE_SIG_ALL`:

**Sign kernel modules (`CONFIG_MODULE_SIG_ALL`)**

Optionally change the hash algorithm if desired.

To enforce that all modules are signed with a valid signature, enable `CONFIG_MODULE_SIG_FORCE` as well:

**Enforce signed kernel modules (`CONFIG_MODULE_SIG_FORCE`)**

To use a custom key, specify the location of this key in `CONFIG_MODULE_SIG_KEY`. If unspecified, the kernel build system will generate a key. It is recommended to generate one manually instead. This can be done with:

`root #``openssl req -new -nodes -utf8 -sha256 -x509 -outform PEM -out kernel_key.pem -keyout kernel_key.pem`
OpenSSL will ask some questions about the user generating the key, it is recommended to fill in these questions as detailed as possible.

Store the key in a safe location, at the very least the key should be readable only by the root user. Verify this with:

`root #``ls -l kernel_key.pem`
-r-------- 1 root root 3164 Jan  4 10:38 kernel\_key.pem

If this outputs anything other then the above, correct the permissions with:

`root #````
chown root:root kernel_key.pem
```
`root #````
chmod 400 kernel_key.pem
```
**Specify signing key (`CONFIG_MODULE_SIG_KEY`)**

To also sign external kernel modules installed by other packages via `linux-mod-r1.eclass`, enable the [modules-sign](https://packages.gentoo.org/useflags/modules-sign) [USE flag globally:](https://wiki.gentoo.org/wiki/USE_flag)

**`/etc/portage/make.conf`**

**Enable module signing**

```
USE="modules-sign"
# Optionally, when using custom signing keys.
MODULES_SIGN_KEY="/path/to/kernel_key.pem"
MODULES_SIGN_CERT="/path/to/kernel_key.pem" # Only required if the MODULES_SIGN_KEY does not also contain the certificate
MODULES_SIGN_HASH="sha512" # Defaults to sha512
```
#### Signing the kernel image (Secure Boot)

When signing the kernel image (for use on systems with [Secure Boot](https://wiki.gentoo.org/wiki/Secure_Boot) enabled) it is recommended to set the following kernel config options:

**Lockdown for secureboot**

Where ""image"" is a placeholder for the architecture specific image name. These options, from the top to the bottom: enforces that the kernel image in a kexec call must be signed (kexec allows replacing the kernel in-place), enforces that kernel modules are signed, enables lockdown `integrity` mode (prevents modifying the kernel at runtime), and enables various keychains.

On arches that do not natively support decompressing the kernel (e.g. **arm64** and **riscv**), the kernel must be built with its own decompressor (zboot):

**zboot (`CONFIG_EFI_ZBOOT`)**

After compilation of the kernel, as explained in the next section, the kernel image must be signed. First install [app-crypt/sbsigntools](https://packages.gentoo.org/packages/app-crypt/sbsigntools) and then sign the kernel image:

`root #``emerge --ask app-crypt/sbsigntools``root #``sbsign /usr/src/linux-x.y.z/path/to/kernel-image --cert /path/to/kernel_key.pem --key /path/to/kernel_key.pem --output /usr/src/linux-x.y.z/path/to/kernel-image`
Then proceed with the installation.

To automatically sign EFI executables installed by other packages, enable the [secureboot](https://packages.gentoo.org/useflags/secureboot) [USE flag globally:](https://wiki.gentoo.org/wiki/USE_flag)

**`/etc/portage/make.conf`**

**Enable Secure Boot**

```
USE="modules-sign secureboot"
# Optionally, to use custom signing keys.
MODULES_SIGN_KEY="/path/to/kernel_key.pem"
MODULES_SIGN_CERT="/path/to/kernel_key.pem" # Only required if the MODULES_SIGN_KEY does not also contain the certificate.
MODULES_SIGN_HASH="sha512" # Defaults to sha512
# Optionally, to boot with secureboot enabled, may be the same or different signing key.
SECUREBOOT_SIGN_KEY="/path/to/kernel_key.pem"
SECUREBOOT_SIGN_CERT="/path/to/kernel_key.pem"
```
### Architecture specific kernel configuration

#### alpha

**alpha recommended options**

#### amd64

Make sure to select IA32 Emulation and 32-bit time\_t if 32-bit programs should be supported (`CONFIG_IA32_EMULATION` and `CONFIG_COMPAT_32BIT_TIME`). Gentoo installs a multilib system (mixed 32-bit/64-bit computing) by default, so unless a no-multilib profile is used, these options are required.

**Selecting processor types and features**

Enable GPT partition label support if that was used previously when partitioning the disk (`CONFIG_PARTITION_ADVANCED` and `CONFIG_EFI_PARTITION`):

**Enable support for GPT**

Enable EFI stub support, EFI variables and EFI Framebuffer in the Linux kernel if UEFI is used to boot the system (`CONFIG_EFI`, `CONFIG_EFI_STUB`, `CONFIG_EFI_MIXED`, `CONFIG_EFIVAR_FS`, and `CONFIG_FB_EFI`):

**Enable support for UEFI**

To enable the Kernel options for the use of [SOF Firmware](https://wiki.gentoo.org/index.php?title=Handbook:Kernel/Installation/Kernel&action=edit&redlink=1) covered earlier:

**Enabling SOF Firmware support (`CONFIG_SND_SOC_SOF_TOPLEVEL`, `CONFIG_SND_SOC_SOF_PCI`, `CONFIG_SND_SOC_SOF_ACPI`, `CONFIG_SND_SOC_SOF_AMD_TOPLEVEL`, `CONFIG_SND_SOC_SOF_INTEL_TOPLEVEL`)**

#### ppc

**ppc32 recommended options**

#### sparc

**sparc recommended options**

#### x86

Due to limitations in the 32-bit address space of the **x86** architecture, a kernel with default configuration can only support up to 896 MB RAM. If a system has more memory, only the first 896 MB will be visible, unless high memory support has been enabled.

High memory support is not enabled by default, because it introduces a small system overhead. Do not be distracted by this, the overhead is insignificant when compared to the performance increase of having more memory available!

Choose the 4 GB option, unless the system has more than 4 GB of RAM:

**Enabling high memory support on x86**

## Compiling and installing

With the configuration now done, it is time to compile and install the kernel. Exit the configuration and start the compilation process:



`root #``make && make modules_install`
When the kernel has finished compiling, copy the kernel image to /boot/. This is handled by the make install command:

`root #``make install`
This command will copy the kernel image to /boot. If [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel) is installed it will call /sbin/installkernel instead and delegate the kernel installation. Instead of simply copying the kernel to /boot, [Installkernel](https://wiki.gentoo.org/wiki/Installkernel) installs each kernel with its version number in the file name. Additionally, installkernel provides a framework for automatically accomplishing various tasks relating to kernel installation, such as: generating an [initramfs](https://wiki.gentoo.org/wiki/Initramfs), building an [Unified Kernel Image](https://wiki.gentoo.org/wiki/Unified_Kernel_Image), and updating the [bootloader](https://wiki.gentoo.org/wiki/Bootloader) configuration.

## Kernel configuration shorthand notation

### Introduction

When reading about kernel configuration, often times settings are described as `CONFIG_<something>`. This short-hand notation is what the kernel configuration actually uses internally, and is what will be found in the kernel configuration file (be it /usr/src/linux/.config or in the auto-generated /proc/config.gz file). Of course, using short-hand notation would not do much good if this cannot translate this to the real location in the kernel configuration. The make menuconfig tool makes this possible.

### Translating CONFIG\_FOO to the real configuration location

Suppose the `CONFIG_TMPFS_XATTR` feature needs to be enabled. Launch the kernel configuration menu (make menuconfig) and press the `/` key. This will open a search box. In the search box, type `CONFIG_TMPFS_XATTR`.

The following is an output of the result of this search:

**Result of searching menuconfig for "CONFIG\_TMPFS\_XATTR"**

This output yields lots of interesting information.

| Entry | Description | 
|---|---|
| Symbol: TMPFS\_XATTR \[=n\] | This identifies the kernel configuration entry being searched for. It also shows that this setting is currently *not enabled* (\[=n\]). | 
| Type: boolean | The setting searched for is a boolean (which means it can be one of two options: enabled or disabled). Some settings are numbers or strings. | 
| Prompt: Tmpfs extended attributes | This is the text found in the make menuconfig entry that controls the variable ( `TMPFS_XATTR`) in the .config file. It is essentially the variable name in a more human readable format. | 
| Depends on: TMPFS \[=y\] | Before this entry can be seen `CONFIG_TMPFS` must be enabled. In this case it is already done (hence the \[=y\]) but if this is not the case, first look for (and enable) `CONFIG_TMPFS`. | 
| Location: ... | This is the location in the make menuconfig structure where the setting can be found. Remember, the setting to look for is *Tmpfs extended attributes*. | 
| Selected by: TMPFS\_POSIX\_ACL \[=n\] && TMPFS \[=y\] | If the settings described here are both enabled (in this case the first one is not), then `CONFIG_TMPFS_XATTR` will be automatically enabled and will not be possible to be *disabled* until one of these settings is de-selected. | 

With this information, it should be possible to translate any `CONFIG_*` requirements fairly easily. In short, it means a user must:

1. Enable the settings described in the *Depends on* field
2. Navigate where *Location:* points
3. Toggle the value referred to by *Prompt:*

## Other kernel configuration documentation

So far only general concepts and specific problems related to kernel configuration has been discussed; precise details have been left up to the user to discover. However, other parts of the Gentoo documentation collection provide specialized details for the topics at hand.

Such documents may be helpful while configuring specific areas of the kernel. Although this warning was mentioned previously in this guide, remember: users who are new to kernel configuration should not be adventurous when attempting to configure their kernels. Start by getting a basic system up and running, support for audio, printing, etc., can always be added at a later date.

Getting the basics of a kernel operational will help users in later configuration steps because the user will know what is breaking their system and what is not. It is always wise to save the base (working) kernel configuration in a folder other than the kernel's sources folder *before* attempting to add new features or hardware.

- The [ALSA article](https://wiki.gentoo.org/wiki/ALSA) details the configuration options required for sound card support. Note that ALSA is an exception to the suggested scheme of not building things as modules: ALSA is actually much easier to configure when the components are modular.

- The [Bluetooth article](https://wiki.gentoo.org/wiki/Bluetooth) details the options needed in order to use Bluetooth devices.

- The [IPv6 router guide](https://wiki.gentoo.org/wiki/IPv6_router_guide) describes how to configure the kernel for routing using the next generation network addressing scheme.

- If the closed-source NVIDIA graphics drivers will be used for improved 3D graphics performance, the [NVIDIA Guide](https://wiki.gentoo.org/wiki/NVIDIA/nvidia-drivers) lists the options that should and should not be selected on such a system.

- Amongst other things, the [Power Management guide](https://wiki.gentoo.org/wiki/Power_management/Guide) explains how to configure the kernel for CPU frequency scaling, and for suspend and hibernate functionality.

- If running a PowerPC system, the [PPC FAQ](https://wiki.gentoo.org/wiki/PPC/FAQ) has a few sections about PPC kernel configuration.

- The [Printing guide](https://wiki.gentoo.org/wiki/Printing) lists the kernel options needed to support printing in Linux.

- The [USB Guide](https://wiki.gentoo.org/wiki/USB/Guide) details the configuration settings required to use common USB devices such as keyboards, mice, storage devices, and USB printers.

## Troubleshooting

### Configuration changes do not take effect

It is very common for users to make a configuration change, but then make a small mistake in the process of actually booting to their newly configured kernel. They reboot into a kernel image that is not the one they just reconfigured, observe that whatever problem they were trying to solve is still present, and conclude that the configuration change does not solve the problem.

The process of compiling and installing kernels is outside the scope of this document; refer to the [Kernel Upgrade Guide](https://wiki.gentoo.org/wiki/Kernel/Upgrade) for general guidance. In short, the process to get a modified kernel is the following:

1. configure
2. compile
3. mount /boot (if not already mounted)
4. copy new kernel image to /boot
5. make sure the bootloader will reference the new kernel
6. reboot

If one of those final stages has been missed, then the changes will not properly take effect.

It is possible to verify if the kernel that has booted matches the newly kernel compiled on the hard disk. This is performed by examining the date and time of the kernel's compilation. Assuming the system architecture is **x86** and the kernel sources are installed at /usr/src/linux, the following command can be used:

`root #``uname -v`
#4 SMP PREEMPT Sat Jul 15 08:49:26 BST 2006

The above command will display the date and time the currently booted kernel was compiled.

`root #``ls -l /usr/src/linux/arch/i386/boot/bzImage`
-rw-r--r-- 1 dsd users 1504118 Jul 15 08:49 /usr/src/linux/arch/i386/boot/bzImage

The above command displays the date and time that the kernel image on the hard disk was last compiled.

If the time stamps from the above commands differ by more than 2 minutes, it indicates a mistake was made during kernel reinstallation and the system has not booted from the newly modified kernel image.

### Modules do not get loaded automatically

As mentioned earlier in this document, the kernel configuration system hides a large behavioral change when selecting a kernel component as a module `(M)` rather than built-in `(Y)`. It is worth repeating this again because so many users fall into this trap.

When selecting a component as built-in, the code is built into the kernel image (bzImage). When the kernel needs to use that component, it can initialize and load it automatically, without any user intervention.

When selecting a component as a module, the code is built into a kernel module file and installed on the filesystem. In general, when the kernel needs to use that component, it will not be able to find it. With some exceptions, the kernel makes no effort to actually load these modules — this task is left up to the user.

If building support for a network card as a module, and it is discovered the network is not accessible, it is probably because the module is not loaded — either this must be done manually or the system must be configured to autoload the module at boot time.

Unless a user has reason to do otherwise, some time can be saved by building these components directly into the kernel image, so that the kernel can automatically configure these small settings by itself.

## See also

- [Dracut](https://wiki.gentoo.org/wiki/Dracut) — an [initramfs](https://wiki.gentoo.org/wiki/Initramfs) infrastructure and aims to have as little as possible hard-coded into the initramfs.
- [Installkernel](https://wiki.gentoo.org/wiki/Installkernel) — a collection of scripts to automatically install new [kernels](https://wiki.gentoo.org/wiki/Kernel) and update [bootloader](https://wiki.gentoo.org/wiki/Bootloader) configuration
- [proc filesystem (Security Handbook)](https://wiki.gentoo.org/wiki/Security_Handbook/Firewalls_and_Network_Security#The_proc_filesystem) — dynamically change kernel parameters and variables on the fly.
