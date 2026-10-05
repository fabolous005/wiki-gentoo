<!-- source: https://wiki.gentoo.org/wiki/Filesystem/Access_Control_List_Guide | group: Gentoo Wiki (Main) | wiki-title: Filesystem/Access Control List Guide -->
---
title: Filesystem/Access Control List Guide
url: https://wiki.gentoo.org/wiki/Filesystem/Access_Control_List_Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-11-22"
fingerprint: "95669bdae2eefb09"
license: CC BY-SA 4.0
---

# Filesystem/Access Control List Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**A**ccess **C**ontrol **L**ist (ACL or POSIX ACL) is an additional security control feature for multiuser systems. POSIX ACL facilitates a more fine-grained control over [filesystem](https://wiki.gentoo.org/wiki/Filesystem) permissions than the basic [POSIX RWX bits](https://wiki.gentoo.org/wiki/Filesystem/Security#The_traditional_Unix_way) do.

## Installation

### Kernel

Enable *POSIX Access Control Lists* (`CONFIG_*_POSIX_ACL`) for each filesystem that is intended to leverage ACLs.

**Enabling Access Control Lists**

### USE flags


### Emerge

Utilities for manipulating ACLs are available in [sys-apps/acl](https://packages.gentoo.org/packages/sys-apps/acl):

`root #``emerge --ask sys-apps/acl`
### Additional software

The [sys-apps/apply-default-acl](https://packages.gentoo.org/packages/sys-apps/apply-default-acl) package provides a utility improving ACL user experience.

## Configuration

Some filesystems, such as [ext4](https://wiki.gentoo.org/wiki/Ext4), [XFS](https://wiki.gentoo.org/wiki/XFS), or [Btrfs](https://wiki.gentoo.org/wiki/Btrfs), enable ACLs by default when [mounted](https://wiki.gentoo.org/wiki/Mount). Other filesystems may require extra mount options to enable POSIX ACLs.

For example, in case of [ext4](https://wiki.gentoo.org/wiki/Ext4) there is the `acl` mount option<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> available. It can be used in /etc/fstab as:

**`/etc/fstab`**

## Usage

The [sys-apps/acl](https://packages.gentoo.org/packages/sys-apps/acl) provides setfacl, getfacl, and chacl utilities.

### Get/read ACL

The getfacl utility is used to read ACLs assigned on files and directories.

For example, to get ACLs on testfile:

`user $``getfacl testfile`
\# file: testfile
# owner: larry
# group: larry
user::rw-
user:notlarry:r-x
group::r--
mask::r-x
other::r--

### Set/modify ACL

The setfacl utility is used to set ACLs on files and directories.

#### Examples

To add larry to have read, write and execute permissions on testfile:

`user $``setfacl -m u:larry:rwx testfile`
To add larry to have +write access on testfile:

`user $``setfacl -m u:larry:+w testfile`
To add default user access right to read and write permissions on testdir:

`user $``setfacl -m d:u:larry:rw testdir/`
To add groupname to have read, write and execute permissions on testfile:

`user $``setfacl -m g:groupname:rwx testfile`
To add groupname to have recursive +execute permissions on testdir:

`user $``setfacl -R -m g:groupname:+x testdir/`
To add default group access right to read and write permissions on testdir:

`user $``setfacl -m d:g:groupname:rw testdir/`
To remove ACLs from testfile:

`user $``setfacl -b testfile`
To remove default ACL from testdir:

`user $``setfacl -k testdir/`
### ACL mask

## Troubleshooting

### Which files/directories leverage ACLs?

The ls command used with the `-l` option displays a `+` sign if the listed file uses ACL.

Notice the `+` sign on both apache2 and named.

`user $``ls -l /var/www/`
total 54632
drwxr-xr-x+ 2 apache  apache       135 Dec 11 17:48 apache2
-rw-r-----  1 root    root       25085 Jan  4 14:26 dmesg
-rw-rw----  1 portage portage    22088 Jan  4 01:06 emerge-fetch.log
-rw-rw----  1 portage portage  1498948 Jan  4 04:06 emerge.log
-rw-------  1 root    root       32480 Dec 30 21:30 faillog
-rw-r--r--  1 root    root      628240 Nov  6 01:47 genkernel.log
-rw-r--r--  1 root    root      296380 Jan  4 18:43 lastlog
-rw-------  1 root    root    47973000 Jan  4 19:40 messages
drwxr-xr-x  2 mysql   mysql         82 Dec 11 22:04 mysql
drwxrwx---+ 2 named   named       4096 Jan  3 18:09 named
drwxr-xr-x  2 root    root          18 May 14  2010 news
drwxr-xr-x  3 root    root      167936 Jan  4 04:24 portage
-rw-r--r--  1 root    root       88301 Jan  4 14:26 rc.log
drwxr-xr-x  3 root    root        4096 Jan  2 02:55 samba
drwxrwx---  2 root    portage       37 Dec 11 15:21 sandbox
-rw-------  1 root    root       64960 Jan  2 02:59 tallylog
-rw-------  1 root    root         560 Nov 11 02:35 vsftpd.log
drwxr-xr-x  2 root    root          63 Sep 12  2010 webmin
-rw-rw-r--  1 root    utmp     1178112 Jan  4 18:43 wtmp
