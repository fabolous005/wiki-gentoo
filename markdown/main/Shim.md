<!-- source: https://wiki.gentoo.org/wiki/Shim | group: Gentoo Wiki (Main) | wiki-title: Shim -->
---
title: Shim
url: https://wiki.gentoo.org/wiki/Shim
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-24"
fingerprint: "1718c41898aa86a0"
license: CC BY-SA 4.0
---

# Shim

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Shim is an alternative method of managing accepted [Secure Boot](https://wiki.gentoo.org/wiki/Secure_Boot) keys without touching the [UEFI](https://wiki.gentoo.org/wiki/UEFI) firmware settings. In its simplest configuration, shim will just chainload any EFI executable.

## Configuration

The global `secureboot` USE flag can be enabled to automatically sign any EFI executables installed by a package. To use this flag the `SECUREBOOT_SIGN_KEY` and `SECUREBOOT_SIGN_CERT` user variables must be set in `make.conf` and point to a valid PEM format [OpenSSL](https://wiki.gentoo.org/wiki/Certificates) key (or PKCS11 URI) and certificate respectively.

## Generating keys

The procedure for generating a new [OpenSSL](https://wiki.gentoo.org/wiki/Certificates) key/certificate pair is documented on the [Secure Boot](https://wiki.gentoo.org/wiki/Secure_Boot) page. Note that when shim is used only one key (called the `Machine Owner Key`) is required. Since Shim is already signed by the 3rd-party Microsoft certificate accepted by default on most motherboards there is no need to create and manage a `Platform Key`, `Key Exchange Key`, or the (forbidden) `Signature Database`.

## Installation

The [sys-boot/shim](https://packages.gentoo.org/packages/sys-boot/shim) package provides a pre-compiled version of shim distributed by Fedora and pre-signed with the `3rd-party Microsoft certificate`. This Microsoft certificate is accepted by default on most UEFI-enabled motherboards, this allows users to delegate secure boot key management to shim without having to touch the firmware's default [UEFI](https://wiki.gentoo.org/wiki/UEFI) [Secure Boot](https://wiki.gentoo.org/wiki/Secure_Boot) keys.

The [sys-boot/mokutil](https://packages.gentoo.org/packages/sys-boot/mokutil) package makes it possible to manage shim's `Machine Owner Key` (MOK) list from within the Linux operating system.

Install both with:

`root #``emerge sys-boot/shim sys-boot/mokutil`
## Setup

For shim to be able to verify the signed boot files it must be loaded before the boot loader. Unfortunately the shim distributed by Fedora is hard coded to load [GRUB](https://wiki.gentoo.org/wiki/GRUB). To use other boot loaders the boot loader EFI file may be renamed to grubx64.efi (where `x64` will need to be adjusted for architectures other then amd64). The detailed process for each supported boot loader is outlined below.

After the steps for the chosen boot loader have been followed, the systems boot order must be adjusted to load shim first instead of the boot loader. Either use [efibootmgr](https://wiki.gentoo.org/wiki/Efibootmgr) or the system's [UEFI](https://wiki.gentoo.org/wiki/UEFI) firmware interface.

Example if you have followed GRUB section, and if /dev/sda holds the EFI partition:

`root #``efibootmgr --disk /dev/sda --part 1 --create -L "shim" -l '\EFI\Gentoo\shimx64.efi'`
### GRUB

[sys-boot/grub](https://packages.gentoo.org/packages/sys-boot/grub) installs a prebuilt and signed stand-alone EFI executable if the [secureboot](https://packages.gentoo.org/useflags/secureboot) [USE flag is enabled. Copy it, Shim, and the MokManager to the same directory on the](https://wiki.gentoo.org/wiki/USE_flag) [EFI System Partition](https://wiki.gentoo.org/wiki/EFI_System_Partition). For example:

`root #````
cp /usr/share/shim/BOOTX64.EFI /efi/EFI/Gentoo/shimx64.efi
```
`root #````
cp /usr/share/shim/mmx64.efi /efi/EFI/Gentoo/mmx64.efi
```
`root #````
cp /usr/lib/grub/grub-x86_64.efi.signed /efi/EFI/Gentoo/grubx64.efi
```
Then register shim with the firmware:

`root #``efibootmgr --disk /dev/sda --part 1 --create -L "GRUB via Shim" -l '\EFI\Gentoo\shimx64.efi'`
Note that this prebuilt and signed stand-alone version of grub reads the `grub.cfg` from a different location then usual. Instead of the default /boot/grub/grub.cfg the config file should be in the same directory that the grub EFI executable is in, e.g. /efi/EFI/Gentoo/grub.cfg. When [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel) is used to install the kernel and update the grub configuration then the `GRUB_CFG` environment variable may be used to override the usual location of the grub config file.

For example:

`root #``grub-mkconfig -o /efi/EFI/Gentoo/grub.cfg`
Or, via [installkernel](https://wiki.gentoo.org/wiki/Installkernel):

**`/etc/env.d/99grub`**

`root #``env-update`
### systemd-boot

Systemd-boot is signed by portage when the [secureboot](https://packages.gentoo.org/useflags/secureboot) [USE flag is enabled. A postinst hook to automatically setup shim with](https://wiki.gentoo.org/wiki/USE_flag) [systemd-boot](https://wiki.gentoo.org/wiki/Systemd-boot), via renaming systemd-boot to grubx64.efi is available on the [systemd-boot](https://wiki.gentoo.org/wiki/Systemd-boot) wiki page. Alternatively, if the firmware supports it, it is possible to create a boot entry to boot systemd-boot via shim:

`root #````
bootctl install --no-variables
```
`root #````
cp /usr/share/shim/BOOTX64.EFI /efi/EFI/systemd/shimx64.efi
```
`root #````
cp /usr/share/shim/mmx64.efi /efi/EFI/systemd/mmx64.efi
```
`root #``efibootmgr --disk /dev/sda --part 1 --create --label "Systemd-boot via Shim" --loader '\EFI\systemd\shimx64.efi' --unicode '\EFI\systemd\systemd-bootx64.efi'`
### rEFInd

To setup [rEFInd](https://wiki.gentoo.org/wiki/REFInd) with shim:

`root #``refind-install ... --shim /usr/share/shim/BOOTX64.EFI ...`
The rEFInd efi file is signed by portage the [secureboot](https://packages.gentoo.org/useflags/secureboot) [USE flag is enabled.](https://wiki.gentoo.org/wiki/USE_flag)

## Signing the kernel

The [Distribution Kernels](https://wiki.gentoo.org/wiki/Project:Distribution_Kernel) are automatically signed if `USE=secureboot` is enabled. If a manually configured and compiled kernel is used instead it must be manually signed using [app-crypt/sbsigntools](https://packages.gentoo.org/packages/app-crypt/sbsigntools) where `SECUREBOOT_SIGN_KEY` and `SECUREBOOT_SIGN_CERT` should be replaced with your respective key and certificate:

`root #``sbsign --key ${SECUREBOOT_SIGN_KEY} --cert ${SECUREBOOT_SIGN_CERT} --output /boot/EFI/Gentoo/kernel-x.y.z-gentoo.efi /boot/EFI/Gentoo/kernel-x.y.z-gentoo.efi`
If `SECUREBOOT_SIGN_KEY` is a PKCS11 URI the `--engine pkcs11` argument has to be added.

## Managing the MOK list

To successfully boot with [Secure Boot](https://wiki.gentoo.org/wiki/Secure_Boot) enabled the used `SECUREBOOT_SIGN_CERT` should be added to the MOK list. Unfortunately while `sbsign` requires keys and certificates in PEM format, the MOK list requires the certificate in DER format. Therefore the certificate must first be converted to DER format:

`root #``openssl x509 -inform pem -in ${SECUREBOOT_SIGN_CERT} -outform der -out /boot/sbcert.der`
An import request for shim can be generated with:

`root #``mokutil --import /boot/sbcert.der`
Next reboot, shim will find that there is a pending import request and launch MokManager. Enter the password and complete the import process.

### Signed kernel modules

To successfully boot with Secure Boot enabled the kernel modules must be signed as well. The kernel build system automatically embeds the certificate that was used to sign the modules into the kernel image, as such it is not required to import this certificate into the MOK list. If however some kernel modules are signed with a different key then was used when building the kernel, then the accompanying certificate must also be imported into the MOK list. This situation may occur when a pre-compiled kernel binpkg was installed (e.g. [sys-kernel/gentoo-kernel-bin](https://packages.gentoo.org/packages/sys-kernel/gentoo-kernel-bin)) as well as one or more external third-party modules. The original key that was used for the kernel binpkg may not be available on this system, and as such the external third-party modules will have to be signed with a different key.

## Non-validating mode

Shim can be configured not to validate any signatures in the EFI executable to be run; the executable can even be unsigned. To do this run

`root #``mokutil --disable-validation`
You will need to key in a password.

After rebooting shim will show an option to "Change secure boot state". Answer the password quiz (it will prompt for a character *within* the password multiple times) then there will be a "No/Yes" prompt, press Yes and then Reboot and then shim will show "Running in insecure mode", and then load grubx64.efi or another EFI executable.

## See also

- [UEFI](https://wiki.gentoo.org/wiki/UEFI) — a firmware standard for boot ROM designed to provide a stable API for interacting with system hardware. On [x86](https://en.wikipedia.org/wiki/x86) it replaced the legacy [BIOS](https://wiki.gentoo.org/wiki/BIOS).
- [Efibootmgr](https://wiki.gentoo.org/wiki/Efibootmgr) — a tool for managing [UEFI](https://wiki.gentoo.org/wiki/UEFI) boot entries.
- [Secure Boot](https://wiki.gentoo.org/wiki/Secure_Boot) — an enhancement of the security of the pre-boot process of a [UEFI](https://wiki.gentoo.org/wiki/UEFI) system.
- [EFI stub](https://wiki.gentoo.org/wiki/EFI_stub)
