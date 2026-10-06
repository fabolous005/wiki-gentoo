<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2013/Ideas/Package_bug_assignment_in_Bugzilla | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2013/Ideas/Package bug assignment in Bugzilla -->
---
title: Google Summer of Code/2013/Ideas/Package bug assignment in Bugzilla
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2013/Ideas/Package_bug_assignment_in_Bugzilla
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-11-09"
fingerprint: ad290ee92872b590
license: CC BY-SA 4.0
---

# Google Summer of Code/2013/Ideas/Package bug assignment in Bugzilla

From Gentoo Wiki

\< [Google Summer of Code](https://wiki.gentoo.org/wiki/Google_Summer_of_Code) | [2013](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2013) | [Ideas](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2013/Ideas)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## [Package bug assignment in Bugzilla]

Currently, bugs are usually bound to packages through naming the relevant package in Summary. Although this works, it is quite limited. It is unsuitable for obtaining the package in an automated way or adding long lists of relevant packages in a searchable manner.

The idea is to write a Bugzilla extension which would allow bindings bugs with actual package lists. Potential uses include:

- providing auto-completion for package names,
- ability to list multiple packages without polluting the summary field,
- ability to clearly obtain all bugs relevant to a package of choice,
- providing an easy way to assign bug to the package maintainers.

Additionally, a database for package metadata cache would need to be designed, and a post-sync hook would need to be written to update it.


| Contacts | Required Skills | 
|---|---|
|  |  |
