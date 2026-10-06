<!-- source: https://wiki.gentoo.org/wiki/Overlay:Kde-sunset | group: Gentoo Overlay | wiki-title: Overlay:Kde-sunset -->
---
title: Overlay:kde-sunset
url: https://wiki.gentoo.org/wiki/Overlay:Kde-sunset
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-08-16"
fingerprint: "8135d0dabc6bfbcc"
license: CC BY-SA 4.0
---

# Overlay:kde-sunset

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

## Contents

The `kde-sunset` overlay contains old KDE-related packages that have been removed from the main Gentoo tree. These packages include:

- Qt1 (historical copy adapted to compile on modern systems)
- KDE 1 base applications (adapted to compile on modern systems)
- KDE SC 4.14.3
- KDE Plasma 4
- Plasmoids for KDE Plasma 4
- KDELibs 4 related software

## Status

The packages in the overlay are completely unsupported by upstream and may contain security vulnerabilities or break without warning. The overlay itself has no dedicated maintainer so there is no guarantee that it is in a consistent state at any given time.

If you wish to modify the overlay, contact the [Proxy Maintainers project](https://wiki.gentoo.org/wiki/Project:Proxy_Maintainers) for one-off changes. If you are interested in co-maintaining it, [file a bug](https://bugs.gentoo.org/enter_bug.cgi?product=Gentoo%20Infrastructure&component=Gentoo%20Overlays) asking for commit access.

## Installation

As it does not have an active maintainer, the `kde-sunset` overlay is no longer available and must be added via [app-eselect/eselect-repository](https://packages.gentoo.org/packages/app-eselect/eselect-repository) or manually:

`root #``eselect repository add kde-sunset git git://anongit.gentoo.org/proj/kde-sunset.git`
or

FILE **`/etc/portage/repos.conf/kde-sunset.conf`**

```
[kde-sunset]
location = /usr/local/overlay/kde-sunset
sync-type = git
sync-uri =  git://anongit.gentoo.org/proj/kde-sunset.git
auto-sync = yes
```
## See also

- [Ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository) — a file-structure  that can provide packages for installation on a Gentoo system.
