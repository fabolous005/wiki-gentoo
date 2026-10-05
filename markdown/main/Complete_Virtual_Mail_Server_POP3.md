<!-- source: https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/POP3 | group: Gentoo Wiki (Main) | wiki-title: Complete Virtual Mail Server/POP3 -->
---
title: Complete Virtual Mail Server/POP3
url: https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/POP3
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-05-06"
fingerprint: be68a35b6d03ee0b
license: CC BY-SA 4.0
---

# Complete Virtual Mail Server/POP3

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



This guide will configure a mail server to use POP3 instead of IMAP.

## Courier-IMAP to Database

### Configuring POP3

POP3 requires little configuring to get working. It is however recommended to skip this section and not enable/use pop3 and thus leave this setting at '*NO*: a user may unwittingly remove all messages that were supposed to be stored on the server for imap usage, then incorrectly configure their mail client and purge the server of their mailbox if configured this way!

**`/etc/courier-imap/pop3d`**

**Enable pop3**

### Testing POP3

Courier-pop3d should be started:

`root #``/etc/init.d/courier-pop3d start`
Once started, telnet could be used to identify initial problems. Once logging in with telnet works, a mail client can be used:

`user $``telnet foo.example.com 110`
+OK Hello there.
user testuser
+OK Password required
Pass secret
+OK logged in

If testing works properly, add courier-pop3d to the default runlevel:

`root #``rc-update add courier-pop3d default`
## SSL Certificates

**`/etc/courier-imap/pop3d-ssl`**

**Configure certificate**

Starting this server should allow pop3 to work through SSL:

`root #``/etc/init.d/courier-pop3d-ssl restart`
