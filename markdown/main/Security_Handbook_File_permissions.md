<!-- source: https://wiki.gentoo.org/wiki/Security_Handbook/File_permissions | group: Gentoo Wiki (Main) | wiki-title: Security Handbook/File permissions -->
---
title: Security Handbook/File permissions
url: https://wiki.gentoo.org/wiki/Security_Handbook/File_permissions
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-02-17"
fingerprint: "1432ba4a2e2c0701"
license: CC BY-SA 4.0
---

# Security Handbook/File permissions

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This section provides instructions on securing system files.

Non-administrative users should not have access to configuration files or passwords. An attacker can steal passwords from databases or web sites and use them to deface - or even worse, delete-data. This is why it is important that each system have correct file permissions. If a certain file is only used by root, assign the 0600 permissions with chmod and change the owner to root using chown.

Find world-writable files and directories:

`root #````
find / -type f \( -perm -2 -o -perm -20 \) -exec ls -lg {} \; 2>/dev/null >writable.txt
```
`root #````
find / -type d \( -perm -2 -o -perm -20 \) -exec ls -ldg {} \; 2>/dev/null >>writable.txt
```
This will create a huge file with permission of all files having either write permission set to the group or everybody. Check the permissions and eliminate world writable files to everyone, by executing /bin/chmod o-w on the files.

Files with the SUID or SGID bit set execute with privileges of the owning user or group and not the user executing the file. Normally these bits are used on files that must run as root in order to do what they do. These files can lead to local root compromises (if they contain security holes). This is dangerous and files with the SUID or SGID bits set should be avoided at any cost. If you do not use these files, use chmod 0 on them or unmerge the package that they came from (check which package they belong to by using equery; if you do not already have it installed simply type emerge --ask app-portage/gentoolkit). Otherwise just turn the SUID bit off with chmod -s.

Find setuid files:

`root #``find / -type f \( -perm -004000 -o -perm -002000 \) -exec ls -lg {} \; 2>/dev/null >suidfiles.txt`
This will create a file containing a list of all the SUID/SGID files.

List of setuid binaries:

`root #``cat suidfiles.txt`
/bin/su
/bin/ping
/bin/mount
/bin/umount
/var/qmail/bin/qmail-queue
/usr/bin/chfn
/usr/bin/chsh
/usr/bin/crontab
/usr/bin/chage
/usr/bin/expiry
/usr/bin/sperl5.6.1
/usr/bin/newgrp
/usr/bin/passwd
/usr/bin/gpasswd
/usr/bin/procmail
/usr/bin/suidperl
/usr/lib/misc/pt\_chown
/usr/sbin/unix\_chkpwd
/usr/sbin/traceroute
/usr/sbin/pwdb\_chkpwd

By default Gentoo does not have a lot of SUID files (though this depends on what has been installed on the system). The list may look similar to the one above. Most of the commands should not be used by normal users, only root. Switch off the SUID bit on system utilities such as ping, mount, umount, chfn, chsh, newgrp, suidperl, pt\_chown, and traceroute by executing chmod -s on every file.

Do not remove the bit on su, qmail-queue, or unix\_chkpwd. Removing setuid from these files will prevent users from su'ing and receiving mail. By removing the bit (where it is safe to do so) will help limit the possibility of a normal user (or an attacker) gaining root access through new vulnerabilities found in any of these executables.

The only SUID files that I have on my system are su, passwd, gpasswd, qmail-queue, unix\_chkpwd and pwdb\_chkpwd.

Note, systems running an X server may have more SUID executables since X needs the elevated access afforded by SUID.

A file is only considered deleted when there are no more links pointing to it. This might sound like a strange concept, but consider that a filename like /usr/bin/perl is actually a link to the inode where the data is stored. Any number of links can point to the file, and until all of them are gone, the file still exists.

If users have access to a partition that is not mounted with `nosuid` or `noexec` (for example, if /tmp, /home, or /var/tmp are not separate partitions) take special care to ensure users do not create hard links to SUID or SGID binaries, so that after Portage updates they still have access to the old versions.

To check how many links a file has, you can use the stat command.

`user $``stat /bin/su`
File: \`/bin/su'
  Size: 29350           Blocks: 64         IO Block: 131072 regular file
Device: 900h/2304d      Inode: 2057419     Links: 1
Access: (4711/-rws--x--x)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2005-02-07 01:59:35.000000000 +0000
Modify: 2004-11-04 01:46:17.000000000 +0000
Change: 2004-11-04 01:46:17.000000000 +0000

To find the SUID and SGID files with multiple links, use find:

`user $``find / -type f \( -perm -004000 -o -perm -002000 \) -links +1 -ls`
