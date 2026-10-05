<!-- source: https://wiki.gentoo.org/wiki/Gentle | group: Gentoo Wiki (Main) | wiki-title: Gentle -->
---
title: gentle
url: https://wiki.gentoo.org/wiki/Gentle
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-01-12"
fingerprint: "87557ce8b5e123fe"
license: CC BY-SA 4.0
---

# gentle

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**gentle** (*Gent*oo *L*azy *E*ntry) is a [metadata.xml](https://wiki.gentoo.org/wiki/Metadata.xml) generator for ebuilds.

## Installation

### USE flags


### Emerge

Install gentle:

`root #``emerge --ask app-portage/gentle`
## Usage

The gentle command takes an ebuild file as input, unpacks sources into a temporary directory and tries to guess upstream metadata.

`user $``gentle foo-1.0.ebuild`
