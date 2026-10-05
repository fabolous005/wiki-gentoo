<!-- source: https://wiki.gentoo.org/wiki/UEFI_Dual_boot_with_Windows_7/8 | group: Gentoo Wiki (Main) | wiki-title: UEFI Dual boot with Windows 7/8 -->
---
title: UEFI Dual boot with Windows 7/8
url: https://wiki.gentoo.org/wiki/UEFI_Dual_boot_with_Windows_7/8
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-11-22"
fingerprint: "949c397b0fa4adcc"
license: CC BY-SA 4.0
---

# UEFI Dual boot with Windows 7/8

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Article status**

This article describes how to dual boot Microsoft Windows on a UEFI computer.

## Prerequisites

This guide assumes you have a computer with Windows 7 or later installed on a GPT-partitioned drive and booting in UEFI mode.

Microsoft dictates the requirements that any computer bearing the windows logo has to follow. That means that any AMD64 computer with windows 8 or later preinstalled, has to be capable of disabling secure boot, and mange the secure boot keys from the UEFI System settings.

On the other hand, ARM devices with windows 8 or later preinstalled are forbidden from allowing the user to disable secure boot.

If the drive is empty, try [installing Windows](http://msdn.microsoft.com/en-us/library/windows/hardware/gg463140.aspx) before installing Linux.

### Disable "Fast Startup"

It is strongly recommended to disable "Fast Startup", aka "hybrid shutdown" or "hybrid boot" in Windows. Without it, Windows' filesystems are *not* unmounted even when you're using Linux, so editing Windows files can result in data loss. Even if you do not intend to share filesystems, the [EFI System Partition](https://wiki.gentoo.org/wiki/EFI_System_Partition) is likely to be damaged on an EFI system.

To disable Fast Startup, see [here for Windows 8](http://www.eightforums.com/tutorials/6320-fast-startup-turn-off-windows-8-a.html) and [here for Windows 10](http://www.tenforums.com/tutorials/4189-fast-startup-turn-off-windows-10-a.html).

### Shrink the Windows partition

Skip this if there's already room for Gentoo partitions.

#### Windows 7

1. Press the Windows-r to open the "Run" dialog, and enter diskmgmt.msc OR go to Control Panel/Administrative Tools and open Computer Management. Select the "Disk Management" option under "Storage" from the tree menu on the left.
2. Right click on the target partition and choose “shrink volume”
3. Provide the size of the shrink

#### Windows 8 or Windows 10:

1. Press `Windows`-`x` (windows key and x key simultaneously).
2. Choose “Disk Management”
3. Right click on the target partition and choose “shrink volume”
4. Provide the size of the shrink

### BitLocker

If using BitLocker to encrypt your windows volumes, decide if this is going to be required moving forward. It is possible to keep using BitLocker, have the drives auto-unlock, and access the contents from Gentoo, but additional steps should be taken.

To remove the hassle of BitLocker, go to control panel > system and security > BitLocker drive encryption. Search for the drive, and click on "Turn off BitLocker". Your drives will begin decrypting, which will take a while.

A bit of background on how BitLocker works. Basically, it uses the computer's TPM to store the decryption keys of the C volume, which in turn contains the keys for the rest of the volumes, if present. BitLocker will require secure boot in order to auto-unlock.

The TPM will only release the decryption keys to the Operating System, if the state of the system is the same as when the encryption material was "sealed" inside the TPM. Any changes you make to the computer, such as disabling secure boot, changing some UEFI firmware configurations, or chain loading the windows boot-loader from grub, will change said state and the TPM will refuse to release the key.

[Suspend bitlocker](https://docs.microsoft.com/en-us/troubleshoot/windows-client/windows-security/suspend-bitlocker-protection-non-microsoft-updates), so BitLocker can keep working even if any significant change is made to the system. While the protection is disabled, the encryption keys aren't protected, so any hardware or settings changes won't prevent BitLocker from accessing the decryption keys. When you resume the protection, the current system state is evaluated, and the decryption material is re-sealed. Any changes made after this point can prevent BitLocker from auto unlocking the boot drive.

**Bottomline**: Archive dual booting while keeping BitLocker enabled, by suspending BitLocker during the Gentoo installation, and making sure to install the Gentoo bootloader as a new boot entry, without changing the default. When the installation is complete, enable secure boot, and boot into windows 2 times.

- **Windows**: Enable secure boot, and choose the Windows bootloader on your bios boot menu or make it the default.
- **Gentoo**: DISABLE secure boot, and choose the Gentoo bootloader on your bios boot menu, or make it the default.


To avoid the hassle of enabling and disabling secure boot, and / or using your bios boot menu, read the Secure Boot section, which will serve as guidance on how to enable secure boot for Gentoo, improving Gentoo's security and allowing its bootloader to chainload the windows bootloader while keeping BitLocker auto-unlock working.

## Optional: Download and install rEFInd in Windows

Extract refind-bin-{version}.zip to a handy location. Suggest C:\.

For simpler booting in some configurations, ensure that you've installed EFI filesystem drivers for the partition that holds the Linux kernel.

## Obtain UEFI bootable Linux media

The latest gentoo LiveCD/USB/DVD is capable of UEFI boot. It is not compatible with secure boot, so there will be a need to disable it prior to trying to boot it.

Alternatively, the UBUNTU liveCD is signed by microsoft, so it should boot with secure boot enabled.

## Install Gentoo

### Quick and easy

With an [EFI System Partition](https://wiki.gentoo.org/wiki/EFI_System_Partition) created by an installation of Windows or manually created, create the root (/) partition (and optionally other partitions) according to [the Handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Disks#Creating_the_partitions) and proceed with the installation until [Architecture specific kernel configuration](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Kernel#Architecture_specific_kernel_configuration).  Complete the kernel configuration according to [EFI stub](https://wiki.gentoo.org/wiki/EFI_stub) and proceed to [Configuring the modules](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Kernel#Configuring_the_modules).

Reboot and enjoy a UEFI dual boot system!!

### Alternative procedure

Exceptions/additions to the [Gentoo Handbook](https://wiki.gentoo.org/wiki/Gentoo_Handbook):

#### Create partitions

Use gdisk *instead of fdisk or parted for GPT disks. It's provided by [sys-apps/gptfdisk](https://packages.gentoo.org/packages/sys-apps/gptfdisk).*

**START OF GDISK EXAMPLE:**

```
 gdisk /dev/sda
 GPT fdisk (gdisk) version 0.8.6
 Partition table scan:
 MBR: protective
 BSD: not present
 APM: not present
 GPT: present
 Found valid GPT with protective MBR; using GPT.
 Command (? for help): p
 Disk /dev/sda: 500118192 sectors, 238.5 GiB
 Logical sector size: 512 bytes
 Disk identifier (GUID): C72786B7-C1FB-4A60-8F5F-216FA9097A98
 Partition table holds up to 128 entries
 First usable sector is 34, last usable sector is 500118158
 Partitions will be aligned on 2048-sector boundaries
 Total free space is 123357805 sectors (58.8 GiB)
 Number  Start (sector)    End (sector)  Size       Code  Name
 1            2048          616447   300.0 MiB   2700  Basic data partition
 2          616448          821247   100.0 MiB   EF00  EFI system partition
 3          821248         1083391   128.0 MiB   0C01  Microsoft reserved part
 4         1083392       376762367   179.1 GiB   0700  Basic data partition
 Command (? for help): n
 Partition number (5-128, default 5):
 First sector (34-500118158, default = 376762368) or {+-}size{KMGTP}:
 Last sector (376762368-500118158, default = 500118158) or {+-}size{KMGTP}: +100M
 Current type is 'Linux filesystem'
 Hex code or GUID (L to show codes, Enter = 8300):
 Changed type of partition to 'Linux filesystem'
 Entering GPTPart::SetName(const UnicodeString...)
 Command (? for help): n
 Partition number (6-128, default 6):
 First sector (34-500118158, default = 376967168) or {+-}size{KMGTP}:
 Last sector (376967168-500118158, default = 500118158) or {+-}size{KMGTP}: +1G
 Current type is 'Linux filesystem'
 Hex code or GUID (L to show codes, Enter = 8300): 8200
 Changed type of partition to 'Linux swap'
 Entering GPTPart::SetName(const UnicodeString...)
 Command (? for help): n
 Partition number (7-128, default 7):
 First sector (34-500118158, default = 379064320) or {+-}size{KMGTP}:
 Last sector (379064320-500118158, default = 500118158) or {+-}size{KMGTP}:
 Current type is 'Linux filesystem'
 Hex code or GUID (L to show codes, Enter = 8300):
 Changed type of partition to 'Linux filesystem'
 Entering GPTPart::SetName(const UnicodeString...)
 Command (? for help): p
 Disk /dev/sda: 500118192 sectors, 238.5 GiB
 Logical sector size: 512 bytes
 Disk identifier (GUID): C72786B7-C1FB-4A60-8F5F-216FA9097A98
 Partition table holds up to 128 entries
 First usable sector is 34, last usable sector is 500118158
 Partitions will be aligned on 2048-sector boundaries
 Total free space is 2014 sectors (1007.0 KiB)
 Number  Start (sector)    End (sector)  Size       Code  Name
 1            2048          616447   300.0 MiB   2700  Basic data partition
 2          616448          821247   100.0 MiB   EF00  EFI System Partition
 3          821248         1083391   128.0 MiB   0C01  Microsoft reserved part
 4         1083392       376762367   179.1 GiB   0700  Basic data partition
 5       376762368       376967167   100.0 MiB   8300  Linux filesystem
 6       376967168       379064319   1024.0 MiB  8200  Linux swap
 7       379064320       500118158   57.7 GiB    8300  Linux filesystem
 Command (? for help): w
 Final checks complete. About to write GPT data. THIS WILL OVERWRITE EXISTING
 PARTITIONS!!
 Do you want to proceed? (Y/N): y
 OK; writing new GUID partition table (GPT) to /dev/sda.
 The operation has completed successfully.
```
Make file systems:

`root #````
mkfs.ext2 /dev/sda5
```
`root #````
mkfs.ext4 /dev/sda7
```
`root #````
mkswap /dev/sda6
```
`root #````
swapon /dev/sda6
```
As long as the EFI stub kernel is in an ext2, ext3, ext4, Btrfs, or FAT32 file system rEFInd will find it and add it to the menu.

Run blkid:

`user $``blkid`
/dev/sda7: UUID="1f43e373-f923-4ec2-a62e-6a0d98927583" TYPE="swap" PARTLABEL="Linux filesystem" PARTUUID="92d3d504-9e7e-4c3d-9e56-15e3bd43511b"

The / partition PARTUUID will be used in the kernel configuration in the form root=PARTUUID=92d3d504-9e7e-4c3d-9e56-15e3bd43511b .

Keep it handy.

Continue with the handbook through "7. Configuring the Kernel".

#### Kernel configuration

Use either "7.b. Default: Manual Configuration" or "7.c. Alternative: Using genkernel" but start genkernel with genkernel --menuconfig all verses just genkernel all. In addition to the items specified in the handbook or set by genkernel, enable the following:

In menuconfig:

General setup
CONFIG\_BLK\_DEV\_INITRD=y
CONFIG\_INITRAMFS\_SOURCE=""
CONFIG\_RD\_GZIP=y
CONFIG\_RD\_BZIP2=y
CONFIG\_RD\_LZMA=y
CONFIG\_RD\_XZ=y
CONFIG\_RD\_LZO=y
CONFIG\_RD\_LZ4=y

If an initramfs is to be used, add an initrd="/boot/\<your initramfs name>" to the kernel configuration item "CONFIG\_CMDLINE" as in the following example:

If systemd is to be used, add "init=/usr/lib/systemd/systemd" to the kernel configuration item "CONFIG\_CMDLINE" as in the following example:

If systemd and an initramfs are to be used; example:

Use make && make modules\_install && make install to build a manual kernel. Finish the Handbook. No need to emerge or install grub or lilo or grub2. rEFInd will act as the boot manager.

## Alternative booting

Consider the boot options suggested by [refind Linux page](https://www.rodsbooks.com/refind/linux.html). If the system is using refind, the config setup would be the best option. In a few words, one should provide refind\_linux.conf in the /boot partition next to the kernel binary rather than hardcoding kernel launch arguments. It's also possible to select the boot options described in refind\_linux.conf at the refind launch screen (press F2 to invoke additional boot options menu). Find additional info with examples of refind\_linux.conf [at refind linux page](https://www.rodsbooks.com/refind/linux.html).

## Dynamic disk

"Dynamic disk", which in Windows can be thought of as analogous to [LVM](https://wiki.gentoo.org/wiki/LVM) in Linux, is not recommended for dual boot. (See [this](https://wiki.archlinux.org/index.php/Dynamic_disk) ArchWiki article for more.)

In [bug #700960](https://bugs.gentoo.org/show_bug.cgi?id=700960), an ebuild of "libldm", which provides read/write access to dynamic disks, is submitted.

## See also

- [EFI stub](https://wiki.gentoo.org/wiki/EFI_stub)
- [EFI System Partition](https://wiki.gentoo.org/wiki/EFI_System_Partition) — a [FAT](https://wiki.gentoo.org/wiki/FAT) formatted partition containing the primary [EFI](https://wiki.gentoo.org/wiki/UEFI) [boot loader(s)](https://wiki.gentoo.org/wiki/Bootloader) for installed operating systems.
- [Efibootmgr](https://wiki.gentoo.org/wiki/Efibootmgr) — a tool for managing [UEFI](https://wiki.gentoo.org/wiki/UEFI) boot entries.
- [REFInd](https://wiki.gentoo.org/wiki/REFInd) — a boot manager for UEFI platforms.
- [NTFS](https://wiki.gentoo.org/wiki/NTFS) — a proprietary disk [filesystem](https://wiki.gentoo.org/wiki/Filesystem) by Microsoft for Windows (NT-based) and WindowsNT-based operating systems.

## External resources

- [How to repair Windows' EFI bootloader ...](https://www.dell.com/support/kbdoc/en-us/000124331/how-to-repair-the-efi-bootloader-on-a-gpt-hdd-for-windows-7-8-8-1-and-10-on-your-dell-pc) ... if it accidentally got deleted
