<!-- source: https://wiki.gentoo.org/wiki/Security_Handbook/Mounting_partitions | group: Gentoo Wiki (Main) | wiki-title: Security Handbook/Mounting partitions -->
---
title: Security Handbook/Mounting partitions
url: https://wiki.gentoo.org/wiki/Security_Handbook/Mounting_partitions
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-12"
fingerprint: b69ee88644a35b89
license: CC BY-SA 4.0
---

# Security Handbook/Mounting partitions

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

System administrators should consider available security related mount options in order to harden any devices that are connected to the system.

When [mounting](https://wiki.gentoo.org/wiki/Mount#Usage) a partition, be it [Btrfs](https://wiki.gentoo.org/wiki/Btrfs), [ext4](https://wiki.gentoo.org/wiki/Ext4), or [XFS](https://wiki.gentoo.org/wiki/XFS), a few security related mount options can be applied in /etc/fstab file to harden the mountpoint and provide better security to the system at large. Some options include:

- `nosuid`
- Ignores the SUID bit and makes it just like an ordinary file.
- `noexec`
- Prevents execution of files from this mount point.
- `nodev`
- Ignores devices.

Unfortunately, these settings can easily be circumvented by executing a non-direct path. However, mounting the /tmp directory with `noexec` will stop the majority of exploits designed to be executed directly from temporary file systems.

For example, hardening the /etc/fstab file may look something like the following:

**`/etc/fstab`**

```
          /boot      ext2     noauto,relatime                     1 2
/dev/sda2          none       swap     sw                                  0 0
/dev/sda3          /          ext4     relatime,errors=remount-ro          0 1
/dev/sda4          /var       ext4     relatime,nodev,nosuid,noexec        0 2
/dev/sda5          /var/tmp   ext2     noatime,nodiratime,nodev,nosuid     0 2
/dev/sda6          /home      ext4     relatime,nodev,nosuid               0 2
/dev/sda7          /usr       ext4     relatime,nodev,ro                   0 2
/dev/cdroms/cdrom0 /mnt/cdrom iso9660  noauto,ro                           0 0
none               /tmp       tmpfs    nodev,nosuid,noexec                 0 0
```
Observe in the example that the /usr mount point is set to read-only mode. This system has been designed to write nothing to /usr until updates are being applied. When it is time for system updates, /usr is remounted in read-write mode, updated, then returned to read-only. This small trick has the potential to keep a server more secure.

Some programs (like [mail-mta/netqmail](https://packages.gentoo.org/packages/mail-mta/netqmail)) will not be able to work properly if /var has `noexec` and `nosuid`. Consider removing those options if they cause problems.

- [Disk quotas](https://wiki.gentoo.org/wiki/Security_Handbook/User_and_group_limitations#Quotas) - Enforcing disk quotas.
