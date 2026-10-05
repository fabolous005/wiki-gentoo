<!-- source: https://wiki.gentoo.org/wiki/DISTDIR | group: Gentoo Wiki (Main) | wiki-title: DISTDIR -->
---
title: DISTDIR
url: https://wiki.gentoo.org/wiki/DISTDIR
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-09-08"
fingerprint: "2f0e3a1e86b70302"
license: CC BY-SA 4.0
---

# DISTDIR

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The `DISTDIR` variable defines the location where Portage will store the downloaded source code archives. Its value defaults to [/var/cache/distfiles](https://wiki.gentoo.org/wiki//var/cache/distfiles) on new installations. Previously the default was ${PORTDIR}/distfiles which resolved to /usr/portage/distfiles by default.

This location, which is also often referred to as the *distfiles* location, will host the source code archives of all software installed (or attempted to install) on the system. This location is not automatically cleaned up, so users should consider using tools such as the eclean-dist command (which comes as part of the [app-portage/gentoolkit](https://packages.gentoo.org/packages/app-portage/gentoolkit) package) to keep the storage used by this location under control. Read the [Eclean article](https://wiki.gentoo.org/wiki/Eclean) for more details.

Users can set the `DISTDIR` variable in [/etc/portage/make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf):

**`/etc/portage/make.conf`**

**Using a different DISTDIR location**

```
DISTDIR=/var/gentoo/distfiles
```
To download source code archives, Portage will download files from servers defined in the `[GENTOO_MIRRORS](https://wiki.gentoo.org/wiki/GENTOO_MIRRORS)` variable first (to alleviate load on upstream project resources and for other reasons). The `SRC_URI` variable in individual [ebuilds](https://wiki.gentoo.org/wiki/Ebuild), points to the package's original source files, which is originally downloaded by the ebuild maintainers during ebuild creation and development.

Part of ebuild development is the creation of [Manifest](https://wiki.gentoo.org/wiki/Repository_format/package/Manifest) files, which ensure the upstream source files are not modified from the time they are downloaded by the ebuild developer, distributed to Gentoo's mirror system, then to their destination on the endpoint system.

To download the source archives bypassing Gentoo mirrors, set the `GENTOO_MIRRORS` variable to an empty value from the command-line. For example:

`root #``GENTOO_MIRRORS="" emerge --ask www-client/firefox`
- [Local distfiles cache](https://wiki.gentoo.org/wiki/Local_distfiles_cache) — details some approaches to setting up a local distfiles cache which will save bandwidth when several machines are running Gentoo on the same local area network.
- [PKGDIR](https://wiki.gentoo.org/wiki/PKGDIR) — is the location [Portage](https://wiki.gentoo.org/wiki/Portage) keeps binary packages.
- [Knowledge Base: Remove obsoleted distfiles](https://wiki.gentoo.org/wiki/Knowledge_Base:Remove_obsoleted_distfiles)
- [Eclean](https://wiki.gentoo.org/wiki/Eclean) — a tool for cleaning repository source files and binary packages.
