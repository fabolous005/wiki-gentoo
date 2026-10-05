<!-- source: https://wiki.gentoo.org/wiki/Postfix/Greylisting | group: Gentoo Wiki (Main) | wiki-title: Postfix/Greylisting -->
---
title: Postfix/Greylisting
url: https://wiki.gentoo.org/wiki/Postfix/Greylisting
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-12"
fingerprint: f4aaf549388a1db6
license: CC BY-SA 4.0
---

# Postfix/Greylisting

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Greylisting** is a process in which systems attempting to connect to your server to deliver mail for the first time are treated differently to known peers, delaying their mail. If the sending mail server is standards-compliant, it will re-send the e-mail, and the server will accept it. Most spam mailers, however, don't re-send the mail, and so the spam is blocked. Servers that re-send the mail will be added to a white list, and will not be delayed in future. This means that the first e-mail from a given sender will be delayed, but subsequent ones will not be.

## Installation

Greylisting for postfix is typically implemented by using the the [mail-filter/postgrey](https://packages.gentoo.org/packages/mail-filter/postgrey) package, so first install that:

`root #``emerge --ask mail-filter/postgrey`
By default postgrey listens to port 10030. This can be changed by modifying POSTGREY\_PORT variable in /etc/conf.d/postgrey.

## Setup

Next, we need to start it, and set it to start automatically.

`root #````
rc-update add postgrey default
```
`root #``/etc/init.d/postgrey start`
Now we have to tell postfix to use it, by adding the `check_policy_service inet:127.0.0.1:10023` entry to the existing `smtpd_recipient_restrictions` directive in your main.cf file, as follows.

**`/etc/postfix/main.cf`**

**Use greylist policy daemon**

## Deployment

Finally, tell postfix to reload its configuration for the changes to take effect.

`root #``/etc/init.d/postfix reload`
