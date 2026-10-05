<!-- source: https://wiki.gentoo.org/wiki/Upstream_repository_shutdowns/Freecode.com | group: Gentoo Wiki (Main) | wiki-title: Upstream repository shutdowns/Freecode.com -->
---
title: Upstream repository shutdowns/Freecode.com
url: https://wiki.gentoo.org/wiki/Upstream_repository_shutdowns/Freecode.com
hostname: gentoo.org
sitename: Upstream repository shutdowns/Freecode.com
date: "2024-12-03"
fingerprint: "7f78dc7dd7ed54e5"
license: CC BY-SA 4.0
---

# Upstream repository shutdowns/Freecode.com

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page intends to organize required actions to prepare the discontinued service of Freecode and that it was used mistakenly in the ebuilds as SRC\_URI and HOMEPAGE. ([bug #637970](https://bugs.gentoo.org/show_bug.cgi?id=637970))

Freecode/Freshmeat is/was never a project primary HOMEPAGE, but a directory of software projects. It is not suited for HOMEPAGE= or SRC\_URI

It is frozen since 2014 and ebuilds should use the real HOMEPAGE or SRC\_URI, or if not available anymore set it to https://wiki.gentoo.org/wiki/No\_homepage and upload the source to devspace or another suited space.

## List of packages

Use the MD5 cache:

`user $``cd /var/db/repos/gentoo/metadata/md5-cache; grep -lR  "freshmeat.net"  | sort | uniq`
## Worklist

- DONE 2017-11-17 [bug #637970](https://bugs.gentoo.org/show_bug.cgi?id=637970) create tracker bug on bugzilla ([jonasstein](https://wiki.gentoo.org/index.php?title=User:Jonasstein&action=edit&redlink=1))
- DONE 2017-11-17 Find out which packages need to be fixed exactly. ([jonasstein](https://wiki.gentoo.org/index.php?title=User:Jonasstein&action=edit&redlink=1))

#### ebuilds to fix (2024-12-03)

app-misc/jot-9.0-r1
games-arcade/pengupop-2.2.5-r1
games-arcade/spout-1.3-r3
games-arcade/yarsrevenge-0.99-r2
games-board/xscrabble-2.10-r4
media-gfx/crwinfo-0.2
media-libs/libjsw-1.5.8
media-sound/taginfo-1.2-r2
net-misc/netsed-0.01b-r1
