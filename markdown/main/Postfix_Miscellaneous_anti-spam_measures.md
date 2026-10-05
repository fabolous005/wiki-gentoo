<!-- source: https://wiki.gentoo.org/wiki/Postfix/Miscellaneous_anti-spam_measures | group: Gentoo Wiki (Main) | wiki-title: Postfix/Miscellaneous anti-spam measures -->
---
title: Postfix/Miscellaneous anti-spam measures
url: https://wiki.gentoo.org/wiki/Postfix/Miscellaneous_anti-spam_measures
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-01-16"
fingerprint: fad1d17bfe3217dd
license: CC BY-SA 4.0
---

# Postfix/Miscellaneous anti-spam measures

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page lists **miscellaneous anti-spam measures** that can help prevent unwanted inbound mail to your postfix server.

## HELO/EHLO spoofing countermeasure

First create the following file, where the IP addresses and domain names in the first three lines represent those of your own server.

**`/etc/postfix/helo.regexp`**

**Define abnormal HELO/EHLO patterns**

We then add a `regexp:/etc/postfix/helo.regexp` entry to the `smtpd_helo_restrictions` directive in `main.cf`, as follows.

**`/etc/postfix/main.cf`**

**Enforce HELO or EHLO formats**

To put this in to action, reload postfix's configuration as follows.

`root #``/etc/init.d/postfix reload`
## Ban obviously dangerous attachment file extensions

If you are looking after windows users, you may wish to reject certain attachment file extensions.

**`/etc/postfix/mime_header_checks.regexp`**

**Define dangerous attachment file extensions**

You will then need to tell Postfix to process this file.

**`/etc/postfix/main.cf`**

**Ban dangerous attachment file extensions**

To put this in to action, reload postfix's configuration as follows.

`root #``/etc/init.d/postfix reload`
## Reducing information leaks

With default settings, smartly written spam bots might just figure out which policy they are running up against when they attempt to send mail and are rejected. The suggestion is therefore to change rejection codes to a single, generic code in order to confuse such bots. What impact this has on legitimate clients is something you will have to test out... *apparently* some people use it and it works.

**`/etc/postfix/main.cf`**

**Genericize SMTP rejection**

To put this in to action, reload postfix's configuration as follows.

`root #``/etc/init.d/postfix reload`
## Enforce complete SMTP implementations

These checks are basic but help to weed out spam bots that have been written poorly and do not confirm to RFCs, as well as spam bots that attempt to enumerate local addresses via the SMTP `VRFY` command.

**`/etc/postfix/main.cf`**

**Enforce complete SMTP implementations**

To put this in to action, reload postfix's configuration as follows.

`root #``/etc/init.d/postfix reload`
## Ban failed authentication attempts

If you are using SASL to authenticate clients on whose behalf you wish to relay mail, then it is strongly recommended that you install a system such as [Fail2ban](https://wiki.gentoo.org/wiki/Fail2ban) that will prohibit brute force username/password enumeration. In addition, you should ensure that your password policy requires hard to guess passwords (not dictionary words, special characters included, decent minimum length, etc.)
