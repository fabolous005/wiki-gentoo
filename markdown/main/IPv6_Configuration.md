<!-- source: https://wiki.gentoo.org/wiki/IPv6/Configuration | group: Gentoo Wiki (Main) | wiki-title: IPv6/Configuration -->
---
title: IPv6/Configuration
url: https://wiki.gentoo.org/wiki/IPv6/Configuration
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-11"
fingerprint: ee9d7f57246fa37e
license: CC BY-SA 4.0
---

# IPv6/Configuration

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page provides information about configuring IPv6 on Gentoo.

For general information about IPv6, please refer to the [IPv6](https://wiki.gentoo.org/wiki/IPv6) page.

For information about configuring a Gentoo system as an IPv6 router, please refer to the [IPv6 router guide](https://wiki.gentoo.org/wiki/IPv6_router_guide) page.

## When IPv6 is not used or required

Systems that don't use or require IPv6, and thus want to prioritise IPv4 over IPv6, can be configured to reflect this by modifying the /etc/gai.conf file.

Uncomment the IPv6 `precedence` lines, and change the final line to have a value of "100" rather than "10":[\[1\]](https://wiki.gentoo.org#cite_note-1)

**`/etc/gai.conf`**

Note that it is not sufficient to only uncomment *one* of these lines; if one of these lines is uncommented, *all* need to be uncommented, otherwise [glibc](https://wiki.gentoo.org/wiki/Glibc)'s `getaddrinfo(3)` won't work properly.[\[2\]](https://wiki.gentoo.org#cite_note-2)

Details about the /etc/gai.conf file can be found in the [gai.conf(5)](https://man.archlinux.org/man/gai.conf.5.en) [man page.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

## IPv6 tunnels

Refer to the [IPv6 tunnels](https://wiki.gentoo.org/wiki/IPv6_tunnels) page.

## Tools

| Package | Description | 
|---|---|
| [net-misc/ipcalc](https://packages.gentoo.org/packages/net-misc/ipcalc) | IP Calculator prints broadcast/network/etc for an IP address and netmask | 
| [net-misc/ipv6calc](https://packages.gentoo.org/packages/net-misc/ipv6calc) | IPv6 address calculator, which can convert an IPv4 address to a 6to4 address. | 
| [net-misc/sipcalc](https://packages.gentoo.org/packages/net-misc/sipcalc) | Advanced console-based IPv4/IPv6 subnet calculator |
