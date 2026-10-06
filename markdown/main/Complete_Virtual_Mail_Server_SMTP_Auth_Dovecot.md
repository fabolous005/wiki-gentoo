<!-- source: https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/SMTP_Auth_Dovecot | group: Gentoo Wiki (Main) | wiki-title: Complete Virtual Mail Server/SMTP Auth Dovecot -->
---
title: Complete Virtual Mail Server/SMTP Auth Dovecot
url: https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/SMTP_Auth_Dovecot
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-12"
fingerprint: "9213cbba280e3b9e"
license: CC BY-SA 4.0
---

# Complete Virtual Mail Server/SMTP Auth Dovecot

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



So far only localhost is allowed to send mail. Now, a connection to dovecot auth will be configured.

The setup will look something like this:

```
  dovecot-imap----------\
                           +-> dovecot -> database
  postfix -------------/
```
## Configuring postfix with dovecot

Postfix needs a few options to tell it to use sasl in its main.cf. These are not mentioned in the default config file so they should be added:

**`/etc/postfix/main.cf`**

**Add sasl support to postfix.**

```
# Postfix to SASL authentication
smtpd_sasl_type = dovecot
smtpd_sasl_path = private/auth
broken_sasl_auth_clients = no
smtpd_sasl_auth_enable = yes
smtpd_sasl_security_options = noanonymous
smtpd_sasl_local_domain =
smtpd_sasl_authenticated_header = yes
smtpd_recipient_restrictions = permit_sasl_authenticated, permit_mynetworks, reject_unauth_destination, reject_rbl_client zen.spamhaus.org, reject_rbl_client bl.spamcop.net
```
## Configuring dovecot

**`/etc/dovecot/conf.d/10-master.conf`**

**Dovecot auth connection**

```
# Postfix smtp-auth
  unix_listener /var/spool/postfix/private/auth {
    mode = 0660
    user = postfix
    group = postfix
  }
  user = postgres
```
## Testing

To verify sasl support telnet can be used to check for the `AUTH` statement:

`user $``telnet example.com 25`
Trying 127.0.0.1...
Connected to example.com.
Escape character is '^\]'.
220 example.com ESMTP NO UCE
EHLO localhost
250-example.com
250-PIPELINING
250-SIZE 20971520
250-ETRN
250-AUTH PLAIN LOGIN
250-ENHANCEDSTATUSCODES
250-8BITMIME
250-DSN
250-SMTPUTF8
250 CHUNKING
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

Optionally, `smtpd_sasl_authenticated_header` can be disabled again. It is very handy for tracking down mailing issues from users. It can however be potentially a security issue, as mentioned above, the users login name is written in the header. On the other hand, if the login name is the *local\_part* of the e-mail address or even the e-mail address then the login name is already known anyway so no big harm there, right? Some caution is advised, but it shouldn't be a huge issue.

**`/etc/postfix/main.cf`**

**Add sasl support to postfix**

```
smtpd_sasl_authenticated_header = no
```
