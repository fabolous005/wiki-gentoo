<!-- source: https://wiki.gentoo.org/wiki/Polkit/upgrade | group: Gentoo Wiki (Main) | wiki-title: Polkit/upgrade -->
---
title: Polkit/upgrade
url: https://wiki.gentoo.org/wiki/Polkit/upgrade
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-11-19"
fingerprint: a0b9432be75bb290
license: CC BY-SA 4.0
---

# Polkit/upgrade

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page only lists changes, which must be considered before updating or risk system brakege. For regular updates, see [sys-auth/polkit's changelog](https://wiki.gentoo.org#See_also) or [upstream's NEWS file](https://wiki.gentoo.org#External_resources).

## polkit 0.106

polkit changed its rules and actions file format. See the *polkit* [man page](https://wiki.gentoo.org/wiki/Man_page) for more informations. Actions are now in /usr/share/polkit-1/actions, rules now in /usr/share/polkit-1/rules.d and /etc/polkit-1/rules.d.

For the rationale and more informations see [David Zeuthen's blog post](http://davidz25.blogspot.de/2012/06/authorization-rules-in-polkit.html).
