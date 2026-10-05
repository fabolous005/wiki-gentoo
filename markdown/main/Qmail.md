<!-- source: https://wiki.gentoo.org/wiki/Qmail | group: Gentoo Wiki (Main) | wiki-title: Qmail -->
---
title: qmail
url: https://wiki.gentoo.org/wiki/Qmail
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2020-01-06"
fingerprint: "7e8411ca98363ae1"
license: CC BY-SA 4.0
---

# qmail

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**qmail** is a fast, popular [Mail Transfer Agent](https://wiki.gentoo.org/wiki/Category:Mail_Transfer_Agents) (MTA).

## Pre-installation

As only one MTA can be installed at the same time on a system, it might be required to deselect an installed MTA. The package manager will report a block when another MTA is still installed. For example, a previously selected [mail-mta/ssmtp](https://packages.gentoo.org/packages/mail-mta/ssmtp) can be marked as removable with this command:

`root #``emerge --deselect mail-mta/ssmtp`
## Installation

[mail-mta/netqmail](https://packages.gentoo.org/packages/mail-mta/netqmail) has several USE flags that may be desired for certain bigger setups. As this article aims at installing and configuring a *basic* netqmail setup, adding qmail plugin support with qmail-spp and ucspi-tcp support is necessary.

`root #````
echo "mail-mta/netqmail qmail-spp" >> /etc/portage/package.use
```
`root #``echo "sys-apps/ucspi-tcp qmail-spp" >> /etc/portage/package.use``root #``emerge --ask netqmail`
## Configuration

The default 16 MiB of memory for qmail is a little sparse. Update the memory to 32 MiB to avoid memory related errors.

`root #``sed -i 's/16000000/32000000/' /var/qmail/control/conf-common``root #``emerge --ask --config netqmail`
#### Setting up non-root account for mail

The design of qmail has been completely around the focus of security. To this end, e-mail is never sent to the user *root*. Select a user on the machine to receive mail that would normally be destined for *root*. The remainder of this guide will that user as *myusername*.

**`/var/qmail/alias/.qmail-root`**

**qmail-root**

**`/var/qmail/alias/.qmail-postmaster`**

**qmail-postmaster**

**`/var/qmail/alias/.qmail-mailer-daemon`**

**qmail-mailer-daemon**

Or to send this email elsewhere, simply put the full address in:

**`/var/qmail/alias/.qmail-root`**

**qmail-root**

**`/var/qmail/alias/.qmail-postmaster`**

**qmail-postmaster**

**`/var/qmail/alias/.qmail-mailer-daemon`**

**qmail-mailer-daemon**

#### Fully Qualified Domain Name (FQDN)

Though not entirely related, for an MTA to function properly, it is imperative that its hostname is set up correctly. Under Gentoo /etc/conf.d/hostname and /etc/conf.d/net are the files responsible for this. In this example, the mail server is named *foo* on the domain *example.com*.

**`/etc/conf.d/net`**

**Setup domain name**

```
dns_domain_lo="example.com"
```
**`/etc/conf.d/hostname`**

**Setup hostname**

```
hostname="foo"
```
Verifying that the FQDN is setup properly for the domain.

#### Files for a 2nd level domain

`user $``cd /var/qmail/control/``user $``hostname --fqdn``user $``cat me``user $``cat defaultdomain``user $``cat plusdomain``user $``cat locals``user $``cat rcpthosts`
#### Files for a 3rd level domain

`user $``cd /var/qmail/control/``user $``hostname --fqdn``user $``cat me``user $``cat defaultdomain``user $``cat plusdomain``user $``cat locals``user $``cat rcpthosts`
### Creating Properly Signed Certificates

Move to the qmail control directory:

`root #``cd /var/qmail/control/`
Upgrade the Cert Info to create a 2048bit key:

`root #``sed -i 's/1024/2048/' /var/qmail/control/servercert.cnf`
Update the Cert Info with information pertinent to this host. CN is the fully qualified domain name ie. foo.domain.com:

**`/var/qmail/control/servercert.cnf`**

**Be certain this is the correct CN**

create the pem files and key:

`root #``openssl req -new -nodes -out req.pem -config /var/qmail/control/servercert.cnf  -keyout /var/qmail/control/servercert.pem`
Get the contents of the request pem file:

`root #``cat /var/qmail/control/req.pem`
Send req.pem to a CA(ie godaddy/Starfield, Versign, etc.) to obtain signed\_req.pem:

`root #````
cat myserver.domain.com.crt  sf_bundle.crt >> servercert.pem
```
`root #``awk '/BEGIN PRIVATE KEY/,/END PRIVATE KEY/' servercert.pem > myserver.domain.com.key`
Alternatively, obtain a key from [Let's\_Encrypt](https://wiki.gentoo.org/wiki/Let%27s_Encrypt)

### Start qmail and add it to the default run level

Run the init scripts and setup supervisor links for qmail:

`root #````
ln -s /var/qmail/supervise/qmail-send /service/qmail-send
```
`root #````
ln -s /var/qmail/supervise/qmail-smtpd /service/qmail-smtpd
```
start and add netqmail to the default run level:

`root #````
/etc/init.d/svscan start
```
`root #````
rc-update add svscan default
```
## vpopmail

vpopmail will handle virtual domains, adding, deleting mail domains, accounts, storing passwords etc.  vpopmail uses mysql in this setup. Please configure [MariaDB](https://wiki.gentoo.org/wiki/MariaDB) or [MySQL](https://wiki.gentoo.org/wiki/MySQL) to follow these instructions.

First we need to tell qmail to use vpopmail when checking smtp passwords:

**`/var/qmail/control/conf-smtpd`**

**tell qmail to use vpopmail for auth**

```
QMAIL_SMTP_CHECKPASSWORD="/var/vpopmail/bin/vchkpw"
```
Let's install and setup [net-mail/vpopmail](https://packages.gentoo.org/packages/net-mail/vpopmail):

`root #``echo 'net-mail/vpopmail clearpasswd mysql' >> /etc/portage/package.use``root #``emerge --ask vpopmail`
Create the vpopmail database:

`root #``mysql -u root -p`
mysql> create database vpopmail;
mysql> create user vpopmail@localhost identified by 'mypassword';
mysql> grant select, insert, update, delete, create, drop on vpopmail.\* to vpopmail@localhost;
mysql> flush privileges; 
mysql> quit

Edit /etc/vpopmail.conf and update the mysql password for the vpopmail user:

**`/etc/vpopmail.conf`**

**set the vpopmail user password**

## dovecot

Finally add [net-mail/dovecot](https://packages.gentoo.org/packages/net-mail/dovecot) to talk to the email clients:

`root #``echo "net-mail/dovecot vpopmail -mysql -pam" >> /etc/portage/package.use``root #``emerge --ask dovecot`
Add vpopmail UID info to the default dovecot config:

`root #````
echo 'first_valid_uid = 89' >> /etc/dovecot/dovecot.conf
```
`root #````
echo 'last_valid_uid = 89' >> /etc/dovecot/dovecot.conf
```
Edit dovecot SSL configs to pass the SSL certificate to email clients when the login to get mail securely:

**`/etc/dovecot/conf.d/10-ssl.conf`**

**set the location of the certs**

**`/etc/dovecot/conf.d/10-auth.conf`**

**edit the dovecot auth configs**

**`/etc/dovecot/conf.d/auth-vpopmail.conf.ext`**

**comment out these two vpopmail lines**

Start dovecot and add to the default runlevel:

`root #````
/etc/init.d/dovecot start
```
`root #````
rc-update add dovecot default
```
