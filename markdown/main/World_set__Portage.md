<!-- source: https://wiki.gentoo.org/wiki/World_set_(Portage) | group: Gentoo Wiki (Main) | wiki-title: World set (Portage) -->
---
title: World set (Portage)
url: https://wiki.gentoo.org/wiki/World_set_(Portage)
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-06"
fingerprint: "1031e23eaec693af"
license: CC BY-SA 4.0
---

# World set (Portage)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The **world set**, also referred to as **@world**, is the combination of the [*system set*](<https://wiki.gentoo.org/wiki/System_set_(Portage)>), the [*selected set*](<https://wiki.gentoo.org/wiki/Selected_set_(Portage)>), and the *@profile set*.

Later, when a world update is requested (through emerge -uDN @world or similar command), Portage will use the world set as the base for its update calculations. A long command version of the command would be as follows:

`root #``emerge --update --deep --newuse @world`
## See also

- [System set (Portage)](<https://wiki.gentoo.org/wiki/System_set_(Portage)>) — the software packages required for a standard Gentoo Linux installation to run properly.
- [Selected set (Portage)](<https://wiki.gentoo.org/wiki/Selected_set_(Portage)>) — contains the packages the admin has explicitly installed
- [Selected-packages set (Portage)](<https://wiki.gentoo.org/wiki/Selected-packages_set_(Portage)>) — contains the user-selected "world" packages that are listed in the /var/lib/portage/world file.
- [/etc/portage/sets](https://wiki.gentoo.org/wiki//etc/portage/sets) — an optional directory that is used to create user defined package sets
- [Knowledge Base: Remove orphaned packages](https://wiki.gentoo.org/wiki/Knowledge_Base:Remove_orphaned_packages#Analysis) - The process of finding orphaned dependencies explained.
