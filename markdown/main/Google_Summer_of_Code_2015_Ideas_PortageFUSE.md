<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2015/Ideas/PortageFUSE | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2015/Ideas/PortageFUSE -->
---
title: Google Summer of Code/2015/Ideas/PortageFUSE
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2015/Ideas/PortageFUSE
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-11-09"
fingerprint: "1d0d8e6903b41c28"
license: CC BY-SA 4.0
---

# Google Summer of Code/2015/Ideas/PortageFUSE

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Create a filesystem based on FUSE for Gentoo's package tree and overlays. Possible tasks could include:

For users:

- Autogenerate metadata-chache in fuse (e.g. for overlays).
- Store filenames only, rsync real files on demand.
- Examine support for squashfs via fuse.

For developers:

- Remanifest when accessing package directories.
- Support different views by changing the directory structure based on metadata.xml (e.g. maintainer/category/package/).
- As we have a git-mirror for cvs ready, switch between both worlds in the same tree (hide CVS / .git directories).



| Contacts | Required Skills | 
|---|---|
|  |  |
