<!-- source: https://wiki.gentoo.org/wiki/Samba/Active_Directory_Guide | group: Gentoo Wiki (Main) | wiki-title: Samba/Active Directory Guide -->
---
title: Samba/Active Directory Guide
url: https://wiki.gentoo.org/wiki/Samba/Active_Directory_Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-09-25"
fingerprint: d21cff533d118984
license: CC BY-SA 4.0
---

# Samba/Active Directory Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**April 07th, 2015**, the information in this article is probably

**outdated**. You can help the Gentoo community by verifying and

[updating this article](https://wiki.gentoo.org/index.php?title=Samba/Active_Directory_Guide&action=edit).

## Centralized authentication with Samba/Win AD

This might look a bit weird at 1st but when working on the migration from samba 3 with LDAP to samba 4 AD.

This seem to be the only choice we have as we have to remove the LDAP Server on the server that running Samba 4 AD.


Else you would have 2 server.

Windows Client using Samba 4 AD and Linux client using an LDAP Server from another which is no longer centralized and defeated the purpose.

## Working method and choice

There are a few method.

1. nslcd or nss\_pam\_ldapd
2. sssd

## nslcd or nss\_pam\_ldapd

If you are using 64 bit system, you will need to unmask it.

**`/etc/portage/package.accept_keywords`**

This package will provide what is currently provide by nss\_ldap and also nss\_pam thus the 2 package have to be removed.

`root #``emerge --ask --depclean --verbose nss_ldap nss_pam`
Now we can start install nss\_pam\_ldapd:

`root #``emerge --ask nss_pam_ldapd`
## Configuration

There are at least 2 method to work on this solution, the result are same but the way of working it are different.

Pick one...

nss-pam-ldapd Setup[\[1\]](https://wiki.gentoo.org#cite_note-nss-pam-ldapd_setup-1)

Samba Wiki:Local\_user\_management\_and\_authentication/nslcd[\[2\]](https://wiki.gentoo.org#cite_note-Local_user_management_and_authentication.2Fnslcd-2)

### Method 1: Connecting to AD via LDAP Bind DN and password

This method will configure /etc/nslcd.conf to make LDAP binding via an AD account. Communication with AD with this setup is unencrypted, unless your AD and nslcd had setup LDAP over SSL.

Please create a new user with username **nslcdconnect** and password **secret** in the AD Server.

You will need to do the following:

- *Enable - disable user change password on next logon*
- *Disable - user change password*
- *Enable - Password never expired*.

Assuming that:


- Samba AD is running locally and accessible via 127.0.0.1
- LDAP Base DN is dc=headoffice,dc=location1,dc=company,dc=com

**`/etc/nslcd.conf`**

**Change some line below**

### Method 2: Connecting to AD via Kerberos

This method are very similar with the 1st method specially in the configuration you will still need to change the configure /etc/nslcd.conf to make LDAP connection to an AD Server with the help of Kerberos. But you don't need to specified a bind account and also the communication with AD with this setup is encrypted.

Please create a new user with username **nslcdconnect** and password **secret** in the AD Server.

You will need to do the following:

- *Enable - disable user change password on next logon*
- *Disable - user change password*
- *Enable - Password never expired*.

Assuming that:


- Samba is running locally and accessible via 127.0.0.1
- LDAP Base DN is dc=headoffice,dc=location1,dc=company,dc=com
- /etc/hosts and also /etc/conf.d/hostname have the same result with your Samba AD DNS (There will be problem if there are not the same of cannot resolve).
- hostname = samba4-1.headoffice.company.com
- AD = headoffice.company.com
- REALM = HEADOFFICE.COMPANY.COM
- DOMAINNAME ( NT Style ) COMPANY
- Already have Kerberos install either mit-krb5 or heimdal.

`root #``samba-tool spn add nslcd/samba4-1.headoffice.company.com nslcdconnect`
Now we should export the keytab from AD server for user nslcdconnect. With this keytab we can connect via Kerberos without the need of key in the password for nslcdconnect if configure correctly.

`root #````
samba-tool domain exportkeytab /etc/krb5.nslcd.keytab --principal=nslcdconnect
```
`root #````
chown nslcd:nslcd /etc/krb5.nslcd.keytab 
```
`root #````
chmod 600 /etc/krb5.nslcd.keytab
```
The command below will kept nslcdconnect to the AD server via kerberos using keytab. You will need [app-crypt/kstart](https://packages.gentoo.org/packages/app-crypt/kstart) so the Kerberos ticket and key will be automatically renew when it expired or needed.

`root #``emerge --ask app-crypt/kstart``root #``k5start -f /etc/krb5.nslcd.keytab -U -o nslcd -K 360 -b -k /run/nslcd/nslcd.tkt`
Now we can change our nslcd.conf to suit Kerberos setup.

**`/etc/nslcd.conf`**

**Change some line below**

## Editing /etc/init.d/nslcd to start k5start together

We need to make some change to start k5strt with nslcd so the kerberos ticket will work.

**`/etc/init.d/nslcd`**

**Add the 2 k5start line below.**

```
#!/sbin/openrc-run
# Copyright 1999-2013 Gentoo Foundation
# Distributed under the terms of the GNU General Public License v2
# $Header: /var/cvsroot/gentoo-x86/sys-auth/nss-pam-ldapd/files/nslcd-init,v 1.2 2013/02/07 18:11:37 prometheanfire Exp $
extra_commands="checkconfig"
cfg="/etc/nslcd.conf"
depend() {
        need net
        use dns logger
}
checkconfig() {
        if [ ! -f "$cfg" ] ; then
                eerror "Please create $cfg"
                eerror "Example config: /usr/share/nss-ldapd/nslcd.conf"
                return 1
        fi
        return 0
}
start() {
    checkpath -q -d /var/run/nslcd -o nslcd:nslcd
        checkconfig || return $?
        ebegin "Starting nslcd"
        start-stop-daemon --start --pidfile /var/run/nslcd/nslcd.pid \
                --exec /usr/sbin/nslcd
        start-stop-daemon --start --pidfile /var/run/nslcd/nslcd.k5start.pid \
                --exec /usr/bin/k5start -- -f /etc/krb5.nslcd.keytab -U -o nslcd -K 360 -b -k /var/run/nslcd/nslcd.tkt -p /var/run/nslcd/nslcd.k5start.pid
        eend $? "Failed to start nslcd"
}
stop() {
        ebegin "Stopping nslcd"
        start-stop-daemon --stop --pidfile /var/run/nslcd/nslcd.pid
        start-stop-daemon --stop --pidfile /var/run/nslcd/nslcd.k5start.pid
        eend $? "Failed to stop nslcd"
```


## nssswitch.conf connfiguration

You will need to edit your /etc/nsswitch.conf according to the following. This meant that nsswitch will use the new nss-pam-ldapd module.

**`/etc/nsswitch.conf`**

## Executing

We can now start nslcd daemon

`root #``/etc/init.d/nslcd start`
to check if our Samba is working fine with our local host use these to verify:

`root #````
getent passwd
```
`root #````
getent group
```
You should see your Users or Groups which have unit UID or GID.

If you don't have it. check your /etc/nslcd.conf again.

You can now add nslcd using rc-update

`root #````
rc-update add nslcd default
```
