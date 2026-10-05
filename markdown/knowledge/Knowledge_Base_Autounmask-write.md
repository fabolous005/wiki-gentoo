<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:Autounmask-write | group: Gentoo Knowledge | wiki-title: Knowledge_Base:Autounmask-write -->
---
title: Knowledge Base:Autounmask-write
url: https://wiki.gentoo.org/wiki/Knowledge_Base:Autounmask-write
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-01-04"
fingerprint: "1fe6ae0d2ae65744"
license: CC BY-SA 4.0
---

# Knowledge Base:Autounmask-write

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The emerge option `--autounmask-write` writes `autounmask` features into the corresponding config files.

If the corresponding package.\* is a directory, changes are written to the lexicographically last file in this directory. In order to prevent your manually managed files from being edited, you can create files just for the purpose of autounmask-write:

`root #``for d in /etc/portage/package.*; do touch $d/zzz_autounmask; done` ## See also

- [Accepting a keyword for a single package](https://wiki.gentoo.org/wiki/Knowledge_Base:Accepting_a_keyword_for_a_single_package) in the Knowledge Base.
- [Knowledge Base:Unmasking a package](https://wiki.gentoo.org/wiki/Knowledge_Base:Unmasking_a_package)
