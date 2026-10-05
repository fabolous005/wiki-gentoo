<!-- source: https://wiki.gentoo.org/wiki/Portage/Uninstall_Package | group: Gentoo Wiki (Main) | wiki-title: Portage/Uninstall Package -->
---
title: Portage/Uninstall Package
url: https://wiki.gentoo.org/wiki/Portage/Uninstall_Package
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-25"
fingerprint: fb9da4f8a773f15
license: CC BY-SA 4.0
---

# Portage/Uninstall Package

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

As part of [maintaining a healthy system](https://wiki.gentoo.org/wiki/Portage/Help/Maintaining_a_Gentoo_system) it's important to remove packages that are no longer needed.

This reduces the number of updates and possibly things that could unnecessarily go wrong.

## deselect

`root #``emerge --deselect <package>`
`--deselect` will tell portage to remove the package from the world file.

When a package is in the world file then it is assumed the user really needs this package working and will do everything it can to make this still happen, even stop an update.

More infomation on this can be learned at [Portage/Help/Maintaining\_a\_Gentoo\_system](https://wiki.gentoo.org/wiki/Portage/Help/Maintaining_a_Gentoo_system)

## depclean

`root #``emerge --depclean <package>`
`--depclean` asks portage to check and make sure removing the package is safe i.e. nothing depends on it and will only be removed if safe to do so. If it can't then the output given will explain what packages on the system are stopping the removal.

## clean

`--clean` will tell portage to just remove the package no matter what. This is how a user will break Gentoo and should never be run for that reason.
