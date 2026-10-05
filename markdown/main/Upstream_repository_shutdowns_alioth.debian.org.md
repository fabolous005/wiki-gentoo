<!-- source: https://wiki.gentoo.org/wiki/Upstream_repository_shutdowns/alioth.debian.org | group: Gentoo Wiki (Main) | wiki-title: Upstream repository shutdowns/alioth.debian.org -->
---
title: Upstream repository shutdowns/alioth.debian.org
url: https://wiki.gentoo.org/wiki/Upstream_repository_shutdowns/alioth.debian.org
hostname: gentoo.org
sitename: Upstream repository shutdowns/alioth.debian.org
date: "2022-07-11"
fingerprint: "4f29166f0431a2a0"
license: CC BY-SA 4.0
---

# Upstream repository shutdowns/alioth.debian.org

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Background

## Worklist

- DONE 2019-04-12 [bug #683184](https://bugs.gentoo.org/show_bug.cgi?id=683184) create tracker bug on bugzilla ([jonasstein](https://wiki.gentoo.org/index.php?title=User:Jonasstein&action=edit&redlink=1))
- DONE Find out which packages need to be fixed exactly.
- DONE make bugs for individual packages and add to tracker bug
- TODO mail to gentoo-dev
- DONE verify, that all ebuilds are mirrored. Mirror all source files (tar balls...), add link here, add link to log which packages failed to mirror.
- TODO a shut down repository makes repoman really sad, so repoman should tell the user about its feelings ;-)  [bug #601476](https://bugs.gentoo.org/show_bug.cgi?id=601476)
- TODO Create statistics on the progress.
- TODO send the mail to maintainers of remaining broken ebuilds

## List of packages (2022-07-05)

Used the following commands to compile a list of packages with either the old host in SRC\_URI or HOMEPAGE:

`user $``cd /var/db/repos/gentoo/metadata/md5-cache; grep "URI=" -R | grep "alioth\.debian\.org"``user $``cd /var/db/repos/gentoo/metadata/md5-cache; grep "HOMEPAGE=" -R | grep "alioth\.debian\.org"`
app-dicts/myspell-nn-2.0.10 (only SRC\_URI)
app-laptop/pommed-1.39-r2 (only SRC\_URI)
app-emacs/analog-1.9.99 (only HOMEPAGE)
