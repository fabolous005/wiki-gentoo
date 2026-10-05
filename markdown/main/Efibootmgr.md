<!-- source: https://wiki.gentoo.org/wiki/Efibootmgr | group: Gentoo Wiki (Main) | wiki-title: Efibootmgr -->
---
title: efibootmgr
url: https://wiki.gentoo.org/wiki/Efibootmgr
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-21"
fingerprint: a69da1be01e25f64
license: CC BY-SA 4.0
---

# efibootmgr

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


efibootmgr is a tool for managing [UEFI](https://wiki.gentoo.org/wiki/UEFI) boot entries.

It is *not* a [bootloader](https://wiki.gentoo.org/wiki/Category:Bootloaders). It is a tool that interacts with the EFI firmware of the system, which itself is acting as a boot manager. Using efibootmgr boot entries can be created, reshuffled and removed.

[`CONFIG_EFIVAR_FS`](https://wiki.gentoo.org/wiki/Efivarfs) support needs to be enabled.

The [sys-boot/efibootmgr](https://packages.gentoo.org/packages/sys-boot/efibootmgr) package does not have any USE flags. All that is needed is to install it:

`root #``emerge --ask sys-boot/efibootmgr`
In order to successfully use efibootmgr the EFI variables filesystem must be accessible. This requires that the system has been booted in EFI mode (and not through the firmware's MBR mode) as otherwise the EFI variables themselves cannot be accessed. If the system is in MBR mode, reboot and do what is necessary in order to tell the system firmware to boot in EFI mode. Usually this involved either changing an option in the firmware's settings or selecting an EFI boot entry in the system's boot menu.

When the system is in EFI mode, run the following command to check for the existence of a mounted [efivarfs](https://wiki.gentoo.org/wiki/Efivarfs):

`root #``mount | grep efivars`
efivarfs on /sys/firmware/efi/efivars type efivarfs (ro,relatime)

It is mounted read-only (ro) through the sysfs init script), so it needs to be remounted read-write (rw) manually using the following command:

`root #``mount -o remount,rw -t efivarfs efivarfs /sys/firmware/efi/efivars`
If an EFI System Partition (ESP) does not exist, one needs to be created, see [EFI System Partition](https://wiki.gentoo.org/wiki/EFI_System_Partition).

When using an older version of efibootmgr the option `--verbose` or `-v` is needed:

`root #``efibootmgr`
BootCurrent: 0002
Timeout: 3 seconds
BootOrder: 0003,0003,0002,0000,0004
Boot0000\* CD/DVD Drive  BIOS(3,0,00)
Boot0001\* Hard Drive    BIOS(2,0,00)
Boot0002\* Gentoo        HD(1,800,61800,6d98f360-cb3e-4727-8fed-5ce0c040365d)File(\EFI\boot\bootx64.efi)
Boot0003\* Hard Drive    BIOS(2,0,00)P0: ST1500DM003-9YN16G

To create an EFI boot entry, a couple of arguments are passed to efibootmgr:

- `--create` or `-c` to create a new entry;
- `--part` or `-p` followed by the partition number on which the [EFI System Partition](https://wiki.gentoo.org/wiki/EFI_System_Partition) is hosted;
- `--disk` or `-d` followed by the disk on which the EFI System Partition is hosted;
- `--label` or `-L` followed by the label to use as the boot entry;
- `--loader` or `-l` followed by the path of the EFI image to boot

Below are some examples of how a UEFI entry can be created. If this is the folder structure:

`root #``tree /efi/ -L 3````
/efi/
└── EFI
    ├── Grub
    │   └── grubx64.efi
    └── Gentoo
        └── bzImage.efi
```
then the loader paths will be:

`root #``efibootmgr -c -L "Grub" -l '\EFI\Grub\grubx64.efi'` `root #``efibootmgr -c -L "Gentoo" -l '\EFI\Gentoo\bzImage.efi'`
For instance:

`root #``efibootmgr -c -d /dev/sda -p 2 -L "Gentoo" -l '\efi\boot\bootx64.efi'`
While [not supported](<https://wiki.gentoo.org/wiki/Dell_Latitude_E6430_(CYRTRY1)#Hardware>) by all UEFI implementations<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>, to add `--unicode` or `-u` kernel command-line parameters issue:

`root #``efibootmgr -c -d /dev/sda -p 2 -L "Gentoo" -l '\efi\boot\bootx64.efi' -u 'root=/dev/sda3 initrd=\efi\boot\initramfs.img quiet'`
For a dm-crypt module initrd example, refer to [efibootmgr](https://wiki.gentoo.org/wiki/Rootfs_encryption#efibootmgr).

Refer to [Why make your own](https://wiki.gentoo.org/wiki/Initramfs_-_make_your_own#Why_make_your_own) for initial ramdisk systems.

Optionally, additional kernels can be installed and made known to the UEFI firmware. This is especially useful when wanting to test more kernels or to dual-boot with another operating system. These will be shown in the boot selection prompt, normally after a keyboard hotkey is pressed at the right time during system initialization. The latest added entry always gets highest boot priority, so it will be default. If the hotkey combination is unknown, search for official documentation from the computer manufacturer. This information is usually not difficult to find.

Before deleting an entry, first figure out what ID the entry has.

To delete the Gentoo entry as shown above (which has *Boot0002* as the identifier), ask efibootmgr to delete the entry with id 2, passing the arguments `--bootnum` or `-b` with the identifier, and `--delete-bootnum` or `-B` to delete the entry:

`root #``efibootmgr -b 2 -B`
EFI bootloaders on removable media are not configured as boot entries, neither are bootloaders using the removable media boot path on the [EFI System Partition](https://wiki.gentoo.org/wiki/EFI_System_Partition) or ESP, i.e. for both efibootmgr is not required. See [removable media boot path](https://wiki.gentoo.org/wiki/EFI_System_Partition#Removable_media) for details.

`root #``emerge --ask --depclean --verbose sys-boot/efibootmgr`
- [Alternative 2: EFI Stub](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Bootloader#Alternative_2:_efibootmgr) in the Gentoo Handbook
- [EFI stub](https://wiki.gentoo.org/wiki/EFI_stub)
- [Efivarfs](https://wiki.gentoo.org/wiki/Efivarfs) — a filesystem in the Linux [kernel](https://wiki.gentoo.org/wiki/Kernel) that enables users to create, delete, and modify [(U)EFI](https://wiki.gentoo.org/wiki/UEFI) variables
- [Initramfs - make your own](https://wiki.gentoo.org/wiki/Initramfs_-_make_your_own) — build an initramfs which does not contain kernel modules.
- [REFInd](https://wiki.gentoo.org/wiki/REFInd) — a boot manager for UEFI platforms.
