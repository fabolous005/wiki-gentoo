<!-- source: https://wiki.gentoo.org/wiki/No_homepage | group: Gentoo Wiki (Main) | wiki-title: No homepage -->
---
title: No homepage
url: https://wiki.gentoo.org/wiki/No_homepage
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-12-25"
fingerprint: d51fe531a77a12e
license: CC BY-SA 4.0
---

# No homepage

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article deals with packages without a valid upstream homepage.

## Information for users

Packages link to this page because their upstream homepage has been removed or never existed.

Sometimes the original upstream URL may still be found as another link in the `HOMEPAGE` [variable](https://devmanual.gentoo.org/ebuild-writing/variables/), or could be commented out in the ebuild.

Developers usually try to figure out whether upstream really vanished, or if the homepage just got moved. Things may still get missed however, websites can come back online, etc. **If you happen to know where upstream went or if the original homepage is online again**, please open a bug report at [https://bugs.gentoo.org/](https://bugs.gentoo.org/) and tell the developers about it!

A missing upstream does not automatically lead to the removal of a package. But if bugs are piling up for a package where upstream is missing, Gentoo developers may decide to remove the package rather than fixing bugs.

The following resources might be helpful:

## Information for developers

If the homepage of a package you maintain is unavailable and no new location can be found, please update the according variable in every ebuild:

HOMEPAGE="https://wiki.gentoo.org/wiki/No\_homepage"

[Virtual packages](https://devmanual.gentoo.org/general-concepts/virtuals/) are an exception as they do not define `HOMEPAGE` variables.

## See also

- [Basic guide to write Gentoo Ebuilds](https://wiki.gentoo.org/wiki/Basic_guide_to_write_Gentoo_Ebuilds) — getting started writing **[ebuilds](https://wiki.gentoo.org/wiki/Ebuild)**, to harness the power of [Portage](https://wiki.gentoo.org/wiki/Portage), to install and manage even more software.
