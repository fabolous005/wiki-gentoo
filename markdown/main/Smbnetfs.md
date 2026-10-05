<!-- source: https://wiki.gentoo.org/wiki/Smbnetfs | group: Gentoo Wiki (Main) | wiki-title: Smbnetfs -->
---
title: Smbnetfs
url: https://wiki.gentoo.org/wiki/Smbnetfs
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-13"
fingerprint: "9219d8789eb59dfd"
license: CC BY-SA 4.0
---

# Smbnetfs

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

smbnetfs is a [FUSE](https://wiki.gentoo.org/wiki/FUSE)-based filesystem for SMB/CIFS shares. It enables each user to mount Windows-based network shares within their own userspace.

## Installation

### Kernel

See the [Kernel section](https://wiki.gentoo.org/wiki/FUSE#Kernel) of the FUSE article for kernel confirmation.

### USE flags


### Emerge

Install the userspace utilities:

`root #``emerge --ask net-fs/smbnetfs`
## Configuration

### Files

Import files related to smbnetfs include:

- [\~/.smb/smbnetfs.conf](https://wiki.gentoo.org#Credentials) - The local (user-specific) configuration file. This file is not installed in user home directories by default (see below).

#### Credentials

Create the \~/.smb directory, then copy the configuration file to the configuration location. Note: depending on which version of the software was installed, the filesystem path could change. Determining the version of the software Portage installed will help determine the name of the first network path in the bunzip2 command:

`user $````
mkdir ~/.smb
```
`user $````
bunzip2 -c /usr/share/doc/smbnetfs-0.6.0/smbnetfs.conf.bz2 > ~/.smb/smbnetfs.conf
```
Next, modify the configuration file in order to gain network authentication. There are different methods of inputting authentication credentials: GNOME Keyring (which can be effective if the GNOME desktop environment is being used) or username/password fields in the smbnetfs.conf file itself.

For example, when entering username/password in the configuration file, uncomment the following line and add appropriate values. `guest` should be substituted with the username and the next set of quotes should include the password:

**`~/.smb/smbnetfs.conf`**

## Usage

### Invocation

smbnetfs needs to know where to mount the shares. If a directory for them does not yet exist create one, then mount things there:

`user $````
mkdir ~/mnt
```
`user $````
smbnetfs ~/mnt
```
## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose net-fs/smbnetfs`
## See also

- [Samba](https://wiki.gentoo.org/wiki/Samba) — a re-implementation of the [SMB/CIFS] networking protocol, a Microsoft Windows alternative to Network File System ([NFS](https://wiki.gentoo.org/wiki/NFS)).
