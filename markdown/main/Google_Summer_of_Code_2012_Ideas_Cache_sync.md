<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Cache_sync | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2012/Ideas/Cache sync -->
---
title: Google Summer of Code/2012/Ideas/Cache sync
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Cache_sync
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-11-09"
fingerprint: b9ffd96c07b47d50
license: CC BY-SA 4.0
---

# Google Summer of Code/2012/Ideas/Cache sync

[Google Summer of Code](https://wiki.gentoo.org/wiki/Google_Summer_of_Code) |

[2012](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012) |

[Ideas](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## [Cache sync]

The portage tree and all its overlays keep growing. Right now only the official portage tree occupies more than 600Mb on a regular filesystem. However the package manager does not need the whole tree of full ebuilds, patches and manifests to perform most of its work. The idea would be to sync a smaller database or a cache of only needed information for global package manager operations, then fetch the required package only when needed. It would speed considerably tree synchronization and reduce the space occupied by portage tree (see the "Repository of Self-Contained Ebuild Source Packages" idea for an alternative approach). Currently the cache system in portage is also really slow and so is the search feature. The project could be inspired by the Debian or RPM system but with the usability and choices offered by Gentoo, and would probably include:

- design and implement automatic cache builder to be produced by a given repository/overlay
- make portage/paludis/pkgcore to do delta-sync with a local cache and fetch only the required files to be installed when requested



| Contacts | Required Skills | 
|---|---|
|  |  |
