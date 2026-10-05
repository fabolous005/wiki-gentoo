<!-- source: https://wiki.gentoo.org/wiki/Systemd/systemd-boot | group: Gentoo Wiki (Main) | wiki-title: Systemd/systemd-boot -->
---
title: systemd/systemd-boot
url: https://wiki.gentoo.org/wiki/Systemd/systemd-boot
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-30"
fingerprint: "1407f31a85a73d94"
license: CC BY-SA 4.0
---

# systemd/systemd-boot

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**systemd-boot**, formerly known as *gummiboot* (rubber dinghy), is a minimal [UEFI](https://wiki.gentoo.org/wiki/UEFI) boot manager.

## Features

- Bootloader integration with [systemd](https://wiki.gentoo.org/wiki/Systemd) provided by the bootctl command.
- Ability to select next boot.
- Easy and simple configuration files which can be generated automatically.
- Auto add Windows and EFI firmware setup entries.
- Change timeout, default entry, edit [kernel command line](https://wiki.gentoo.org/wiki/Kernel/Command-line_parameters) options on the fly from the boot menu.

## Pre-Deployment Considerations

The [Boot Loader Specification](https://uapi-group.org/specifications/specs/boot_loader_specification/) outlines a standard for bootloaders to follow. This specification is one of the methods used by systemd-boot to determine the location of the [ESP](https://wiki.gentoo.org/wiki/EFI_System_Partition) and XBOOTLDR partitions.

The ESP and XBOOTLDR partitions must be partitions on a block device. [Mirrored partitions, for example mdadm RAID1 devices, are not supported](https://github.com/systemd/systemd/issues/17298#issuecomment-1288810057). Systems which mirror the bootloader partitons for reliability should not use systemd-boot.

When using XBOOTLDR, the ESP is used to store the bootloader's PE32+ files (.efi files) and their configuration files, while the XBOOTLDR partition is used to store [Linux kernels](https://wiki.gentoo.org/wiki/Kernel), [initramfs](https://wiki.gentoo.org/wiki/Initramfs), and configuration files.

Use [partitioning tools](https://wiki.gentoo.org/wiki/Partitioning_tools) to create an XBOOTLDR partition with Partition Type GUID `bc13c2ff-59e6-4262-a352-b275fd6f7172`, and then format that partition with any file system that the target EFI implementation supports (or can be made to support). All EFI implementations support the use of [FAT](https://wiki.gentoo.org/wiki/FAT) filesystems.

- If using fdisk, the Partition Type GUID can be set using the **t** command to change the partition type, and then selecting **136** to set it to "Linux extended boot".
- If using gdisk, it can be set using the **t** command to change the partition type code, and then specifying hex code **EA00** to set it to "XBOOTLDR partition".

The Boot Loader Specification recommends that, when a XBOOTLDR partition exists, it be mounted at /boot, and the ESP, at /efi. These should be mounted using an autofs or automount implementation so that they are only mounted when needed.

There are many mechanisms for implementing this; for [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) users, the [sys-fs/autofs](https://packages.gentoo.org/packages/sys-fs/autofs) package can be used. For systemd users, either adding the `x-systemd.automount` option to the /etc/fstab entry for the XBOOTLDR and ESP partitions, creating a [systemd automount unit](https://blog.tomecek.net/post/automount-with-systemd/), or [Discoverable Partitions Specification](https://uapi-group.org/specifications/specs/discoverable_partitions_specification/) automatically created mounts can be used.

## Installation

systemd-boot is included within [sys-apps/systemd](https://packages.gentoo.org/packages/sys-apps/systemd) and, for users of non-systemd-init systems, [sys-apps/systemd-utils](https://packages.gentoo.org/packages/sys-apps/systemd-utils).

### Kernel

Because systemd-boot can only load EFI executables, the desired kernel must support EFI stub (`CONFIG_EFI_STUB=y`):

**Enable EFI stub support (`CONFIG_EFI_STUB`)**

### OpenRC

Install the [sys-apps/systemd-utils](https://packages.gentoo.org/packages/sys-apps/systemd-utils) package with the [boot](https://packages.gentoo.org/useflags/boot) [USE flag enabled:](https://wiki.gentoo.org/wiki/USE_flag)

`root #````
mkdir -p /etc/portage/package.use
```
`root #````
echo "sys-apps/systemd-utils boot kernel-install" >> /etc/portage/package.use/systemd-utils
```
`root #````
emerge --ask --oneshot --verbose sys-apps/systemd-utils
```
### systemd

For versions of systemd >= 254, emerge [sys-apps/systemd](https://packages.gentoo.org/packages/sys-apps/systemd) with the [boot](https://packages.gentoo.org/useflags/boot) [USE flag enabled:](https://wiki.gentoo.org/wiki/USE_flag)

`root #````
mkdir -p /etc/portage/package.use
```
`root #````
echo "sys-apps/systemd boot" >> /etc/portage/package.use/systemd
```
`root #````
emerge --ask --oneshot --verbose sys-apps/systemd
```
For older versions, the [gnuefi](https://packages.gentoo.org/useflags/gnuefi) [USE flag toggles this functionality:](https://wiki.gentoo.org/wiki/USE_flag)

`root #````
mkdir -p /etc/portage/package.use
```
`root #````
echo "sys-apps/systemd gnuefi" >> /etc/portage/package.use/systemd
```
`root #````
emerge --ask --oneshot --verbose sys-apps/systemd
```
### Installation to the ESP (EFI system partition)

First ensure that the system has booted in UEFI mode - if following command returns an error the system is not booted in UEFI mode:

`root #``ls /sys/firmware/efi/efivars`
Then, use bootctl to install systemd-boot to the ESP:

`root #``bootctl install`
## Configuration

Overview:

- Main configuration for systemd-boot is done in file /loader/loader.conf of the [EFI System Partition](https://wiki.gentoo.org/wiki/EFI_System_Partition) (ESP).
- Boot menu entries are generated for each file ending with .conf located in /loader/entries of the ESP.
- EFI PE32+ executable files (including kernel EFI stubs) and initramfs files can be placed anywhere in the ESP.

### loader.conf

Although its syntax is well documented in [loader.conf(5)](https://man.archlinux.org/man/loader.conf.5.en)[, here is the example:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

**`/efi/loader/loader.conf`**

The name of the *default* entry is the file name of the menu entry file, as created in the next section, without the .conf suffix.

### Menu entry files

The boot menu will show one entry for each .conf file.

Following is an example menu entry file named "gentoo-sources-kernel" where the kernel and initramfs are at /efi/vmlinuz and /efi/initramfs respectively, assuming that the ESP is mounted at /efi like in the example from the Handbook:

**`/efi/loader/entries/gentoo-sources-kernel.conf`**

**Menu entry file**

For more options please refer to the [Bootloader Specification](https://uapi-group.org/specifications/specs/boot_loader_specification/#type-1-boot-loader-specification-entries)

Systemd's `kernel-install` can be used to automate the process of generating menu entry files (and initramfs) whenever `make install` is called during a kernel build. It is enabled with the [systemd-boot](https://packages.gentoo.org/useflags/systemd-boot) [flag on](https://wiki.gentoo.org/wiki/USE_flag) [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel):

**`/etc/portage/package.use/sd-boot`**

`root #``emerge --ask sys-kernel/installkernel`
The options documented in [kernel-install(8)](https://man.archlinux.org/man/kernel-install.8.en) [can be used to customize the installed menu entries.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

As an example, the [kernel cmdline](https://wiki.gentoo.org/wiki/Kernel/Command-line_parameters) can be customized by updating /etc/kernel/cmdline:

**`/etc/kernel/cmdline`**

#### Unified Kernel Images

[Unified kernel images](https://wiki.gentoo.org/wiki/Unified_kernel_image) (UKIs) do not require a bootloader entry. Systemd-boot automatically discovers UKIs in the EFI/Linux directory on the [EFI System Partition](https://wiki.gentoo.org/wiki/EFI_System_Partition). If custom options should be applied when booting the UKI then it is possible to automate adding a bootloader entry with these custom options using a kernel-install plugin:

**`/etc/kernel/install.d/95-uki-with-custom-opts.install`**

```
#!/usr/bin/env bash
COMMAND="${1}"
KERNEL_VERSION="${2}"
BOOT_DIR_ABS="${3}"
KERNEL_IMAGE="${4}"
if [[ "${KERNEL_INSTALL_LAYOUT}" != "uki" ]]; then
    exit 0
fi
if [[ ${COMMAND} == add ]]; then
    cat > "${BOOT_DIR_ABS}/loader/entries/1-gentoo-uki-${KERNEL_VERSION}.conf" <<- EOF
        title Gentoo
        linux /EFI/Linux/${ENTRY_TOKEN}-${KERNEL_VERSION}.efi
        options <my custom options>
        sort-key <my custom sort key>
    EOF
elif [[ ${COMMAND} == remove ]]; then
    rm -f "${BOOT_DIR_ABS}/loader/entries/1-gentoo-uki-${KERNEL_VERSION}.conf"
fi
```
The default entry, increase/decrease timeout, edit command line options, and change resolution are accessible right from boot menu. Refer to [systemd-boot(7)](https://man.archlinux.org/man/systemd-boot.7.en)[’s KEY-BINDINGS section for keyboard shortcuts.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

## Usage

If the [secureboot](https://packages.gentoo.org/useflags/secureboot) [USE flag is enabled on](https://wiki.gentoo.org/wiki/USE_flag) [sys-apps/systemd](https://packages.gentoo.org/packages/sys-apps/systemd) or [sys-apps/systemd-utils](https://packages.gentoo.org/packages/sys-apps/systemd-utils), [Portage](https://wiki.gentoo.org/wiki/Portage) recognizes the `SECUREBOOT_SIGN_KEY` and `SECUREBOOT_SIGN_CERT` environment variables which allow specifying a key (or pkcs11 URI) and certificate to sign the built EFI executable for use with [Secure Boot](https://wiki.gentoo.org/wiki/Secure_Boot). When bootctl install or bootctl update are called the signed version will be installed to the [EFI System Partition](https://wiki.gentoo.org/wiki/EFI_System_Partition).

**`/etc/portage/make.conf`**

**make.conf**

```
SECUREBOOT_SIGN_KEY="..."
SECUREBOOT_SIGN_CERT="..."
```
#### Shim

To successfully boot with [Secure Boot](https://wiki.gentoo.org/wiki/Secure_Boot) enabled the firmware must be configured to accept the used certificate. Alternatively [sys-boot/shim](https://packages.gentoo.org/packages/sys-boot/shim) can be used to chain-load systemd-boot, the [Shim](https://wiki.gentoo.org/wiki/Shim) binary is pre-signed with the 3rd-party Microsoft certificate which is accepted by default on most motherboards. After installing [sys-boot/shim](https://packages.gentoo.org/packages/sys-boot/shim), copy the installed EFI executables to the [EFI System Partition](https://wiki.gentoo.org/wiki/EFI_System_Partition). The following is an example for amd64 and an EFI System Partition mounted at /efi:

`root #``emerge --ask sys-boot/shim``root #``cp /usr/share/shim/BOOTX64.EFI /efi/EFI/systemd/shimx64.efi``root #``cp /usr/share/shim/mmx64.efi /efi/EFI/systemd/mmx64.efi`
Then configure shim to load systemd-boot during boot:

`root #``echo "systemd-bootx64.efi," | iconv -f us-ascii -t utf-16le > /efi/EFI/systemd/options.csv`
And import the used `SECUREBOOT_SIGN_CERT` to the Machine Owner Key (MOK) list, to do so the certificate must usually be converted from PEM format to DER format first:

`root #``openssl x509 -in /path/to/my/secureboot_cert.pem -inform pem -out /path/to/my/secureboot_cert.der -outform der``root #``mokutil --import /path/to/my/secureboot_cert.der`
Mokutil will prompt to set a password, this can be any password. After rebooting the MokManager will ask for this password to confirm the import of the new certificate.

Finally, configure the [UEFI](https://wiki.gentoo.org/wiki/UEFI) to boot \EFI\systemd\shimx64.efi instead of \EFI\systemd\systemd-bootx64.efi using [efibootmgr](https://wiki.gentoo.org/wiki/Efibootmgr).

### ESP file update process

Although updates to the systemd-boot related files are maintained by [Portage](https://wiki.gentoo.org/wiki/Portage), it will still be necessary for bootloader related files within the EFI System Partition to be updated each time the package manager updates the files within the package. This will provide important feature enhancement and bug fixes to the files within the ESP.

#### systemd service

systemd users can simply enable the service unit systemd-boot-update - if a handbook installation was completed this is probably already enabled. systemd-boot-update will check if the installed systemd-boot is outdated every time the system boots and will update it if required.

`root #``systemctl enable --now systemd-boot-update.service`
## Troubleshooting

### Failed to open boot loader directory /usr/lib/systemd/boot/efi: No such file or directory

Problem: Running the bootctl command with the install or update subcommands results in the following output:

`root #``bootctl install`
Failed to open boot loader directory /usr/lib/systemd/boot/efi: No such file or directory

This is because the bootctl command cannot locate the architecture specific EFI files within the /usr/lib/systemd/boot/efi. Resolve by enabling the [boot](https://packages.gentoo.org/useflags/boot) [USE flag and recompiling](https://wiki.gentoo.org/wiki/USE_flag) [sys-apps/systemd](https://packages.gentoo.org/packages/sys-apps/systemd).

**`/etc/portage/package.use/systemd`**

**Enable boot USE flag**

`root #``emerge -1uN sys-apps/systemd`
## See also

- [Systemd](https://wiki.gentoo.org/wiki/Systemd) — a modern SysV-style init and [rc](https://wiki.gentoo.org/wiki/Rc) replacement for Linux systems.
- [UEFI](https://wiki.gentoo.org/wiki/UEFI) — a firmware standard for boot ROM designed to provide a stable API for interacting with system hardware. On [x86](https://en.wikipedia.org/wiki/x86) it replaced the legacy [BIOS](https://wiki.gentoo.org/wiki/BIOS).
