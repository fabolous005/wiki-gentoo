<!-- source: https://wiki.gentoo.org/wiki/Memtest86%2B | group: Gentoo Wiki (Main) | wiki-title: Memtest86+ -->
---
title: Memtest86+
url: https://wiki.gentoo.org/wiki/Memtest86%2B
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-26"
fingerprint: "921f34488667c9c8"
license: CC BY-SA 4.0
---

# Memtest86+

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

memtest86+ is memory test software based on the commercially available (from Passmark) [memtest86](https://www.memtest86.com/) program.

There are multiple versions of memtest with similar names, among which Gentoo provides two. In this article we explain [sys-apps/memtest86+](https://packages.gentoo.org/packages/sys-apps/memtest86+).

There is also a different commercial binary [sys-apps/memtest86-bin](https://packages.gentoo.org/packages/sys-apps/memtest86-bin) (memtest86-bin v9.3+) masked behind a `LICENSE=Passmark-EULA`. See [package.license](https://wiki.gentoo.org/wiki/Package.license) for more information on unmasking a EULA license for a specific package.

These two are called from UEFI/BIOS. There is also a *userspace* program [sys-apps/memtester](https://packages.gentoo.org/packages/sys-apps/memtester).

Note: `sys-apps/memtest86` was removed from the Portage tree in 2024: \[[1\]](https://gitweb.gentoo.org/repo/gentoo.git/commit/sys-apps/memtest86?id=b584dd49228a5c8eaa3542d0155350a4f7c02f9c).

## Installation

### USE flags


| [+boot](https://packages.gentoo.org/useflags/+boot) | Install to /boot in addition to /usr/share/memtest86+/ | 
| [bios32](https://packages.gentoo.org/useflags/bios32) | Compile a BIOS-bootable 32bit memtest image | 
| [bios64](https://packages.gentoo.org/useflags/bios64) | Compile a BIOS-bootable 64bit memtest image | 
| [iso32](https://packages.gentoo.org/useflags/iso32) | Compile a 32bit ISO image | 
| [iso64](https://packages.gentoo.org/useflags/iso64) | Compile a 64bit ISO image | 
| [secureboot](https://packages.gentoo.org/useflags/secureboot) | Automatically sign efi executables using user specified key | 
| [uefi32](https://packages.gentoo.org/useflags/uefi32) | Compile a EFI32-bootable 32bit memtest image | 
| [uefi64](https://packages.gentoo.org/useflags/uefi64) | Compile a UEFI-bootable 64bit memtest image | 

### Prerequisites

Be sure /boot (where the GRUB files are typically installed) is available *before* installing memtest86+! If /boot is on a separate partition (which it *WILL BE* if the partitioning scheme in the Gentoo handbook was used for this Gentoo install), then be sure the mount command is run before the emerge section below:

`root #``mount /boot`
If no errors are return then boot should be successfully mounted. Check by running the mount command without any arguments and parsing the output with grep:

`user $``mount | grep boot`
boot

### Emerge

Install it through Portage with emerge:

`root #``emerge --ask sys-apps/memtest86+`
## Configuration (bootloader)

### GRUB2

For GRUB2 just run grub-mkconfig. As long as the package has been emerged, a configuration file has already been installed to /etc/grub.d/39\_memtest86+:

`root #``grub-mkconfig -o /boot/grub/grub.cfg`
### GRUB legacy

Replace the `?` (question marks) in the file below with correct numbers for the system's boot partition:

**`/boot/grub/grub.conf`**

### LILO

For LILO, add this to the lilo.conf configuration file:

**`/etc/lilo.conf`**

Then rebuild LILO's MBR entry:

`root #``lilo`
### Syslinux

Add this to the configuration file (change *.efi64* by *.bios* depending in your harware):

**`/boot/extlinux/extlinux.conf`**

### Systemd-boot

**`/boot/loader/entries/memtest.conf`**

## Usage

To use memtest86+ the system needs to be rebooted using the newly added boot item.

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose sys-apps/memtest86+`
## See also

- [https://www.memtest86.com/tech\_configuring-grub.html](https://www.memtest86.com/tech_configuring-grub.html) - Official document explaining grub configuration.
- [bug #660504](https://bugs.gentoo.org/show_bug.cgi?id=660504) - memtest86 version bump request, explaining the version forks. There an up-to-date ebuild of by Ben Kohler, a Gentoo developer (As of Oct 2019, it packages v8.2.) This has since been transitioned into a new memtest86-bin package. (As of Dec 2021, memtest86-bin packages v9.3)
- [Badblocks](https://wiki.gentoo.org/wiki/Badblocks) — a small program for stress testing [block devices](https://en.wikipedia.org/wiki/Block_device#Block_devices).
- [stress](https://wiki.gentoo.org/index.php?title=Stress&action=edit&redlink=1)
- [stress-ng](https://wiki.gentoo.org/index.php?title=Stress-ng&action=edit&redlink=1)

## External resources

- [https://www.memtest86.com/](https://www.memtest86.com/) - Official commercial Passmark website.
- [https://www.pcworld.com/article/232640/memtest86.html](https://www.pcworld.com/article/232640/memtest86.html) - A PCWorld article describing the usefulness and features of memtest86+.
