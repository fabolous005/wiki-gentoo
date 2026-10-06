<!-- source: https://wiki.gentoo.org/wiki/DKMS | group: Gentoo Wiki (Main) | wiki-title: DKMS -->
---
title: DKMS
url: https://wiki.gentoo.org/wiki/DKMS
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-04"
fingerprint: "5693323e8ce23f21"
license: CC BY-SA 4.0
---

# DKMS

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**DKMS** (Dynamic Kernel Module System) is a distribution agnostic framework for managing out-of-tree [kernel modules](https://wiki.gentoo.org/wiki/Kernel_Modules).

DKMS supports:

- Dynamically building and installing missing kernel modules at boot, via an [init system](https://wiki.gentoo.org/wiki/Init_system) service.
- Building and installing out-of-tree kernel modules during installation of a new kernel, via an [Installkernel](https://wiki.gentoo.org/wiki/Installkernel) hook.
- Managing, building and installing out-of-tree kernel modules, via a command line utility.
- Automatically [signing](https://wiki.gentoo.org/wiki/Signed_kernel_module_support) and/or compressing built kernel modules, as well as generating signing keys.

## Installation

To use DKMS, install the [sys-kernel/dkms](https://packages.gentoo.org/packages/sys-kernel/dkms) package:

`root #``emerge --ask sys-kernel/dkms`

### USE flags for
            [sys-kernel/dkms](https://packages.gentoo.org/packages/sys-kernel/dkms)
            
            Dynamic Kernel Module Support

| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 

## Configuration

DKMS is configured via /etc/dkms/framework.conf. Every option is extensively documented in the configuration file, in the subsections below we highlight the options for kernel module signing and compression.

### Kernel module signing

DKMS will automatically sign built kernel modules if the target kernel supports this. By default it will use the key and certificate pair at /var/lib/dkms/mok.key and /var/lib/dkms/mok.pub respectively. Another key may be used by specifying the `mok_signing_key` and `mok_certificate` variables in /etc/dkms/framework.conf as shown below:

**`/etc/dkms/framework.conf`**

**configuring kernel module signing in DKMS**

```
# Location of the key and certificate files used for Secure boot. $kernelver
# can be used in path to represent the target kernel version.
#
# NOTE: If any of the files specified by `mok_signing_key` and
# `mok_certificate` are non-existant, dkms will re-create both files.
#
# mok_signing_key can also be a "pkcs11:..." string for PKCS#11 engine, as
# long as the sign_file program supports it.
# (default: /var/lib/dkms):
mok_signing_key=/root/kernel_key.pem
mok_certificate=/root/kernel_key.pem
```
### Kernel module compression

DKMS will automatically compress kernel modules if the module install tree for the target kernel (/lib/modules/KV\_FULL) contains compressed kernel modules. The used compression options can be customized as shown below.

**`/etc/dkms/framework.conf`**

**configuring kernel module compression in DKMS**

```
# Compression settings DKMS uses when compressing modules. The defaults are
# used, for reasonable compression times. One might instead wish to use
# maximum compression, at the expense of speed when compressing.
compress_gzip_opts="-6"
compress_xz_opts="--check=crc32 --lzma2=dict=1MiB -6"
compress_zstd_opts="-q -T0 -3"
```
## Integration with Portage

Integration with [portage](https://wiki.gentoo.org/wiki/Portage) is provided by the global [dkms](https://packages.gentoo.org/useflags/dkms) [USE flag](https://wiki.gentoo.org/wiki/USE_flag). When this flag is enabled, the affected packages will install the sources required to build the kernel module(s) to a subdirectory of /usr/src/, and then register the contained kernel modules with DKMS. Portage will then instruct DKMS to build and install the kernel modules for the [currently selected kernel version](https://wiki.gentoo.org/wiki/Kernel/Upgrade#Default:_Setting_the_link_with_eselect). To enable the [dkms](https://packages.gentoo.org/useflags/dkms) [USE flag](https://wiki.gentoo.org/wiki/USE_flag) for all packages containing kernel modules, set the flag in [make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf):

**`/etc/portage/make.conf`**

**Enabling DKMS**

```
USE="dkms"
```
To trigger a rebuild and reinstallation of a kernel module provided by a dkms-enabled package for the currently running kernel, one can use the `--config` argument for `emerge`:

`root #``emerge --config example/package`
## Usage

DKMS integration with [Installkernel](https://wiki.gentoo.org/wiki/Installkernel) is setup automatically. To additionally also build and install missing kernel modules at boot, enable the DKMS init service:

`root #``systemctl enable --now dkms`
To automatically build and install all DKMS registered kernel modules for the currently running kernel:

`root #``dkms autoinstall`
To `autoinstall` for a different kernel version instead, add the `-k` argument followed by the target kernel version:

`root #``dkms autoinstall -k 6.x.y-gentoo-dist`
