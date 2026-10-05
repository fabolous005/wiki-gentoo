<!-- source: https://wiki.gentoo.org/wiki/ZFS/ZFSBootMenu | group: Gentoo Wiki (Main) | wiki-title: ZFS/ZFSBootMenu -->
---
title: ZFS/ZFSBootMenu
url: https://wiki.gentoo.org/wiki/ZFS/ZFSBootMenu
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-15"
fingerprint: "7605281f81bbbf21"
license: CC BY-SA 4.0
---

# ZFS/ZFSBootMenu

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

ZFS Boot Menu (ZBM) is a bootloader specifically for systems using [ZFS](https://wiki.gentoo.org/wiki/ZFS) for the root filesystem. A guide for setting up such a system can be found at [ZFS/rootfs](https://wiki.gentoo.org/wiki/ZFS/rootfs).

ZBM provides a number of ZFS specific features, such as snapshot management, support for multiple boot environments, and support for ZFS native encryption.

## Installation

### Pre-built EFI image (AMD64 only)

ZBM offers downloadable pre-built EFI images, which do not require the installation of any packages. They can be download from [ZBM's homepage](https://docs.zfsbootmenu.org/en/v3.1.x/), either using a graphical browser or from CLI:

After downloading, the EFI image should be placed in the system's EFI partition. If the image is placed at /efi/EFI/BOOT/BOOTX64.EFI, the image should be detected by the firmware. If the user prefers some other location, for example /efi/EFI/zbm/ZBM.EFI, [sys-boot/efibootmgr](https://packages.gentoo.org/packages/sys-boot/efibootmgr) can be used to make an boot entry. For example, if the user's EFI partition is /dev/sdX1:

`root #``efibootmgr -c -d /dev/sdX -p 1 -L "ZFSBootMenu" -l '\EFI\ZBM\ZBM.EFI'`
### From source (AMD64 and ARM64)

The ZBM package is available in the [GURU overlay](https://wiki.gentoo.org/wiki/Project:GURU), which must be enable first. Using [app-eselect/eselect-repository](https://packages.gentoo.org/packages/app-eselect/eselect-repository):

`root #``eselect repository enable guru`
As with all GURU packages, ZBM is masked as testing, so must be unmasked.

**`/etc/portage/package.accept_keywords/zfsbootmenu`**

**Unmasking zfsbootmenu for installation**

The user should replace \~amd64 with \~arm64 if on an ARM system.

[sys-boot/zfsbootmenu](https://github.com/gentoo/guru/tree/master/sys-boot/zfsbootmenu) can then be installed.

`root #``emerge --ask --verbose sys-boot/zfsbootmenu`
#### Installing

Installation using [sys-boot/zfsbootmenu](https://github.com/gentoo/guru/tree/master/sys-boot/zfsbootmenu) is done through the generate-zbm command, which is configured using /etc/zfsbootmenu/config.yaml. generate-zbm searches /boot for usable kernel/initramfs pairs, and selects the newest version. It can then be configured to output either a kernel and initramfs, to be used with a separate bootloader, such as [Limine](https://wiki.gentoo.org/wiki/Limine) or [GRUB](https://wiki.gentoo.org/wiki/GRUB), or an EFI stub, which is directly bootable. The configuration for each of these options is as follows:

##### Kernel+Initramfs

If only a kernel and initramfs are desired, the EFI section of the config file should be disabled.

**`/etc/zfsbootmenu/config.yaml`**

**generate-zbm config for kernel+initramfs**

ManageImages must be true for any action to be taken. BootMountPoint specifies the location where the system's EFI partition is mounted, and ImageDir specifies where the kernel and initramfs will be output.

##### EFI Stub

To generate an EFI stub, generate-zbm requires an EFI stub loader. The most reliable way to obtain this is through [systemd-boot](https://wiki.gentoo.org/wiki/Systemd/systemd-boot), which can be installed according to its wiki page. After installation, an EFI stub will be available at /usr/lib/systemd/boot/efi/linuxx64.efi.stub (or /usr/lib/systemd/boot/efi/linuxaa64.efi.stub on ARM64 systems), allowing the config file to be modified.

**`/etc/zfsbootmenu/config.yaml`**

**generate-zbm config for EFI stub**

This will output an EFI image to /efi/EFI/BOOT/vmlinuz.EFI, which can then be copied to /efi/EFI/BOOT/BOOTX64.EFI, or [sys-boot/efibootmgr](https://packages.gentoo.org/packages/sys-boot/efibootmgr) can be used.

`root #``efibootmgr -c -d /dev/sdX -p 1 -L "ZFSBootMenu" -l '\EFI\BOOT\VMLINUZ.EFI'`
## Dataset Customization

At boot time, ZBM imports all pools present and scans each dataset with mountpoint=/. Several settings can be customized by setting custom properties for the dataset.

### Kernel

By default, ZBM will select and boot the newest kernel, but this can be overwritten with the org.zfsbootmenu:kernel property.

`root #``zfs org.zfsbootmenu:kernel=vmlinuz-6.19.6-gentoo tank/os/gentoo`
If the property value does not match any kernel in /boot, ZBM will fallback to the latest.

### Command line

The kernel command line can be specified through the org.zfsbootmenu:commandline property.

`root #``zfs org.zfsbootmenu:commandline="loglevel=7 zfs.zfs_arc_max=8589934592" tank/os/gentoo`
Other properties can be found in [ZBM's documentation](https://docs.zfsbootmenu.org/en/v3.1.x/man/zfsbootmenu.7.html#zfs-pool-properties)

## ARM64 Specific Configuration

ZBM added ARM64 support in v3.1.0, and supports both including a device tree in the generated EFI image as well as specifying the device tree used at boot time.

### EFI Image

The path of a device tree blob should be specified in EFI section of /etc/zfsbootmenu/config.yaml, which may be relative or absolute. Absolute paths may contain %{kernel}, which will be replaced with the kernel version generate-zbm selects to build the EFI image. /Relative paths are interpreted as children of /boot/dtbs/dtbs-%{kernel}/.

**`/etc/zfsbootmenu/config.yaml`**

**Config for Device Tree**

By default, DTBs are installed to /boot/dtbs/%{kernel}/, so using /boot/dtbs/%{kernel}/{vendor}/{device}.dtb should suffice in most situations.

### Dataset Property

To configure which device tree ZBM uses at boot time, the org.zfsbootmenu:devicetree property can be set. This may also be a relative or absolute path, and also supports the %{kernel} substitution.

`root #``zfs org.zfsbootmenu:devicetree="/boot/dtbs/%{kernel}/qcom/x1e80100-hp-omnibook-x14.dtb" tank/os/gentoo`
