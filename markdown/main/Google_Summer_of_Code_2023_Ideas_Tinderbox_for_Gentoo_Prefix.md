<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2023/Ideas/Tinderbox_for_Gentoo_Prefix | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2023/Ideas/Tinderbox for Gentoo Prefix -->
---
title: Google Summer of Code/2023/Ideas/Tinderbox for Gentoo Prefix
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2023/Ideas/Tinderbox_for_Gentoo_Prefix
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-02-09"
fingerprint: "51bdeee00c034338"
license: CC BY-SA 4.0
---

# Google Summer of Code/2023/Ideas/Tinderbox for Gentoo Prefix

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Gentoo has a CI system to automatically test installation of packages with various configurations. However, during development changes to ebuilds may break packages in ways that only affect Gentoo prefix. It would be great to have some CI infrastructure in place for Gentoo Prefix bootstraps, but also a tinderbox running not on vanilla Gentoo, but on Gentoo prefix to quickly identify problems with new packages as they are introduced into the tree.


| Contacts | Required Skills | 
|---|---|
| [mailto:amadio@gentoo.org](mailto:amadio@gentoo.org) | Bash, Python | 
| Expected Project Size | Expected Outcomes | 
| 250 hours | Running tinderbox testing Gentoo prefix packages and CI for automated bootstrap of Gentoo on a few host platforms | 
| Project Difficulty |  | 
| medium |  |
