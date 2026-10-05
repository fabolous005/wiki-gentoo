<!-- source: https://wiki.gentoo.org/wiki/Samba/Samba_4_Migration | group: Gentoo Wiki (Main) | wiki-title: Samba/Samba 4 Migration -->
---
title: Samba/Samba 4 Migration
url: https://wiki.gentoo.org/wiki/Samba/Samba_4_Migration
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-09-25"
fingerprint: ee596953c4d67ca8
license: CC BY-SA 4.0
---

# Samba/Samba 4 Migration

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This guide introduces the migration of Samba 3 to Samba 4 with LDAP on Gentoo boxes.

## Pre-requisites

- A working samba 3.6.x NT PDC with LDAP backend(Must be PDC as it will be **Promoted** to AD)
- Samba AD DNS Planning
- Samba AD and DNS understanding
- LDAP Auth Backend Database (Optional)
- Python 2.7 as ABI
- Access Control List (ACL) on file system
- extended attributes (xattr) on file system

### Samba DNS Planning

- Moving from samba3 to samba AD is not easy due to the fact that the idea wasn't the same.
- Samba AD requires you to have a resolvable DNS.
- MS suggest using a FQDN as an AD Server as it is easily scalable in future.
- There are some suggestions to use suffixes of .local, .lan .corp but these are bad idea, very bad idea indeed, as we have no understanding what suffixes ICANN will use in future. And having a DNS with that suffix will conflict with the external DNS.

Thus we would hope that you use the following suggestion.

#### FQDN subdomain DNS setup

Example you own "company.com" and it is hosting by your web hosting company.

| FQDN | Description | 
|---|---|
| company.com. | main internet address point to your web server. | 
| www.company.com. | points to webserver ( Public IP Address, by your web hosting company) | 
| mail01.company.com. | points to mail server 1 ( Public IP Address, by your web hosting company ) | 
| mail02.company.com. | points to mail Server 2 ( Public IP Address, by your web hosting company ) | 
| smtp1.company.com. | points to smtp server 1 ( Public IP Address, by your web hosting company ) | 
| smtp2.company.com. | points to smtp server 2 ( Public IP Address, by your web hosting company ) | 
| smtp3.company.com. | points to smtp server 3 ( Public IP Address, by your web hosting company ) | 

Samba AD and internal subdomain DNS setup

| FQDN | Description | 
|---|---|
| headoffice.location1.company.com. | head office subdomain | 
| samba4-1.headoffice.location1.company.com. | Samba AD FQDN | 

in the above example:

NETBIOS NAME: HEADOFFICE

**So the most important setup.**

hostname = samba4-1.headoffice.company.com

AD = headoffice.company.com

REALM = HEADOFFICE.COMPANY.COM

DOMAINNAME ( NT Style )  COMPANY


**Benefits**

1. A clear cut on internal and external DNS.
2. There will not be any conflict between internal and external DNS.
3. In case if there are Branch Site, the Branch AD FQDN can be another subdomain: *samba4-2.branch\_CA.company.com.*
4. We can also make the subdomain public if needed and that makes this design future proof.

#### Samba AD DNS setup

Samba AD requires DNS Server to work, so if you have an DNS server running on the same server before, you will need to change it to support Samba or replace it to Samba internal DNS. The choice is yours.

If you would like to run 2 DNS server separately, it will be slightly wasteful as samba's internal DNS provides a DNS forwarding feature, just you need some tweaks.

There are 2 Options in Samba DNS setup.

- Samba Internal DNS : This is simplest and easier. Configuration using Microsoft DNS Management tools.
- Bind 9.8/9.8 Server : This is another choice that Samba team provides. If you are currently running Bind DNS, you might prefer this method.

### What is Samba AD

Samba AD has the following features

- Samba file Sharing (the most important)
- LDAP Server with MS Specified Schema which works as an Active Directory
- DNS Server which work together with Active Directory
- Kerberos Server

All 4 features need to run for a successfully deployed Active Directory. If you have been using LDAP Centralize Authentication and Management, you might want to run LDAP from a different virtual machine than current Samba AD until you have migrated all authentication to Samba AD.

Port Usage on the Samba AD[\[1\]](https://wiki.gentoo.org#cite_note-Samba_port_usage-1)[\[2\]](https://wiki.gentoo.org#cite_note-Microsoft_port_assignment-2)

| Port | Type | Description | 
|---|---|---|
| 53 | TCP/UDP | DNS Server(Bind/Samba) DNS Server port | 
| 88 | TCP/UDP | Kerberos krb5 | 
| 135 | TCP | Client/Server Communication | 
| 137 | UDP | Browsing requests of NetBIOS over TCP/IP | 
| 138 | UDP | Browsing datagram responses of NetBIOS over TCP/IP | 
| 139,445 445 | TCP UDP | Common Internet File System (CIFS) | 
| 389 | TCP/UDP | LDAP | 
| 464 | TCP/UDP | Kerberos kpasswd | 
| 636 | TCP/UDP | LDAP (SSL) | 
| 1024-5000 | TCP | Dynamic RPC Ports | 
| 3268 | TCP | Global Cataloge | 
| 3269 | TCP | Global Cataloge (SSL) | 
| 5353 | TCP/UDP | Multicast DNS | 

Have a look at the following documentation [Samba/Active Directory Guide](https://wiki.gentoo.org/wiki/Samba/Active_Directory_Guide)
You can choose any of the following:

- winbind from samba4
- nslcd/nss-pam-ldapd (a replacement of nss-pam and nss-ldap)
- sssd (must have mit-krb)

### Checking SambaSID for duplication

We will now check for SambaSID duplication
You can use the following code which is from the samba ClassUpgrade/HOWTO[\[3\]](https://wiki.gentoo.org#cite_note-classicupgrade-3)

### Checking Samba username and groupname for duplication

Unfortunately, there are no program for this. You will have to do it manually.

if you are using smbldap-tools, you can use the following command and manually compare their differences.

`root #````
smbldap-grouplist
```
`root #````
smbldap-userlist
```
Due to a bug in smbldap-tools, you probably need >=net-nds/smbldap-tools-0.9.9 <sup>[\[4\]](https://wiki.gentoo.org#cite_note-smbldaptoolsbug-4)</sup>.

### ACL and xattr support on your Files system

Samba 4 relies heavily on ACL and xattr because of the nature on how Windows controls the file sharing.

Running without these would be like running a windows share server on a FAT32 disk, where users other than owner and some file attributes would not be saved.


Due to the topic and it size, we will move it to another HOWTO.
[Filesystem/Access Control List Guide](https://wiki.gentoo.org/wiki/Filesystem/Access_Control_List_Guide)

## Getting Samba 4

### Getting ready before Samba 4

#### Emerge Samba

For more on Samba 4 bugs please have a look on the bugs tracker below.

[bug #489762](https://bugs.gentoo.org/show_bug.cgi?id=489762) Samba 4 unmask bugs tracker.


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

`root #``emerge --ask =net-fs/samba-4.1.2`
Please wait and allow samba 4 to build.

When it is done we can continue configuration Samba 4.

## Migration from Samba 3 to Samba 4 configuration

We will now prepared to do an classic upgrade from samba 3 to samba 4. Before that please run test-parm to make sure all configuration on the samba3 is correct. The migration will fail if it sees some outdated/obsolete config

`root #``testparm /etc/samba/smb.conf`
Load smb config files from /etc/samba/smb.conf
WARNING: The "password level" option is deprecated
Processing section "\[homes\]"
WARNING: The "username" option is deprecated

Change or remove any **WARNING** configuration.

We assume that both old and new server are on the same host. Else you can copy them to another host.


`root #````
scp -r /var/lib/samba/private/ /var/lib/samba/samba3tdb/
```
`root #````
scp /etc/samba/smb.conf /var/lib/samba/samba3tdb/
```
Your New host should also have a basic configure like below. Samba-tools migratation requires talking to the ldap server to get your existing information.

**`/etc/openldap/ldap.conf`**

If you wish to change the host name of the new server, you can change the netbios name in the Samba3 conf file that you have copied over.


### Start migration

Now it is time to stop samba.

`root #``/etc/init.d/samba stop`
And run your migratation. Choose one of the following for different DNS configuration. You can change it later after running.

Samba 4 AD with Bind9 DLZ Module DLZ is for windows client to do dns update to bind9

`root #``samba-tool domain classicupgrade --dbdir=/var/lib/samba/samba3tdb/  --use-xattrs=yes  --realm=headoffice.location1.company.com --dns-backend=BIND9_DLZ /var/lib/samba/samba3tdb/samba3.conf`
Samba 4 AD with internal DNS Module

`root #``samba-tool domain classicupgrade --dbdir=/var/lib/samba/samba3tdb/  --use-xattrs=yes  --realm=headoffice.location1.company.com /var/lib/samba/samba3tdb/samba3.conf`
You should not see any error message from this command. If you do you will have to re-run the above command again.

### Testing of Migration

As said above, Samba AD provides LDAP and DNS, so you have to turn off your slapd and also other DNS if you are using samba internal DNS.

`root #``samba -i -M single``root #``samba-tool user setpassword Administrator`
Smbclient test

`root #``smbclient //localhost/netlogon -UAdministrator -c 'ls'`
Domain=\[SAMDOM\] OS=\[Unix\] Server=\[Samba 4.x.y\]
  .                                   D        0  Tue Dec 11 20:00:00 2012
  ..                                  D        0  Tue Dec 11 20:00:00 2012

### Internal Samba DNS setup

If you want to forward DNS to 8.8.8.8 (Google) for all PC clients to the Internet, please setup the following in your new /etc/samba/smb.conf

Also you should also allow dns update from Windows clients to samba DNS.

**`/etc/samba/smb.conf`**

**add this line to forward dns**

### Bind DNS DLZ module setup

Having Bind DNZ DLZ working with Samba 4 AD is somewhat simple, but still we need to do the following.

1. Change of Samba DLZ Module version.
2. Change the permission on /var/lib/samba/private/ so that named can access
3. Change /etc/bind/named.conf to include the files inside /var/lib/samba/private/

#### Change of /var/lib/samba/private/named.conf

Uncomment the proper bind module according to the bind version you have.

`root #``named -v`
BIND 9.9.3-P2 (Extended Support Version)

**`/var/lib/samba/private/named.conf`**

#### Getting permission on /var/lib/samba/private/

This is tricky but not hard to do, still doubt arises about its security where named needs to access samba private folder, which is default 400.

We will try to use ACL to make life simple (since it is required by default for samba4)

`root #````
setfacl -m u:named:rx /var/lib/samba/private/
```
`root #````
setfacl -m u:named:rx /var/lib/samba/private/named.*
```
`root #````
setfacl -m u:named:rwx -R /var/lib/samba/private/dns
```
`root #````
setfacl -m u:named:rx /var/lib/samba/private/dns*
```
#### Change of /etc/bind/named.conf

We now need to make 2 changes in named.conf


1\. Samba gssapi keytab 
If you follow [BIND/Guide](https://wiki.gentoo.org/wiki/BIND/Guide) put it under "options section"

**`/etc/bind/named.conf`**

2\. Samba4 AD DLZ
If you follow [BIND/Guide](https://wiki.gentoo.org/wiki/BIND/Guide) put it under "Internal view/External view"

According to your AD design.

**`/etc/bind/named.conf`**

You are now done; reload bind.

### Sub function test

As said before, samba include DNS, ldap and kerberos in a full AD environment.

We will need to do a full test it now.

Before that we need to change our /etc/resolv.conf so that it is using samba DNS. Krb and ldap don't work fully without that.

Set nameserver ip to your new samba server ip. e.g 192.168.0.10.

**`/etc/resolv.conf`**

**change to the following line**

#### DNS sub function test

Your result on the following dns query should be identical or very similar.

`root #``host -t SRV _kerberos._udp.headoffice.company.com.`
\_kerberos.\_udp.headoffice.company.com has SRV record 0 100 88 samba4-1.headoffice.company.com.

`root #``host -t SRV _ldap._tcp.headoffice.company.com.`
\_ldap.\_tcp.headoffice.company.com has SRV record 0 100 389 samba4-1.headoffice.company.com.

`root #``host -t A samba4-1.headoffice.company.com.`
samba4-1.headoffice.company.com has address 192.168.0.10

#### Kerberos Test

There shouldn't be any errors when you get the initial TGT (Ticket Granting Ticket).

`root #``kinit administrator@HEADOFFICE.COMPANY.COM`
Password:

`root #``klist````
Ticket cache: FILE:/tmp/krb5cc_0
Default principal: administrator@HEADOFFICE.COMPANY.COM
Valid starting       Expires              Service principal
02/05/2014 03:15:46  02/05/2014 13:15:46  krbtgt/HEADOFFICE.COMPANY.COM@HEADOFFICE.COMPANY.COM
        renew until 02/06/2014 03:15:41
```
### Final setup

Finally we will need to make the following change

**`/etc/conf.d/samba`**

**Changing to samba4 deamon**

## FAQ

**1. Where are my shares after the new migration?**

A: They will not be migrated. According to samba design you should have a clean DC (No user shares).
You shares should be done by a domain member and you will have to manually move you old share over.

However, you should keep your configurable share as simple as possible and use xAttrs/ACLs to set appropriate permissions.

E.g.


**2. My netlogon is not working...**

A: Netlogon script setting from ldap is not being migrated when you move from samba3 PDC to Samba4.

Why? You are advised to use GPO Drive mapping, which in most cases makes the deployment simpler.

Some Examples:


- Map Network Shared Drive in Group Policy<sup>[\[6\]](https://wiki.gentoo.org#cite_note-Map_Network_Shared_Drive_in_Group_Policy-6)</sup>
- Using Group Policy Preferences to Map Drives Based on Group Membership<sup>[\[7\]](https://wiki.gentoo.org#cite_note-Using_Group_Policy_Preferences_to_Map_Drives_Based_on_Group_Membership-7)</sup>
- Windows XP Drive Maps GPO not working<sup>[\[8\]](https://wiki.gentoo.org#cite_note-Windows_XP_Drive_Maps_GPO_not_working-8)</sup>

But still if you still want to use netlogon script, you can copy all your existing scripts to the \[netlogon\] path above.

Open the AD Users profiles under setting and put the script file name in (without path).

e.g: netlog.bat

You can do it for all users by selecting them all together.


**3. My \[homes\] is not accessible, and I have not changed anything.**

A: a basic \[homes\] share can be as simple as below.

However something is missing, how would the system know your home path?

By default it uses path is read by /etc/nsswitch.conf but as you know our user information is in Samba AD so we can configure nss to winbind, nslcd or sssd.

We will use winbind in this example as it doesn't require an extra package or installation.

But strongly recommend that you have a look on nslcd setup on the link below if you don't have samba on your other linux systems.

You would have to configure /etc/nsswitch.conf and add winbind to be like the following:

**`/etc/nsswitch.conf`**

For more about PAM support winbind, please check on the docs below.
[\[9\]](https://wiki.gentoo.org#cite_note-Samba4.2FWinbind-9)

With this configuration the system will be able to know the user and group. Use these commands to verify:

`root #````
getent passwd
```
`root #````
getent group
```
You might see some strange entry on the password where the path are /home/COMPANY/username which is the cause of why your \[homes\] don't work. So just tweak smb.conf a bit on winbind under \[global\] but before any share

**`/etc/samba/smb.conf`**

Restart Samba and run the above command again...
