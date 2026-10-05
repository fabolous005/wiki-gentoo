<!-- source: https://wiki.gentoo.org/wiki/Upstream_repository_shutdowns/bitbucket.org | group: Gentoo Wiki (Main) | wiki-title: Upstream repository shutdowns/bitbucket.org -->
---
title: Upstream repository shutdowns/bitbucket.org
url: https://wiki.gentoo.org/wiki/Upstream_repository_shutdowns/bitbucket.org
hostname: gentoo.org
sitename: Upstream repository shutdowns/bitbucket.org
date: "2023-04-14"
fingerprint: cb38341684104566
license: CC BY-SA 4.0
---

# Upstream repository shutdowns/bitbucket.org

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page intends to organize required actions to prepare the [BitBucket retires support for mercurial repositories](https://bitbucket.org/blog/sunsetting-mercurial-support-in-bitbucket) ([bug #737896](https://bugs.gentoo.org/show_bug.cgi?id=737896)).

## List of packages

Use script and grep hackery:

`user $``cd /usr/portage ; dirname $(grep -r '//bitbucket.org' --exclude-dir=metadata --include=*.ebuild  -l) | sort -u | python query.py`
**`query.py`**

```
import fileinput
import portage
import re
import requests
def check_uri(uri):
    if re.search(r'//bitbucket\.org', uri):
        response = requests.get(uri)
        if response.status_code > 399:
            # print("URI failed:", uri)
            return True
    return False
p = portage.db[portage.root]["porttree"].dbapi
for package in fileinput.input():
    src_uri_affected = False
    homepage_affected = False
    pkg_info = p.cp_list(package.strip())
    ebuild_info = p.aux_get(pkg_info[-1], ["SRC_URI", "HOMEPAGE"])
    for src_uri in ebuild_info[0].split(" "):
        src_uri_affected = src_uri_affected or check_uri(src_uri)
    for homepage in ebuild_info[1].split(" "):
        homepage_affected = homepage_affected or check_uri(homepage)
    if src_uri_affected or homepage_affected:
        print(package.strip(), src_uri_affected, homepage_affected)
```
## Howto fix

Find and replace all URLs contains [https://bitbucket.org](https://bitbucket.org) in HOMEPAGE, SRC\_URI and HG\_URI\_REPO (for live ebuilds). Most of packages already moved their repositories to another sites (like Github or Heptapod).

If there no active project mirror available, you can try [https://bitbucket-archive.softwareheritage.org/](https://bitbucket-archive.softwareheritage.org/) tor download & mirror tarballs in mirror.gentoo.org. If there no tarballs, you can make them from repository snapshot from bitbucket-archive site as last resort.

## Worklist

- DONE 2020-08-19 [bug #737896](https://bugs.gentoo.org/show_bug.cgi?id=737896) Tracker for activity ([Winterheart](https://wiki.gentoo.org/index.php?title=User:Winterheart&action=edit&redlink=1) ([talk](https://wiki.gentoo.org/wiki/User_talk:Winterheart)))
- DONE 2020-08-20 List of affected packages ([Winterheart](https://wiki.gentoo.org/index.php?title=User:Winterheart&action=edit&redlink=1) ([talk](https://wiki.gentoo.org/wiki/User_talk:Winterheart)))
- DONE 2020-08-20 [Send message to gentoo-dev](https://archives.gentoo.org/gentoo-dev/message/dc1ed4aa39bf0582c4884bf391ca8888) ([Winterheart](https://wiki.gentoo.org/index.php?title=User:Winterheart&action=edit&redlink=1) ([talk](https://wiki.gentoo.org/wiki/User_talk:Winterheart)))
- TODO after 2020-??-?? Pmask
- TODO verify, that all ebuilds are mirrored. Mirror all source files (tarballs...), add link here, add link to log which packages failed to mirror.
- TODO Create statistics on the progress.
- TODO send the mail to maintainers of remaining broken ebuilds
