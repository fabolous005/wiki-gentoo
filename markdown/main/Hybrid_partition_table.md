<!-- source: https://wiki.gentoo.org/wiki/Hybrid_partition_table | group: Gentoo Wiki (Main) | wiki-title: Hybrid partition table -->
---
title: Hybrid partition table
url: https://wiki.gentoo.org/wiki/Hybrid_partition_table
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-09-25"
fingerprint: "9d65c01b8fececdf"
license: CC BY-SA 4.0
---

# Hybrid partition table

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Most x86 and x86\_64 systems only use either the [MBR](https://en.wikipedia.org/wiki/Master_boot_record) (MS-DOS based) partition table layout, or the [GPT](https://en.wikipedia.org/wiki/GUID_Partition_Table) (GUID Partition Table) layout. However, in certain situations it might be necessary for a GPT partition layout to have an "MBR view" overlayed. This is called **Hybrid MBR/GPT**.

## Using hybrid partition table

Using a hybrid partition table might be necessary when dual-booting operating systems that are blind to a GPT-partitioned disk. In these situations, the partition definition requires a tricky workaround.

### Creating with gdisk

Creating a hybrid MBR is easy using gdisk: at the main menu press the `r` key (for recovery and transformation), then press `h` key (CHS recompute), then enter the series of partitions (less or equal to 3) in the order they should be be listed in the MBR. Finally enter the hexadecimal partition type without the `0x` prefix or accept the defaults by repeatedly pressing the `y` key.

Enable only a single boot flag as found the following example:

`root #``gdisk /dev/sdg`
GPT fdisk (gdisk) version 0.8.1
 
Partition table scan:
  MBR: MBR only
  BSD: not present
  APM: not present
  GPT: present
 
Found valid MBR and GPT. Which do you want to use?
 1 - MBR
 2 - GPT
 3 - Create blank GPT
 
Your answer: 2

`root #``gdisk /dev/sdg`
...
Using GPT and creating fresh protective MBR.
 
Command (? for help): r
Recovery/transformation command (? for help): p
Disk /dev/sdg: 625142448 sectors, 298.1 GiB
Logical sector size: 512 bytes
Disk identifier (GUID): 744E8BF3-39A4-4908-8646-AC2E5661C8CF
Partition table holds up to 128 entries
First usable sector is 34, last usable sector is 625142414
Partitions will be aligned on 2048-sector boundaries
Total free space is 2014 sectors (1007.0 KiB)
 
Number  Start (sector)    End (sector)  Size       Code  Name
   1            2048       566233087   270.0 GiB   8300  Linux filesystem
   2       566233088       625142414   28.1 GiB    0700  Microsoft basic data
Recovery/transformation command (? for help): h
 
WARNING! Hybrid MBRs are flaky and dangerous! If you decide not to use one,
just hit the Enter key at the below prompt and your MBR partition table will
be untouched.
 
Type from one to three GPT partition numbers, separated by spaces, to be
added to the hybrid MBR, in sequence: 2 1
Place EFI GPT (0xEE) partition first in MBR (good for GRUB)? (Y/N): N
 
 
Creating entry for GPT partition #2 (MBR partition #1)
Enter an MBR hex code (default 07): 
Set the bootable flag? (Y/N): Y
 
Creating entry for GPT partition #1 (MBR partition #2)
Enter an MBR hex code (default 83): 
...
Unused partition space(s) found. Use one to protect more partitions? (Y/N): N
 
Recovery/transformation command (? for help): o
 
Disk size is 625142448 sectors (298.1 GiB)
MBR disk identifier: 0x00000000
MBR partitions:
 
Number  Boot  Start Sector   End Sector   Status      Code
   1      \*      566233088    625142414   primary     0x07
   2                  2048    566233087   primary     0x83
   4                     1         2047   primary     0xEE
 
 
Recovery/transformation command (? for help): w

## Supporting boot loaders

### GRUB

GRUB supports booting from GPT and MBR partition tables via the `part_gpt` and `part_msdos` modules. However, a successful setup and GRUB's ability to boot correctly depends both on loaded partition modules and on correctly installing GRUB. A determining factor in how GRUB is installed (BIOS-MBR vs BIOS-GPT) is the presence of a EE00 (GPT protective) partition as the first partition in the MBR.

With a hybrid MBR, GRUB will install *if* the first partition in the MBR table is an `EE00` partition. An `EF02` partition should be present anywhere in the disk and not necessarily in the MBR list. Leave enough open sectors before the first partition!

GRUB requires an `EE00` partition type at the first listing in the MBR partition list entry, however it may pose a problem when booting other operating systems on the same disk (especially Windows). A workaround is available in order to recognize a GPT partitioned disk with (or without) hybrid MBR. Removing any protective `EE00` partition with the fdisk command.

Use other disk tools such as parted ([sys-block/parted](https://packages.gentoo.org/packages/sys-block/parted)) or gparted ([sys-block/gparted](https://packages.gentoo.org/packages/sys-block/gparted)) to resize after that. Otherwise use gdisk to add protective `EE00` partitions to protect at least GPT main and backup the partition before using any non-GPT aware tools if necessary.

Another issue should be addressed when attempting to use a GPT Hybrid MBR configuration: Windows MSDOS based bootloaders may not find suitable partitions to boot from if there is a `EE00` partition protecting the GPT main partition table (sectors 1-2047). If this happens remove the protective `EE00` partition before getting a valid MBR partition list in \[MS\]DOS tools (to format a partition for example, the tools recognized a GPT partitioned disk at least). And *then* use gdisk to recompute the CHS value for hybrid MBR (in the *recovery and transformation* menu, or in the *expert* section of gdisk's main menu).

## See also

- [GRUB](https://wiki.gentoo.org/wiki/GRUB) — a multiboot secondary [bootloader](https://wiki.gentoo.org/wiki/Bootloader) capable of loading kernels from a variety of [filesystems](https://wiki.gentoo.org/wiki/Filesystem) on most system architectures.
