<!-- source: https://wiki.gentoo.org/wiki/Eclass | group: Gentoo Wiki (Main) | wiki-title: Eclass -->
---
title: eclass
url: https://wiki.gentoo.org/wiki/Eclass
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-08-07"
fingerprint: "1a931a0c8bbb6238"
license: CC BY-SA 4.0
---

# eclass

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


An eclass is a collection of code which can be used by more than one [ebuild](https://wiki.gentoo.org/wiki/Ebuild).<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> At the time of writing, all eclasses live in the [eclass/ directory](https://gitweb.gentoo.org/repo/gentoo.git/tree/eclass) of the [Gentoo ebuild repository](https://gitweb.gentoo.org/repo/gentoo.git/tree/).

To use an eclass, it must be *inherited*. This is done via the `inherit` function, which is provided by ebuild.sh. The inherit statement must come at the top of the ebuild, before any functions.[\[2\]](https://wiki.gentoo.org#cite_note-2)

FILE **`autotools-example-9999.ebuild`****eclass usage snippet**

```
# Copyright 2022 Gentoo Authors
# Distributed under the terms of the GNU General Public License v2
EAPI=8
inherit autotools
DESCRIPTION="Example ebuild using the autotools eclass"
# ...
```
