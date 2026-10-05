<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2017/Ideas/Porthole_Updates | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2017/Ideas/Porthole Updates -->
---
title: Google Summer of Code/2017/Ideas/Porthole Updates
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2017/Ideas/Porthole_Updates
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-11-09"
fingerprint: f2219248da3da5d8
license: CC BY-SA 4.0
---

# Google Summer of Code/2017/Ideas/Porthole Updates

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Porthole is a Gentoo tree browser with basic emerge capabilities. Its primary focus is for package browsing and viewing various information about the packages.

It is is need of updates to bring it back in line with changes to the Gentoo ebuild eco-system and the package managers code used to gather the data it displays.

Main areas needing work are:

- python 3 porting
- portage backend updates for api changes
- pkgcore backend updates for api changes
- gtk3 porting
- changelog view needs a git module, rsync changelogs is now a sub-repo, so need a way to sync those.

Other code needing completion or just new:

- finish modifying the primary view windows to allow plug-ins to make use of it's windows to display data
- Update exisiting plug-in modules
- create a new gentoolkit plugin for many of the common queries.
- create a layman plugin
- other new ideas



| Contacts | Required Skills | 
|---|---|
|  |  |
