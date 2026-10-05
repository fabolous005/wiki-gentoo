<!-- source: https://wiki.gentoo.org/wiki/Fwupd | group: Gentoo Wiki (Main) | wiki-title: Fwupd -->
---
title: fwupd
url: https://wiki.gentoo.org/wiki/Fwupd
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-11-06"
fingerprint: ec00b81c08d6b9c2
license: CC BY-SA 4.0
---

# fwupd

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


fwupd is a daemon that provides a safe, reliable way of applying firmware updates on Linux. Vendors can upload device firmware images to the [Linux Vendor Firmware Service (LVFS)](https://fwupd.org/lvfs/devicelist) web portal. These firmware images are in turn distributed to devices running the firmware update daemon.

fwupd is colloquially referred to as **L**inux **V**endor **F**irmware **S**ervice (LVFS), which is the name of the web portal used by vendors to upload device firmware.

## Installation

## Emerge

`root #``emerge --ask sys-apps/fwupd`
### USE flags


| [+archive](https://packages.gentoo.org/useflags/+archive) | Use app-arch/libarchive for archives support | 
| [+gnutls](https://packages.gentoo.org/useflags/+gnutls) | Prefer net-libs/gnutls as SSL/TLS provider (ineffective with USE=-ssl) | 
| [amdgpu](https://packages.gentoo.org/useflags/amdgpu) | Build and install AMD dGPU (Navi3x and above) plugin | 
| [bash-completion](https://packages.gentoo.org/useflags/bash-completion) | Enable bash-completion support | 
| [bluetooth](https://packages.gentoo.org/useflags/bluetooth) | Enable Bluetooth Support | 
| [cbor](https://packages.gentoo.org/useflags/cbor) | Enable CBOR support for coSWID and uSWID via dev-libs/libcbor | 
| [elogind](https://packages.gentoo.org/useflags/elogind) | Enable session tracking via sys-auth/elogind | 
| [flashrom](https://packages.gentoo.org/useflags/flashrom) | Enable flashrom plugin via sys-apps/flashrom | 
| [gtk-doc](https://packages.gentoo.org/useflags/gtk-doc) | Build and install gtk-doc based developer documentation for dev-util/devhelp, IDE and offline use | 
| [introspection](https://packages.gentoo.org/useflags/introspection) | Add support for GObject based introspection | 
| [lzma](https://packages.gentoo.org/useflags/lzma) | Support for LZMA compression algorithm | 
| [minimal](https://packages.gentoo.org/useflags/minimal) | Install a very minimal build (disables, for example, plugins, fonts, most drivers, non-critical features) | 
| [modemmanager](https://packages.gentoo.org/useflags/modemmanager) | Build and install Modem Manager plugin | 
| [nvme](https://packages.gentoo.org/useflags/nvme) | Build and install NVMe plugin | 
| [policykit](https://packages.gentoo.org/useflags/policykit) | Enable PolicyKit (polkit) authentication support | 
| [protobuf](https://packages.gentoo.org/useflags/protobuf) | Enable protobuf plugins | 
| [readline](https://packages.gentoo.org/useflags/readline) | Enable support for libreadline, a GNU line-editing library that almost everyone wants | 
| [seccomp](https://packages.gentoo.org/useflags/seccomp) | Enable seccomp (secure computing mode) to perform system call filtering at runtime to increase security of programs | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [spi](https://packages.gentoo.org/useflags/spi) | Install Intel-SPI plugin | 
| [synaptics](https://packages.gentoo.org/useflags/synaptics) | Install synaptics plugin | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [tpm](https://packages.gentoo.org/useflags/tpm) | Install Trusted Platform Module plugin | 
| [uefi](https://packages.gentoo.org/useflags/uefi) | Enable UEFI support | 
| [valgrind](https://packages.gentoo.org/useflags/valgrind) | Enable annotations for accuracy. May slow down runtime slightly. Safe to use even if not currently using dev-debug/valgrind | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

The above list of USE flag is not comprehensive. Use equery (part of the [app-portage/gentoolkit](https://packages.gentoo.org/packages/app-portage/gentoolkit) package) for a full list:

`user $``equery uses sys-apps/fwupd`
## Configuration

### Service

#### OpenRC

Start fwupd:

`root #``rc-service fwupd start`
Start fwupd at boot:

`root #``rc-update add fwupd default`
#### systemd

fwupd.service is launched through [systemd](https://wiki.gentoo.org/wiki/Systemd) via [D-Bus](https://wiki.gentoo.org/wiki/D-Bus) automatically whenever it is needed.

## Usage

Get help on what fwupd can do:

`user $``fwupdmgr --help`
List detected and supported devices. When the device is [listed](https://fwupd.org/lvfs/devicelist) but not displayed, a plugin (e.g. the `uefi` USE flag) is missing.

`user $``fwupdmgr get-devices`
Refresh metadata from remote server:

`user $``fwupdmgr refresh`
Check available updates for the user devices:

`user $``fwupdmgr get-updates`
Install updates:

`user $``fwupdmgr update`
## Troubleshooting

### fwupdmgr: /usr/lib64/libxmlb.so.2: no version information available (required by /usr/lib64/fwupd-2.0.16/libfwupdplugin.so)

[dev-libs/libxmlb](https://packages.gentoo.org/packages/dev-libs/libxmlb) was updated after installation of [sys-apps/fwupd](https://packages.gentoo.org/packages/sys-apps/fwupd). These warning are safe to ignore, but to get rid of them, rebuild fwupd against new libxmlb:

`root #``emerge --oneshot sys-apps/fwupd`
## See also

- [Linux firmware](https://wiki.gentoo.org/wiki/Linux_firmware) — contains [binary blobs](https://en.wikipedia.org/wiki/binary_blobs) of firmware necessary for partial or full functionality of certain hardware devices on Linux systems.
- [Kernel](https://wiki.gentoo.org/wiki/Kernel) — a central part of the Gentoo [operating system (OS)](https://en.wikipedia.org/wiki/operating_system)
- [BIOS Update](https://wiki.gentoo.org/wiki/BIOS_Update) — describes how to apply a BIOS update on a Gentoo system.

## External Resources

- UEFI revocation database: [https://uefi.org/revocationlistfile](https://uefi.org/revocationlistfile)
