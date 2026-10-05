<!-- source: https://wiki.gentoo.org/wiki/Pkgcore/Configuration | group: Gentoo Wiki (Main) | wiki-title: Pkgcore/Configuration -->
---
title: Pkgcore/Configuration
url: https://wiki.gentoo.org/wiki/Pkgcore/Configuration
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2014-10-02"
fingerprint: "96014b5e0499b330"
license: CC BY-SA 4.0
---

# Pkgcore/Configuration

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The pkgcore configuration files support various formats, default is
the [ini syntax](https://docs.python.org/3/library/configparser.html).

Each section has an associated class which defines the scope of the section. We will structure the following by these classes.

## Domain

Class: pkgcore.ebuild.domain.domain

This is the fundamental configuration object. A domain provides all configuration options (mostly by reference onto other configuration objects). By providing multiple domains these can be selected at runtime.

Important attributes:

- default (boolean, e.g. True)
- root (string, e.g. '/' )
- fetcher (reference)
- profile (reference)
- repositories (references)
- triggers (references)
- vdb (reference)

### Fetcher

Classes: pkgcore.fetch.base.fetcher and pkgcore.fetch.custom.fetcher

Configure how to obtain distfiles and where to store them.

### Profile

Classes: pkgcore.ebuild.profile.ProfileStack and pkgcore.ebuild.profile.OnDiskProfile

Configure location of profiles and which is applicable. These can be combined out multiple sources.

### Repositories

Classes: pkgcore.ebuild.repository.tree, pkgcore.ebuild.repository.slavedtree, pkgcore.binpkg.repository.tree

Metaobject for repositories. We have different flavors: ebuild.repository.tree is for the Gentoo tree, slavedtree is for overlays and binpkg.repository.tree is for prebuilt package instances (the last one is pretty much like a raw repo).

Important attributes:

- cache (references, not binpkg.repository.tree)
- raw\_repo (reference, not binpkg.repository.tree)
- parent\_repo (reference, only for slavedtree)

#### Repo-Cache

Classes: pkgcore.cache.flat\_hash.md5\_cache and pkgcore.cache.flat\_hash.database

Configure nature and location of caches. These are used for lookup of metadata and similar.

#### Raw Repository

Class: pkgcore.ebuild.repo\_objs.RepoConfig

Configure location. This is the bare repository.

Important attributes:

- syncer (reference)

##### Syncer

Class: pkgcore.sync.base.AutodetectSyncer and pkgcore.sync.rsync.rsync\_timestamp\_syncer

Configure how to sync a repository.

### VDB

Class: pkgcore.vdb.ondisk.tree

Configure location of the current system state.
