<!-- source: https://wiki.gentoo.org/wiki/Pam_mount | group: Gentoo Wiki (Main) | wiki-title: Pam mount -->
---
title: pam_mount
url: https://wiki.gentoo.org/wiki/Pam_mount
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-13"
fingerprint: b668521c9ee3b9d9
license: CC BY-SA 4.0
---

# pam\_mount

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The **pam\_mount.so** PAM module allows systems to automatically mount file systems when a user logs on, and unmount file systems when the user logs off.

## Installation

### USE flags

The [sys-auth/pam\_mount](https://packages.gentoo.org/packages/sys-auth/pam_mount) package has a few USE flags that it supports:


### Emerge

To install the package, just emerge it:

`root #``emerge --ask sys-auth/pam_mount`
## Configuration

No specific configuration is needed for the installation itself. The actual configuration entries are mentioned below under the [Usage](https://wiki.gentoo.org#Usage) section.

## Usage

### Mounting regular file systems

Edit the PAM configuration file in which the mount action has to be configured. Add the required call to pam\_mount.so for `auth` and `session` as shown in the next example:

**`/etc/pam.d/system-login`**

**"Enable pam\_mount in the proper service"**

Next, edit or create the following configuration file:

**`/etc/security/pam_mount.conf.xml`**

**"Configure pam\_mount"**

```
<pam_mount>
  <volume user="your username" fstype="ext4" path="/dev/sdxn" mountpoint="/somewhere" option="fsck" />
  <debug enable="1" />
</pam_mount>
```
This file will establish the file systems to mount when a particular user logs on. Of course, replace the example values with actual ones.

### Mounting encrypted file systems (dm-crypt/LUKS)

One might want to mount devices encrypted with cryptsetup. At the moment it's managed by pam\_mount automatically, just add `fstype="crypt"` to the configuration file:

**`/etc/security/pam_mount.conf.xml`**

```
<pam_mount>
  <volume user="username" fstype="crypt" path="/dev/sdXN" mountpoint="/somewhere" option="fsck" />
  <debug enable="1" />
</pam_mount>
```
For other kind of encrypted file systems specify the appropriate customization for mount programs.

**`/etc/security/pam_mount.conf.xml`**

```
<cryptmount>mount.crypt ...</cryptmount>
<cryptumount>umount.crypt %(MNTPT)</cryptumount>
```
See man pam\_mount.conf for details.

### Unmerge

Before removing the package, make sure that no PAM configuration file refers to the module anymore:

`user $``grep pam_mount /etc/pam.d/*`
If no file refers to it anymore, then the package is safe to unmerge:

`root #``emerge --ask --depclean --verbose sys-auth/pam_mount`
## See also

- [PAM](https://wiki.gentoo.org/wiki/PAM) — allows (third party) services to provide an authentication module for their service which can then be used on PAM enabled systems.
