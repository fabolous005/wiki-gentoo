<!-- source: https://wiki.gentoo.org/wiki/GRUB/Chainloading | group: Gentoo Wiki (Main) | wiki-title: GRUB/Chainloading -->
---
title: GRUB/Chainloading
url: https://wiki.gentoo.org/wiki/GRUB/Chainloading
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-01-28"
fingerprint: "1e86e91baf6ebd95"
license: CC BY-SA 4.0
---

# GRUB/Chainloading

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

GRUB2 was designed with an improved chainload mode when compared to GRUB Legacy.

## Another grub.cfg

It is possible to load a grub configuration file from another filesystem, using the UUID of that filesystem as it is visible from /dev/disk/by-uuid/.

**`grub.cfg`**

**Chainloading another GRUB configuration file**

## ISO images

The new ISO (or loop) chainload mechanism makes chainloading a breeze. It is possible to chainload ISO images (LiveCD/DVDs) with GRUB Legacy, however there exists no way to pass kernel cmdline arguments before boot. In any case, the ISO images in question should be built keeping kernel cmdline arguments in mind.

Without kernel cmdline options, booting ISO images with GRUB2 will fail in the physical media check/test stage of the ISOs boot process. Gentoo liveCD is handy because it has a minimal shell which lets the user mount the squashed image to the correct location and then press the `Esc` key to continue the boot process. This makes it possible to have a handy way to install an operating system with everything in RAM, especially for "light weight" LiveCDs. No more need to listen to a whining CD/DVD drive on each new command!

To chainload an ISO with custom or default kernel command line arguments, an entry similar to the following can be added to GRUB2's grub.cfg file:

**`/boot/grub/grub.cfg`**

**Example entry for chainloading an ISO file**

For a permanent and automatic entry to GRUB2's grub.cfg file, a custom script could be added to the /etc/grub.d script location:

**`/etc/grub.d/40_custom`**

**Custom script to load a CD**

Do not forget to make the script executable:

`root #``chmod +x /etc/grub.d/40_custom`
Finally regenerate GRUB2's grub.cfg file using grub2-mkconfig command.

## Another bootloader

Chainloading another bootloader to GRUB2 is fairly easy.

Something as simple as the following example is enough to boot another disk that uses a "Custom Super Bootloader":

**`/etc/grub.d/40_custom`**

**Chainloading another bootloader**

## TrueCrypt

Chainloading the TrueCrypt bootloader on a *separate* disk is relatively simple and can be done in GRUB2 like any other bootloader:

**`/etc/grub.d/40_custom`**

**Chainloading TrueCrypt bootloader on a separate disk**

Chainloading a disk with TrueCrypt in the MBR or a rescue CD image located in encrypted partitions is not possible with GRUB2 (see [bug #385619](https://bugs.gentoo.org/show_bug.cgi?id=385619)). Use either [GRUB Legacy](https://wiki.gentoo.org/wiki/GRUB_Legacy) or [GRUB4DOS](https://github.com/chenall/grub4dos) as workaround. GRUB4DOS has an interface very similar to GRUB Legacy and a menu.lst entry which can be used to chainload the TrueCrypt bootloader or to boot a rescue CD (from an encrypted partition on the same disk).

Another workaround is to boot from TrueCrypt as the main boot loader and then hit the `Esc` to chainload the following partition (if one exists) or the following disk. Then have GRUB2 installed on the partition instead of in the MBR itself so that GRUB2 is chainloaded.

## Windows (MSDOS based boot loaders)

When Windows (or another MS DOS based boot loader) is installed on another disk, then regular chainloading in the grub.cfg may be sufficient to boot it. However, if Windows is on the same disk on a different partition, or if regular chainloading doesn't work, then read on.

Microsoft Windows 8 (and above versions) are no longer installed using MSDOS partitions by default, however they do maintain backwards compatibility with [BIOS](https://wiki.gentoo.org/wiki/BIOS) MBR systems. In order to specify Windows 8 (and above) to use MSDOS partitioning the Windows installation DVD needs to be booted in BIOS mode (a non-UEFI boot mode) in order for Windows to install into MSDOS partitions. Manually create a MSDOS partition layout, then manually boot the Windows DVD using a BIOS option in the boot menu list. Sometimes it is necessary in the BIOS firmware configuration tool to disable UEFI mode completely in order to force BIOS MBR mode.

The simplest way to dual boot Windows (or MS-DOS) is to add an MBR menu entry to GRUB2's grub.cfg file for each Windows operating system installed.

For instance, to boot Windows 7, add the following to the grub.cfg file:

**`/etc/grub.d/40_custom`**

**Windows 7 example**

A Windows XP example:

**`/etc/grub.d/40_custom`**

**Windows XP example**

Instead of using GRUB2's device syntax, the UUID of the partition containing the Windows bootloader can be used like so:

**`/etc/grub.d/40_custom`**

**UUID example**

Filesystem UUIDs can be obtained with blkid.

An entry for a GPT hybrid MBR works a bit different than the previous BIOS-MBR examples. Booting multiple versions of Windows can be achieved with remapping and/or hiding partitions with GRUB2's `parttool` option:

**`/etc/grub.d/40_custom`**

**Example for GPT hybrid MBR**

Remapping the devices to set the primary boot disk to other disks can be achieved by using the `drivemap` option like so:

**`/etc/grub.d/40_custom`**

**Remapping devices example**

#### Probing

GRUB2 is capable of automatically finding Windows partitions and assigning the root partitions. The Windows partition must first be mounted before the probe will be successful. See notes at the end of this section concerning missing C:\bootmgr and C:\Boot files and folders; it is wise to make sure these folders do exist before trying to boot Windows using GRUB2.

GRUB2's probing feature requires the [sys-boot/os-prober](https://packages.gentoo.org/packages/sys-boot/os-prober) package which is not initially pulled in when installing GRUB2.

`root #``emerge --ask --newuse sys-boot/os-prober``root #``grub2-probe --target=hints_string /mnt/windows7/bootmgr`
--hint-bios=hd1,msdos1 --hint-efi=hd1,msdos1 --hint-baremetal=ahci1,msdos1

`root #``grub2-probe --target=fs_uuid /mnt/windows7/bootmgr`
2ABF87DC395CFC02

From the output provided by the above two commands, the `search` line within GRUB2's grub2.cfg file (below) can be constructed. Remapping the drive and partition as the first hard drive and first partition will make Windows XP or Windows 8 more free of silent errors while loading.

**`/etc/grub.d/40_custom`**

**Constructing the search line**

Seeing a boot error message concerning a missing bootmgr file after attempting to boot one of the previously mentioned grub.cfg entries is the indication the C:\Boot folder is missing. This happens when using Windows 8 since the C:\Boot folder does not seem to be generated by default.

### Dual-booting Windows on UEFI with GPT

In the case the Windows bootloader was overwritten with GRUB2 or if bootmgr doesn't do the trick, a UEFI dual boot could be achieved using the following menu entry:

**`/etc/grub.d/40_custom`**

**UEFI dual-boot**

### See also

- [UEFI Dual boot with Windows 7/8](https://wiki.gentoo.org/wiki/UEFI_Dual_boot_with_Windows_7/8) — describes how to dual boot Microsoft Windows on a UEFI computer.
