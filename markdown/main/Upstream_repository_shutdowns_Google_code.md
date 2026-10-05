<!-- source: https://wiki.gentoo.org/wiki/Upstream_repository_shutdowns/Google_code | group: Gentoo Wiki (Main) | wiki-title: Upstream repository shutdowns/Google code -->
---
title: Upstream repository shutdowns/Google code
url: https://wiki.gentoo.org/wiki/Upstream_repository_shutdowns/Google_code
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-07-11"
fingerprint: c6b904ef006033e8
license: CC BY-SA 4.0
---

# Upstream repository shutdowns/Google code

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page intends to organize required actions to prepare the [announced](https://opensource.googleblog.com/2015/03/farewell-to-google-code.html) shutdown of Google code. ([bug #544092](https://bugs.gentoo.org/show_bug.cgi?id=544092))

## Urgency

A wrong URL in an ebuild is a bug. It is necessary to prevent a flood of manually written tickets in Bugzilla. We do not have the manpower and the tools to manage 500 new, manually written bugs plus duplicates efficiently. Hence it is important that the ebuilds are fixed soon.

"These tarballs will be available throughout the rest of **2016**."

## List of packages

Use the md5 cache:

`user $``cd /var/db/repos/gentoo/metadata/md5-cache; grep "URI=" -R | grep "googlecode\.com"`
[Output (googlecode-shutdown.txt)](https://dev.gentoo.org/~jstein/googlecode-shutdown.txt) of the command as run on 2016-11-05.

### Who maintains how many broken packages and what are their names?

`user $````
 ( curl https://dev.gentoo.org/~jstein/googlecode-shutdown.txt |  cut -f1 -d":" | while IFS="" read -r arg; do echo -n "$arg: " ; equery meta -mH $arg 2>/dev/null | tr "\n" " "; echo ; done ) | tee /tmp/package_owners.txt;
sed 's/:\s*$/: maintainer-needed/;s/\@/<at>/g' < /tmp/package_owners.txt > /tmp/owners_obfu.txt
```
`user $` `cut -d" " -f 2-  < /tmp/owners_obfu.txt | tr " " "\n" | grep "^\w" | sort | uniq -c | sort -n -k 1  > /tmp/owner_histogram.txt``user $` `sort -k 2 < /tmp/owners_obfu.txt > /tmp/packages_by_owner.txt`
- Who maintains how many broken packages? [owner\_histogram.txt](https://dev.gentoo.org/~jstein/owner_histogram.txt) (NumberOfPackages Contact)
- Who maintains which broken package? [packages\_by\_owner.txt](https://dev.gentoo.org/~jstein/packages_by_owner.txt) (PackageName Contact)
- Updated list online: [http://gentoo.levelnine.at/wwwtest/sort-by-filter/code.google.com.txt](http://gentoo.levelnine.at/wwwtest/sort-by-filter/code.google.com.txt)

### Alternative Script

This script only checks the `SRC_URI` fields and nothing else.

Download the script:

`user $` `wget https://raw.githubusercontent.com/gktrk/gentoo-scripts/master/src_uri_match.py``user $` `chmod +x ./src_uri_match.py`
To list all the ebuilds with maintainers:

`user $` `./src_uri_match.py`
To list all the packages for a single maintainer:

`user $` `./src_uri_match.py -n -m foo@gentoo.org`
## Brainstorming

- First step: just 'clone' all `SRC_URI` to a random web server, and fix the current one to point at it.
- Second step: figure out if the project has moved or has been frozen.

## Mirrors

**Q:** Are there sources on googlecode which we must not mirror?

**A:** No:

`user $``cd /var/db/repos/gentoo/metadata/md5-cache; grep "URI=.*googlecode\.com" -R -l | xargs grep "RESTRICT"`
Returns all RESTRICT variables of the googlecode ebuilds. They do not use restrictions which would forbid mirroring in general.

## Statistics

![](https://wiki.gentoo.org/images/thumb/8/8c/Google_code_shutdown_statistics.png/350px-Google_code_shutdown_statistics.png)

In the case of [BerliOS](https://bugs.gentoo.org/show_bug.cgi?id=494678) it took about one year to fix 50% of the ebuilds. Perhaps it is interesting to keep some statistics.

Command to collect date and number of packages with googlecode in the \*URI variable:

`user $``printf "%s,%s\n" "$(date --iso)" "$(grep "URI=.*googlecode\.com.*" -R "$(portageq get_repo_path / gentoo)"/metadata/md5-cache | wc -l)"`
## Worklist

- DONE 2015-03-22 [bug #544092](https://bugs.gentoo.org/show_bug.cgi?id=544092) create tracker bug on bugzilla ([jonasstein](https://wiki.gentoo.org/index.php?title=User:Jonasstein&action=edit&redlink=1))
- DONE 2016-10-28 Find out which packages need to be fixed exactly. Need bash scripts. ([jonasstein](https://wiki.gentoo.org/index.php?title=User:Jonasstein&action=edit&redlink=1))
- DONE 2016-10-28 Create statistics on the progress. ([dilfridge](https://wiki.gentoo.org/wiki/User:Dilfridge),[jonasstein](https://wiki.gentoo.org/index.php?title=User:Jonasstein&action=edit&redlink=1))
- DONE 2016-11-05 send [mail to gentoo-dev](https://archives.gentoo.org/gentoo-dev/message/3ae58dc716b8c85304a9f6bd6e4f01a1) ([jonasstein](https://wiki.gentoo.org/index.php?title=User:Jonasstein&action=edit&redlink=1))
- DONE 2016-11-05 create script to generate maintainer:package lists ([kentnl](https://wiki.gentoo.org/wiki/User:Kentnl))
- DONE 2016-11-12 create the statistics on some historical dates for the plot. ([dilfridge](https://wiki.gentoo.org/wiki/User:Dilfridge))
- DONE 2016-11-24 send the mail to maintainers of remaining broken ebuilds ([jonasstein](https://wiki.gentoo.org/index.php?title=User:Jonasstein&action=edit&redlink=1))
- DONE 2016-12-20 find out, which packages do not allow a mirror in the license: **none** ([jonasstein](https://wiki.gentoo.org/index.php?title=User:Jonasstein&action=edit&redlink=1))
- TODO after 2016-12-01 create bug tickets for remaining broken ebuilds
- DONE verify, that all ebuilds are mirrored. Mirror all source files (tar balls...), add link here, add link to log which packages failed to mirror.
- TODO find out how to proceed with mirrors if upstream is dead. Do we have a policy on that?
- TODO a google code repository makes repoman really sad, so repoman should tell the user about its feelings ;-)  [bug #601476](https://bugs.gentoo.org/show_bug.cgi?id=601476)
