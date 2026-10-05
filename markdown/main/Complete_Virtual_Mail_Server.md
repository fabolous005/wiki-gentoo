<!-- source: https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server | group: Gentoo Wiki (Main) | wiki-title: Complete Virtual Mail Server -->
---
title: Complete Virtual Mail Server
url: https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-16"
fingerprint: bb5b2c3a90a8e9d7
license: CC BY-SA 4.0
---

# Complete Virtual Mail Server

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The purpose of this guide is to **establish a virtual mail system** that can handle multiple domains with a variety of different interface options. This is not intended to be used by the average user who is looking for a mail client, this is a full-scale Mail Transfer Agent (MTA) intended for individuals who are hosting their own domains and/or need to provide support for virtual domains.

This guide uses [Postfix](https://wiki.gentoo.org/wiki/Postfix) as the MTA.

By the end of this guide, an easy method to manage a mail server that supports the following features has passed the revue:

- Web based system administration
- Unlimited number of domains
- Virtual mail users without the need for shell accounts
- Domain (specific) user names
- Mailbox quotas
- Web access to email accounts
- IMAP and (very optional) POP3 support
- SMTP Authentication for secure relaying
- SSL for transport layer security
- Strong SPAM filtering
- Anti-Virus filtering
- Log Analysis

The real plus is that all of this is managed by a single database.

- This section outlines a system setup (a multi-server implementation) as well as the core packages that were used. This is a MUST READ before reading on any further (don't worry, it's short).

- Mailboxes are stored on a normal filesystem and thus needs a user and group for security.

- [www-apps/postfixadmin](https://packages.gentoo.org/packages/www-apps/postfixadmin) and [www-servers/apache](https://packages.gentoo.org/packages/www-servers/apache) were key tools in getting through testing and getting this to hang together. While the details of an Apache/PHP setup are not here, there is good information in here all the same.

- [mail-mta/postfix](https://packages.gentoo.org/packages/mail-mta/postfix) will be coupled to a database backend allowing virtual users on multiple domains.

- [Linking Dovecot to database backend](https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/Dovecot_to_Database)
- [net-mail/dovecot](https://packages.gentoo.org/packages/net-mail/dovecot) will be coupled to the same database.

- [Linking Courier-imap to database backend](https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/Courier-IMAP_to_Database)
- [net-mail/courier-imap](https://packages.gentoo.org/packages/net-mail/courier-imap) will be coupled to the same database.

- [SMTP Authentication - Dovecot route](https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/SMTP_Auth_Dovecot)
- Having a mailserver that relays local mail is good enough for most, being able to relay mail after authentication is extremely handy.

- [SMTP Authentication - Courier route](https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/SMTP_Authentication)
- Having a mailserver that relays local mail is good enough for most, being able to relay mail after authentication is extremely handy.

- Now that a basic mailserver has been setup, web access can be both useful and helpful during testing.

- Securing the mail server with SSL certificates.

- DKIM will sign all outgoing messages with verification keys to prevent ending up in the junk box. SPF will ensure that the only verified servers/IP addresses may send mail from a given domain. DMARC ensures that both DKIM and SPF are properly enforced.

- Using default Postfix configuration options, the server gets some performance tweaks and security settings.

- Defending against spam using Amavis, SpamAssassin and ClamAV for virus protection.

- Always important is monitoring. To do so AWStats is used to get a useful overview of passed messages.

- [POP3 protocol](https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/POP3)
- POP3 is an old protocol and should not be used. For the sake of completeness, it is included in this guide.
