<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2023/Ideas/Better_testing_infrastructure_for_Gentoo_Prefix | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2023/Ideas/Better testing infrastructure for Gentoo Prefix -->
---
title: Google Summer of Code/2023/Ideas/Better testing infrastructure for Gentoo Prefix
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2023/Ideas/Better_testing_infrastructure_for_Gentoo_Prefix
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-02-23"
fingerprint: "513fe8e60c034338"
license: CC BY-SA 4.0
---

# Google Summer of Code/2023/Ideas/Better testing infrastructure for Gentoo Prefix

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Gentoo has a CI system (the tinderbox) to automatically test installation of packages with various configurations. However, during development, changes to ebuilds may break packages in ways that only affect Gentoo prefix and which are not caught by the regular testing. It would be great to have some CI infrastructure in place for Gentoo Prefix bootstraps, but also a similar CI system to the tinderbox running not on vanilla Gentoo, but on Gentoo prefix, to quickly identify problems with new packages as they are introduced into the tree.


| Contacts | Required Skills | 
|---|---|
|  |  | 
| Expected Project Size | Expected Outcomes | 
| 350 hours | Running CI system testing Gentoo prefix packages and automated bootstraps of Gentoo Prefix on a few host platforms. | 
| Project Difficulty |  | 
| medium |  |
