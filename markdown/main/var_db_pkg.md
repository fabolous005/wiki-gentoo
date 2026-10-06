<!-- source: https://wiki.gentoo.org/wiki//var/db/pkg | group: Gentoo Wiki (Main) | wiki-title: /var/db/pkg -->
---
title: "/var/db/pkg"
url: https://wiki.gentoo.org/wiki//var/db/pkg
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-06-20"
fingerprint: "1f8f8e0f8637a943"
license: CC BY-SA 4.0
---

# /var/db/pkg

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

[Portage](https://wiki.gentoo.org/wiki/Portage) stores information about installed packages in /var/db/pkg. It can be thought of as a database, and contains all the information needed to query a given package and manage its lifecycle.

## Directory structure

At the top level, its layout is similar to that of an [ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository): a directory is present for each category, inside which is a directory for each installed package belonging to that category.

For example, if [app-shells/bash-5.2\_p37](https://packages.gentoo.org/packages/app-shells/bash) is installed, information associated with that installation would be stored in /var/db/pkg/app-shells/bash-5.2\_p37.

### Information on each package

A directory describing an installed package contains a full set of information describing how the package was built and its resulting impact on the system. Some of this information includes:
