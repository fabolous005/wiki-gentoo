<!-- source: https://wiki.gentoo.org/wiki/Postfix/policyd-weight | group: Gentoo Wiki (Main) | wiki-title: Postfix/policyd-weight -->
---
title: Postfix/policyd-weight
url: https://wiki.gentoo.org/wiki/Postfix/policyd-weight
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2017-03-01"
fingerprint: "76f90c21b4babb9f"
license: CC BY-SA 4.0
---

# Postfix/policyd-weight

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**policyd-weight** is a weighted rejection-policy daemon for Postfix. Its website is at [www.policyd-weight.org](http://www.policyd-weight.org/). The Gentoo package is [mail-filter/policyd-weight](https://packages.gentoo.org/packages/mail-filter/policyd-weight).

Compared to standard email server configurations, it intercepts mail before it hits the MTA for queuing, is capable of querying RBLs and verifying sender IP addresses versus SMTP `HELO/EHLO`-claimed identities versus `MAIL FROM` (sender) addresses and generating a weighted email HAM vs. SPAM (ie. legitimacy) score. You can set a threshold value beneath which mail is rejected, thereby saving CPU time and resources. You can see a [diagram of the approach at the policyd-weight website](http://www.policyd-weight.org/scheme.html).

## Installation

`root #``emerge --ask mail-filter/policyd-weight`
Further configuration is necessary.
