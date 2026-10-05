<!-- source: https://wiki.gentoo.org/wiki/USE_ORDER | group: Gentoo Wiki (Main) | wiki-title: USE ORDER -->
---
title: USE_ORDER
url: https://wiki.gentoo.org/wiki/USE_ORDER
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2019-06-05"
fingerprint: ede3167af6ee15af
license: CC BY-SA 4.0
---

# USE\_ORDER

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


This variable is a colon delimited list of where USE flags are pulled from when building packages. If the USE flag is listed in multiple sources, the first one has highest priority.

| Keyword | USE Source | 
|---|---|
| env | From the current environment variables (USE and those listed in `USE_EXPAND`) | 
| pkg | /etc/portage/package.use | 
| conf | /etc/portage/make.conf | 
| defaults | From make.defaults and package.use in the profile (e.g. /etc/portage/make.profile/package.use) (see man 5 portage). | 
| pkginternal | From the ebuild itself | 
| repo | From make.defaults and package.use in the repo's profiles/ top directory (e.g. /var/db/repos/gentoo/profiles/package.use) (see man 5 portage). | 
| env.d | /etc/env.d | 

The default USE ordering is `USE_ORDER="env:pkg:conf:defaults:pkginternal:repo:env.d"`.
