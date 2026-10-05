<!-- source: https://wiki.gentoo.org/wiki/Profile_set_(Portage) | group: Gentoo Wiki (Main) | wiki-title: Profile set (Portage) -->
---
title: Profile set (Portage)
url: https://wiki.gentoo.org/wiki/Profile_set_(Portage)
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-12"
fingerprint: "403b727e86d3a726"
license: CC BY-SA 4.0
---

# Profile set (Portage)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The **profile set**, also referred to as **@profile** in Portage development, contains the software packages selected by the selected profile.

The profile packages are defined by the Gentoo [profiles](<https://wiki.gentoo.org/wiki/Profile_(Portage)>) (through the packages files when [profile-formats in layout.conf](https://wiki.gentoo.org/wiki/Repository_format/metadata/layout.conf#profile-formats) has the **profile-set** option set). An end-user can easily see which packages are seen as part of the system set by running the following emerge command:

`user $``emerge --pretend @profile`
## See also

- [Package sets](https://wiki.gentoo.org/wiki/Package_sets) — describes package sets in high detail and includes a list of all typically available sets on a Gentoo system.
- [/etc/portage/sets](https://wiki.gentoo.org/wiki//etc/portage/sets) — an optional directory that is used to create user defined package sets
- [System set (Portage)](<https://wiki.gentoo.org/wiki/System_set_(Portage)>) — the software packages required for a standard Gentoo Linux installation to run properly.
- [World set (Portage)](<https://wiki.gentoo.org/wiki/World_set_(Portage)>) — the combination of the [*system set*](<https://wiki.gentoo.org/wiki/System_set_(Portage)>), the [*selected set*](<https://wiki.gentoo.org/wiki/Selected_set_(Portage)>), and the *@profile set*.
- [Selected set (Portage)](<https://wiki.gentoo.org/wiki/Selected_set_(Portage)>) — contains the packages the admin has explicitly installed
- [Selected-packages\_set\_(Portage)](<https://wiki.gentoo.org/wiki/Selected-packages_set_(Portage)>) — contains the user-selected "world" packages that are listed in the /var/lib/portage/world file.
- [Portage/Profiles](https://wiki.gentoo.org/wiki/Portage/Profiles) — a core [Portage](https://wiki.gentoo.org/wiki/Portage) feature that allow the highly flexible Gentoo metadistribution to be primed for use on target systems
