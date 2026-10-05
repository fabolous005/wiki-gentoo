<!-- source: https://wiki.gentoo.org/wiki/Kexec | group: Gentoo Wiki (Main) | wiki-title: Kexec -->
---
title: Kexec
url: https://wiki.gentoo.org/wiki/Kexec
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-24"
fingerprint: "5482195f10a4bb27"
license: CC BY-SA 4.0
---

# Kexec

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

[Kexec](https://en.wikipedia.org/wiki/Kexec) (**k**ernel **exec**ute) is a system call that enables the kernel to load and boot into another kernel from the currently running kernel. This is useful for kernel developers or other people who need to reboot very quickly without waiting for the whole BIOS boot process to finish.

## Installation

### Kernel

**Configuring the kernel for Kexec**

```
    General setup --->
         Kexec and crash features  --->
              [*] Enable kexec system call 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_KEXEC</code> to find this item.
              [*] Enable kexec file based system call [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_KEXEC_FILE</code> to find this item.
### Emerge

After installing old (which are loaded already) and new (which will be loaded) kernel, next install [sys-apps/kexec-tools](https://packages.gentoo.org/packages/sys-apps/kexec-tools) userspace tools:

`root #``emerge --ask --verbose sys-apps/kexec-tools`
kexec can now be called either with a manual (kexec -l) or automatic (with kexec openrc/systemd service) methods.

## Usage

### Manual kernel reloading

Kexec utility documentation:

`user $``man kexec`
## Quick steps

Stage 1: load the new kernel image:

`root #``kexec -l path_to_kernel_image --initrd=path_to_initrd_image --append=command-line-options`
Instead of manually providing the parameters, there is a handy utility which can be configured through `/etc/kexec.conf` and loads the last installed kernel as a default:

`root #``kexec-auto-load`
Stage 2: reboot to new kernel:

`root #``reboot -k`
## Automatically kernel reloading with openrc

Edit openrc config with the following options:

**`/etc/conf.d/kexec`**

```
# Load kexec kernel image into memory during shutdown instead of bootup
# (default: yes)
LOAD_DURING_SHUTDOWN="yes"
# Additional arguments passed to kexec (8)
#KEXEC_OPT_ARGS=""
# Kernel image partition. Mounted automatically if not.
# (default: /boot)
BOOTPART="/boot"
# Root partition (should be autodetected)
#ROOTPART="/dev/hda3"
# Kernel image pathname, relative from BOOTPART.
# If it's one of 
# {kernel-genkernel,bzImage,vmlinuz,kernel}-<currently running kernel version>,
# or bzImage, vmlinuz (without suffix),
# then it's automaticaly detected.
# Setting it to "-" will disable kexec.
#KNAME="vmlinuz-3.9.0"
# Initrd
# Same automatic detection restriction as for KNAME apply.
# initramfs-genkernel-<currently running kernel version>,
# initrd{,.img}-<currently running kernel version>{,.img}
# will be detected.
#INITRD="/boot/fbsplash-emergence-1024x768"
# Kernel parameters (should be autodetected)
#KPARAM="initrd=intel-uc.img rootfstype=f2fs init=/sbin/openrc-init"
# Do not try to mount /boot
DONT_MOUNT_BOOT="yes"
```
Then start the service:

`root #``rc-service kexec start`
And reboot to new kernel:

`root #``reboot -k`
