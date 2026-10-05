<!-- source: https://wiki.gentoo.org/wiki/System_set_(Portage) | group: Gentoo Wiki (Main) | wiki-title: System set (Portage) -->
---
title: System set (Portage)
url: https://wiki.gentoo.org/wiki/System_set_(Portage)
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-04"
fingerprint: "39fa2e87c38706"
license: CC BY-SA 4.0
---

# System set (Portage)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The **system set**, also referred to as **@system** in Portage development, contains the software packages required for a standard Gentoo Linux installation to run properly.

The system packages are defined by the Gentoo [profiles](https://wiki.gentoo.org/wiki/Portage/Profiles) (through the [packages](https://gitweb.gentoo.org/repo/gentoo.git/tree/profiles/base/packages) files). An end-user can easily see which packages are seen as part of the system set by running the following emerge command:

`user $``emerge --pretend @system`
## See also

- [Package sets](https://wiki.gentoo.org/wiki/Package_sets) — describes package sets in high detail and includes a list of all typically available sets on a Gentoo system.
- [/etc/portage/sets](https://wiki.gentoo.org/wiki//etc/portage/sets) — an optional directory that is used to create user defined package sets
- [Profile set (Portage)](<https://wiki.gentoo.org/wiki/Profile_set_(Portage)>) — contains the software packages selected by the selected profile.
- [World set (Portage)](<https://wiki.gentoo.org/wiki/World_set_(Portage)>) — the combination of the [*system set*], the [*selected set*](<https://wiki.gentoo.org/wiki/Selected_set_(Portage)>), and the *@profile set*.
- [Selected set (Portage)](<https://wiki.gentoo.org/wiki/Selected_set_(Portage)>) — contains the packages the admin has explicitly installed
- [Selected-packages\_set\_(Portage)](<https://wiki.gentoo.org/wiki/Selected-packages_set_(Portage)>) — contains the user-selected "world" packages that are listed in the /var/lib/portage/world file.
