<!-- source: https://wiki.gentoo.org/wiki/Vsftpd/AD_Authentication | group: Gentoo Wiki (Main) | wiki-title: Vsftpd/AD Authentication -->
---
title: vsftpd/AD Authentication
url: https://wiki.gentoo.org/wiki/Vsftpd/AD_Authentication
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2016-04-08"
fingerprint: ee01295cd986a8c6
license: CC BY-SA 4.0
---

# vsftpd/AD Authentication

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**vsftpd** (**V**ery **S**ecure **FTP D**aemon) is a major FTP server. 

**pam** (**P**luggable **A**uthentication **M**odules for linux) is a system of libraries that handle the authentication tasks of applications (services) on the system.

**winbind**. Name Service Switch daemon for resolving names from NT servers

## Preamble

This article HOWTO describes possibility to authenticate domain users to access FTP server based on linux daemon. This HOWTO checked-out on Active Directory with 200K+ domain users. Good luck!

## Installation

### Vsftpd

#### Vsftpd USE Flags


| [pam](https://packages.gentoo.org/useflags/pam) | Add support for PAM (Pluggable Authentication Modules) - DANGEROUS to arbitrarily flip | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [ssl](https://packages.gentoo.org/useflags/ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 
| [tcpd](https://packages.gentoo.org/useflags/tcpd) | Add support for TCP wrappers | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

We should enable a **pam tcpd caps** and, optionally, **ssl** (for security reasons) use flags:

`root #``echo "net-ftp/vsftpd pam tcpd caps ssl" > /etc/portage/package.use/vsftpd`
#### Install vsftpd

Install [net-ftp/vsftpd](https://packages.gentoo.org/packages/net-ftp/vsftpd):

`root #``emerge --ask net-ftp/vsftpd`
### Samba

#### Samba USE Flags


| [+regedit](https://packages.gentoo.org/useflags/+regedit) | Enable support for regedit command-line tool | 
| [+system-mitkrb5](https://packages.gentoo.org/useflags/+system-mitkrb5) | Use app-crypt/mit-krb5 instead of app-crypt/heimdal. | 
| [acl](https://packages.gentoo.org/useflags/acl) | Add support for Access Control Lists | 
| [addc](https://packages.gentoo.org/useflags/addc) | Enable Active Directory Domain Controller support | 
| [ads](https://packages.gentoo.org/useflags/ads) | Enable Active Directory support | 
| [ceph](https://packages.gentoo.org/useflags/ceph) | Enable support for Ceph distributed filesystem via sys-cluster/ceph | 
| [client](https://packages.gentoo.org/useflags/client) | Enables the client part | 
| [cluster](https://packages.gentoo.org/useflags/cluster) | Enable support for clustering | 
| [cups](https://packages.gentoo.org/useflags/cups) | Add support for CUPS (Common Unix Printing System) | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [fam](https://packages.gentoo.org/useflags/fam) | Enable FAM (File Alteration Monitor) support | 
| [glusterfs](https://packages.gentoo.org/useflags/glusterfs) | Enable support for Glusterfs filesystem via sys-cluster/glusterfs | 
| [gpg](https://packages.gentoo.org/useflags/gpg) | Use app-crypt/gpgme for AD DC | 
| [iprint](https://packages.gentoo.org/useflags/iprint) | Enabling iPrint technology by Novell | 
| [json](https://packages.gentoo.org/useflags/json) | Enable json audit support through dev-libs/jansson | 
| [ldap](https://packages.gentoo.org/useflags/ldap) | Add LDAP support (Lightweight Directory Access Protocol) | 
| [llvm-libunwind](https://packages.gentoo.org/useflags/llvm-libunwind) | Use llvm-runtimes/libunwind instead of sys-libs/libunwind | 
| [lmdb](https://packages.gentoo.org/useflags/lmdb) | Enable LMDB backend for bundled ldb | 
| [pam](https://packages.gentoo.org/useflags/pam) | Add support for PAM (Pluggable Authentication Modules) - DANGEROUS to arbitrarily flip | 
| [profiling-data](https://packages.gentoo.org/useflags/profiling-data) | Enables support for collecting profiling data | 
| [python](https://packages.gentoo.org/useflags/python) | Add optional support/bindings for the Python language | 
| [quota](https://packages.gentoo.org/useflags/quota) | Enables support for user quotas | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [snapper](https://packages.gentoo.org/useflags/snapper) | Enable vfs\_snapper module (requires sys-apps/dbus) | 
| [spotlight](https://packages.gentoo.org/useflags/spotlight) | Enable support for spotlight backend | 
| [syslog](https://packages.gentoo.org/useflags/syslog) | Enable support for syslog | 
| [system-heimdal](https://packages.gentoo.org/useflags/system-heimdal) | Use app-crypt/heimdal instead of bundled heimdal. | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [unwind](https://packages.gentoo.org/useflags/unwind) | Enable libunwind usage for backtraces | 
| [winbind](https://packages.gentoo.org/useflags/winbind) | Enables support for the winbind auth daemon | 
| [zeroconf](https://packages.gentoo.org/useflags/zeroconf) | Support for DNS Service Discovery (DNS-SD) | 

We should enable a **ads** use flag

`root #``echo "net-fs/samba ads" > /etc/portage/package.use/samba`
#### Install samba

Install [net-fs/samba](https://packages.gentoo.org/packages/net-fs/samba):

`root #``emerge --ask net-fs/samba`
## Configuration

### /etc/krb5.conf

Note: parameters are case-sensitive

**`/etc/krb5.conf`**

### /etc/vsftpd/vsftpd.conf

FTP-Server will authenticate users in Microsoft Active Directory via pam + winbind.

**`/etc/vsftpd/vsftpd.conf`**

#### Chroot to user's home directory

Note: If you want to chroot all users to one fixed directory, just add the following to your /etc/vsftpd/vsftpd.conf:

local\_root=/var/ftp

#### SECCOMP Filtering and 64-bit Kernels with =net-ftp/vsftpd-3.0.x

Note: If running an amd64 kernel, you will need to add the following to your /etc/vsftpd/vsftpd.conf:

seccomp\_sandbox=NO

If the above change is not added, the following error may occur on the client side: Fatal error:
500 OOPS: priv\_sock\_get\_cmd
For further information, refer to [https://bugzilla.redhat.com/show\_bug.cgi?id=845980](https://bugzilla.redhat.com/show_bug.cgi?id=845980).

### /etc/samba/smb.conf

Note: parameters in file are case-sensitive!

**`/etc/samba/smb.conf`**

#### Samba localization

Note: If using samba in localized network, just add following to your /etc/samba/smb.conf (change codepage to yours):

dos charset = cp866

#### pam configuration

**`/etc/pam d/ftp`**

**`/etc/pam.d/vsftpd-winbind`**

#### Winbind service

Making winbindd daemon to start with samba service. Just change following string in /etc/conf.d/samba:

daemon\_list="smbd winbind"

### OpenRC

`root #````
rc-update add samba default
```
`root #````
/etc/init.d/samba start
```
`root #````
rc-update add vsftpd default
```
`root #``/etc/init.d/vsftpd start`
### systemd

`root #````
systemctl enable smbd
```
`root #````
systemctl start smbd
```
`root #````
systemctl enable winbindd
```
`root #````
systemctl start winbindd
```
`root #````
systemctl enable vsftpd
```
`root #````
systemctl start vsftpd
```
## Joining samba to Windows Domain

**user@corp.domain.com** should have permittions to join computers in Windows Domain

`root #``net ads join user@corp.domain.com`
Enter password for user.

## User Home Directories

By default, user will have /home/CORP/%user as home directory. To change this directory, you need to change attribute **unixHomeDirectory** for user in Microsoft AD Users and Computers
