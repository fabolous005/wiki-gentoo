<!-- source: https://wiki.gentoo.org/wiki//etc/portage | group: Gentoo Wiki (Main) | wiki-title: /etc/portage -->
---
title: "/etc/portage"
url: https://wiki.gentoo.org/wiki//etc/portage
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-11-06"
fingerprint: "1013de9dbeeea2a0"
license: CC BY-SA 4.0
---

# /etc/portage

[/etc](https://wiki.gentoo.org/wiki//etc)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

/etc/portage is the primary configuration directory for [Portage](https://wiki.gentoo.org/wiki/Portage), Gentoo's package manager.

This article can be used as a quick reference for commonly used files in the Portage configuration directory. For a complete list, see the [Portage man page](https://dev.gentoo.org/~zmedico/portage/doc/man/portage.5.html).

Some of these file names are simply optional (e.g. eselect-repo.conf) or conventional, and may vary.

[/etc/portage] 

├── [bashrc](https://wiki.gentoo.org/wiki//etc/portage/bashrc) 

├── [binrepos.conf](https://wiki.gentoo.org/wiki//etc/portage/binrepos.conf) 

├── [categories](https://wiki.gentoo.org/wiki//etc/portage/categories) 

├── [color.map](https://wiki.gentoo.org/wiki//etc/portage/color.map) 

├── [env](https://wiki.gentoo.org/wiki//etc/portage/package.env) 

│   └── ... 

├── [license\_groups](https://wiki.gentoo.org/wiki//etc/portage/license_groups) 

├── [make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf) 

├── [make.profile](https://wiki.gentoo.org/wiki//etc/portage/make.profile) 

│   └── ... 

├── [mirrors](https://wiki.gentoo.org/wiki//etc/portage/mirrors) 

├── [modules](https://wiki.gentoo.org/wiki//etc/portage/modules) 

├── [package.accept\_keywords](https://wiki.gentoo.org/wiki//etc/portage/package.accept_keywords) 

├── [package.env](https://wiki.gentoo.org/wiki//etc/portage/package.env) 

├── [package.license](https://wiki.gentoo.org/wiki//etc/portage/package.license) 

├── [package.mask](https://wiki.gentoo.org/wiki//etc/portage/package.mask) 

├── [package.properties](https://wiki.gentoo.org/wiki//etc/portage/package.properties) 

├── [package.unmask](https://wiki.gentoo.org/wiki//etc/portage/package.unmask) 

├── [package.use](https://wiki.gentoo.org/wiki//etc/portage/package.use) 

├── [patches](https://wiki.gentoo.org/wiki//etc/portage/patches) 

│   └── ... 

├── [postsync.d](https://wiki.gentoo.org/wiki/Handbook:AMD64/Portage/Advanced#Executing_tasks_after_ebuild_repository_syncs) 

│   └── ... 

├── profile 

│   ├── [use.mask](https://wiki.gentoo.org/wiki//etc/portage/profile/use.mask) 

│   ├── [package.use.mask](https://wiki.gentoo.org/wiki//etc/portage/profile/package.use.mask) 

│   └── [package.provided](https://wiki.gentoo.org/wiki//etc/portage/profile/package.provided) 

├── [repo.postsync.d](https://wiki.gentoo.org/wiki//etc/portage/repo.postsync.d) 

│   └── ... 

├── [repos.conf](https://wiki.gentoo.org/wiki//etc/portage/repos.conf) 

│   ├── [eselect-repo.conf](https://wiki.gentoo.org/wiki/Eselect/Repository#Files) 

│   ├── gentoo.conf 

│   ├── [local.conf](https://wiki.gentoo.org/wiki/Creating_an_ebuild_repository) 

│   └── ... 

├── [savedconfig](https://wiki.gentoo.org/wiki//etc/portage/savedconfig) 

│   └── ... 

└── [sets](https://wiki.gentoo.org/wiki//etc/portage/sets) 


`└── ...`

## See also

- [Repository format](https://wiki.gentoo.org/wiki/Repository_format) — A quick reference to Gentoo ebuild repository (overlay) format.
- [/var/db/repos/gentoo](https://wiki.gentoo.org/wiki//var/db/repos/gentoo) — the default path for the [Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository), as configured in the `location` setting in the /usr/share/portage/config/repos.conf file.
