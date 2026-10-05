<!-- source: https://wiki.gentoo.org/wiki/KDE/Ebuild_repository | group: Gentoo Wiki (Main) | wiki-title: KDE/Ebuild repository -->
---
title: KDE/Ebuild repository
url: https://wiki.gentoo.org/wiki/KDE/Ebuild_repository
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-03-05"
fingerprint: "9b85122e81a9bbac"
license: CC BY-SA 4.0
---

# KDE/Ebuild repository

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The [Gentoo KDE team](https://wiki.gentoo.org/wiki/Project:KDE) maintains the [KDE ebuild repository](https://gitweb.gentoo.org/proj/kde.git/). This [ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository) contains live ebuilds, upstream pre-releases, works-in-progress, and other things not yet ready or otherwise unsuitable for the main Gentoo ebuild repository. This article provides instructions on adding Gentoo's KDE ebuild development repository to a system.

## Using the ebuild repository

The easiest way to enable the KDE repository is using [eselect repository](https://wiki.gentoo.org/wiki/Eselect/Repository) which will function with emerge --sync without any extra software (besides [dev-vcs/git](https://packages.gentoo.org/packages/dev-vcs/git)):

`root #````
emerge --ask app-eselect/eselect-repository dev-vcs/git
```
`root #``eselect repository enable kde`
## Sets

In addition to the standard [packages](https://wiki.gentoo.org/wiki/KDE#Packages), a wide range of package [sets](https://gitweb.gentoo.org/proj/kde.git/tree/sets) are provided. For example:

- Installation of latest stable KDE Frameworks 6:

`root #``emerge --ask @kde-frameworks`
- Installation of KDE Plasma 6.6:

`root #``emerge --ask @kde-plasma-6.6`
- Installation of KDE Frameworks master branch:

`root #``emerge --ask @kde-frameworks-live`
- Installation of everything:

`root #``( emerge --list-sets | sed -n '/kde.*live/s/^/@/p' | { mapfile -t a; emerge -av --select "${a[@]}" <&3; } ) 3<&0`
## Keywording and unmasking

To assist users with stable systems and those who wish to test specific package versions, the ebuild repository provides a set of package.accept\_keywords, package.mask, and package.unmask files. All available files are in the [Documentation](https://gitweb.gentoo.org/proj/kde.git/tree/Documentation) directory.

For example, to keyword KDE Frameworks 6 master branch:

`root #````
cd /etc/portage/package.accept_keywords
```
`root #``ln -s /path/to/repository/kde/Documentation/package.accept_keywords/kde-frameworks-live.keywords`
## Reporting bugs

Please [file bugs](https://bugs.gentoo.org/enter_bug.cgi?product=Gentoo%20Linux&component=KDE) on Bugzilla, prepending the summary with `[kde overlay]`. Additionally, pull requests are accepted at the [Github mirror](https://github.com/gentoo/kde/).
