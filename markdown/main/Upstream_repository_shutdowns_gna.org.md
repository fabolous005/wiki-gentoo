<!-- source: https://wiki.gentoo.org/wiki/Upstream_repository_shutdowns/gna.org | group: Gentoo Wiki (Main) | wiki-title: Upstream repository shutdowns/gna.org -->
---
title: Upstream repository shutdowns/gna.org
url: https://wiki.gentoo.org/wiki/Upstream_repository_shutdowns/gna.org
hostname: gentoo.org
sitename: Upstream repository shutdowns/gna.org
date: "2024-12-03"
fingerprint: "6a04564f04612661"
license: CC BY-SA 4.0
---

# Upstream repository shutdowns/gna.org

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page intends to organize required actions to prepare the [announced shutdown of gna.org](http://web.archive.org/web/20170327102552/https://mail.gna.org/public/project/2016-11/msg00001.html). ([bug #612500](https://bugs.gentoo.org/show_bug.cgi?id=612500))

## List of packages

Use the MD5 cache:

`user $``cd $(portageq get_repo_path / gentoo)/metadata/md5-cache; grep -lR  "gna.org"  | sort | uniq`
## Worklist

- DONE 2017-03-13 [bug #612500](https://bugs.gentoo.org/show_bug.cgi?id=612500) report on bugzilla (Harald Weiner)
- DONE 2017-06-11 [bug #612500](https://bugs.gentoo.org/show_bug.cgi?id=612500) create tracker bug on bugzilla ([jonasstein](https://wiki.gentoo.org/index.php?title=User:Jonasstein&action=edit&redlink=1))
- DONE 2017-06-11 Find out which packages need to be fixed exactly. ([jonasstein](https://wiki.gentoo.org/index.php?title=User:Jonasstein&action=edit&redlink=1))
- DONE 2017-06-11 send [mail to gentoo-dev](https://archives.gentoo.org/gentoo-dev/message/264736c3c9fdd7724dc7bc7e3f05ec91) ([jonasstein](https://wiki.gentoo.org/index.php?title=User:Jonasstein&action=edit&redlink=1))
- TODO after 2017-??-?? Pmask
- TODO verify, that all ebuilds are mirrored. Mirror all source files (tar balls...), add link here, add link to log which packages failed to mirror.
- TODO a shut down repository makes repoman really sad, so repoman should tell the user about its feelings ;-)  [bug #601476](https://bugs.gentoo.org/show_bug.cgi?id=601476)
- TODO Create statistics on the progress.
- TODO send the mail to maintainers of remaining broken ebuilds

#### packages to fix (2024-12-03)

app-misc/mx5000tools-0.1.2\_p20190613
net-nds/smbldap-tools-0.9.10-r1
