<!-- source: https://wiki.gentoo.org/wiki/Encrypt_a_home_directory_with_ECryptfs | group: Gentoo Wiki (Main) | wiki-title: Encrypt a home directory with ECryptfs -->
---
title: Encrypt a home directory with ECryptfs
url: https://wiki.gentoo.org/wiki/Encrypt_a_home_directory_with_ECryptfs
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-07-29"
fingerprint: a31dde1967cba3c5
license: CC BY-SA 4.0
---

# Encrypt a home directory with ECryptfs

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The option to encrypt the disk or a partition is usually performed during system installation.

But instead of encrypting the entire disk, there is the option to encrypt just the user's home directory. Maybe it doesn't offer the same level of security, but for some users it may be enough.

The encrypted home directory will not require any extra passwords besides the user login password, as is normally done. Even if someone has access to the system as root, and changes the user's password, they won't be able to access the encrypted files - it is necessary to know the original password.

## Procedure

Install the necessary software. As this is masked software, it is necessary to unmask it, see the [unmasking a package](https://wiki.gentoo.org/wiki/Knowledge_Base:Unmasking_a_package) article on how to do that. The [suid](https://packages.gentoo.org/useflags/suid) [flag may need setting on](https://wiki.gentoo.org/wiki/USE_flag) [sys-fs/ecryptfs-utils](https://packages.gentoo.org/packages/sys-fs/ecryptfs-utils). Then emerge [sys-fs/ecryptfs-utils](https://packages.gentoo.org/packages/sys-fs/ecryptfs-utils):

`root #``emerge --ask ecryptfs-utils`
Restart the computer. At the login screen, **do not** log in.

Open a new tty, for example: Ctrl + Alt + F2.

Log in with the root user.

Activate the module:

`root #``modprobe ecryptfs`
Migrate user's home directory to an encrypted home directory. It will prompt for the user's password:

`root #``ecryptfs-migrate-home -u <USERNAME>`
Switch to the daily user. It will prompt for the user's password again:

`root #``su <USERNAME>`
Go to home folder:

`user $``$ cd`
Run the mount wrapper script. Use the user password when prompted:

`user $``ecryptfs-mount-private`
Unwrap an eCryptfs mount passphrase. Enter user password, again:

`user $``ecryptfs-unwrap-passphrase`
Run the unmount wrapper script:

`user $````
ecryptfs-umount-private
```
`user $``exit`
If logging in as root again, in the home directory there will be a copy of user's directory followed by "." and some characters. It's a backup, to restore personal files if something goes wrong. When the whole process is done and it's working, that directory may be deleted.

## Auto-mounting

To allow the encrypted home directory to be mounted automatically on login, proceed as follows.

Make a backup of the "/etc/pam.d/system-auth":

`root #``cp /etc/pam.d/system-auth /etc/pam.d/system-auth.ori`
Add two lines to the file.

`root #``nano /etc/pam.d/system-auth`
Compare the differences, and see which lines have been added:

`user $``diff -u /etc/pam.d/system-auth.ori /etc/pam.d/system-auth`
--- /etc/pam.d/system-auth.ori  2022-08-21 19:02:30.070080014 -0300
+++ /etc/pam.d/system-auth      2022-08-21 19:03:52.649811213 -0300
@@ -2,6 +2,7 @@
 auth           requisite       pam\_faillock.so preauth
 auth            \[success=1 default=ignore\]      pam\_unix.so nullok  try\_first\_pass
 auth           \[default=die\]   pam\_faillock.so authfail
+auth           optional        pam\_ecryptfs.so unwrap
 account                required        pam\_unix.so
 account         required        pam\_faillock.so
 password       required        pam\_passwdqc.so config=/etc/security/passwdqc.conf
@@ -9,3 +10,4 @@
 session                required        pam\_limits.so
 session                required        pam\_env.so
 session                required        pam\_unix.so
+session                optional        pam\_ecryptfs.so unwrap

Save and exit. Restart and log in normally with username.
