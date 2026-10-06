<!-- source: https://wiki.gentoo.org/wiki/GRUB/Troubleshooting | group: Gentoo Wiki (Main) | wiki-title: GRUB/Troubleshooting -->
---
title: GRUB/Troubleshooting
url: https://wiki.gentoo.org/wiki/GRUB/Troubleshooting
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-08-30"
fingerprint: "1e0dc81f876d9dc9"
license: CC BY-SA 4.0
---

# GRUB/Troubleshooting

[GRUB](https://wiki.gentoo.org/wiki/GRUB)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article documents issues that might arise when trying to use or install GRUB.

## Installation errors

### Installing GRUB from within a chroot

When a separate /boot partition exists, be sure to mount the target root partition before chrooting, but do not mount the /boot partition until *after* chrooting to the new environment. This is required because grub-mkconfig will not detect /boot as a separate partition and will assume the /boot directory and the root (/) directory are on the same partition.

### Reinstalling the GRUB EFI boot manager entry

In case the boot manager entries need to be restored, they can be re-added without invoking grub-install. Load the `efivars` kernel module and install [sys-boot/efibootmgr](https://packages.gentoo.org/packages/sys-boot/efibootmgr):

`root #``modprobe efivars``root #``emerge --ask sys-boot/efibootmgr`
Next add a boot menu entry:

`root #``efibootmgr --create --gpt --disk /dev/sda --part 1 --write-signature --label "gentoo" --loader "\\EFI\\gentoo\\grubx86.efi"`
### grub-install: error: cannot find EFI directory.

`root #``grub-install --target=x86_64-efi`
grub-install: error: cannot find EFI directory.

Properly setting the `GRUB_PLATFORMS` variable in /etc/portage/make.conf (the first time!) as listed above should resolve this error. Set the `GRUB_PLATFORMS` variable then re-emerge GRUB using the following command:

`root #``emerge --ask --newuse sys-boot/grub:2`
In the case this error persists try forcing the install to the EFI directory by issuing the grub-install command with correct parameters for the `--target` and `--efi-directory` options:

`root #``grub-install --target=x86_64-efi --efi-directory=/boot`
### grub-install: Attempting to install GRUB to a disk or partition

`root #``grub-install /dev/sda1` Attempting to install GRUB to a partition disk or to a partition.  This is a BAD idea.
Embedding is not possible.  GRUB can only be installed in this setup by using blocklists. 
However, blocklists are UNRELIABLE and their use is discouraged.

There could be many reasons for this error to come up.

#### No mounted /boot

A /boot partition is not mounted.

`root #``mount /boot`
#### No BIOS boot partition while using BIOS-GPT setup

This could mean there is not a `EF02` or `bios_boot` partition to embed the core.img file in.

Creating a little partition of a few hundreds kilobytes may be enough, although it is good practice to align this partition to 2048 sectors to avoid a performance penalty with the disk. A 1 MiB partition should work well.

#### Attempting to install GRUB in a partition

When attempting to install GRUB in a partition e.g. /dev/sda5 (for instance because the boot loader will be chainloaded by a different boot loader), simply add `--force` option switch to force install.

#### Missing target detection

If grub-install does not print any errors message or if the error message is about scanning disk files and several platforms are enabled in /etc/portage/make.conf while nothing is installed on the disk, then edit the /sbin/grub-install script with a text editor of choice.

Change the `platform` variable to the appropriate platform for the current use case. The platform *must* be rightly set when emerging [sys-boot/grub](https://packages.gentoo.org/packages/sys-boot/grub), else it will be incorrectly configured in the /sbin/grub-install script.

For example, to set the platform for "PC", change `platform=qemu` to `platform=pc`.

### grub-setup: no post-MBR gap

`root #``grub-bios-setup````
grub-setup: warn: This msdos-style partition label has no post-MBR gap; embedding won't be possible!
grub-bios-setup: warn: Embedding is not possible. GRUB can only be installed in this setup by using blocklists.
            However, blocklists are UNRELIABLE and its use is discouraged.
grub-bios-setup: error: If you really want blocklists, use --force.
```
This error means that GRUB could not find the usual gap between the MBR and first partition of a disk. It is possible that the gap does not exist. Some tools (e.g. LiveUSB disk installer) format disks so that the first partition starts at sector 1 instead of 2048.

Repartitioning might be necessary to resolve the issue.

### grub-install: embedding area is unusually small

`root #``grub-install /dev/sda`
grub-install: warn: Your embedding area is unusually
small. core.img won't fit in it.
grub-install: warn: Embedding is not possible. GRUB can
only be installed in this setup by using blocklists. However,
blocklists are UNRELIABLE and their use is discouraged.
grub-install: error: will not proceed with blocklists.

There is not enough room at the beginning of the disk before the first partition.

In general this is a bit of trouble to fix. One or more partitions need to be resized in order to leave some free space at the beginning of the drive. The [GParted LiveCD](https://gparted.sourceforge.net/livecd.php) or [SystemRescueCD](https://sourceforge.net/projects/systemrescuecd/) are excellent tools to make this job less difficult.

If the swap partition is the first partition on the disk (e.g. /dev/sda1) it is possible to simply remove the existing swap space partition and re-partition the space. This resolution would avoid the need for booting into a live medium in order to resize a root (/) partition.

### grub-install: EFI variables are not supported on this system

`root #``grub-install --target=x86_64-efi --efi-directory=/boot`
Installing for x86\_64-efi platform.
EFI variables are not supported on this system.
EFI variables are not supported on this system.
grub-install: error: efibootmgr failed to register the boot entry: No such file or directory.

This error may be caused by the fact that you have booted in legacy or compatible mode.

The OS must be loaded in the EFI mode for this command to work.

### grub-install: Could not prepare boot variable: No space left on device

`root #``grub-install --target=x86_64-efi --efi-directory=/efi`
Installing for x86\_64-efi platform.
grub-install: error: Could not prepare boot variable: No space left on device

By default, GRUB uses efibootmgr to add a boot entry to the UEFI/BIOS firmware and makes the executable /\<ESP>/EFI/gentoo/grub\<arch>.efi. If `--removable` is used, then GRUB simply makes the executable /\<ESP>/EFI/BOOT/BOOT\<arch>.efi without modifying the UEFI firmware. /\<ESP>/EFI/BOOT/BOOT\<arch>.efi is a known path used by UEFI; this path can be used if the UEFI does not support arbitrary path names well, or there is no more free space in the NVRAM for efibootmgr to add an entry. If the NVRAM is full, we will get the error, "Could not prepare boot variable: No space left on device".

There are two solutions: delete *junk* files from the NVRAM, or use the `--removable` option with grub-install.

If the boot path /\<ESP>/EFI/BOOT/BOOT\<arch>.efi is going to be used, keep in mind that other operating systems' bootloaders (Windows) can override this path; to prevent this from happening on a multi-boot system, there are several solutions available on the Arch wiki<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.

To delete the junk files from the NVRAM:

`root #``rm /sys/firmware/efi/efivars/dump-*`
## Boot errors

Troubleshooting steps in this section presume GRUB has been installed correctly, but issues are encountered when booting the operating system.

### error: no such device

This error occurs when GRUB cannot detect a device for which to boot. This can occur for one or many of the following reasons:

- **Physical issues**
  - The device has been *intentionally* removed/disconnected from the system.
  - The device has been *unintentionally* removed/disconnected from the system. Perhaps a power or data cable is loose or disconnected, the device has been damaged via water, impact, electrical surge, or other type of damage, the device has failed to age or end-of-life component failure.
- **Logical issues**
  - The device is connected, but the motherboard firmware has not detected the device. A common cause is a [time-of-check to time-of-use](https://en.wikipedia.org/wiki/Time-of-check_to_time-of-use) style [race condition](https://en.wikipedia.org/wiki/Race_condition): the device itself or the bus subsystem hosting the device (PCI, SATA, USB, etc.) has not (yet) been make visible to GRUB at the time GRUB performs device detection. This results in the device not being detected. GRUB does not automatically perform a device redetection, therefore the device will not be discovered. This can be fixed by disabling fast/quick boot on via the motherboard's firmware. If disabling quick boot does not solve the problem, then it may be possible to add additional delays to the [power-on self-test](https://en.wikipedia.org/wiki/Power-on_self-test) process so that the motherboard will give more time for all bus subsystems to initialize.

### error: unknown filesystem

Unknown filesystem errors could be the result of GRUB not having the correct filesystem module inserted (loaded) at run time. This should be a result of the filesystem not being supported by the GRUB bootloader, or an issue within the grub.cfg configuration file. Generally GRUB does a good job of automatically probing filesystem when updating its configuration file.

This issue could also be a result of the device containing the filesystem not (yet) being detected by the system's firmware. See [error: no such device](https://wiki.gentoo.org#error:_no_such_device).

### booting into BIOS/EFI settings

This is typically a side effect of some GRUB updates that require a re-install.

First, chroot into the system and re-install GRUB:

For EFI systems:

`root #``mount /dev/sda1 /efi``root #``grub-install --efi-directory=/efi`
Or for BIOS systems:

`root #``grub-install /dev/sda`
Next, regenerate the config with:

`root #``grub-mkconfig -o /boot/grub/grub.cfg`
GRUB should now be fixed.
