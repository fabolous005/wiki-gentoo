<!-- source: https://wiki.gentoo.org/wiki/MATE_(project_archive) | group: Gentoo Wiki (Main) | wiki-title: MATE (project archive) -->
---
title: MATE (project archive)
url: https://wiki.gentoo.org/wiki/MATE_(project_archive)
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-09-22"
fingerprint: "919df044c885e3ee"
license: CC BY-SA 4.0
---

# MATE (project archive)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **archived (obsolete)**. Contents are surely incorrect for current usage, and are intended for historical reference only.

TLDR:

**Do not use this article!**


This project's purpose was to provide a complete and up-to-date offering of [MATE Desktop](https://wiki.gentoo.org/wiki/MATE) packages for Gentoo. Additionally, the project existed to help facilitate cooperative efforts amongst Gentoo developers to that goal.

## State of packaging

MATE packages in Gentoo was officially available in two repositories, the main Gentoo repository and the [gentoo-mate](https://gitweb.gentoo.org/proj/gentoo-mate.git/) repository. The gentoo-mate repository was added by manually via repos.conf or via eselect repository.

The main Gentoo repository will generally hold the current stable and the next release after that. The project repository will generally contain the current stable, and the next 1-2 releases after that as well live ebuilds (9999s). As of September 11, 2016, the main Gentoo repository has 1.10 stable, and 1.12 in \~arch, intended to be stabilized the first week of October. The project repository has 1.12 and 1.14 in \~arch and 9999s for all packages.

Prior to MATE 1.12, all MATE packaging for Gentoo is GTK+2 only. MATE 1.12 adds support for GTK+3, either per package, or for all MATE packages via the gtk3 USE flag on [mate-base/mate](https://packages.gentoo.org/packages/mate-base/mate). Starting with 1.14, upstream MATE has dropped support for GTK+2 in some packages, and periodically, a package will lose its gtk3 flag as it migrates to GTK+3 only.  We understand that many MATE users on Gentoo are not pleased with some UI changes that come with GTK+3, and so we will be adding a LTS GTK+2 repository to provided the latest and last available GTK+2 versions.

## User contributors

| User | Name | Role | 
|---|---|---|
| [alexandervdm](https://wiki.gentoo.org/index.php?title=User:Alexandervdm&action=edit&redlink=1) | Alexander van der Meij | Ebuild Contributor | 
| [ernsteiswuerfel](https://wiki.gentoo.org/index.php?title=User:Ernsteiswuerfel&action=edit&redlink=1) | Erhard | PPC Arch Tester | 
| [herbmillerjr](https://wiki.gentoo.org/index.php?title=User:Herbmillerjr&action=edit&redlink=1) | Herb Miller Jr. | Desktop Tester | 
| [joost\_op](https://wiki.gentoo.org/index.php?title=User:Joost_op&action=edit&redlink=1) | Joost Ruis | ARM Arch Tester | 
| [kuzetsa](https://wiki.gentoo.org/wiki/User:Kuzetsa) | kuzetsa | Ebuild Contributor, Desktop Tester | 
| [oz123](https://wiki.gentoo.org/wiki/User:Oz123) | Oz Tiram | Ebuild Contributor | 
| [thican](https://wiki.gentoo.org/wiki/User:Thican) | thican | Desktop Tester |
