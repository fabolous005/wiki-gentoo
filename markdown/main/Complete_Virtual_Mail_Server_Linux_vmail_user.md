<!-- source: https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/Linux_vmail_user | group: Gentoo Wiki (Main) | wiki-title: Complete Virtual Mail Server/Linux vmail user -->
---
title: Complete Virtual Mail Server/Linux vmail user
url: https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/Linux_vmail_user
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-07-16"
fingerprint: fc9e0b1695bd8d36
license: CC BY-SA 4.0
---

# Complete Virtual Mail Server/Linux vmail user

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## The vmail user

Because valid UNIX user and group id's are needed to store the mailboxes, those should be created as well:

- Most services get a system ID under 1000.
- In Gentoo the user ID's start at 1000.
- An ID of 5000 is chosen for the vmail user.

If there are hundreds of shell users on the system, a different ID can be used as well. For the vmail group the same is done. This will not be a shell account for anybody to log in with.

`root #````
groupadd -g 5000 vmail
```
`root #``useradd -m -d /var/vmail -s /bin/false -u 5000 -g vmail vmail`
## Storage space

Next to think about is the mail storage. This can be a partition, an NFS share or any ordinary sub-directory. Here /var/vmail is chosen as noted above and created as a 32GiB raid10 partition. Wherever it is chosen to be stored, ownership should be changed appropriately.

`root #``chown vmail:vmail /var/vmail/`
Also permissions should be set up properly:

`root #``chmod 2770 /var/vmail/`
Check the permissions to make sure that there will not be any permission error later:

`root #``ls -ld /var/vmail`
drwxrws--- 3 vmail vmail 4096 Aug  2 07:24 /var/vmail

## Vmail user and Postfix

Postfix needs to know where and under what ownership to store mail.

**`/etc/postfix/main.cf`**

**Binding UID and GID's to postfix**

```
# Link the mailbox uid and gid to postfix.
virtual_uid_maps = static:5000
virtual_gid_maps = static:5000
 
# Set the base address for all virtual mailboxes
virtual_mailbox_base = /var/vmail
```
