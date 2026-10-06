<!-- source: https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/SSL_Certificates | group: Gentoo Wiki (Main) | wiki-title: Complete Virtual Mail Server/SSL Certificates -->
---
title: Complete Virtual Mail Server/SSL Certificates
url: https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/SSL_Certificates
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-05-06"
fingerprint: "3f53e24e0f2a3e95"
license: CC BY-SA 4.0
---

# Complete Virtual Mail Server/SSL Certificates

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

With security and privacy being an increasingly important issue nowadays, Using SSL to secure the server seems like a no-brainer. Apache, Courier-imap and postfix all can be secured using SSL.

## Installing SSL

SSL is a dependency and a compile time option to most packages. [dev-libs/openssl](https://packages.gentoo.org/packages/dev-libs/openssl) being the key ingredient, should be pulled in from the `ssl` USE flag. If it wasn't set before enable it and update all packages using it:

`root #``emerge --newuse @world`
Also install certbot to use free certificates from Let's Encrypt:

`root #``emerge --ask app-crypt/certbot`
## SNI

There are a few issues that arise when using multiple domains on a single IP. Apache has solved this issue using [SNI](https://cwiki.apache.org/confluence/display/httpd/NameBasedSSLVHostsWithSNI) that makes it possible to have several certificates on a single IP. Both the browser and the server need to support this however. IMAP (and POP3) nor SMTP really support this. The only real way to support multiple hosts on a single IP is have a certificate, that covers all domains. Not pretty but can work.

## Obtaining an SSL Certificate

There are currently a few ways to obtain an SSL certificate. Purchase a certificate from one of the [reputable](https://en.wikipedia.org/wiki/DigiNotar#Issuance_of_fraudulent_certificates) providers is an option. Using a self-signed certificate can also be done, though may have implications with warnings on users clients. The recommended option, though, is to use a free certificate from Let's Encrypt.

### Let's Encrypt

`root #``certbot --apache --rsa-key-size 4096 --staple-ocsp --hsts`
The certificates will be saved under /etc/letsencrypt/live/.

### Self signed

Most applications pre-generate self signed certificates and tend to install then into /etc/ssl. A new self-signed certificate can be easily created using OpenSSL:

`root #``mkdir -p /etc/ssl/postfix/``root #``cd /etc/ssl/postfix/``root #``/etc/ssl/misc/CA.pl -newca``root #``/etc/ssl/misc/CA.pl -newreq-nodes``root #``/etc/ssl/misc/CA.pl -sign`
This should have created three files: /etc/ssl/postfix/newkey.pem, /etc/ssl/postfix/newreq.pem, /etc/ssl/postfix/newcert.pem and it's accompanying CA Root certificate /etc/ssl/postfix/demoCA/cacert.pem. It is probably advised to rename them to something more logical.

### CACert.org signed

CACert.org offers a simple script to assist with generating SSL certificates. The [csr](http://wiki.cacert.org/CSRGenerator) script should be downloaded and executed. In this example, the mail server will be called imap but will have aliases configured in DNS for mail, pop, pop3, pop3s, imaps and foo. More can be added of course as fit. Note that foo was added because that is the name of the system offering the imap service. It is not named foo because the postfix or web or any other server is named foo.

`user $``sh csr`
Private Key and Certificate Signing Request Generator
This script was designed to suit the request format needed by
the CAcert Certificate Authority. www.CAcert.org
 
Short Hostname (ie. imap big\_srv www2): foo
FQDN/CommonName (ie. www.example.com) : foo.example.com
Type SubjectAltNames for the certificate, one per line. Enter a blank line to finish
SubjectAltName: DNS:mail.example.com
SubjectAltName: DNS:smtp.example.com
SubjectAltName: DNS:smtps.example.com
SubjectAltName: DNS:imap.example.com
SubjectAltName: DNS:imaps.example.com
SubjectAltName: DNS:pop.example.com
SubjectAltName: DNS:pop3.example.com
SubjectAltName: DNS:pop3s.example.com
SubjectAltName: DNS:mail.example.net
SubjectAltName: DNS:imap.example.net
SubjectAltName: DNS:imaps.example.net
SubjectAltName: DNS:pop.example.net
SubjectAltName: DNS:pop3.example.net
SubjectAltName: DNS:pop3s.example.net
SubjectAltName: DNS:
Running OpenSSL...
Generating a 2048 bit RSA private key
.................+++
..................................+++
writing new private key to '/root/imap.example.com\_privatekey.pem'
-----
Copy the following Certificate Request and paste into CAcert website to obtain a Certificate.
When you receive your certificate, you 'should' name it something like foo.example.com\_server.pem
 
-----BEGIN CERTIFICATE REQUEST-----
MIIDHTsdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfsdfasdf
\<snip>
asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfpyqT
-----END CERTIFICATE REQUEST-----
 
 
The Certificate request is also available in /root/foo\_csr.pem
The Private Key is stored in /root/foo\_privatekey.pem

This has generated a *certificate sign request*, which can be used by any root CA to sign with, not only CACert.org. In the case of CACert.org however, under *Server Certificates* there is a link named *New* which opens an edit box for the above certificate request. The bit including `-----BEGIN CERTIFICATE REQUEST-----` until, including `-----END CERTIFICATE REQUEST-----` needs to be pasted into the edit field and then submitted via the submit button. The server will then verify the request and upon that generate the certificate. The certificate then needs to be copied and pasted, including the `BEGIN` and `END` markers again into a new file, named foo.example.com\_crt.pem. A link to the certificate will also be e-mailed to the e-mail address bound to the ca-cert.org account.

`root #``cat > foo.example.com_crt.pem`
-----BEGIN CERTIFICATE-----
MIIGhzClkjhlkjhlkjhkljhkljhkljhkljhkljhkljhlkjhkljhkljhlkjhkljhk
\<snip>
kljhlkjhkljhlkjhlkjhlkjhkljhkljhlkjhlkjhkljhk==
-----END CERTIFICATE-----

This should leave 3 files, foo.example.com\_\[privatekey, csr, crt\].pem. This will also be the naming convention followed during the rest of this document.

## Apache

### Hardening

While Let's Encrypt provides sane standarts for security, those can be further improved.

**`/etc/letsencrypt/options-ssl-apache.conf`**

**Hardening options**

```
SSLProtocol             all -SSLv2 -SSLv3 -TLSv1 -TLSv1.1
SSLCipherSuite          ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES256-GCM-SHA384
SSLHonorCipherOrder     off
SSLUseStapling on
```
Roundcube has a nice option to force all incoming requests over HTTPS. This means that when a users opens [http://mail.example.com](http://mail.example.com), he will get immediately redirected to [https://mail.example.com](https://mail.example.com). If using a proper TLS certificate this is strongly recommended. When using a self-signed certificate, or a CA-cert.org certificate that does not have the root installed to all users, this should remain off however.

**`/var/www/mail/htdocs/config/main.inc.php`**

**Force https**

```
$rcmail_config['force_https'] = true;
```
## Courier-imap

If anything, securing IMAP with SSL is extremely recommended. Using a secure connection for IMAP, means that if the password is sent in plain text, this is still done over the secured IMAP connection so no security issue comes up.

### Using a Let's Encrypt certificate

Courier-imap needs to be told where to find the certificates. This should be done in the config file:

**`/etc/courier-imap/imapd-ssl`**

**Use Let's Encrypt**

```
TLS_CERTFILE=/etc/letsencrypt/live/example.com/fullchain.pem
TLS_PRIVATE_KEYFILE=/etc/letsencrypt/live/example.com/privkey.pem
```
### Using a CACert.org signed certificate

The mkimapdcert script creates a self-signed certificate and combines them into one file as Courier-imap does actually not use the three separate files as most applications do and needs them specially formatted.

The file starts with the private key.

`root #``cat foo.example.com_privatekey.pem > /etc/ssl/courier-imap/foo.example.com.pem`
This then is followed by the signed certificate:

`root #``cat foo.example.com_crt.pem >> /etc/ssl/courier-imap/foo.example.com.pem`
And to the end, Diffie-Hellman parameters are added:

`root #``openssl gendh 1024 >> /etc/ssl/courier-imap/foo.example.com.pem`
The resulting file should have a contents like this:

**`/etc/ssl/courier-imap/foo.example.com.pem`**

**imap certificate**

```
-----BEGIN PRIVATE KEY-----
MIIEvaasdfasdfSfasdfadfasdfasdfasdfasdfasdfasdfasdfasdsahdahhgfh
<snip>
asdfasdfasdfsdfsdf
-----BEGIN CERTIFICATE REQUEST-----
MIIDHTsdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfsdfasdf
<snip>
asdfasdfasdfasdfaswYEdpa+rdFfs=
-----END PRIVATE KEY-----
-----END CERTIFICATE REQUEST-----
-----BEGIN CERTIFICATE-----
MIIGhzClkjhlkjhlkjhkljhkljhkljhkljhkljhkljhlkjhkljhkljhlkjhkljhk
<snip>
kljhlkjhkljhlkjhlkjhlkjhkljhkljhlkjhlkjhkljhk==
-----END CERTIFICATE-----
-----BEGIN DH PARAMETERS-----
MIGHAoGBAPF7fJnfw+VPPev9FAkf2XJNFimn4ik+zkXXuHD5t9Oke1Yx224WTocq
KJ+Zv9onecK0MPYRUj8PPqqy+Q00pScW9+qPSr9T2sEG/meKjLqqA3XQf4Gwzqco
SUG0PEjiYNNfe966p9E1vp6yN5+gSyu6zv9Vn+cfYY2q7d3a4x9rAgEC
-----END DH PARAMETERS-----
```
### Configure SSL

**`/etc/courier-imap/imapd-ssl`**

**Configure certificate**

```
##NAME: IMAPDSSLSTART:0
IMAPDSSLSTART=YES
```
Starting this server should allow imap to work through SSL:

`root #``/etc/init.d/courier-imapd-ssl restart`
### Testing SSL

Testing becomes more difficult, as telnet can not be used anymore. The best option is to start up a mail-client such as thunderbird, configure a normal connection first to verify everything works, which should as telnet worked properly before and then enable the SSL option for the account and see if it is still working. The default imap-ssl port is 993. `STARTTLS` can be checked whether it is enabled only, via telnet, by checking for the STARTTLS Capability.

If SSL secured services now work as expected, they can be added to the default runlevel:

`root #``rc-update add courier-imapd-ssl default`
## Securing incoming connections with TLS

### Let's Encrypt

If the Let's Encrypt certificate is being used, certificates should be stored in /etc/letsencrypt/live/foo.example.com/.

**`/etc/postfix/main.cf`**

**Certificate configuration for SMTP using Let's Encrypt**

```
# TLS Authentication
smtpd_tls_security_level = may
smtpd_tls_auth_only = no
smtpd_tls_loglevel = 3
smtpd_tls_key_file = /etc/letsencrypt/live/foo.example.com/privkey.pem
smtpd_tls_cert_file = /etc/letsencrypt/live/foo.example.com/fullchain.pem
smtpd_tls_received_header = yes
smtpd_tls_session_cache_timeout = 3600s
tls_random_source = dev:/dev/urandom
```
### Other certificate providers

The certificates for use with postfix should be stored in /etc/ssl/postfix/ or if using the same certificates as with courier-imap they should be stored in /etc/ssl/postfix/. If using CACert.org, then its root certificate needs to be used. Gentoo pre-installs the CACert.org root certificate and should be used.

**`/etc/postfix/main.cf`**

**Certificate configuration for SMTP**

```
# TLS Authentication
smtpd_tls_security_level = may
smtpd_tls_auth_only = no
smtpd_tls_loglevel = 3
smtpd_tls_key_file = /etc/ssl/postfix/foo.example.com_privatekey.pem
smtpd_tls_cert_file = /etc/ssl/postfix/foo.example.com_crt.pem
smtpd_tls_CAfile = /etc/ssl/certs/cacert.org_root.pem
smtpd_tls_received_header = yes
smtpd_tls_session_cache_timeout = 3600s
tls_random_source = dev:/dev/urandom
```
### Postfix configuration

Now STARTTLS can be used to use an authenticated connection over port 25. **SSL/TLS** support on port *465* (smtp**s**) however should be enabled as well. Courier-imap did this automatically, postfix needs a change to master.cf:

**`/etc/postfix/master.cf`**

**Enable smtps support**

```
smtps     inet  n       -       n       -       -       smtpd
  -o smtpd_tls_wrappermode=yes
```
Restart postfix to start the SSL secured daemons:

`root #``/etc/init.d/postfix restart`
### Testing

Telnet can only be minimally used for testing. Actually it can only be used to verify supported options. *STARTTLS* should be listed as one of the supported options:

`user $``telnet foo.example.com 25`
220 foo.example.com ESMTP Postfix
EHLO example.com
250-mail.example.com
250-PIPELINING
250-SIZE 10240000
250-VRFY
250-ETRN
250-STARTTLS
250-AUTH LOGIN PLAIN
250-ENHANCEDSTATUSCODES
250-8BITMIME
250 DSN
quit
221 2.0.0 Bye
Connection closed by foreign host.

To test if all connections work as wanted, a recent version of Thunderbird works best. When adding a new account, Thunderbird will try to connect using STARTTLS on the default port. If that would fail, or if a manual connection is tried, it's possible to use TLS via the 993 and 465 ports and hit the re-test button. This should allow the account to be created using secure connections.

## Securing outgoing connections with TLS

Postfix can try to use secure connections for sending mails to other SMTP servers. Although it may not be supported by all servers, it makes sense to activate it:

**`/etc/postfix/main.cf`**

**Try to use TLS for transferring mails**

```
# Enable TLS for sending mails if supported by other server
smtp_use_tls = yes
smtp_tls_security_level = may
smtp_tls_key_file = /etc/letsencrypt/live/example.com/privkey.pem
smtp_tls_cert_file = /etc/letsencrypt/live/example.com/fullchain.pem
```
Restart postfix afterwards:

`root #``/etc/init.d/postfix restart`
To test if it works, a mail can be sent to an address, which uses a server that supports TLS. The log should then contain a line like this:

`user $``tail -n 10 /var/log/mail/mailinfo.log` ...
\[postfix/smtpd\] Anonymous TLS connection established from \<...>: TLSv1.3 with cipher TLS\_AES\_256\_GCM\_SHA384 (256/256 bits) key-exchange ECDHE (P-256) server-signature RSA-PSS (4096 bits) server-digest SHA256
...

## Wrapping it up

Once everything is working as expected, logging can be disabled again:

**`/etc/postfix/main.cf`**

**Disable logging**

```
smtpd_tls_loglevel = 0
smtp_tls_loglevel = 0
```
