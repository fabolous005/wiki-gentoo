<!-- source: https://wiki.gentoo.org/wiki/Upstream_repository_shutdowns/codehaus.org | group: Gentoo Wiki (Main) | wiki-title: Upstream repository shutdowns/codehaus.org -->
---
title: Upstream repository shutdowns/codehaus.org
url: https://wiki.gentoo.org/wiki/Upstream_repository_shutdowns/codehaus.org
hostname: gentoo.org
sitename: Upstream repository shutdowns/codehaus.org
date: "2022-07-11"
fingerprint: "9a0c1ccd042126e0"
license: CC BY-SA 4.0
---

# Upstream repository shutdowns/codehaus.org

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page intends to organize required actions to prepare the [announced](https://web.archive.org/web/20150601030130/http://www.codehaus.org/termination.html) shutdown of codehaus.org. ([bug #550054](https://bugs.gentoo.org/show_bug.cgi?id=550054))

## List of packages

Use the MD5 cache:

`user $``cd $(portageq get_repo_path / gentoo)/metadata/md5-cache; grep -lR  "codehaus.org"  | sort | uniq`
## Worklist

- DONE 2015-05-21 [bug #550054](https://bugs.gentoo.org/show_bug.cgi?id=550054) report on bugzilla ([Monsieurp](https://wiki.gentoo.org/wiki/User:Monsieurp))
- DONE 2017-07-04 [bug #550054](https://bugs.gentoo.org/show_bug.cgi?id=550054) create tracker bug on bugzilla ([jonasstein](https://wiki.gentoo.org/index.php?title=User:Jonasstein&action=edit&redlink=1))
- DONE 2017-07-04 Find out which packages need to be fixed exactly. ([Monsieurp](https://wiki.gentoo.org/wiki/User:Monsieurp),[jonasstein](https://wiki.gentoo.org/index.php?title=User:Jonasstein&action=edit&redlink=1))
- TODO after 2017-??-?? Pmask
- TODO verify, that all ebuilds are mirrored. Mirror all source files (tar balls...), add link here, add link to log which packages failed to mirror.
- TODO a shut down repository makes repoman really sad, so repoman should tell the user about its feelings ;-)  [bug #601476](https://bugs.gentoo.org/show_bug.cgi?id=601476)
- TODO Create statistics on the progress.
- TODO send the mail to maintainers of remaining broken ebuilds

## Forks/Redirects

- please verify forks before trusting them. Excerpt from the announcement: "If you would like your projects links redirected then please see our redirector [repository](https://github.com/codehaus) - create a sane pull request and it will be added to the redirection system - you may add some 302s initially, but ultimately all redirects will be amended to 301s over time."
