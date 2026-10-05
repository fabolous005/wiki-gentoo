<!-- source: https://wiki.gentoo.org/wiki/Installkernel | group: Gentoo Wiki (Main) | wiki-title: Installkernel -->
---
title: Installkernel
url: https://wiki.gentoo.org/wiki/Installkernel
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-13"
fingerprint: "5713963b04eaadc6"
license: CC BY-SA 4.0
---

# Installkernel

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Installkernel** is a collection of scripts to automatically install new [kernels](https://wiki.gentoo.org/wiki/Kernel) and update [bootloader](https://wiki.gentoo.org/wiki/Bootloader) configuration.

Additional automation plugins, for example:

- generate an [initramfs](https://wiki.gentoo.org/wiki/Initramfs)
- generate a [Unified Kernel Image](https://wiki.gentoo.org/wiki/Unified_Kernel_Image)
- update the bootloader configuration

are installed and/or enabled via the USE flags on [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel) as shown below:

## Quick start

This wiki page provides a complete overview of the capabilities and options offered by the Installkernel ecosystem; however, this may be an overwhelming amount of information which is not relevant for the vast majority of users. Thus, a briefer summary is provided here.

At the most basic level [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel) is set up similar to packages that one would otherwise find in the `app-alternatives/` category meaning that the intention is for the user to enable or disable certain `USE` flags based on the desired configuration and then emerge the package. The package will take care of setting up the appropriate configuration, installing the required scripts, and pulling in the necessary dependencies. Only users who wish to use unsupported, advanced, or otherwise custom configurations might need to dig deeper and write their own configuration files or scripts; for those users, the full documentation of the Gentoo installkernel ecosystem is provided in the remaining sections.

For example:

- A user who opts for a simple and classic setup with [GRUB](https://wiki.gentoo.org/wiki/GRUB) as the bootloader and [Dracut](https://wiki.gentoo.org/wiki/Dracut) as the [initramfs](https://wiki.gentoo.org/wiki/Initramfs) generator should enable the [grub](https://packages.gentoo.org/useflags/grub)[dracut](https://packages.gentoo.org/useflags/dracut)`USE` flags.
- A user who instead wishes to use [systemd-boot](https://wiki.gentoo.org/wiki/Systemd-boot) without any [initramfs](https://wiki.gentoo.org/wiki/Initramfs) should enable only the [systemd-boot](https://packages.gentoo.org/useflags/systemd-boot)`USE` flag.
- A user who does not wish to use any [bootloader](https://wiki.gentoo.org/wiki/Bootloader) and instead would like to boot directly from [UEFI](https://wiki.gentoo.org/wiki/UEFI) into an [Unified Kernel Image](https://wiki.gentoo.org/wiki/Unified_Kernel_Image) generated with [ukify](https://packages.gentoo.org/useflags/ukify)[initramfs](https://wiki.gentoo.org/wiki/Initramfs) generated with [Dracut](https://wiki.gentoo.org/wiki/Dracut) should enable the [efistub](https://packages.gentoo.org/useflags/efistub)[uki](https://packages.gentoo.org/useflags/uki)[ukify](https://packages.gentoo.org/useflags/ukify)[dracut](https://packages.gentoo.org/useflags/dracut)`USE` flags.


A full overview of the available `USE` flags is provided below.


| [dracut](https://packages.gentoo.org/useflags/dracut) | Generate an initramfs or UKI on each kernel installation | 
| [efistub](https://packages.gentoo.org/useflags/efistub) | EXPERIMENTAL: Update UEFI configuration on each kernel installation | 
| [grub](https://packages.gentoo.org/useflags/grub) | Re-generate grub.cfg on each kernel installation, used grub.cfg is overridable with GRUB\_CFG env var | 
| [refind](https://packages.gentoo.org/useflags/refind) | Install a Gentoo icon for rEFInd alongside the (unified) kernel image, used icon is overridable with REFIND\_ICON env var | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Use systemd's kernel-install to install kernels, overridable with SYSTEMD\_KERNEL\_INSTALL env var | 
| [systemd-boot](https://packages.gentoo.org/useflags/systemd-boot) | Use systemd-boot's native layout by default | 
| [ugrd](https://packages.gentoo.org/useflags/ugrd) | Generate an initramfs using UGRD on each kernel installation | 
| [uki](https://packages.gentoo.org/useflags/uki) | Install UKIs to ESP/EFI/Linux for EFI stub booting and/or bootloaders with support for auto-discovering UKIs | 
| [ukify](https://packages.gentoo.org/useflags/ukify) | Build an UKI with systemd's ukify on each kernel installation | 

## Implementations

The package [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel) provides two different paths of managing kernel installation. The first is [systemd](https://wiki.gentoo.org/wiki/Systemd)'s kernel-install, the second is the more traditional installkernel originating from Debian. Gentoo strives to ensure a rough feature parity between both implementations.

The [systemd](https://packages.gentoo.org/useflags/systemd) [USE flag changes which implementation is used by default, this default may be overridden with the](https://wiki.gentoo.org/wiki/USE_flag) `SYSTEMD_KERNEL_INSTALL` environment variable or with the `--(no-)systemd` argument. In general systemd's kernel-install is the more modern implementation, and it is therefore recommended and enabled by default on systemd profiles. Users who do not wish to use systemd tooling may fallback on Gentoo's Debian-based installkernel implementation instead.

## Systemd's kernel-install

To select this installation method, enable the [systemd](https://packages.gentoo.org/useflags/systemd) [USE flag or set](https://wiki.gentoo.org/wiki/USE_flag) `SYSTEMD_KERNEL_INSTALL=1` in the environment.

**`/etc/portage/package.use/installkernel`**

```
 systemd
```
`root #``emerge --ask sys-kernel/installkernel`
Or

**`/etc/env.d/99systemd-kernel-install`**

```
SYSTEMD_KERNEL_INSTALL=1
```
`root #``env-update`
### Configuration

Configuration of kernel-install is done in /etc/kernel/install.conf and /usr/lib/kernel/install.conf, where the former takes precedence over the latter. These configuration options can be set in the configuration files:

**`/etc/kernel/install.conf`**

```
layout=
initrd_generator=
uki_generator=
```
In addition, these configuration options can be set either in /etc/kernel/install.conf or via environment variables. `MACHINE_ID` overrides the [machine ID](https://wiki.gentoo.org/wiki/Systemd#Machine_ID). `BOOT_ROOT` sets the root path under which kernel-install plugins install new kernels.

**`/etc/kernel/install.conf`**

```
MACHINE_ID=
BOOT_ROOT=
```
The default /usr/lib/kernel/install.conf configuration file is provided by [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel), whose `USE` flags control the options' values in the configuration file. Of course the configuration may also be changed manually in /etc/kernel/install.conf.

kernel-install also supports [drop-in files](https://wiki.gentoo.org/wiki/Systemd#Customizing_unit_files) for its configuration, like systemd does for unit files, which allows the user to override just a few settings in a configuration file without the need to create a full copy of the original file. To create a drop-in file for kernel-install's configuration file, first create directory /etc/kernel/install.conf.d:

`root #``mkdir /etc/kernel/install.conf.d`
Then, drop-in files can be created in this directory. A drop-in file must have filename suffix .conf.

**`/etc/kernel/install.conf.d/no-initramfs.conf`**

**Override only`initrd_generator` to skip initramfs generation**

```
initrd_generator=none
```
#### layout

Upstream systemd supports the `Boot Loader Specification` type 1 (layout=bls) and type 2 (layout=uki) layout. The type 1 layout is used by [systemd-boot](https://wiki.gentoo.org/wiki/Systemd-boot), the type 2 layout is intended to be used for Unified Kernel Images and is supported by [GRUB](https://wiki.gentoo.org/wiki/GRUB), systemd-boot and [refind](https://wiki.gentoo.org/wiki/Refind).

Gentoo also supports a more traditional layout intended for use with GRUB (layout=grub), which is very similar (but not identical) to the layout used by Debian's installkernel as described above. This layout may also be used in other cases where a more basic and traditional layout is desired. To use GRUB in combination with [Unified Kernel Images](https://wiki.gentoo.org/wiki/Unified_Kernel_Image), use the uki layout instead.

When the [grub](https://packages.gentoo.org/useflags/grub)[,](https://wiki.gentoo.org/wiki/USE_flag) [systemd-boot](https://packages.gentoo.org/useflags/systemd-boot)[,](https://wiki.gentoo.org/wiki/USE_flag) [efistub](https://packages.gentoo.org/useflags/efistub)[, and](https://wiki.gentoo.org/wiki/USE_flag) [uki](https://packages.gentoo.org/useflags/uki) [USE flags are all disabled, the kernels will be installed in a layout that is mostly backwards compatible with Debian's installkernel (layout=compat).](https://wiki.gentoo.org/wiki/USE_flag)

When multiple layout-specifying flags are enabled, the `uki` layout (enabled by the [uki](https://packages.gentoo.org/useflags/uki) [USE flag) takes precedence over the](https://wiki.gentoo.org/wiki/USE_flag) `bls` layout (enabled by the [systemd-boot](https://packages.gentoo.org/useflags/systemd-boot) [USE flag), which in turn takes precedence over the](https://wiki.gentoo.org/wiki/USE_flag) `grub` layout (enabled by the [grub](https://packages.gentoo.org/useflags/grub) [USE flag).](https://wiki.gentoo.org/wiki/USE_flag)

An overview of each layout is shown in the sections below:

##### compat

The compat layout is very similar, but not identical, to the layout used by Debian's traditional installkernel:

##### efistub

The efistub layout is identical to the compat layout, but relocated to the [EFI system partition](https://wiki.gentoo.org/wiki/EFI_system_partition) for [EFI stub](https://wiki.gentoo.org/wiki/EFI_stub) booting. The kernel image gains the `.efi` suffix as some firmware vendors enforce this:

##### grub

The grub layout is identical to the `compat` layout, with an added grub.cfg, used by [GRUB](https://wiki.gentoo.org/wiki/GRUB):

##### bls

The `Bootloader Specification Type 1` or `bls` layout, used by [systemd-boot](https://wiki.gentoo.org/wiki/Systemd-boot):

##### uki

The `Bootloader Specification Type 2` or `uki` layout:

#### initrd\_generator

This setting controls which plugin should be used to generate the [initramfs](https://wiki.gentoo.org/wiki/Initramfs). Currently the only package that installs such a plugin is [Dracut](https://wiki.gentoo.org/wiki/Dracut) from [sys-kernel/dracut](https://packages.gentoo.org/packages/sys-kernel/dracut). This setting is exposed to the plugins as `${KERNEL_INSTALL_INITRD_GENERATOR}`. When the [dracut](https://packages.gentoo.org/useflags/dracut) [USE flag is enabled, this setting is automatically set to](https://wiki.gentoo.org/wiki/USE_flag) `dracut`. Otherwise this setting is automatically set to `none`.

#### uki\_generator

This setting controls which plugin should be used to generate the Unified Kernel Image. Currently two packages provide such a plugin: [sys-kernel/dracut](https://packages.gentoo.org/packages/sys-kernel/dracut) and [systemd](https://wiki.gentoo.org/wiki/Systemd) (via the [ukify](https://packages.gentoo.org/useflags/ukify) [flag on](https://wiki.gentoo.org/wiki/USE_flag) [sys-apps/systemd](https://packages.gentoo.org/packages/sys-apps/systemd) and [sys-apps/systemd-utils](https://packages.gentoo.org/packages/sys-apps/systemd-utils)). This setting is exposed to the plugins as `${KERNEL_INSTALL_UKI_GENERATOR}` When the [ukify](https://packages.gentoo.org/useflags/ukify) [USE flag is enabled, this setting is automatically set to](https://wiki.gentoo.org/wiki/USE_flag) `ukify`. When the [ukify](https://packages.gentoo.org/useflags/ukify) [USE flag is disabled, but the](https://wiki.gentoo.org/wiki/USE_flag) [dracut](https://packages.gentoo.org/useflags/dracut) [and](https://wiki.gentoo.org/wiki/USE_flag) [uki](https://packages.gentoo.org/useflags/uki) [USE flags are enabled, this setting is automatically set to](https://wiki.gentoo.org/wiki/USE_flag) `dracut`. Otherwise this setting is automatically set to `none`.

### kernel-install commands

Below an overview is provided of the available commands in systemd's kernel-install.

#### kernel-install add

(Re-)installs a kernel version. This command is called by [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel) if the [systemd](https://packages.gentoo.org/useflags/systemd) [USE flag is enabled.](https://wiki.gentoo.org/wiki/USE_flag)

`root #``kernel-install [OPTIONS...] add KERNEL-VERSION KERNEL-IMAGE [INITRD-FILE...]`
#### kernel-install remove

Uninstalls a kernel version. This command is called by [app-admin/eclean-kernel](https://packages.gentoo.org/packages/app-admin/eclean-kernel) if it is available.

`root #``kernel-install [OPTIONS...] remove KERNEL-VERSION`
#### kernel-install inspect

Prints an overview of parameters that will be used when installing a kernel version.

`root #``kernel-install [OPTIONS...] inspect [KERNEL-VERSION] [KERNEL-IMAGE] [INITRD-FILE...]`
#### kernel-install list

Prints an overview of all installed kernel versions. Meaning all kernel versions for which a directory is present in /lib/modules.

`root #``kernel-install [OPTIONS...] list`
#### kernel-install add-all

(Re-)installs all kernel versions. Iterates kernel-install add over each kernel for which a vmlinuz file is present in the associated /lib/modules directory.

`root #``kernel-install [OPTIONS...] add-all`
### Runtime overrides

When the dracut and ukify plugins are enabled (i.e. the [dracut](https://packages.gentoo.org/useflags/dracut) [and/or](https://wiki.gentoo.org/wiki/USE_flag) [ukify](https://packages.gentoo.org/useflags/ukify) [USE flags are enabled) they may be skipped at runtime by overriding the default configuration provided by](https://wiki.gentoo.org/wiki/USE_flag) [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel) at /usr/lib/kernel/install.conf with a custom configuration at /etc/kernel/install.conf. This may be useful on systems that have both the distribution kernels installed and manually configured kernels where the former enforces the enablement of the [dracut](https://packages.gentoo.org/useflags/dracut) [USE flag but the latter might not require an initramfs at all.](https://wiki.gentoo.org/wiki/USE_flag)

For example:

**`/etc/kernel/install.conf`**

```
layout=compat
initrd_generator=none
uki_generator=none
```
Note that this override will apply to all installed kernels. It is also possible to specify several different configurations and switch between them at runtime using the `KERNEL_INSTALL_CONF_ROOT` environment variable.

For example:

**`/etc/manual-kernel/install.conf`**

```
layout=compat
initrd_generator=none
uki_generator=none
```
`root #``KERNEL_INSTALL_CONF_ROOT=/etc/manual-kernel make install`
Another method of overriding the default kernel installation is to use the `KERNEL_INSTALL_PLUGINS` environment variable. When this variable is set, only the specified plugins are run.

For example:

`root #``KERNEL_INSTALL_PLUGINS="90-compat.install 90-loaderentry.install 90-uki-copy.install" make install`
### Customization

Custom plugins, which for example, generate a initramfs or UKI, or update the bootloader configuration, may be installed into /etc/kernel/install.d. An initramfs plugin should install a file named initrd on the ${KERNEL\_INSTALL\_STAGING\_AREA}. A UKI plugin should install a file named uki.efi in the ${KERNEL\_INSTALL\_STAGING\_AREA}. All plugin files must have the .install suffix. Plugins in /etc/kernel/install.d override default plugins in /usr/lib/kernel/install.d with the same name.

### Descriptions of plugin scripts

In exection order, the `install.d` plugins

- /usr/lib/kernel/install.d/00-00machineid-directory.install
- Create the `ENTRY_DIR_ABS` directory where kernels will be installed for [systemd-boot](https://wiki.gentoo.org/wiki/Systemd-boot).
- Installed by: [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel)
- Executed if: layout=bls and kernel-install command is add

- /usr/lib/kernel/install.d/05-check-config.install
- Verifyes that a layout, initramfs generator, and ukify generator are set (or explicitly set to none) by the install.conf configuration file.
- Installed by: [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel)
- Executed if: kernel-install command is add

- /usr/lib/kernel/install.d/10-copy-prebuilt.install
- Copy prebuilt initramfs and Unified Kernel Image to the staging area if they exist. A prebuilt initramfs and Unified Kernel Image are present when the distribution kernels are installed with the [generic-uki](https://packages.gentoo.org/useflags/generic-uki)- Installed by: [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel)
- Executed if: kernel-install command is add

- /usr/lib/kernel/install.d/35-amd-microcode-systemd.install
- Builds an AMD CPU microcode early initramfs if an initramfs generator is used that does not already bundle the CPU microcode.
- Installed by: [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) if the [initramfs](https://packages.gentoo.org/useflags/initramfs)- Executed if: kernel-install command is add and initrd\_generator!=dracut.

- /usr/lib/kernel/install.d/35-intel-microcode-systemd.install
- Builds an Intel CPU microcode early initramfs if an initramfs generator is used that does not already bundle the CPU microcode.
- Installed by: [sys-firmware/intel-microcode](https://packages.gentoo.org/packages/sys-firmware/intel-microcode) if the [initramfs](https://packages.gentoo.org/useflags/initramfs)- Executed if: kernel-install command is add and initrd\_generator!=dracut.

- /usr/lib/kernel/install.d/40-dkms.install
- Automatically rebuild kernel modules registered with [DKMS](https://wiki.gentoo.org/wiki/DKMS) on kernel installation, and clean them up on kernel removal.
- Installed by: [sys-kernel/dkms](https://packages.gentoo.org/packages/sys-kernel/dkms)
- Executed if: Always

- /usr/lib/kernel/install.d/50-depmod.install
- Update kernel module dependencies on kernel installation, and cleanup depmod files on kernel removal.
- Installed by: [sys-apps/systemd](https://packages.gentoo.org/packages/sys-apps/systemd) or [sys-apps/systemd-utils](https://packages.gentoo.org/packages/sys-apps/systemd-utils)
- Executed if: Always

- /usr/lib/kernel/install.d/52-dracut.install
- Builds a initramfs or Unified Kernel Image (UKI) with dracut. A initramfs is built if initrd\_generator=dracut. If uki\_generator=dracut then an UKI is built, this will be the case if the [uki](https://packages.gentoo.org/useflags/uki)[sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel) but the [ukify](https://packages.gentoo.org/useflags/ukify)- Installed by: [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel) (prior to version 107 this was provided by [sys-kernel/dracut](https://packages.gentoo.org/packages/sys-kernel/dracut)
- Executed if: kernel-install command is add and initrd\_generator=dracut (USE [dracut](https://packages.gentoo.org/useflags/dracut)[dracut](https://packages.gentoo.org/useflags/dracut)[uki](https://packages.gentoo.org/useflags/uki)[ukify](https://packages.gentoo.org/useflags/ukify)

- /usr/lib/kernel/install.d/52-ugrd.install
- Builds an initramfs using the ugrd initramfs generator.
- Installed by: [sys-kernel/ugrd](https://packages.gentoo.org/packages/sys-kernel/ugrd)
- Executed if: kernel-install command is add and initrd\_generator=ugrd.

- /usr/lib/kernel/install.d/85-check-diskspace.install
- Checks if there is enough disk space on the target partition to install the kernel.
- Installed by: [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel)
- Executed if: Always

- /usr/lib/kernel/install.d/90-compat.install
- Install the staged (unified) kernel image (and initramfs) in a backwards compatibility layout similar to Debian's installkernel. And clean them up when the kernel is removed.
- Installed by: [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel)
- Executed if: layout=compat or layout=grub

- /usr/lib/kernel/install.d/90-loaderentry.install
- Install the staged kernel image (and initramfs) in [systemd-boot](https://wiki.gentoo.org/wiki/Systemd-boot)'s native format and register the new kernel with systemd-boot. And clean up these files when the kernel is removed.
- Installed by: [sys-apps/systemd](https://packages.gentoo.org/packages/sys-apps/systemd) or [sys-apps/systemd-utils](https://packages.gentoo.org/packages/sys-apps/systemd-utils)
- Executed if: layout=bls

- /usr/lib/kernel/install.d/90-runlilo.install
- Update [LILO](https://wiki.gentoo.org/wiki/LILO)'s configuration.
- Installed by: [sys-boot/lilo](https://packages.gentoo.org/packages/sys-boot/lilo)
- Executed if: Always

- /usr/lib/kernel/install.d/90-uki-copy.install
- Copy the staged Unified Kernel Image to the EFI/Linux directory on the [EFI System Partition](https://wiki.gentoo.org/wiki/EFI_System_Partition). And clean it up on kernel removal. This plugin script will exit fatally if there is no UKI to install.
- Installed by: [sys-apps/systemd](https://packages.gentoo.org/packages/sys-apps/systemd) or [sys-apps/systemd-utils](https://packages.gentoo.org/packages/sys-apps/systemd-utils)
- Executed if: layout=uki

- /usr/lib/kernel/install.d/90-zz-update-static.install
- Update version-less files or symlinks if they exist at the target location.
- Installed by: [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel)
- Executed if: kernel-install command is add and layout=uki or layout=efistub or layout=compat or layout=grub

- /usr/lib/kernel/install.d/91-grub-mkconfig.install
- Update [GRUB](https://wiki.gentoo.org/wiki/GRUB)'s configuration by running grub-mkconfig. Automatically finds Unified Kernel Images in the EFI/Linux directory on the [EFI System Partition](https://wiki.gentoo.org/wiki/EFI_System_Partition). Which grub.cfg to use is overridable with the `GRUB_CFG` environment variable.
- Installed by: [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel) with [grub](https://packages.gentoo.org/useflags/grub)- Executed if: layout=uki or layout=grub

- /usr/lib/kernel/install.d/91-sbctl.install
- Sign the installed (unified) kernel image with sbctl's keys and add it to the sbctl database. Upon kernel removal it is removed from the database. The plugin script will exit fatally if sbctl's keys are not already set up. Note that both dracut and ukify are capable of signing generated [unified kernel images](https://wiki.gentoo.org/wiki/Unified_kernel_image) by themselves and distribution kernels can already be signed using the [secureboot](https://packages.gentoo.org/useflags/secureboot)- Installed by: [app-crypt/sbctl](https://packages.gentoo.org/packages/app-crypt/sbctl)
- Executed if: Always

- /usr/lib/kernel/install.d/95-efistub-kernel-bootcfg.install
- Adds and removes UEFI boot entries on kernel installation or removal using kernel-bootcfg from [app-emulation/virt-firmware](https://packages.gentoo.org/packages/app-emulation/virt-firmware).
- Installed by: [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel) with USE [efistub](https://packages.gentoo.org/useflags/efistub)- Executed if: layout=efistub or layout=uki

- /usr/lib/kernel/install.d/95-refind-copy-icon.install
- Install a Gentoo icon for the [rEFInd](https://wiki.gentoo.org/wiki/REFInd) bootloader alongside the (unified) kernel image. Which icon file to use is overridable with the `REFIND_ICON` environment variable.
- Installed by: [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel) with USE [refind](https://packages.gentoo.org/useflags/refind)- Executed if: layout=uki or layout=compat

- /usr/lib/kernel/install.d/99-write-log.install
- Appends last installed kernel to /var/log/installkernel.log and updates state file at /var/lib/installkernel.
- Installed by: [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel)
- Executed if: Always

## Debian's installkernel

To select this installation method, disable the [systemd](https://packages.gentoo.org/useflags/systemd) [USE flag or set](https://wiki.gentoo.org/wiki/USE_flag) `SYSTEMD_KERNEL_INSTALL=0` in the environment.

**`/etc/portage/package.use/installkernel`**

```
 -systemd
```
`root #``emerge --ask sys-kernel/installkernel`
Or

**`/etc/env.d/99no-systemd-kernel-install`**

```
SYSTEMD_KERNEL_INSTALL=0
```
`root #``env-update`
### Configuration

Configuration of the traditional installkernel is done in /etc/kernel/install.conf and /usr/lib/kernel/install.conf, where the former takes precedence over the latter. Three configuration options can be set:

**`/etc/kernel/install.conf`**

```
layout=
initrd_generator=
uki_generator=
```
The default /usr/lib/kernel/install.conf configuration file is provided by [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel). Of course the configuration may also be changed manually in /etc/kernel/install.conf.

#### layout

Debian's traditional installkernel installs the kernel and initramfs or Unified Kernel Image in a layout that looks like this:

##### efistub

If the [efistub](https://packages.gentoo.org/useflags/efistub) [USE flag is enabled, then the install tree is relocated to the EFI System Partition (ESP) and the kernel image gains the](https://wiki.gentoo.org/wiki/USE_flag) `.efi` suffix.

##### uki

If the [uki](https://packages.gentoo.org/useflags/uki) [USE flag is enabled, then generated Unified Kernel Image is installed to the EFI/Linux directory on the EFI System Partition (ESP).](https://wiki.gentoo.org/wiki/USE_flag)

#### initrd\_generator

This setting controls which utility should be used to generate the [initramfs](https://wiki.gentoo.org/wiki/Initramfs). This setting is exposed to the plugins as `${INSTALLKERNEL_INITRD_GENERATOR}`.

The following options are available:

- [dracut](https://packages.gentoo.org/useflags/dracut)- [ugrd](https://packages.gentoo.org/useflags/ugrd)[UgRD](https://wiki.gentoo.org/wiki/UgRD) to generate an initramfs.
- `none` - Do not generate an initramfs on kernel installs.

When the [dracut](https://packages.gentoo.org/useflags/dracut)`dracut`. This setting is otherwise automatically set

#### uki\_generator

This setting controls which plugin should be used to generate the Unified Kernel Image. Currently two packages provide such a plugin: [sys-kernel/dracut](https://packages.gentoo.org/packages/sys-kernel/dracut) and [systemd](https://wiki.gentoo.org/wiki/Systemd) (via the [ukify](https://packages.gentoo.org/useflags/ukify) [flag on](https://wiki.gentoo.org/wiki/USE_flag) [sys-apps/systemd](https://packages.gentoo.org/packages/sys-apps/systemd) and [sys-apps/systemd-utils](https://packages.gentoo.org/packages/sys-apps/systemd-utils)). This setting is exposed to the plugins as `${INSTALLKERNEL_UKI_GENERATOR}` When the [ukify](https://packages.gentoo.org/useflags/ukify) [USE flag is enabled, this setting is automatically set to](https://wiki.gentoo.org/wiki/USE_flag) `ukify`. When the [ukify](https://packages.gentoo.org/useflags/ukify) [USE flag is disabled, but the](https://wiki.gentoo.org/wiki/USE_flag) [dracut](https://packages.gentoo.org/useflags/dracut) [and](https://wiki.gentoo.org/wiki/USE_flag) [uki](https://packages.gentoo.org/useflags/uki) [USE flags are enabled, this setting is automatically set to](https://wiki.gentoo.org/wiki/USE_flag) `dracut`. Otherwise this setting is automatically set to `none`.

### Runtime overrides

When the dracut and ukify plugins are installed (i.e. the [dracut](https://packages.gentoo.org/useflags/dracut) [and/or](https://wiki.gentoo.org/wiki/USE_flag) [ukify](https://packages.gentoo.org/useflags/ukify) [USE flags are enabled) they may be skipped at runtime using the](https://wiki.gentoo.org/wiki/USE_flag) `INSTALLKERNEL_INITRD_GENERATOR` and `INSTALLKERNEL_UKI_GENERATOR` environment variables. This may be useful on systems that have both the distribution kernels installed and manually configured kernels where the former enforces the enabling of the [dracut](https://packages.gentoo.org/useflags/dracut) [USE flag but the latter might not require an initramfs at all.](https://wiki.gentoo.org/wiki/USE_flag)

`root #``INSTALLKERNEL_INITRD_GENERATOR=none INSTALLKERNEL_UKI_GENERATOR=none make install`
Alternatively, these settings may be set permanently using /etc/kernel/install.conf:

**`/etc/kernel/install.conf`**

```
layout=compat
initrd_generator=none
uki_generator=none
```
Note that this override will apply to all installed kernels. It is also possible to specify several different configurations and switch between them at runtime using the `INSTALLKERNEL_CONF_ROOT` environment variable.

For example:

**`/etc/manual-kernel/install.conf`**

```
layout=compat
initrd_generator=none
uki_generator=none
```
`root #``INSTALLKERNEL_CONF_ROOT=/etc/manual-kernel make install`
Another method of overriding the default kernel installation is to use the `INSTALLKERNEL_PREINST_PLUGINS` and `INSTALLKERNEL_POSTINST_PLUGINS` environment variables. When these variables are set, only the specified plugins are run.

For example:

`root #``INSTALLKERNEL_POSTINST_PLUGINS="91-grub-mkconfig.install" make install`
### Customization

Custom plugins, which for example, generate [initramfs](https://wiki.gentoo.org/wiki/Initramfs) or [Unified Kernel Image](https://wiki.gentoo.org/wiki/Unified_Kernel_Image) (UKI) may be installed into /etc/kernel/preinst.d. An initramfs plugin should install a file named initrd on the ${INSTALLKERNEL\_STAGING\_AREA}, and it should respect the `INSTALLKERNEL_INITRD_GENERATOR` environment variable. A UKI plugin should install a file named uki.efi on the ${INSTALLKERNEL\_STAGING\_AREA} and it should respect the `INSTALLKERNEL_UKI_GENERATOR` environment variable.

Additionally custom plugins, which for example, update the bootloader configuration may be installed in /etc/kernel/postinst.d.

Plugins in /etc/kernel/preinst.d or /etc/kernel/postinst.d override default plugins in /usr/lib/kernel/preinst.d and /usr/lib/kernel/postinst.d with the same name.

### Descriptions of plugin scripts

In exection order, the `preinst.d` plugins

- /usr/lib/kernel/preinst.d/35-amd-microcode.install
- Builds an AMD CPU microcode early initramfs if an initramfs generator is used that does not already bundle the CPU microcode.
- Installed by: [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) if the [initramfs](https://packages.gentoo.org/useflags/initramfs)- Executed if: initrd\_generator!=dracut.

- /usr/lib/kernel/preinst.d/35-intel-microcode.install
- Builds an Intel CPU microcode early initramfs if an initramfs generator is used that does not already bundle the CPU microcode.
- Installed by: [sys-firmware/intel-microcode](https://packages.gentoo.org/packages/sys-firmware/intel-microcode) if the [initramfs](https://packages.gentoo.org/useflags/initramfs)- Executed if: initrd\_generator!=dracut.

- /usr/lib/kernel/preinst.d/52-dracut.install
- Builds a initramfs (initrd\_generator=dracut) or Unified Kernel Image (UKI) (uki\_generator=dracut) with dracut.
- Installed by: [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel) with USE [dracut](https://packages.gentoo.org/useflags/dracut)- Executed if: initrd\_generator=dracut or uki\_generator=dracut

- /usr/lib/kernel/preinst.d/52-ugrd.install
- Builds an initramfs using the ugrd initramfs generator.
- Installed by: [sys-kernel/ugrd](https://packages.gentoo.org/packages/sys-kernel/ugrd)
- Executed if: initrd\_generator=ugrd.

- /usr/lib/kernel/preinst.d/60-ukify.install
- Builds a Unified Kernel Image with systemd's ukify, will include a initramfs if one was built by an earlier plugin.
- Installed by: [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel) with USE [ukify](https://packages.gentoo.org/useflags/ukify)- Executed if: uki\_generator=ukify and layout=uki.

- /usr/lib/kernel/preinst.d/99-check-diskspace.install
- Checks if there is enough disk space on the target partition to install the kernel.
- Installed by: [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel)
- Executed if: Always

In execution order, the `postinst.d` plugins

- /usr/lib/kernel/postinst.d/40-dkms.install
- Automatically rebuild kernel modules registered with [DKMS](https://wiki.gentoo.org/wiki/DKMS) on kernel installation.
- Installed by: [sys-kernel/dkms](https://packages.gentoo.org/packages/sys-kernel/dkms)
- Executed if: Always

- /usr/lib/kernel/postinst.d/91-grub-mkconfig.install
- Update [GRUB](https://wiki.gentoo.org/wiki/GRUB)'s configuration by running grub-mkconfig. Automatically finds Unified Kernel Images in the EFI/Linux direcotry on the [EFI System Partition](https://wiki.gentoo.org/wiki/EFI_System_Partition). Which grub.cfg to use is overridable with the `GRUB_CFG` environment variable.
- Installed by: [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel) with USE [grub](https://packages.gentoo.org/useflags/grub)- Executed if: Always

- /usr/lib/kernel/postinst.d/95-efistub-uefi-mkconfig.install
- Adds UEFI boot entries for newly installed kernels and removes entries for removed kernels using [sys-boot/uefi-mkconfig](https://packages.gentoo.org/packages/sys-boot/uefi-mkconfig).
- Installed by: [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel) with USE [efistub](https://packages.gentoo.org/useflags/efistub)- Executed if: Always

- /usr/lib/kernel/postinst.d/95-refind-copy-icon.install
- Install a Gentoo icon for the [rEFInd](https://wiki.gentoo.org/wiki/REFInd) bootloader alongside the (unified) kernel image. Which icon file to use is overridable with the `REFIND_ICON` environment variable.
- Installed by: [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel) with USE [refind](https://packages.gentoo.org/useflags/refind)- Executed if: Always

- /usr/lib/kernel/postinst.d/99-write-log.install
- Appends last installed kernel to /var/log/installkernel.log and updates state file at /var/lib/installkernel.
- Installed by: [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel)
- Executed if: Always

- /usr/lib/kernel/postinst.d/90-runlilo.install
- Update [LILO](https://wiki.gentoo.org/wiki/LILO)'s configuration.
- Installed by: [sys-boot/lilo](https://packages.gentoo.org/packages/sys-boot/lilo)
- Executed if: Always

## USE configuration to boot layout mapping

The code boxes below map possible [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel) USE configurations to how the kernel and related files will be installed. It may be useful for users who are unsure which USE configuration suits their setup. `ESP` refers to the mount point of the [EFI System Partition](https://wiki.gentoo.org/wiki/EFI_System_Partition) which may be /efi, /boot, /boot/efi or /boot/EFI. `generic-uki` refers to the [generic-uki](https://packages.gentoo.org/useflags/generic-uki) [USE flag on the](https://wiki.gentoo.org/wiki/USE_flag) [Distribution kernels](https://wiki.gentoo.org/wiki/Project:Distribution_Kernel).

### Systemd kernel-install (USE=+systemd)

#### Layouts with GRUB

Plain kernel image installation.

Plain kernel image installation, with initramfs generated by dracut.

Plain kernel image installation, initramfs is pregenerated by the distribution kernel package.

##### Layouts with Unified Kernel Images

Unified kernel image installation, uki is generated with ukify and is then installed to the ESP.

Unified kernel image installation, uki is generated with dracut and is then installed to the ESP.

Unified kernel image installation, uki is generated with ukify and includes a initramfs generated with dracut and is then installed to the ESP.

Unified kernel image installation, uki is pregenerated by the distribution kernel package.

#### Layouts with systemd-boot

Plain kernel image installation.

Plain kernel image installation, with initramfs generated by dracut.

Plain kernel image installation, initramfs is pregenerated by the distribution kernel package.

##### Layouts with Unified Kernel Images

Unified kernel image installation, uki is generated with ukify and is then installed to the ESP.

Unified kernel image installation, uki is generated with dracut and is then installed to the ESP.

Unified kernel image installation, uki is generated with ukify and includes a initramfs generated with dracut and is then installed to the ESP.

Unified kernel image installation, uki is pregenerated by the distribution kernel package.

#### Layouts with rEFInd

Plain kernel image installation with icon for rEFInd.

Plain kernel image installation, with a initramfs generated by dracut and icon for rEFInd.

Plain kernel image installation with icon for rEFInd, initramfs is pregenerated by the distribution kernel package.

##### Layouts with Unified Kernel Images

Unified kernel image installation with icon for rEFInd, uki is generated with ukify and is then installed to the ESP.

Unified kernel image installation with icon for rEFInd, uki is generated with dracut and is then installed to the ESP.

Unified kernel image installation with icon for rEFInd, uki is generated with ukify and includes a initramfs generated with dracut and is then installed to the ESP.

Unified kernel image installation with icon for rEFInd, uki is pregenerated by the distribution kernel package.

#### Layouts without GRUB/systemd-boot/rEFInd (other bootloaders)

Plain kernel image installation.

Plain kernel image installation, with a initramfs generated by dracut.

Plain kernel image installation, initramfs is pregenerated by the distribution kernel package.

##### Layouts with Unified Kernel Images

Unified kernel image installation, uki is generated with ukify and is then installed to the ESP.

Unified kernel image installation, uki is generated with dracut and is then installed to the ESP.

Unified kernel image installation, uki is generated with ukify and includes a initramfs generated with dracut and is then installed to the ESP.

Unified kernel image installation, uki is pregenerated by the distribution kernel package.

### Traditional installkernel (USE=-systemd)

#### Layouts with GRUB

Plain kernel image installation.

Plain kernel image installation, with initramfs generated by dracut.

Plain kernel image installation, initramfs is pregenerated by the distribution kernel package.

##### Layouts with Unified Kernel Images

Unified kernel image installation, with an uki generated by dracut (`uki` has no effect since there is no unified kernel image).

Unified kernel image installation, uki is generated with ukify and is then copied to the ESP.

Unified kernel image installation, uki is generated with ukify and includes a initramfs generated with dracut and is then copied to the ESP.

Unified kernel image installation, uki is pregenerated by the distribution kernel package and is then copied to the ESP.

#### Layouts with rEFInd

Plain kernel image installation with icon for rEFInd.

Plain kernel image installation, with a initramfs generated by dracut and icon for rEFInd.

Plain kernel image installation with icon for rEFInd, initramfs is pregenerated by the distribution kernel package.

##### Layouts with Unified Kernel Images

Unified kernel image installation, with a initramfs generated by dracut.

Unified kernel image installation with icon for rEFInd, uki is generated with ukify and is then copied to the ESP.

Unified kernel image installation with icon for rEFInd, uki is generated with ukify and includes a initramfs generated with dracut and is then copied to the ESP.

Unified kernel image installation with icon for rEFInd, uki is pregenerated by the distribution kernel package and is then copied to the ESP.

#### Layouts without GRUB/systemd-boot/rEFInd (other bootloaders)

Plain kernel image installation.

Plain kernel image installation, with a initramfs generated by dracut.

Plain kernel image installation, initramfs is pregenerated by the distribution kernel package.

##### Layouts with Unified Kernel Images

Unified kernel image installation, with uki generated by dracut.

Unified kernel image installation, uki is generated with ukify and is then copied to the ESP.

Unified kernel image installation, uki is generated with ukify and includes a initramfs generated with dracut and is then copied to the ESP.

Unified kernel image installation, uki is pregenerated by the distribution kernel package and is then copied to the ESP.

## See also

- [GRUB](https://wiki.gentoo.org/wiki/GRUB) — a multiboot secondary [bootloader](https://wiki.gentoo.org/wiki/Bootloader) capable of loading kernels from a variety of [filesystems](https://wiki.gentoo.org/wiki/Filesystem) on most system architectures.
- [systemd-boot](https://wiki.gentoo.org/wiki/Systemd-boot) — a minimal [UEFI](https://wiki.gentoo.org/wiki/UEFI) boot manager.
- [EFI stub](https://wiki.gentoo.org/wiki/EFI_stub)
- [initramfs](https://wiki.gentoo.org/wiki/Initramfs) — is used to prepare Linux systems during boot before the **init** process starts.
- [Project:Distribution\_Kernel](https://wiki.gentoo.org/wiki/Project:Distribution_Kernel) — maintains sys-kernel/\*-kernel packages.
