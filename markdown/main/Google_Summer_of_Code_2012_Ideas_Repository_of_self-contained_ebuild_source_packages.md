<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Repository_of_self-contained_ebuild_source_packages | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2012/Ideas/Repository of self-contained ebuild source packages -->
---
title: Google Summer of Code/2012/Ideas/Repository of self-contained ebuild source packages
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Repository_of_self-contained_ebuild_source_packages
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2019-01-11"
fingerprint: a763ab7d0637a5b0
license: CC BY-SA 4.0
---

# Google Summer of Code/2012/Ideas/Repository of self-contained ebuild source packages

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This proposal is similar in scope to the Cache sync proposal, except that it will focus on implementing support for repositories that host self-contained ebuild source packages that are analogous to source RPMs (SRPMs). The repository layout will be similar to existing PORTAGE\_BINHOST repositories (like those hosted at [tinderbox.dev.gentoo.org](http://tinderbox.dev.gentoo.org/default-linux/x86/)), and will include a metadata index file which is similar to [$PKGDIR/Packages](http://tinderbox.dev.gentoo.org/default-linux/x86/Packages). Each source package hosted in the repository will contain a single ebuild, its metadata, and all files it requires from the portage tree (including all inherited eclasses and any additional files such as patches from the files directory). A zip file will be a suitable container for one of these source packages.

A synchronization script will generate source packages from a source portage tree (or overlay), and it will avoid unnecessary regeneration of a given source package in cases when no relevant files have changed timestamps or been added/removed. This script will be run as often as the repository maintainer wants to synchronize with the source tree.

The syncronization script will have an option to preserve source packages that no longer exist in the source tree, perhaps updating such packages if they happen to contain outdated versions of eclasses. The ability to retain packages that no longer exist in the source tree may be useful for some cases of the stable tree idea which was proposed in [GLEP 19](http://www.gentoo.org/proj/en/glep/glep-0019.html).

In order to ensure that repository updates do not interfere with clients, it may be desirable to publish the repository as a series of snapshots that are contained in directories which are named corresponding to snapshot date/time. This will ensure that a previous snapshot is still accessible when a newer snapshot becomes available. So, clients will never have any trouble downloading a specific source package that corresponds to a metadata index which was downloaded earlier and used for a dependency calculation. Without some kind of snapshot mechanism like this (similar to [RCU](https://en.wikipedia.org/wiki/Read-copy-update)), a race condition would exist such that dependency calculations would be somewhat unreliable, and downloaded source packages might have different checksums and dependencies from those listed in a metadata index that was downloaded only a minute earlier.



| Contacts | Required Skills | 
|---|---|
|  |  |
