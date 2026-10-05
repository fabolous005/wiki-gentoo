<!-- source: https://wiki.gentoo.org/wiki//var/db/repos/gentoo | group: Gentoo Wiki (Main) | wiki-title: /var/db/repos/gentoo -->
---
title: "/var/db/repos/gentoo"
url: https://wiki.gentoo.org/wiki//var/db/repos/gentoo
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-11-24"
fingerprint: fb93ba9d9ca3336c
license: CC BY-SA 4.0
---

# /var/db/repos/gentoo

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**/var/db/repos/gentoo** is the default path for the [Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository), as configured in the `location` setting in the /usr/share/portage/config/repos.conf file.

If the default has been changed, current path can be shown by running the [portageq](https://wiki.gentoo.org/wiki/Portageq) utility:

`user $``portageq get_repo_path / gentoo`
/var/db/repos/gentoo

For more information, see [/etc/portage/repos.conf](https://wiki.gentoo.org/wiki//etc/portage/repos.conf).

## See also

- [/etc/portage](https://wiki.gentoo.org/wiki//etc/portage) — the primary configuration directory for [Portage](https://wiki.gentoo.org/wiki/Portage), Gentoo's package manager.
- [Repository format](https://wiki.gentoo.org/wiki/Repository_format) — A quick reference to Gentoo ebuild repository (overlay) format.
- [Gentoo ebuild repository (AMD64 handbook)](https://wiki.gentoo.org/wiki/Handbook:AMD64/Portage/Files#Gentoo_ebuild_repository)
- [bug #378603](https://bugs.gentoo.org/show_bug.cgi?id=378603)
