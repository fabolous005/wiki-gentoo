<!-- source: https://wiki.gentoo.org/wiki/CDROM | group: Gentoo Wiki (Main) | wiki-title: CDROM -->
---
title: CDROM
url: https://wiki.gentoo.org/wiki/CDROM
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-22"
fingerprint: b605ac548ee6f14d
license: CC BY-SA 4.0
---

# CDROM

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes the setup of an internal optical drive like CD, DVD, and Blu-Ray drives. For external drives you'll need highly probable [USB](https://wiki.gentoo.org/wiki/USB) support instead of ATA drivers as described below.

## Installation

### Hardware detection

To choose the right driver, first detect the used storage controller. [lspci](https://wiki.gentoo.org/wiki/Pciutils) can be used for this task:

`root #``lspci | grep --color -E "IDE|SATA"`
### Kernel

Activate the following kernel options:

KERNEL **Kernel options for optical storage media**

```
Device Drivers --->
   <*> Serial ATA and Parallel ATA drivers  --->
      [*] ATA ACPI Support
  
      # If the drive is connected to a SATA Port Multiplier:
      [*] SATA Port Multiplier support
  
      # Select the driver for the SATA controller, e.g.:
      <*> AHCI SATA support (ahci)
  
      # If the drive is connected to an IDE controller:
      [*] ATA SFF support
      [*] ATA BMDMA support
  
      # Select the driver for the IDE controller, e.g.:
        <*> Intel ESB, ICH, PIIX3, PIIX4 PATA/SATA support (ata_piix)
  
   SCSI device support  ---> 
      <*> SCSI device support
      <*> SCSI CDROM support
      <*> SCSI generic support
  
File systems  --->
   CD-ROM/DVD Filesystems  --->
      <M> ISO 9660 CDROM file system support
      [*] Microsoft Joliet CDROM extensions
      [*] Transparent decompression extension
      <M> UDF file system support
```
## Usage

Filesystems can be mounted in several ways:

- [mount](https://wiki.gentoo.org/wiki/Mount) - Command for mounting file systems
- [fstab](https://wiki.gentoo.org/wiki//etc/fstab) - Automatic mount at boot time.
- [removable media](https://wiki.gentoo.org/wiki/Removable_media) - Mount on demand.
- [AutoFS](https://wiki.gentoo.org/wiki/AutoFS) - Automatic mount on demand.

## Troubleshooting

If the optical drive is constantly checking for a new disk causing it to make unnecessary noise, consider turning SATA "Hot Plug" on for the optical drive in [BIOS](https://wiki.gentoo.org/wiki/BIOS).

See the [Libata error messages](https://ata.wiki.kernel.org/index.php/Libata_error_messages) article on the Libata wiki.

## See also

- [Blu-ray](https://wiki.gentoo.org/wiki/Blu-ray) — **Blu-ray** is the optical media successor to DVD
- [CD/DVD/BD writing](https://wiki.gentoo.org/wiki/CD/DVD/BD_writing) — how to **burn optical disks** on Gentoo from the [command line](https://wiki.gentoo.org/wiki/Terminal_emulator) with the [app-cdr/cdrtools](https://packages.gentoo.org/packages/app-cdr/cdrtools) or [app-cdr/dvd+rw-tools](https://packages.gentoo.org/packages/app-cdr/dvd+rw-tools) packages
- [hdparm](https://wiki.gentoo.org/wiki/Hdparm) — a command-line utility to set and view ATA and SATA [hard disk drive](https://wiki.gentoo.org/wiki/HDD) hardware parameters.
- [FAQ - How do I burn an ISO file?](https://wiki.gentoo.org/wiki/FAQ#How_do_I_burn_an_ISO_file.3F)
- [Recommended GUI burners](https://wiki.gentoo.org/wiki/Recommended_applications#Optical_disk_burners)
