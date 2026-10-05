<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:No_space_left_on_device_while_there_is_plenty_of_space_available | group: Gentoo Knowledge | wiki-title: Knowledge_Base:No_space_left_on_device_while_there_is_plenty_of_space_available -->
---
title: Knowledge Base:No space left on device while there is plenty of space available
url: https://wiki.gentoo.org/wiki/Knowledge_Base:No_space_left_on_device_while_there_is_plenty_of_space_available
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-07-06"
fingerprint: b92cba1b01898f8d
license: CC BY-SA 4.0
---

# Knowledge Base:No space left on device while there is plenty of space available

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This knowledge base article provides details for solving the "No space left on device" while there is plenty of space available on the device.

## Synopsis

During an operation that writes to the disk, the operation fails with the error message *No space left on device*. However, after validating this with the df command, plenty of space is still available.

`root #``env-update`
\>>> Regenerating /etc/ld.so.cache...
/sbin/ldconfig: Can't create temporary cache file /etc/ld.so.cache\~: No space left on device

`root #``df -h /`
Filesystem            Size  Used Avail Use% Mounted on 
/dev/sda3           3.3G  1.7G  1.5G  54% /

## Environment

This article applies to any Gentoo Linux installation.

## Analysis

When space is concerned, there are two important factors that a system has to consider.

1. The first one is the most obvious: there must be space available on the file system, meaning that there are still unused data blocks available on the file system.
2. However, an often overlooked second factor is that there must still be metadata blocks available on the file system as well. In most file systems, these are called *i-nodes* or *inodes*. Whenever a file is created on a file system, an inode is used to contain information about the file. But many file systems have a fixed amount of these inodes (which is set during the mkfs operation of the file system).

To check the state of the inodes on a Linux system, use df -i.

## Resolution

Verify the system has free inodes available:

`root #``df -i /`
Filesystem            Inodes   IUsed   IFree IUse% Mounted on 
/dev/sda3             216000  216000       0  100% /

If indeed short on inodes, try removing obsolete or unnecessary files on the file system. There is, sadly enough, no way to increase the number of inodes on a file system once the file system has been created.
