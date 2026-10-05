<!-- source: https://wiki.gentoo.org/wiki/Security_Handbook/Staying_up-to-date | group: Gentoo Wiki (Main) | wiki-title: Security Handbook/Staying up-to-date -->
---
title: Security Handbook/Staying up-to-date
url: https://wiki.gentoo.org/wiki/Security_Handbook/Staying_up-to-date
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-06-19"
fingerprint: a411f9320431871c
license: CC BY-SA 4.0
---

# Security Handbook/Staying up-to-date

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This section is on keeping the system up-to-date.

The Security team distributes *Gentoo Linux Security Advisories* (GLSAs) through [Portage's glsa-check](https://wiki.gentoo.org/wiki/Portage#glsa-check). They are received, like the news, after a sync and detail some security vulnerabilities related to ::gentoo repository packages.

More work is required after a successful install to ensure a good level of security. Security is an ongoing process much like development; the vast majority of intrusions result from known vulnerabilities in unpatched systems.

Keeping the system up-to-date is the single most valuable step to take for greater security.

First sync the Portage tree:

`root #``emerge --sync`
To check if the system is up to date security-wise:

`root #``glsa-check --list`
\[A\] means this GLSA was marked as applied (injected),
\[U\] means the system is not affected and
\[N\] indicates that the system might be affected.
200406-03 \[N\] sitecopy: Multiple vulnerabilities in included libneon ( net-misc/sitecopy )
200406-04 \[U\] Mailman: Member password disclosure vulnerability ( net-mail/mailman )
.......

All lines with a `[A]` and `[U]` can be almost safely ignored as the system is not affected by this GLSA.

To check all GLSAs:

`root #``glsa-check --test all`
This system is not affected by any of the listed GLSAs

`root #``glsa-check -t all`
This system is affected by the following GLSA:
200504-06
200510-08
200506-14
200501-35
200508-12
200507-16

To see what packages would be emerged:

`root #``glsa-check -p $(glsa-check -t all)````
Checking GLSA 200504-06
The following updates will be performed for this GLSA:
     app-arch/sharutils-4.2.1-r11 (4.2.1-r10)
     **********************************************************************
     Checking GLSA 200510-08
     The following updates will be performed for this GLSA:
          media-libs/xine-lib-1.1.0-r5 (1.1.0-r4)
```
To apply required fixes:

`root #``glsa-check -f $(glsa-check -t all)`
Then restart any upgraded running services.

Keeping the [kernel up-to-date](https://wiki.gentoo.org/wiki/Kernel/Upgrade) is also recommended.

To receive an email each time a GLSA is released, subscribe to the [gentoo-announce mailing list](https://archives.gentoo.org/gentoo-announce/). Instructions for joining it and many other great mailing lists can be found in the [Gentoo mailing lists](https://www.gentoo.org/get-involved/mailing-lists/all-lists.html).

Another great security resource is the [Bugtraq mailing list](https://www.securityfocus.com/archive/1).
