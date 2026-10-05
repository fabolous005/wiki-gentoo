<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:FEATURES_variable_contains_unknown_values_loadpolicy | group: Gentoo Knowledge | wiki-title: Knowledge_Base:FEATURES_variable_contains_unknown_values_loadpolicy -->
---
title: Knowledge Base:FEATURES variable contains unknown values loadpolicy
url: https://wiki.gentoo.org/wiki/Knowledge_Base:FEATURES_variable_contains_unknown_values_loadpolicy
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-04"
fingerprint: "2a6612e6ea408dcf"
license: CC BY-SA 4.0
---

# Knowledge Base:FEATURES variable contains unknown values loadpolicy

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Synopsis

When running `emerge`, the following error is shown:

`root #``emerge ...`
FEATURES variable contains unknown value(s): loadpolicy

## Environment

This article is applicable to systems who have `loadpolicy` listed in the Portage `[FEATURES](https://wiki.gentoo.org/wiki/FEATURES)` variable:

`root #``emerge --info | grep ^FEATURES | grep loadpolicy````
FEATURES="assume-digests binpkg-logs distlocks ebuild-locks fixlafiles loadpolicy 
          news parallel-fetch protect-owned sandbox selinux sesandbox sfperms
          strict unknown-features-warn unmerge-logs unmerge-orphans userfetch"
```
## Analysis

This is a remnant of the older SELinux policy module set where policy packages might require this `FEATURE` to be available. Older SELinux [profiles](https://wiki.gentoo.org/wiki/Portage/Profiles) set this variable, but this has since been removed from the tree. However, some users might still have this set in their /etc/portage/make.conf file.

## Resolution

As `loadpolicy` is not used anymore, it can be removed from the `FEATURES` variable.

1. Make sure a recent SELinux profile is used (the profile should end with `/selinux`)
2. Make sure the `FEATURES` variable, defined in /etc/portage/make.conf, does not contain `loadpolicy`
