<!-- source: https://wiki.gentoo.org/wiki/PORTDIR | group: Gentoo Wiki (Main) | wiki-title: PORTDIR -->
---
title: PORTDIR
url: https://wiki.gentoo.org/wiki/PORTDIR
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-05-14"
fingerprint: ab9190d98ca83bb9
license: CC BY-SA 4.0
---

# PORTDIR

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **deprecated (obsolete)**. Contents are <u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

The `PORTDIR` variable was<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> used to point to the main package repository hosted on the system. It has since been [deprecated](https://bugs.gentoo.org/show_bug.cgi?id=546210) in favor of the `location` attribute (whose default value is [/var/db/repos/gentoo](https://wiki.gentoo.org/wiki//var/db/repos/gentoo)) of the Gentoo repository configuration inside [/etc/portage/repos.conf](https://wiki.gentoo.org/wiki//etc/portage/repos.conf).

The current value of the `location` attribute can be obtained by using [portageq](https://wiki.gentoo.org/wiki/Portageq):

`user $``portageq get_repo_path / gentoo`
/var/db/repos/gentoo
