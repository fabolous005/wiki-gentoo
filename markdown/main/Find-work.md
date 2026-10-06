<!-- source: https://wiki.gentoo.org/wiki/Find-work | group: Gentoo Wiki (Main) | wiki-title: Find-work -->
---
title: find-work
url: https://wiki.gentoo.org/wiki/Find-work
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-05"
fingerprint: "819fa2a888e3e1de"
license: CC BY-SA 4.0
---

# find-work

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**find-work**  is a utility to discover what you can do for Gentoo as a package maintainer. It contains filters to show only packages you might be interested in. It was created by [User:CyberTailor](https://wiki.gentoo.org/wiki/User:CyberTailor).

## Installation

### Emerge

find-work is currently only available in the GURU repository, so [enable that repo](https://wiki.gentoo.org/wiki/Project:GURU/Information_for_End_Users#Adding_the_GURU_repository) before attempting installation.

`root #``emerge --ask dev-util/find-work`
## Usage

List packages with a new version available upstream (only supports [gentoo](https://repos.gentoo.org/#gentoo), [guru](https://repos.gentoo.org/#guru), [pentoo](https://repos.gentoo.org/#pentoo) and [science](https://repos.gentoo.org/#science)):

`user $``find-work -IO repology -r gentoo outdated`
List packages with open bugs:

`user $``find-work -IO bugzilla list`
List packages with QA issues:

`user $``find-work -IO pkgcheck -r gentoo scan`
