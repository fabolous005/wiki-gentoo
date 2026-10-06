<!-- source: https://wiki.gentoo.org/wiki//etc/portage/binrepos.conf | group: Gentoo Wiki (Main) | wiki-title: /etc/portage/binrepos.conf -->
---
title: "/etc/portage/binrepos.conf"
url: https://wiki.gentoo.org/wiki//etc/portage/binrepos.conf
hostname: gentoo.org
sitename: "/etc/portage/binrepos.conf"
date: "2026-02-06"
fingerprint: eda13e860dc26390
license: CC BY-SA 4.0
---

# /etc/portage/binrepos.conf

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

/etc/portage/binrepos.conf specifies the location and settings for binary package repositories configured with [Portage](https://wiki.gentoo.org/wiki/Portage).

## Manage repositories

Currently adding a binary package repository can only be done by hand, an example binrepos.conf config looks like the following:

FILE **`/etc/portage/binrepos.conf/gentoobinhost.conf`****UK Mirror Example, amd64**

```
[gentoo]
priority = 9999
sync-uri = https://www.mirrorservice.org/sites/distfiles.gentoo.org/releases/amd64/binpackages/23.0/x86-64/
 
# Introduced in portage-3.0.74 for per-repo verification choices
verify-signature = true
# Default value with >=portage-3.0.77
location = /var/cache/binhost/gentoo
```
## List repositories

Binary package repositories can be seen by running

`root #``emerge --info`
## See also

- [/etc/portage/repos.conf](https://wiki.gentoo.org/wiki//etc/portage/repos.conf) — specifies current [Portage](https://wiki.gentoo.org/wiki/Portage) configured repositories' location and settings
- [Binary package quickstart](https://wiki.gentoo.org/wiki/Binary_package_quickstart) — how to install packages from the Gentoo binary package host, how to set up Portage to do this by default, and associated information on Gentoo binhost usage
