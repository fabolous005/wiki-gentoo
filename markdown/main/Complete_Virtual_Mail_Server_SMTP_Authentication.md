<!-- source: https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/SMTP_Authentication | group: Gentoo Wiki (Main) | wiki-title: Complete Virtual Mail Server/SMTP Authentication -->
---
title: Complete Virtual Mail Server/SMTP Authentication
url: https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/SMTP_Authentication
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-05-06"
fingerprint: fa03e9ba09003fde
license: CC BY-SA 4.0
---

# Complete Virtual Mail Server/SMTP Authentication

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

So far only localhost is allowed to send mail. Unfortunately postfix cannot work with courier-authlib directly. An intermediary solution exists however, [dev-libs/cyrus-sasl](https://packages.gentoo.org/packages/dev-libs/cyrus-sasl). There are three ways cyrus-sasl can get authentication information. Either directly from the database, locally or remotely. A setup using this approach would look like this:

```
  courier-imap -> courier-authlib --\
                                     +--> database 
  postfix ------> cyrus-sasl -------/
```
Making things only slightly more complex, cyrus-sasl can be used to communicate through courier-authlib and thus letting courier-authlib do the authentication:

```
  courier-imap -----------\
                           +-> courier-authlib -> database
  postfix -> cyrus-sasl --/
```
Ideally, the last option would be the used solution, as one authentication back-end would be used, courier-authlib. The cyrus-sasl plugin to talk to courier-authlib however will only work via a unix socket, and thus if courier-authlib is not running on the same host as cyrus-sasl, this would not work. The first approach should therefore only be used if courier-authlib can not be used.

## Installing cyrus-sasl

A key feature of cyrus-sasl that is required is the `crypt` USE flag. It needs to be enabled or crypted passwords from the database cannot be authenticated with. Cyrus-sasl with the correct USE flag should have been pulled in earlier whilst emerging postfix.


### USE flags for
            [dev-libs/cyrus-sasl](https://packages.gentoo.org/packages/dev-libs/cyrus-sasl)
            
            The Cyrus SASL (Simple Authentication and Security Layer)

| [authdaemond](https://packages.gentoo.org/useflags/authdaemond) | Add Courier-IMAP authdaemond unix socket support (net-mail/courier-imap, mail-mta/courier) | 
| [berkdb](https://packages.gentoo.org/useflags/berkdb) | Add support for sys-libs/db (Berkeley DB) | 
| [gdbm](https://packages.gentoo.org/useflags/gdbm) | Add support for sys-libs/gdbm (GNU database libraries) | 
| [kerberos](https://packages.gentoo.org/useflags/kerberos) | Add kerberos support | 
| [ldapdb](https://packages.gentoo.org/useflags/ldapdb) | Enable ldapdb plugin | 
| [mysql](https://packages.gentoo.org/useflags/mysql) | Add mySQL Database support | 
| [openldap](https://packages.gentoo.org/useflags/openldap) | Add ldap support for saslauthd | 
| [pam](https://packages.gentoo.org/useflags/pam) | Add support for PAM (Pluggable Authentication Modules) - DANGEROUS to arbitrarily flip | 
| [postgres](https://packages.gentoo.org/useflags/postgres) | Add support for the postgresql database | 
| [sample](https://packages.gentoo.org/useflags/sample) | Enable sample client and server | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [sqlite](https://packages.gentoo.org/useflags/sqlite) | Add support for sqlite - embedded sql database | 
| [srp](https://packages.gentoo.org/useflags/srp) | Enable SRP authentication | 
| [ssl](https://packages.gentoo.org/useflags/ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 
| [static-libs](https://packages.gentoo.org/useflags/static-libs) | Build static versions of dynamic libraries as well | 
| [urandom](https://packages.gentoo.org/useflags/urandom) | Use /dev/urandom instead of /dev/random | 

## Configuring postfix with cyrus-sasl

Postfix needs a few options to tell it to use sasl in its main.cf. These are not mentioned in the default config file so they should be added:

**`/etc/postfix/main.cf`**

**Add sasl support to postfix.**

```
# Postfix to SASL authentication
broken_sasl_auth_clients = no
smtpd_sasl_auth_enable = yes
smtpd_sasl_security_options = noanonymous
smtpd_sasl_local_domain =
smtpd_sasl_authenticated_header = yes
smtpd_recipient_restrictions = permit_sasl_authenticated, permit_mynetworks, reject_unauth_destination
```
## Configuring cyrus-sasl

### With authdaemond

Postfix queries the socket created by authdaemond which is protected by the mail user and group and thus postfix needs to be granted access:

`root #``gpasswd -a postfix mail`
Next cyrus-sasl needs to be told to authenticate with authdaemond:

**`/etc/sasl2/smtpd.conf`**

**Authenticate with authdaemond**

```
pwcheck_method: authdaemond
mech_list: LOGIN PLAIN
sql_select: dummy 
authdaemond_path: /var/lib/courier/authdaemon/socket
 
log_level: 5
```
### With postgresql

**`/etc/sasl2/smtpd.conf`**

**Direct database authentication**

```
sasl_pwcheck_method: auxprop
sasl_auxprop_plugin: pgsql
password_format: crypt
mech_list: LOGIN PLAIN
 
sql_engine: pgsql
#sql_hostnames: localhost
sql_database: postfix
sql_user: postfix
sql_passwd: $password
sql_select: SELECT password FROM mailbox WHERE local_part='%u' AND active='1'
```
## Testing

To verify sasl support telnet can be used to check for the `AUTH` statement:

`user $``telnet foo.example.com 25`
220 foo.example.com ESMTP Postfix
EHLO example.com
250-foo.example.com
250-PIPELINING
250-SIZE 10240000
250-VRFY
250-ETRN
250-AUTH LOGIN PLAIN
250-ENHANCEDSTATUSCODES
250-8BITMIME
250 DSN
quit
221 2.0.0 Bye
Connection closed by foreign host.

The next test is to use a remote host and try to login to send a test message.

If perl with the base64 module is installed, it can be used to generate base64 encoded data. Otherwise [base64 conversion](http://farhadi.ir/works/base64) can be done online. Again, be very careful when using production data on untrusted sites.

`user $``perl -MMIME::Base64 -e 'print encode_base64("testuser");'`
dGVzdHVzZXI=

`user $``telnet foo.example.com 25`
Trying 1.2.3.4...
Connected to foo.example.com.
Escape character is '^\]'.
220 foo.example.com ESMTP Postfix
HELO example.com
250 foo.example.com
AUTH LOGIN
334 VXNlcm5hbWU6 (base64 decode: 'Username:')
dGVzdHVzZXI= (base64 encoded from: 'testuser')
334 UGFzc3dvcmQ6 (base64 decode: 'Password:')
c2VjcmV0 (base64 encoded from: 'secret')
235 2.7.0 Authentication successful
mail from:me@you.com
250 2.1.0 Ok
rcpt to:\<validuser>@\<validexternaldomain>.\<tld>
250 2.1.5 Ok
data
354 End data with \<CR>\<LF>.\<CR>\<LF>
Subject: Test message
Test message to ensure Postfix is only relaying with smtp authorization.
.
250 2.0.0 Ok: queued as 82F97606
quit
221 2.0.0 Bye
Connection closed by foreign host.

## Wrapping it up

Once everything is working as expected, debugging can be disabled (or the line can be removed entirely):

**`/etc/sasl2/smtpd.conf`**

**Disable debugging**

```
log_level: 0
```
Optionally, `smtpd_sasl_authenticated_header` can be disabled again. It is very handy for tracking down mailing issues from users. It can however be potentially a security issue, as mentioned above, the users login name is written in the header. On the other hand, if the login name is the *local\_part* of the e-mail address or even the e-mail address then the login name is already known anyway so no big harm there, right? Some caution is advised, but it shouldn't be a huge issue.

**`/etc/postfix/main.cf`**

**Add sasl support to postfix**

```
smtpd_sasl_authenticated_header = no
```
