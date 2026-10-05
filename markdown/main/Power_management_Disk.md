<!-- source: https://wiki.gentoo.org/wiki/Power_management/Disk | group: Gentoo Wiki (Main) | wiki-title: Power management/Disk -->
---
title: Power management/Disk
url: https://wiki.gentoo.org/wiki/Power_management/Disk
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-15"
fingerprint: e645bf44c1f2e928
license: CC BY-SA 4.0
---

# Power management/Disk

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes the setup of [power management](https://wiki.gentoo.org/wiki/Power_management) of storage devices (disks).

## Configuration

### Kernel

KERNEL

Device Drivers  --->
  \<M> Serial ATA and Parallel ATA drivers (libata) ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_ATA\</code> to find this item.
    \[\*\] SATA Zero Power Optical Disc Drive (ZPODD) support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SATA\_ZPODD\</code> to find this item.
    \*\*\* Controllers with non-SFF native interface \*\*\*
    \<M> AHCI SATA support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SATA\_AHCI\</code> to find this item.
    (3) Default SATA Link Power Management policy [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SATA\_MOBILE\_LPM\_POLICY\</code> to find this item.

### Udev

Make the following [udev](https://wiki.gentoo.org/wiki/Udev) rule file to automate power management:

FILE **`/etc/udev/rules.d/10-my-disk-power.rules`**

## See also

- [CDROM](https://wiki.gentoo.org/wiki/CDROM) — describes the setup of an internal optical drive like CD, DVD, and Blu-Ray drives
- [HDD](https://wiki.gentoo.org/wiki/HDD) — describes the setup of an internal SATA or PATA (IDE) rotational **hard disk drive**.
- [SSD](https://wiki.gentoo.org/wiki/SSD) — provides guidelines for basic maintenance, such as enabling discard/trim support, for **SSD**s ([Solid State Drives](https://en.wikipedia.org/wiki/Solid-state_drive)) on Linux.
- [NVMe](https://wiki.gentoo.org/wiki/NVMe) — flash memory chips connected to a system via the PCI-E bus (use four-lane max).
