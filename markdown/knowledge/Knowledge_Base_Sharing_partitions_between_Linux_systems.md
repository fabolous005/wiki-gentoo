<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:Sharing_partitions_between_Linux_systems | group: Gentoo Knowledge | wiki-title: Knowledge_Base:Sharing_partitions_between_Linux_systems -->
---
title: Knowledge Base:Sharing partitions between Linux systems
url: https://wiki.gentoo.org/wiki/Knowledge_Base:Sharing_partitions_between_Linux_systems
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-05-26"
fingerprint: bd7c3a1e0fa38f2d
license: CC BY-SA 4.0
---

# Knowledge Base:Sharing partitions between Linux systems

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Synopsis

Users that dual-boot their Linux system with another Linux system might be inclined to share file systems between the Linux systems. This article will cover which parts of the file system can be shared and how users may do so.

## Environment

This article is applicable to all systems that dual-boot with other *Linux* systems. This article does *not* apply to systems that dual-boot with any version of the Windows operating system, and may not apply to other operating systems.

## Analysis

When sharing file systems (partitions usually) users should take special care to avoid:

- Mounting a file system read-write while it is in use by a suspended (hibernating) instance
- Mixing of user IDs;
- Collision of files managed by the (potentially different) package managers;
- The use of extended attributes in the file system.

### Suspend-to-disk

Assume a system with two boot entries A and B. If A mounts a file system and suspends to disk (i.e. hibernates) and then resumes, the running processes accessing said file system assume that the file system has not changed.

If, after hibernating A, B is booted instead, the file system should not be mounted read-write from B. Mounting it as read-write risks making modifications that conflict with the running processes in A. If A is resumed from disk and its file system has been modified, the following could happen:

- Partitions which were/are going to be mounted read-write may become corrupted.
- The swap partition will still be in use by A.

It is therefore ill-advised to mount the same /efi file system from two instances if one could be suspended to disk, unless the other instance only mounts the /efi partition as read-only. This can be configured in [/etc/fstab](https://wiki.gentoo.org/wiki//etc/fstab).

### User IDs

Each file on a file system contains ownership information. On the file system itself, this is stored as the numerical value of the user ID (UID) and group ID (GID).

`user $``id`
uid=1001(larry) gid=100(users) groups=100(users)16(cron),18(audio),27(video),85(usb)

When dual-booting between Linux systems while using the same partition for operating system files, pay close attention to make the file systems share the host files that have the same mappings (UID and GID) for the users and groups. If special attention is not provided to this detail, then ownership and group-ownership will change between the systems. This will potentially cause disruption of services or even security issues between systems.

### Package managers

Each Linux system uses a package manager to manage software installations. These package managers keep track of the installed files (and to which package they belong). When sharing partitions between Linux systems, the files on these partitions may be altered by their package managers (dependencies added or removed, etc.). Adding or removing software in one of the operating systems will most likely effect the package managers' databases on the *other* system; one operating system will host wrong data or even become corrupt.

It is therefore advisable not to share file systems whose content is managed by a package manager, or make sure that the sharing is only done on a read-only basis.

### Extended attributes

Some systems use extended attributes on the files to manage more in-depth security ([SELinux](https://wiki.gentoo.org/wiki/SELinux) is a one of those systems). If the other Linux system does not support these extended attributes (or ignores them) then the settings will become stale or even disappear between reboots, causing potential service malfunctions.

## Resolution

When sharing of partitions is wanted, the following locations can be shared as long as the Linux systems do not use services that rely on particular file metadata (like [SELinux](https://wiki.gentoo.org/wiki/SELinux) with extended attributes).

- The /boot partition is a candidate that has probably been considered by most users. After all, dual booting means that the boot loader must be shared. Make sure though that the Linux systems do not try to manage the boot loader configuration themselves. Instead, manage them manually (this is already the case for Gentoo Linux);
- The swap partition can be shared between Linux systems because the partition is always considered *empty* after shutdown of a Linux system. It is an extension of the memory of a booted system, and dual-booting means that the two operating systems will never run simultaneously;
- Temporary locations like /tmp and /var/tmp can be shared between Linux systems (if they reside on separate partitions of course).

The following can be shared but is not recommended:

- The /home partition, but only under the condition that the user IDs and group IDs are the *same* on both Linux systems. This can be verified by comparing /etc/passwd and /etc/group on both systems. However, this is not recommended since the /home partition also contains the users' configuration files (for applications). When the two Linux systems use different versions of the applications, then the configuration files will become mangled or might even become corrupt.

- It is better to create a dedicated data partition where users on both Linux systems have access and share that partition instead.
