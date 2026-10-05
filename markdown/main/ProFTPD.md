<!-- source: https://wiki.gentoo.org/wiki/ProFTPD | group: Gentoo Wiki (Main) | wiki-title: ProFTPD -->
---
title: ProFTPD
url: https://wiki.gentoo.org/wiki/ProFTPD
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2019-07-18"
fingerprint: fe032a5c498639c4
license: CC BY-SA 4.0
---

# ProFTPD

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

ProFTPD is a highly configurable FTP server.

## Installation

### USE flags


| [+caps](https://packages.gentoo.org/useflags/+caps) | Use Linux capabilities library to control privilege | 
| [+pcre](https://packages.gentoo.org/useflags/+pcre) | Add support for Perl Compatible Regular Expressions | 
| [acl](https://packages.gentoo.org/useflags/acl) | Add support for Access Control Lists | 
| [authfile](https://packages.gentoo.org/useflags/authfile) | Enable support for the auth-file module | 
| [ban](https://packages.gentoo.org/useflags/ban) | Enable support for the mod\_ban module | 
| [case](https://packages.gentoo.org/useflags/case) | Enable support for the mod\_case module | 
| [clamav](https://packages.gentoo.org/useflags/clamav) | Add support for Clam AntiVirus software (usually with a plugin) | 
| [copy](https://packages.gentoo.org/useflags/copy) | Enable support for the mod\_copy module | 
| [ctrls](https://packages.gentoo.org/useflags/ctrls) | Enable support for the mod\_ctrls and mod\_ctrls\_admin modules | 
| [deflate](https://packages.gentoo.org/useflags/deflate) | Enable support for the mod\_deflate module | 
| [diskuse](https://packages.gentoo.org/useflags/diskuse) | Enable support for the mod\_diskuse module | 
| [dso](https://packages.gentoo.org/useflags/dso) | Enable support for the mod\_dso module | 
| [dynmasq](https://packages.gentoo.org/useflags/dynmasq) | Enable support for the mod\_dynmasq module, for dynamically updating MasqueradeAddress for dyndns-like scenarios) | 
| [exec](https://packages.gentoo.org/useflags/exec) | Enable support for the mod\_exec module. WARNING: this could be a security risk | 
| [ident](https://packages.gentoo.org/useflags/ident) | Enable support for the mod\_ident module | 
| [ifsession](https://packages.gentoo.org/useflags/ifsession) | Enable support for the ifsession module | 
| [ifversion](https://packages.gentoo.org/useflags/ifversion) | Enable support for the mod\_ifversion module | 
| [kerberos](https://packages.gentoo.org/useflags/kerberos) | Add kerberos support | 
| [ldap](https://packages.gentoo.org/useflags/ldap) | Add LDAP support (Lightweight Directory Access Protocol) | 
| [log-forensic](https://packages.gentoo.org/useflags/log-forensic) | Enable support for the mod\_log\_forensic module, log only suspicious actions. | 
| [memcache](https://packages.gentoo.org/useflags/memcache) | Enable support for the mod\_memcache module, for using memcached servers | 
| [msg](https://packages.gentoo.org/useflags/msg) | Enable support for the mod\_msg module, allows system users to send messages to connected clients via the ftpdctl program. | 
| [mysql](https://packages.gentoo.org/useflags/mysql) | Add mySQL Database support | 
| [ncurses](https://packages.gentoo.org/useflags/ncurses) | Add ncurses support (console display library) | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [pam](https://packages.gentoo.org/useflags/pam) | Add support for PAM (Pluggable Authentication Modules) - DANGEROUS to arbitrarily flip | 
| [postgres](https://packages.gentoo.org/useflags/postgres) | Add support for the postgresql database | 
| [qos](https://packages.gentoo.org/useflags/qos) | Enable support for the mod\_qos module | 
| [radius](https://packages.gentoo.org/useflags/radius) | Add support for RADIUS authentication | 
| [ratio](https://packages.gentoo.org/useflags/ratio) | Enable support for the mod\_ratio module | 
| [readme](https://packages.gentoo.org/useflags/readme) | Enable support for the mod\_readme module | 
| [rewrite](https://packages.gentoo.org/useflags/rewrite) | Enable support for the rewrite module | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [sftp](https://packages.gentoo.org/useflags/sftp) | Enable support for the mod\_sftp module and optionally mod\_sftp\_sql and mod\_sftp\_pam if matching USE flags are enabled | 
| [shaper](https://packages.gentoo.org/useflags/shaper) | Enable support for the mod\_shaper module | 
| [sitemisc](https://packages.gentoo.org/useflags/sitemisc) | Enable support for the sitemisc module | 
| [snmp](https://packages.gentoo.org/useflags/snmp) | Add support for the Simple Network Management Protocol if available | 
| [sodium](https://packages.gentoo.org/useflags/sodium) | Use dev-libs/libsodium for password encryption an key exchange | 
| [softquota](https://packages.gentoo.org/useflags/softquota) | Enable support for the quotatab module | 
| [sqlite](https://packages.gentoo.org/useflags/sqlite) | Add support for sqlite - embedded sql database | 
| [ssl](https://packages.gentoo.org/useflags/ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 
| [tcpd](https://packages.gentoo.org/useflags/tcpd) | Add support for TCP wrappers | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [unique-id](https://packages.gentoo.org/useflags/unique-id) | Enable support for the mod\_unique\_id module, every connection gets unique ID. | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 
| [vroot](https://packages.gentoo.org/useflags/vroot) | Enable support for the virtual root module | 

### Emerge

`root #``emerge --ask net-ftp/proftpd`
## Configuration

### Anonymous read access

Anonymous read only access requires `-acl` USE flag set.  Permissions on /home/ftp require chmod 555 ran on the directory.

**`/etc/proftpd/proftpd.conf`**

## Service

### OpenRC

To start ProFTPD on boot:

`root #``rc-update add proftpd default`
To start ProFTPD now:

`root #``rc-service proftpd start`
### systemd

To start ProFTPD on boot:

`root #``systemctl enable proftpd`
To start ProFTPD now:

`root #``systemctl start proftpd`
