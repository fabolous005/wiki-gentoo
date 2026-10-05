<!-- source: https://wiki.gentoo.org/wiki//usr/portage | group: Gentoo Wiki (Main) | wiki-title: /usr/portage -->
---
title: "/usr/portage"
url: https://wiki.gentoo.org/wiki//usr/portage
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-10-09"
fingerprint: "3eda1ada9497228a"
license: CC BY-SA 4.0
---

# /usr/portage

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**/usr/portage** was the default `location` value in the /usr/share/portage/config/repos.conf file for the Gentoo ebuild repository. It has been replaced by **[/var/db/repos/gentoo](https://wiki.gentoo.org/wiki//var/db/repos/gentoo)**. The [portageq utility](https://wiki.gentoo.org/wiki/Portageq) can be used to show the configuration of some of these locations.

For commonly used files and directories, the mapping from old to new locations is as follows:

| Old location | New location | 
|---|---|
| /usr/portage | [/var/db/repos/gentoo](https://wiki.gentoo.org/wiki//var/db/repos/gentoo) | 
| /usr/portage/licenses | [/var/db/repos/gentoo/licenses](https://wiki.gentoo.org/wiki//var/db/repos/gentoo/licenses) | 
| /usr/portage/metadata | [/var/db/repos/gentoo/metadata](https://wiki.gentoo.org/wiki//var/db/repos/gentoo/metadata) | 
| /usr/portage/profiles | [/var/db/repos/gentoo/profiles](https://wiki.gentoo.org/wiki//var/db/repos/gentoo/profiles) | 
| /usr/portage/profiles/license\_groups | [/var/db/repos/gentoo/profiles/license\_groups](https://wiki.gentoo.org/wiki//var/db/repos/gentoo/profiles/license_groups) | 
| /usr/portage/profiles/package.mask | [/var/db/repos/gentoo/profiles/package.mask](https://wiki.gentoo.org/wiki//var/db/repos/gentoo/profiles/package.mask) | 
| /usr/portage/distfiles | [/var/cache/distfiles](https://wiki.gentoo.org/wiki//var/cache/distfiles) | 
| /usr/portage/packages | [/var/cache/binpkgs](https://wiki.gentoo.org/wiki//var/cache/binpkgs) |
