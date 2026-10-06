<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Improved_binary_package_support | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2012/Ideas/Improved binary package support -->
---
title: Google Summer of Code/2012/Ideas/Improved binary package support
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Improved_binary_package_support
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-11-09"
fingerprint: f6c08d35d1279d90
license: CC BY-SA 4.0
---

# Google Summer of Code/2012/Ideas/Improved binary package support

[Google Summer of Code](https://wiki.gentoo.org/wiki/Google_Summer_of_Code) |

[2012](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012) |

[Ideas](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## [Improved binary package support]

Gentoo better for derived binary distros. One of them is more intelligent handling of library versions with binpkgs (and installed packages, which are a form of binpkg). For example, it's possible to build a binpkg against an old version of a library, then install it against a new version and have it be broken by default because of a shared-library version bump. It's also possible to break reverse ABI dependencies when upgrading a package, and there is currently no convenient way for package managers to detect such breakage in advance. Ideally, a package would have a way to specify its ABI dependencies in the built state instead of just which versions it can build against from source. It is possible to create an ABI dependency abstraction that is flexible enough to cover all possible kinds of ABI dependencies. Using an ABI abstraction, it will not matter whether or not there exists a specific soname to be referenced by dependencies. See [bug #192319](https://bugs.gentoo.org/show_bug.cgi?id=192319).

Another problem is saving binpkgs with different USE flag and other build settings on the same host. See [bug #150031](https://bugs.gentoo.org/show_bug.cgi?id=150031). The way forward is one or more hashes of the metadata. A third problem is the lack of binpkg support for the kernel. This could be changed through modifying the kernel eclass to support a binary USE flag that also did configuration & build, or perhaps some kind of genkernel modification, or both. See [bug #154495](https://bugs.gentoo.org/show_bug.cgi?id=154495).

Two other minor problems:

- Compilation related messages are thrown to user when installing a binary package. This should be avoided somehow. Bad developers habit.
- It would be nice to have elog output stored in /var/db for later consumption and perhaps, have it embedded in xpak metadata. This would improve PackageKit support, which doesn't allow any output from package phases during install.



| Contacts | Required Skills | 
|---|---|
|  |  |
