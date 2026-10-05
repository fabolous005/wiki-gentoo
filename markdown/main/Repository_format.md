<!-- source: https://wiki.gentoo.org/wiki/Repository_format | group: Gentoo Wiki (Main) | wiki-title: Repository format -->
---
title: Repository format
url: https://wiki.gentoo.org/wiki/Repository_format
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2020-05-17"
fingerprint: f9b3149f9ca272e8
license: CC BY-SA 4.0
---

# Repository format

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

A quick reference to Gentoo ebuild repository (overlay) format.

Each ebuild repository has its own `` repository location`. It is set in [repos.conf](https://wiki.gentoo.org/wiki//etc/portage/repos.conf) and can be found with the [portageq](https://wiki.gentoo.org/wiki/Portageq) utility:` ``

`user $``portageq get_repo_path / gentoo`
/var/db/repos/gentoo

## See also

- [/etc/portage](https://wiki.gentoo.org/wiki//etc/portage) — the primary configuration directory for [Portage](https://wiki.gentoo.org/wiki/Portage), Gentoo's package manager.
- [/etc/portage/repos.conf](https://wiki.gentoo.org/wiki//etc/portage/repos.conf) — specifies current [Portage](https://wiki.gentoo.org/wiki/Portage) configured repositories' location and settings
