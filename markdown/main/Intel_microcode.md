<!-- source: https://wiki.gentoo.org/wiki/Intel_microcode | group: Gentoo Wiki (Main) | wiki-title: Intel microcode -->
---
title: Intel microcode
url: https://wiki.gentoo.org/wiki/Intel_microcode
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-28"
fingerprint: "621ada7d32f61773"
license: CC BY-SA 4.0
---

# Intel microcode

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes the process of updating the [microcode](https://wiki.gentoo.org/wiki/Microcode) on Intel processors.

The following kernel support is required to be built-in:

**Enable CONFIG\_BLK\_DEV\_INITRD, CONFIG\_MICROCODE, and CONFIG\_MICROCODE\_INTEL**

### USE flags


| [+initramfs](https://packages.gentoo.org/useflags/+initramfs) | Install a small initramfs for use with CONFIG\_MICROCODE\_EARLY | 
| [+split-ucode](https://packages.gentoo.org/useflags/+split-ucode) | Install the split binary ucode files (used by the kernel directly) | 
| [dist-kernel](https://packages.gentoo.org/useflags/dist-kernel) | Delegate microcode initramfs generation to sys-kernel/installkernel | 
| [hostonly](https://packages.gentoo.org/useflags/hostonly) | Only install ucode(s) supported by currently available (=online) processor(s) | 
| [vanilla](https://packages.gentoo.org/useflags/vanilla) | Only install microcode updates from Intel's official microcode tarball | 

By default, the [sys-firmware/intel-microcode](https://packages.gentoo.org/packages/sys-firmware/intel-microcode) installs all available firmware files, which can consume a significant amount of space. To install only the firmware files required for the current system, [hostonly](https://packages.gentoo.org/useflags/hostonly) [flag can be enabled. During installation, the emerge will automatically detect the processor family and install the only compatible firmware files.](https://wiki.gentoo.org/wiki/USE_flag)

**`/etc/portage/package.use/intel-microcode`**

**Enable hostonly USE flag**

```
sys-firmware/intel-microcode hostonly
```
Install the microcode firmware package and the manipulation tool:

`root #``emerge --ask --noreplace sys-firmware/intel-microcode`
To manually generate the microcode [cpio](https://wiki.gentoo.org/wiki/Cpio) archive use iucode\_tool:

`root #``iucode_tool -S --write-earlyfw=/boot/early_ucode.cpio /lib/firmware/intel-ucode/*`
iucode\_tool: system has processor(s) with signature 0x000306c3
iucode\_tool: Writing selected microcodes to: /boot/early\_ucode.cpio

If genkernel is used to generate the initrd then add the --microcode-initramfs option to have it prepend an early cpio with the Intel and AMD microcode inside. No modifications to the bootloader config are necessary below.

Multiple initrd files are separated by commas in the `INITRD` line. Set early\_ucode.cpio to load first:

**`/boot/syslinux.cfg`**

Starting with version 2.02-r1, GRUB supports loading an early microcode. If the microcode file is named after one of the following: intel-uc.img, intel-ucode.img, amd-uc.img, amd-ucode.img, early\_ucode.cpio, or microcode.cpio, it will be automatically detected when running grub-mkconfig. To declare a microcode file named differently, e.g. ucode.cpio, add this line to /etc/default/grub:

**`/etc/default/grub`**

```
GRUB_EARLY_INITRD_LINUX_CUSTOM="ucode.cpio"
```
Regenerate the grub.cfg with:

`root #``grub-mkconfig -o /boot/grub/grub.cfg`
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-4.6.3-gentoo
Found initrd image: /boot/early\_ucode.cpio /initramfs-genkernel-x86\_64-4.6.3-gentoo
done

Finally, reboot.

**`/efi/EFI/refind/refind.conf`**

This example system has the EFI partition /dev/sda1 mounted to /efi. The Linux kernel and initrd files have been placed in /boot on the Linux rootfs.

If using the **initrd** keyword instead of the **options** keyword for specifying initrd, then try specifying multiple initrd files via separate **initrd** keywords, or migrate the declarations into **options**. Specifying multiple initrd via one **initrd** keyword fails on rEFInd. As always, make sure boot/early\_code.cpio is the first initrd specified.

Review and edit the kernel `cmdline` options from the rEFInd bootloader. With the Gentoo OS entry highlighted, press `F2` to access the menu entries, and press `F2` again over the desired entry to review and edit. This is very useful for quick experimenting without need to edit refind.conf.

**`/boot/refind_linux.conf`**

Finalize the configuration in /boot/refind\_linux.conf. Keep in mind that rEFInd searches initramfs relatively partition, so if the
/boot partition is separate, search it with "initrd=intel-ucode.img initrd=initramfs-%v.img" (because boot partition don't have /boot folder). Use backslashes \, or the kernel may not find the files.
See refind.conf for keyword descriptions and [The rEFInd Homepage](https://www.rodsbooks.com/refind/) for more on how to use rEFInd.

Add the microcode as an argument to an **initrd** line. If an initrd line already exists, ensure the microcode entry occurs first. The path to the microcode should be absolute to the root of the ESP.

**`/boot/EFI/loader/entries/example`**

For more information, see [The Boot Loader Specification](https://www.freedesktop.org/wiki/Specifications/BootLoaderSpec/).

Add a line to the xen.cfg with the `ucode` option. The path to the microcode is relative to the xen.efi binary. Ensure to write the microcode into the correct location (default is /boot/EFI/Gentoo) or copy it there.

**`/boot/EFI/Gentoo/xen.cfg`**

```
[global]
default=gentoo
 
[gentoo]
kernel=vmlinuz-4.4.6-gentoo root=/dev/sda1
ramdisk=initrd-4.4.6-gentoo.img
ucode=early_ucode.cpio
```
For more information, see the [Xen EFI](https://xenbits.xen.org/docs/unstable/misc/efi.html) documentation.

The Linux kernel allows firmware files to be embedded directly into the kernel executable itself. To do this, the processor signature must be known; the iucode\_tool from [sys-firmware/intel-microcode](https://packages.gentoo.org/packages/sys-firmware/intel-microcode) utility can be used to identify processor signature.

`user $``iucode_tool -S`
iucode\_tool: system has processor(s) with signature 0x000306c3

To find the appropriate filename(s) for the listed signature(s) use:

`user $``iucode_tool -S -l /lib/firmware/intel-ucode/*`
iucode\_tool: system has processor(s) with signature 0x000306c3
\[...\]
microcode bundle 49: /lib/firmware/intel-ucode/06-3c-03
\[...\]
selected microcodes:
  049/001: sig 0x000306c3, pf\_mask 0x32, 2017-01-27, rev 0x0022, size 22528

The signature is found in microcode bundle `49`, so the filename to use is /lib/firmware/intel-ucode/06-3c-03.

Enable and configure the `CONFIG_MICROCODE`, `CONFIG_MICROCODE_INTEL`, `CONFIG_FIRMWARE_IN_KERNEL`, `CONFIG_EXTRA_FIRMWARE` and `CONFIG_EXTRA_FIRMWARE_DIR` kernel options. Last two options need to be set to the values identified by iucode\_tool. In this example for an Intel i7-4790K processor, `CONFIG_EXTRA_FIRMWARE` is set to `intel-ucode/06-3c-03` and `CONFIG_EXTRA_FIRMWARE_DIR` is set to `/lib/firmware`.

**Bundling microcode into kernel**

[Rebuild and install](https://wiki.gentoo.org/wiki/Kernel/Rebuild) the kernel as usual.

After the next reboot, the loaded microcode revision can be verified by running:

`user $``grep microcode /proc/cpuinfo`
microcode	: 0x22
microcode	: 0x22

The dmesg output should include:

`root #``dmesg | grep microcode`
\[    0.000000\] microcode: microcode updated early to revision 0x22, date = 2017-01-27
\[    1.153262\] microcode: sig=0x306c3, pf=0x2, revision=0x22
\[    1.153815\] microcode: Microcode Update Driver: v2.2.

Here is an example of a CPU with no available microcode updates (microcode already current) or the system was not configured to load them properly:

`root #``dmesg | grep microcode`
\[    1.196567\] microcode: CPU0 sig=0x6fd, pf=0x80, revision=0xa3
\[    1.196575\] microcode: CPU1 sig=0x6fd, pf=0x80, revision=0xa3
\[    1.196623\] microcode: Microcode Update Driver: v2.01 \<tigran@aivazian.fsnet.co.uk>, Peter Oruba

Here is the same CPU but with microcode updates being applied successfully:

`root #``dmesg | grep microcode`
\[    0.000000\] microcode: microcode updated early to revision 0xa4, date = 2010-10-02
\[    1.207385\] microcode: CPU0 sig=0x6fd, pf=0x80, revision=0xa4
\[    1.207393\] microcode: CPU1 sig=0x6fd, pf=0x80, revision=0xa4
\[    1.207445\] microcode: Microcode Update Driver: v2.01 \<tigran@aivazian.fsnet.co.uk>, Peter Oruba

- [Microcode](https://wiki.gentoo.org/wiki/Microcode) — describes various ways to update a CPU's microcode in Gentoo.
- [AMD microcode](https://wiki.gentoo.org/wiki/AMD_microcode) — describes updating the [microcode](https://wiki.gentoo.org/wiki/Microcode) for [AMD](https://wiki.gentoo.org/wiki/AMD) processors.
