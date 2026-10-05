<!-- source: https://wiki.gentoo.org/wiki/HDD | group: Gentoo Wiki (Main) | wiki-title: HDD -->
---
title: HDD
url: https://wiki.gentoo.org/wiki/HDD
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-02-19"
fingerprint: c631d99695fe9ba9
license: CC BY-SA 4.0
---

# HDD

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes the setup of an internal SATA or PATA (IDE) rotational **hard disk drive**.

## Installation

### Hardware detection

To choose the right driver, first detect the used storage controller. [lspci](https://wiki.gentoo.org/wiki/Pciutils) can be used for this task:

`root #``lspci | grep --color -E "IDE|SATA"`
(At runtime) show identification and feature info (replace `/dev/sdX` with the right device):

`root #``hdparm -I /dev/sdX`
For more detailed information see the [hdparm](https://wiki.gentoo.org/wiki/Hdparm) article.

### BIOS

For AHCI SATA controllers, check the system's [BIOS](https://wiki.gentoo.org/wiki/BIOS) or firmware to see if if AHCI has been activated.

### Kernel

Activate the following kernel options:

**`CONFIG_SCSI`, `CONFIG_BLK_DEV_SD`, `CONFIG_ATA_ACPI`, `CONFIG_SATA_PMP`, `CONFIG_SATA_AHCI`, `CONFIG_ATA_BMDMA`, `CONFIG_ATA_SFF`, `CONFIG_ATA_PIIX`**

## Configuration

Generally when configuring a hard disk drive one or more [partitions](https://wiki.gentoo.org/wiki/Partition) will need to be created and [filesystems](https://wiki.gentoo.org/wiki/Filesystem) written into them.

## Usage

Filesystems can be mounted in several ways. Notable methods include:

- The [mount](https://wiki.gentoo.org/wiki/Mount) command.
- [/etc/fstab](https://wiki.gentoo.org/wiki//etc/fstab) file - Automatic mount at boot time (does not support on demand mount).
- [removable media](https://wiki.gentoo.org/wiki/Removable_media) - Automated mount on demand.
- [AutoFS](https://wiki.gentoo.org/wiki/AutoFS) - Automated mount on demand.

## Troubleshooting

## See also

- [SSD](https://wiki.gentoo.org/wiki/SSD) — provides guidelines for basic maintenance, such as enabling discard/trim support, for **SSD**s ([Solid State Drives](https://en.wikipedia.org/wiki/Solid-state_drive)) on Linux.
