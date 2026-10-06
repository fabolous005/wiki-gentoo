<!-- source: https://wiki.gentoo.org/wiki/EFI_stub | group: Gentoo Wiki (Main) | wiki-title: EFI stub -->
---
title: EFI stub
url: https://wiki.gentoo.org/wiki/EFI_stub
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-01"
fingerprint: "5793811e90a73fe5"
license: CC BY-SA 4.0
---

# EFI stub

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

- CONFIG\_PM\_STD\_PARTITION for hibernation

An **EFI (boot) stub**<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> is a kernel that is an EFI executable (i.e., can boot directly from UEFI); EFI stub kernels may boot with or without a bootloader.

## Kernel configuration

### EFI stub support

The following kernel configuration options must be enabled:

**Enable EFI stub support for Kernels 6.1+**

```
Processor type and features  --->
    [*] EFI runtime service support 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_EFI</code> to find this item.
    [*]     EFI stub support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_EFI_STUB</code> to find this item.
    [ ]     EFI mixed-mode support (OPTIONAL) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_EFI_MIXED</code> to find this item.
## Installation

### Automated

Automated EFI stub booting is provided by [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel) when the [efistub](https://packages.gentoo.org/useflags/efistub) [USE flag is enabled; this relocates the regular boot layout from /boot to the EFI/Gentoo directory on the ESP.](https://wiki.gentoo.org/wiki/USE_flag)

#### Systemd kernel-install

When both the [efistub](https://packages.gentoo.org/useflags/efistub) [and](https://wiki.gentoo.org/wiki/USE_flag) [systemd](https://packages.gentoo.org/useflags/systemd) [USE flags are enabled on](https://wiki.gentoo.org/wiki/USE_flag) [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel), kernel-install calls kernel-bootcfg from [app-emulation/virt-firmware](https://packages.gentoo.org/packages/app-emulation/virt-firmware) to add or remove a boot entry for the installed or removed kernel. [Installkernel](https://wiki.gentoo.org/wiki/Installkernel) is called automatically by the kernel's make install or by the [Distribution Kernels'](https://wiki.gentoo.org/wiki/Distribution_Kernel) post-install phase; therefore, no special action is required when installing a new kernel, though the `kernel-bootcfg-boot-successful` init service from [app-emulation/virt-firmware](https://packages.gentoo.org/packages/app-emulation/virt-firmware) should be enabled to automatically make an entry for a new kernel permanent when it successfully boots.

For systemd systems:

`root #``systemctl enable --now kernel-bootcfg-boot-successful.service`
For OpenRC systems:

`root #``rc-update add kernel-bootcfg-boot-successful default`
When the to-be-registered kernel image is not a [Unified Kernel Image](https://wiki.gentoo.org/wiki/Unified_Kernel_Image) (UKI), a kernel command line for the new entry is read from (in order):

- /etc/kernel/cmdline, or
- /usr/lib/kernel/cmdline, or
- /proc/cmdline

In addition, the `initrd=` kernel command line argument is automatically added if an [initramfs](https://wiki.gentoo.org/wiki/Initramfs) was generated while installing the kernel. If on the other hand the to-be-registered kernel is a UKI, then no command line is added to the new entry; instead, the command line built into the UKI is used and the contents of this built-in command line are usually read from the same files when the UKI is generated.

#### Traditional installkernel

When the [efistub](https://packages.gentoo.org/useflags/efistub) [USE flag is enabled on](https://wiki.gentoo.org/wiki/USE_flag) [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel) but the [systemd](https://packages.gentoo.org/useflags/systemd) [USE flag is disabled](https://wiki.gentoo.org/wiki/USE_flag) [Installkernel](https://wiki.gentoo.org/wiki/Installkernel) calls uefi-mkconfig from [sys-boot/uefi-mkconfig](https://packages.gentoo.org/packages/sys-boot/uefi-mkconfig) to dynamically update the UEFI configuration; if the [shim](https://wiki.gentoo.org/wiki/Shim) EFI executable is present in the same directory as the kernel image, the kernels will be chainloaded via Shim.

### Manual

With the kernel configured with EFI Stub support and assuming the [ESP](https://wiki.gentoo.org/wiki/EFI_System_Partition) is mounted at /efi, create a separate directory below /efi/EFI:

`root #``mkdir -p /efi/EFI/example`
The kernel is created from the current kernel directory and copied to the new directory. This will install the kernel to /efi/EFI/example/bzImage.efi:

`/usr/src/linux #``make && make modules_install && cp arch/x86/boot/bzImage /efi/EFI/example/bzImage.efi`
#### Root partition configuration

To boot directly from [UEFI](https://wiki.gentoo.org/wiki/UEFI), the kernel or its initramfs must know where to find the root partition of the system to be booted; when using a bootmanager like grub, the kernel gets the root's path from the bootmanager via the command line; when using a stub kernel, **two options** may be used to give the kernel this information - choose one of these options:

##### Option 1: Configuring it into the kernel

**Root Partition information for Kernels 6.1+**

```
Processor type and features  --->
    [*] Built-in kernel command line
    (root=PARTUUID=adf55784-15d9-4ca3-bb3f-56de0b35d88d rw)
```
##### Option 2: Configuring it into UEFI

To add an entry with command line arguments:

`root #``efibootmgr --create --disk /dev/sda --label "Gentoo EFI Stub" --loader "\EFI\example\bzImage.efi" -u "root=/dev/sda3"`
More examples may be found in [Creating a boot entry](https://wiki.gentoo.org/wiki/Efibootmgr#Creating_a_boot_entry).

#### Optional: Kernel with initramfs

When using a kernel with an external initramfs (as a CPIO archive), additional steps are necessary. There is always an initramfs file when building a dist-kernel or using genkernel; when using a dist-kernel, this initramfs is named "initrd" and is in /usr/src/linux-6.1.57-gentoo-dist/arch/x86/boot/initrd and it must be copied into the [ESP](https://wiki.gentoo.org/wiki/EFI_System_Partition):

`root #``cp /path/to/my/initramfs/myinitrd.cpio.gz /efi/EFI/example/initrd.cpio.gz`
Now the kernel needs its initramfs, and the initramfs needs its root; UEFI must provide both:

`root #``efibootmgr -c -d /dev/sda -p 1 -L "Gentoo EFI Stub" -l '\EFI\example\bzImage.efi' -u 'root=UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx initrd=\EFI\example\initrd.cpio.gz'`
A Forum post explains in more detail, solving some user errors in the process: [Forums topic - Booting UEFI without Grub](https://forums.gentoo.org/viewtopic-p-8805827.html#8805827)

When using [Early Userspace Mounting](https://wiki.gentoo.org/wiki/Early_Userspace_Mounting), the [Generating the Initramfs](https://wiki.gentoo.org/wiki/Early_Userspace_Mounting#Generating_the_Initramfs) and [Using a Stub Kernel](https://wiki.gentoo.org/wiki/Early_Userspace_Mounting#Using_a_Stub_Kernel) sections also provide a more thorough explanation.

#### Optional: Embedded initramfs

It's also possible to embed the initramfs directly into the kernel. Advantages include: the initramfs being verified by [Secure Boot](https://wiki.gentoo.org/wiki/Secure_Boot) when verifying the kernel, a simplified boot process and EFI partition, and making loading the kernel by hand easier (because callers needn't specify the initramfs). Disadvantages include reduced flexibility, potential mistakes, and an unconventional boot setup.

The kernel supports both CPIO files (e.g., as produced by [Dracut](https://wiki.gentoo.org/wiki/Dracut)) and source directories (which are to be compressed into a CPIO archive). The following shows the latter with /usr/src/initramfs; however, it should be substituted with /path/to/my/initramfs/myinitrd.cpio.gz if the former case is desired (it usually is, unless using a [Custom Initramfs](https://wiki.gentoo.org/wiki/Custom_Initramfs)).

**Embedding the initramfs into the kernel**

```
General Setup  --->
    [*] Initial RAM filesystem and RAM disk (initramfs/initrd) support
    (/usr/src/initramfs) Initramfs source file(s)
```
##### EFI configuration

To ensure everything is functioning properly, the kernel may be booted without the initrd command line argument.

To create the UKI entry:

`root #``efibootmgr --create --disk /dev/sda --label "Gentoo EFI Stub" --loader "\EFI\example\bzImage.efi"`
#### Optional: Booting without dedicated UEFI entry

UEFI specifies that when booting from a particular ESP, the default behavior is to load an EFI executable from a specific path, dependent on the host architecture: for example, on an AMD64 system, an EFI stub placed at $(ESP)\efi\boot\bootx64.efi would be automatically loaded when booting from that ESP.

Combined with setting the relevant drive's boot priority, this setup can automatically boot the kernel upon power-on without entering a boot menu whilst still preserving the option to do so if desired.

This can be used to circumvent the use of efibootmgr and the creation of UEFI entries with the cost of less flexibility; it runs the risk of rendering the system unbootable if a new, broken kernel were to be installed to the default path.

#### Backup kernel

It is recommended to always have a backup kernel; if a bootmanager like grub is already installed, it should not be uninstalled because grub can boot a stub kernel just like a normal kernel. Another possibility working with an additional UEFI entry: before installing a new kernel, the current one can be copied from /efi/EFI/example to /efi/EFI/backup. In the example below, other names were used and the second UEFI entry was created with [efibootmgr](https://wiki.gentoo.org/wiki/Efibootmgr):

`root #``efibootmgr`
BootCurrent: 0002
Timeout: 1 seconds
BootOrder: 0002,0000,0001
Boot0000\* Secure        HD(1,GPT,0adcbfee-21aa-42ea-9a9a-2e53bd05e6a2,0x800,0x7f800)/File(\EFI\secure\bzImage.efi)
Boot0001\* gentoo        HD(1,GPT,0adcbfee-21aa-42ea-9a9a-2e53bd05e6a2,0x800,0x7f800)/File(\EFI\gentoo\grubx64.efi)
Boot0002\* Backup        HD(1,GPT,0adcbfee-21aa-42ea-9a9a-2e53bd05e6a2,0x800,0x7f800)/File(\EFI\backup\bzImage.efi)

## Microcode loading

When using a kernel without an initramfs, it is recommended to load the microcode described in the following articles:

## Optional: Signing for Secure Boot

If [Secure Booting](https://wiki.gentoo.org/wiki/Secure_Boot) this kernel, it must be signed with **sbsign**, part of [app-crypt/sbsigntools](https://packages.gentoo.org/packages/app-crypt/sbsigntools):

`root #``sbsign --key {db key} --cert {db cert} /efi/EFI/example/bzImage.efi`
More information is available at [Secure Boot](https://wiki.gentoo.org/wiki/Secure_Boot#Signing_Boot_Files).

## Troubleshooting

- Older kernels compiled with [gcc:10](https://archives.gentoo.org/gentoo-dev/message/60c4f4b7c219f85aa1cfeba5bd630893) [crashed at boot](https://trofi.github.io/posts/213-gcc-10-in-gentoo.html#linux-crash) ([bug #721734#c4](https://bugs.gentoo.org/show_bug.cgi?id=721734#c4)).

- Users of [sys-kernel/gentoo-kernel-bin](https://packages.gentoo.org/packages/sys-kernel/gentoo-kernel-bin) can specify the root partition path with the `root=` parameter using [efibootmgr](https://wiki.gentoo.org/wiki/Efibootmgr):

`root #``efibootmgr -c -L "Gentoo Linux" -l '\EFI\Gentoo\bootx64.efi' -u 'root=PARTUUID=XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX'` - To create a boot entry with hibernation on swap partition:

`root #``efibootmgr -c -L "Gentoo Linux" -l '\EFI\Gentoo\bootx64.efi' -u 'root=PARTUUID=XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX resume=PARTUUID=XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX'` ## See also

- [UEFI](https://wiki.gentoo.org/wiki/UEFI) — a firmware standard for boot ROM designed to provide a stable API for interacting with system hardware. On [x86](https://en.wikipedia.org/wiki/x86) it replaced the legacy [BIOS](https://wiki.gentoo.org/wiki/BIOS).
- [Efibootmgr](https://wiki.gentoo.org/wiki/Efibootmgr) — a tool for managing [UEFI](https://wiki.gentoo.org/wiki/UEFI) boot entries.
- [Architecture specific kernel configuration (AMD64 Handbook)](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Kernel#Architecture_specific_kernel_configuration)
- [REFInd](https://wiki.gentoo.org/wiki/REFInd) — a boot manager for UEFI platforms.
- [Unified Kernel Image](https://wiki.gentoo.org/wiki/Unified_Kernel_Image) — a single executable which can be [booted directly from UEFI firmware], or automatically sourced by boot-loaders with little or no configuration.

## External resources

- [Linux Kernel Documentation on EFI Stub](https://www.kernel.org/doc/html/latest/admin-guide/efi-stub.html)
- [EFI Stub - booting without a bootloader](http://blog.realcomputerguy.com/2012/05/efi-stub-booting-without-bootloader.html) Blog posting which this article is partially based on.
- [EFI bootloaders](http://www.rodsbooks.com/efi-bootloaders/) listing alternative ways to boot a UEFI system.
- [Gentoo Forums: Suspend and Hibernate with UEFI](https://forums.gentoo.org/viewtopic-p-8111048.html#8111048)
- [http://www.kroah.com/log/blog/2013/09/02/booting-a-self-signed-linux-kernel/](http://www.kroah.com/log/blog/2013/09/02/booting-a-self-signed-linux-kernel/)
