<!-- source: https://wiki.gentoo.org/wiki/REFInd | group: Gentoo Wiki (Main) | wiki-title: REFInd -->
---
title: rEFInd
url: https://wiki.gentoo.org/wiki/REFInd
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-22"
fingerprint: "168b5b3f89f6afc7"
license: CC BY-SA 4.0
---

# rEFInd

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



**rEFInd** is a boot manager for UEFI platforms. It provides a graphical interface for launching EFI-based operating systems and accessing EFI-based utilities.

rEFInd can boot traditional Linux kernels, but also any compatible EFI bootloader. Examples include [unified kernel images](https://wiki.gentoo.org/wiki/Unified_kernel_image) (see [below](https://wiki.gentoo.org/wiki/REFInd#Unified_kernel_image)), aside from additional operating systems such as BSD Unix (like FreeBSD), macOS and Windows.

For a kernel to be bootable from rEFInd, its EFI stub support (`CONFIG_EFI_STUB`) has to be enabled:

**Enable EFI stub support for Kernels 6.1+**

EFI framebuffer support (`CONFIG_FB_EFI`) or a vendor specific [Framebuffer](https://wiki.gentoo.org/wiki/Framebuffer) is required to display video when launching the kernel from an EFI such as rEFInd:

**Enable EFI framebuffer support**

rEFInd has optional support for scanning several filesystems for EFI executables before loading the operating system. This allows to keep the kernels outside of the [EFI System Partition](https://wiki.gentoo.org/wiki/EFI_System_Partition) (ESP) but needs the rEFInd built with the respective [USE flags](https://wiki.gentoo.org/wiki/USE_flag) enabled.


| [+ext2](https://packages.gentoo.org/useflags/+ext2) | Builds the EFI binary ext2 filesystem driver | 
| [+ext4](https://packages.gentoo.org/useflags/+ext4) | Builds the EFI binary ext4 filesystem driver | 
| [+iso9660](https://packages.gentoo.org/useflags/+iso9660) | Builds the EFI binary iso9660 filesystem driver | 
| [btrfs](https://packages.gentoo.org/useflags/btrfs) | Builds the EFI binary btrfs filesystem driver | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [hfs](https://packages.gentoo.org/useflags/hfs) | Builds the EFI binary hfs filesystem driver | 
| [ntfs](https://packages.gentoo.org/useflags/ntfs) | Builds the EFI binary ntfs filesystem driver | 
| [reiserfs](https://packages.gentoo.org/useflags/reiserfs) | Builds the EFI binary reiserfs filesystem driver | 
| [secureboot](https://packages.gentoo.org/useflags/secureboot) | Automatically sign efi executables using user specified key | 

`root #``emerge --ask sys-boot/refind````
rEFInd has been built and installed into ${EROOT%/}/usr/share/${P}
You will need to use the command 'refind-install' to install
the binaries into your EFI System Partition
```
Once the rEFInd package has been emerged, a second step is needed to install the binaries to the ESP. If an ESP does not exist, one needs to be created. See [EFI System Partition](https://wiki.gentoo.org/wiki/EFI_System_Partition).

For kernel storage there are multiple options.

During boot, rEFInd can automatically find EFI boot images and Linux kernels. It looks for files ending in .efi or beginning with vmlinuz, bzImage or kernel. On the filesystems it can read (based on above USE flags), it scans in the following locations[\[1\]](https://www.rodsbooks.com/refind/configfile.html#hiding):

- File system root (/).
- /boot directory.
- Most of the sub-directories of /EFI. (See example in the [Kernel image at ESP](https://wiki.gentoo.org/wiki/REFInd#Kernel_image_at_ESP) section).

Boot partition configuration is quite flexible. For example, choose one of:

- Separate boot, ESP and root partitions
- Separate ESP partition with /boot part of the root filesystem. (Provided it is a non-encrypted, non-LVM and one of the above supported filesystems).
- Only ESP partition mounted at /boot, or unified kernel images living in /EFI/Linux/.

The rEFInd package comes with the refind-install command. Running it will:

1. Looks if the ESP is already mounted. If not, automount the ESP according to [/etc/fstab](https://wiki.gentoo.org/wiki/EFI_System_Partition#Mount_point)
2. Install its refind\_x64.efi application and other stuff into the ESP
3. Call [efibootmgr](https://wiki.gentoo.org/wiki/Efibootmgr) to set itself as the default boot manager.

`root #``refind-install`
ShimSource is none
Installing rEFInd on Linux....
ESP was found at /boot using vfat
Copied rEFInd binary files
 
Copying sample configuration file as refind.conf; edit this file to configure
rEFInd.
 
Installing it!
rEFInd has been set as the default boot manager.
Creating //boot/refind\_linux.conf; edit it to adjust kernel options.
 
Installation has completed successfully.

`user $``tree -L 3 /boot`
/boot
├── EFI
│   ├── refind
│   │   ├── icons
│   │   ├── keys
│   │   ├── refind.conf
│   │   └── refind\_x64.efi
│   └── tools
└── refind\_linux.conf

`user $``efibootmgr -v`
Boot000x\* rEFInd Boot Manager   HD(1,GPT,1729a003-cf0d-4bd4-88c9-cc24d8d418c4,0x800,0x2f000)/File(\EFI\refind\refind\_x64.efi)

Boot000x\* can vary depending on existing entries.

rEFInd can be installed to a disk using the default/fallback filename of EFI/BOOT/bootx64.efi. The computer's NVRAM entries will not be modified when installing in this way. Most EFI and UEFI firmware support a fallback EFI image to boot from if the configured EFI file cannot be found, and some will also override the configured boot selection if the fallback boot image is found. This can be used to boot into EFI mode when doing so otherwise is difficult.

`root #````
refind-install --usedefault /dev/sda1
```
Where /dev/sda1 is the ESP. This installation method can be used as either a permanent setup to create a bootable USB flash drive or install rEFInd on a computer that tends to "forget" its NVRAM settings or as a temporary bootstrap to get the system to boot in EFI mode.

Before upgrading rEFInd, it is possible to check it and see if it works correctly. The existing rEFInd is able to start a new rEFInd. This can be accomplished by copying /usr/lib64/refind/refind to the ESP (for example EFI/refind).

## Kernel management

Regardless if /boot is a separate partition or part of the root file system, rEFInd should be able to find a kernel if standard naming convention is used. This makes it compatible with (semi-) automatic kernel installation methods such as genkernel --install or make install **without further configuration**.

### Unified kernel image

rEFInd can boot [unified kernel images](https://wiki.gentoo.org/wiki/Unified_kernel_image) (UKI) too. For UKI kernels refind\_linux.conf is not refereed to. So even for rEFInd users, UKI simplifies the things, and with rEFInd, UKIs can be stored in a Linux partition, instead of the ESP.

refind\_linux.conf should live in the same directory as the kernels. It is automatically generated in /boot during refind\_install or with [mkrlconf](https://www.rodsbooks.com/refind/mkrlconf.html). Each entry will show up as an option for each kernel.

The default entry is based on the current /proc/cmdline. Single is the same as default, but with single added. And minimal is contains only the current root device, with the ro argument. None of the entries contain the initramfs, as it is established automatically at boot.

This file usually works out of the box when it is generated from the same boot session as it is supposed to start. For instance, when replacing the bootloader for rEFInd. However, when generated from another OS, for instance during installation of Gentoo, care must be taken and the entries must be corrected manually.

Simple example configuration:

**`/boot/refind_linux.conf`**

In simple cases, users do not have specify initramfs, because rEFInd automatically passes the appropriate initramfs. See [below](https://wiki.gentoo.org/wiki/REFInd#Initial_RAM_filesystem_.28initramfs.29) for the details.

Custom (static) initramfs and [microcode](https://wiki.gentoo.org/wiki/Intel_microcode#rEFInd) loading:

**`/boot/refind_linux.conf`**

The main selection screen for rEFInd will use the first option as the default option, however alternate boot entries can be accessed by highlighting the kernel and pressing `F2`. Also cmdline can be modified on-the-fly by pressing `F2` on a menu item to open it in an editor. When ready, press `Enter` to boot the kernel.

### Initial RAM filesystem (initramfs)

rEFInd can automatically pass the appropriate initramfs to the kernel without configuration. More precisely at boot, rEFInd looks for an initial RAM disk (initramfs) that starts with init and ends in a kernel version string. For example initramfs-5.4.66-gentoo.img matches with vmlinuz-5.4.66-gentoo. Provided that refind\_linux.conf does not specify an initrd, it is automatically appended to the kernel command line.

When using tools like [Genkernel](https://wiki.gentoo.org/wiki/Genkernel) or [Dracut](https://wiki.gentoo.org/wiki/Dracut), no additional configuration is required. However, if a [Custom Initramfs](https://wiki.gentoo.org/wiki/Custom_Initramfs) is used some care is required. Either, the initramfs should be named using the same convention, or its name should be specified in [refind\_linux.conf](https://wiki.gentoo.org/wiki/REFInd#Linux_command_line_options).

rEFInd comes with a collection of icons for different Linux distributions. In order to [set an icon](https://www.rodsbooks.com/refind/configfile.html#icons) on the menu entry, it needs to know the OS name. For that it looks in the following places, in this order:

1. An icon base name matches the kernel base name. For instance vmlinuz-5.4.66-gentoo.png
2. The kernel is in an ESP sub-directory, named after the OS. For instance the kernel is at /EFI/Gentoo/vmlinuz-5.4.66-gentoo.png See "[Kernel image at ESP](https://wiki.gentoo.org/wiki/REFInd#Kernel_image_at_ESP)" for more info.
3. Filesystem label contains a space, underscore or dash delimited OS name. For instance with tunetfs -L Gentoo-boot /dev/sdaX.
4. GPT partition name, following the same convention as filesystem labels.
5. From the /etc/os-release file on the same partition. For instance, when /boot is part of the root partition.
6. Hard coded rules based on words kernel names. For instance, vmlinux and bzImage default to the Linux "Tux" icon.

It is possible to store the kernels and initial RAM disks on the ESP by mounting it at /boot. Because the kernel is on a separate partition refind can not automatically determine which distribution the kernel belongs to and will therefore fall back to the default Tux logo. To automatically install a Gentoo icon file for refind to pickup alongside the kernel image, enable the [refind](https://packages.gentoo.org/useflags/refind) [USE flag on](https://wiki.gentoo.org/wiki/USE_flag) [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel).

A similar problem occurs when using [Unified Kernel Images](https://wiki.gentoo.org/wiki/Unified_Kernel_Image), these will be installed in the EFI/Linux directory on the ESP. Refind is then unable to get any information on which distribution this unified kernel image belongs to. To automatically install a Gentoo icon file for refind to pickup alongside the unified kernel image, enable the [refind](https://packages.gentoo.org/useflags/refind) [USE flag on](https://wiki.gentoo.org/wiki/USE_flag) [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel).

## ISO images

rEFInd can find and boot iso images. For that purpose, each iso image has to be burnt to one partition using tools like [dd](https://wiki.gentoo.org/wiki/Dd). It also requires the USE flag iso9660.

## See also

- [Kernel/Configuration](https://wiki.gentoo.org/wiki/Kernel/Configuration) — describes the manual configuration and setup of the [Linux kernel](https://wiki.gentoo.org/wiki/Kernel).
- [Dracut](https://wiki.gentoo.org/wiki/Dracut) — an [initramfs](https://wiki.gentoo.org/wiki/Initramfs) infrastructure and aims to have as little as possible hard-coded into the initramfs.
- [GRUB](https://wiki.gentoo.org/wiki/GRUB) — a multiboot secondary [bootloader](https://wiki.gentoo.org/wiki/Bootloader) capable of loading kernels from a variety of [filesystems](https://wiki.gentoo.org/wiki/Filesystem) on most system architectures.
- [Syslinux](https://wiki.gentoo.org/wiki/Syslinux) — a package that contains a family of [bootloaders](https://wiki.gentoo.org/wiki/Bootloader).
- [UEFI Dual boot with Windows 7/8](https://wiki.gentoo.org/wiki/UEFI_Dual_boot_with_Windows_7/8) — describes how to dual boot Microsoft Windows on a UEFI computer.
