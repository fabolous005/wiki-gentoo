<!-- source: https://wiki.gentoo.org/wiki/Selected_set_(Portage) | group: Gentoo Wiki (Main) | wiki-title: Selected set (Portage) -->
---
title: Selected set (Portage)
url: https://wiki.gentoo.org/wiki/Selected_set_(Portage)
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-22"
fingerprint: "1039f2beaed39324"
license: CC BY-SA 4.0
---

# Selected set (Portage)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The **selected set** contains the packages the admin has explicitly installed, those specified in the world and world\_sets files. The selected set, together with the profile set and the [system set](<https://wiki.gentoo.org/wiki/System_set_(Portage)>), make up the [world set](<https://wiki.gentoo.org/wiki/World_set_(Portage)>).
With a couple of exceptions, Portage will register selected set package atoms to the [/var/lib/portage/world](<https://wiki.gentoo.org/wiki/World_file_(Portage)>) file.

## See also

- [System set (Portage)](<https://wiki.gentoo.org/wiki/System_set_(Portage)>) — the software packages required for a standard Gentoo Linux installation to run properly.
- [World set (Portage)](<https://wiki.gentoo.org/wiki/World_set_(Portage)>) — the combination of the [*system set*](<https://wiki.gentoo.org/wiki/System_set_(Portage)>), the [*selected set*], and the *@profile set*.
- [Selected-packages\_set\_(Portage)](<https://wiki.gentoo.org/wiki/Selected-packages_set_(Portage)>) — contains the user-selected "world" packages that are listed in the /var/lib/portage/world file.
- [/etc/portage/sets](https://wiki.gentoo.org/wiki//etc/portage/sets) — an optional directory that is used to create user defined package sets
- [Profile (Portage)](<https://wiki.gentoo.org/wiki/Profile_(Portage)>) — a core [Portage](https://wiki.gentoo.org/wiki/Portage) feature that allow the highly flexible Gentoo metadistribution to be primed for use on target systems

## External resources

- [https://forums.gentoo.org/viewtopic-t-1042252-start-11.html](https://forums.gentoo.org/viewtopic-t-1042252-start-11.html) - A forum thread that discusses dependency calculation using the world file.
