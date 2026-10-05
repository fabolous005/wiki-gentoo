<!-- source: https://wiki.gentoo.org/wiki/Vsftpd | group: Gentoo Wiki (Main) | wiki-title: Vsftpd -->
---
title: vsftpd
url: https://wiki.gentoo.org/wiki/Vsftpd
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-07"
fingerprint: fa20105e51c7dbe6
license: CC BY-SA 4.0
---

# vsftpd

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**vsftpd** (**V**ery **S**ecure **FTP D**aemon) is an FTP server for UNIX-like systems.

## Installation

### USE flags


| [pam](https://packages.gentoo.org/useflags/pam) | Add support for PAM (Pluggable Authentication Modules) - DANGEROUS to arbitrarily flip | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [ssl](https://packages.gentoo.org/useflags/ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 
| [tcpd](https://packages.gentoo.org/useflags/tcpd) | Add support for TCP wrappers | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

### Emerge

`root #``emerge --ask net-ftp/vsftpd`
## Configuration

You can find an example configuration file and documentation in the `/usr/share/doc/vsftpd-3.0.5-r2` directory. Note that you can't run the server listening at both IPv4 and IPv6. To run multiple listening non-inetd instances of vsftpd, create appropriate configuration files in `/etc/vsftpd/`, symlink *\<name>*.conf`/etc/init.d/vsftpd` to `/etc/init.d/vsftpd.` and start the newly created cervices.
*\<name>*

#### Anonymous read access

**`/etc/vsftpd.conf`**

#### Anonymous read/write access

`root #``chown ftp /home/ftp`
**`/etc/vsftpd.conf`**

## Service

### OpenRC

`root #````
rc-update add vsftpd default
```
`root #````
/etc/init.d/vsftpd start
```
### systemd

`root #````
systemctl enable vsftpd
```
`root #````
systemctl start vsftpd
```
## Troubleshooting

### seccomp filter sanboxing

Following error might show using ftp clients with vsftpd 3.0.x version:

500 OOPS: priv\_sock\_get\_cmd

This is caused by [seccomp filter sanboxing](https://en.wikipedia.org/wiki/Seccomp), and enabled by default on **amd64**. To workaround this issue, disable seccomp filter sanboxing:

**`/etc/vsftpd/vsftpd.conf`**

For further information, refer to Red Hat [bug #845980](https://bugzilla.redhat.com/show_bug.cgi?id=845980).
