<!-- source: https://wiki.gentoo.org/wiki/Kernel_Crash_Dumps | group: Gentoo Wiki (Main) | wiki-title: Kernel Crash Dumps -->
---
title: Kernel Crash Dumps
url: https://wiki.gentoo.org/wiki/Kernel_Crash_Dumps
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-08-05"
fingerprint: "56831d3ed4a63ba5"
license: CC BY-SA 4.0
---

# Kernel Crash Dumps

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article explains how to capture the kernel crash dumps (also known as **kdumps**). Kdumps are produced by kernel panic or lockup. To be simple, just a single kernel is used both for the ordinary system and recovery. The described method is *almost* distribution independent.

This article is based on [KDump on Gentoo](http://rich0gentoo.wordpress.com/2011/11/11/kdump-on-gentoo/) by [Richard Freeman (rich0)](https://wiki.gentoo.org/wiki/User:Rich0)

## Installation

### Kernel

Activate the following kernel options:

CONFIG\_KEXEC, CONFIG\_CRASH\_DUMP, CONFIG\_RELOCATABLE

CONFIG\_DEBUG\_KERNEL, CONFIG\_DEBUG\_INFO

CONFIG\_PROC\_FS, CONFIG\_PROC\_KCORE, CONFIG\_PROC\_VMCORE

### USE flags


| [booke](https://packages.gentoo.org/useflags/booke) | Include support for Book-E memory management | 
| [lzma](https://packages.gentoo.org/useflags/lzma) | Enables support for LZMA compressed kernel images | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [xen](https://packages.gentoo.org/useflags/xen) | Enable extended xen support | 
| [zlib](https://packages.gentoo.org/useflags/zlib) | Add support for zlib compression | 
| [zstd](https://packages.gentoo.org/useflags/zstd) | Enable support for ZSTD compression | 

### Emerge

Merge:

`root #``emerge --ask sys-apps/kexec-tools`
## Configuration

### local.d script

Create /etc/local.d/kdump.start containing:

**`/etc/local.d/kdump.start`**

```
#!/bin/bash
kexec -p /[path-to-kernel] --append="root=[root-device] single irqpoll maxcpus=1 reset_devices"
```
When using an [initramfs](https://wiki.gentoo.org/wiki/Initramfs), a reference to it will need passed as a parameter. For example:

**`/etc/local.d/kdump.start`**

```
#!/bin/bash
kexec -p /boot/kernel-genkernel-x86_64-3.16.1-gentoo \
      --initrd=/boot/initramfs-genkernel-x86_64-3.16.1-gentoo \
      --append="root=/dev/mapper/lvm-slash single irqpoll maxcpus=1 reset_devices dolvm softlevel=kdump"
```
Now make this file executable:

`root #``chmod u+x /etc/local.d/kdump.start`
Note the kernel has to be readable. A typical Gentoo configuration leaves /boot unmounted, so either remove *noauto* from the [/etc/fstab](https://wiki.gentoo.org/wiki//etc/fstab) file or place a copy of the kernel in a place that is mounted during a crash.

### Bootloader

Add the `crashkernel=64M nokaslr` argument to the kernel command-line via the bootloader (most likely [GRUB](https://wiki.gentoo.org/wiki/GRUB)) for systems with up to around 12 GB of RAM.

## Usage

First, run the above script:

`root #``/etc/local.d/kdump.start`
It loads the rescue kernel image which is run after kernel crash.

Whenever a kernel panic or lockup (hard/soft if the kernel is set to detect them) occurs, kexec runs the kernel in crash mode, relocated to a reserved area of memory. The rest of RAM will be untouched. When the system boots up log in and copy /proc/vmcore to a file - this is the crash dump. Then reboot the system to get back to a normal configuration; the system might not be stable and should not continue to operate in this state.

A kernel panic can be forced on demand by executing the following command (do not forget to save all data, log-out other users, and leave the filesystems in a clean state by the invocation of the sync command before doing this):

`root #``echo c | tee /proc/sysrq-trigger`
## Troubleshooting

### Kernel is not loading

If the kernel is not loading when kexec is called, check to to see if kernel compression was set to xz (lzma) format.

If xz compression is used the [sys-apps/kexec-tools](https://packages.gentoo.org/packages/sys-apps/kexec-tools) package will need to be re-emerged with the `lzma` USE flag enabled.

### VGA not resetting

After loading a kexec crash kernel and after a kernel panic kexec does not appear to load the crash kernel. The output on the display freezes.

This might be caused by the VGA port not being reset. The solution may be to tell kexec to reset the display output on the VGA port. Something like the following could work (the important options being `--reset-vga --console-vga`):

`root #``kexec -p /boot/kernel-gentoo --initrd=/boot/initramfs-gentoo --reset-vga --console-vga --command-line="root=/dev/sda3 maxcpus=1 irqpoll"`
