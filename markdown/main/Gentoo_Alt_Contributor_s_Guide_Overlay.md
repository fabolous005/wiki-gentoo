<!-- source: https://wiki.gentoo.org/wiki/Gentoo_Alt/Contributor%27s_Guide/Overlay | group: Gentoo Wiki (Main) | wiki-title: Gentoo Alt/Contributor's Guide/Overlay -->
---
title: Gentoo Alt/Contributor's Guide/Overlay
url: https://wiki.gentoo.org/wiki/Gentoo_Alt/Contributor%27s_Guide/Overlay
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-01-02"
fingerprint: df19de4fca7c46ef
license: CC BY-SA 4.0
---

# Gentoo Alt/Contributor's Guide/Overlay

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This document informs you about the Gentoo/Alt overlay and how to maintain ebuilds in it.

## The Overlay

### Reasoning behind

Important: This guide is still a draft. It does not contain true policies about the use of the overlay as they are not being delineated yet.

Gentoo/Alt projects often need to change current ebuilds to let them know about their userland, libc or what else. These changes can be non-trivial, and should usually be held until the maintainer of the ebuild can take a look to the patches and make sure that they work as intended.

The drawback is then that Gentoo/Alt users can't make use of the ebuilds on the fly, and developers might try to do the same thing more than one time as they might not know what the other developers worked on.

The overlay idea was taken from Gentoo/FreeBSD, that used the overlay to apply patches that involved, for example, malloc patches, userland checks, and other things that should have been reviewed before going into main tree.

### In and out

The overlay is intended to work as a cache of ebuilds while they are being tested (for totally new packages) or while the patches are being reviewed by ebuild maintainers.

In the case of packages added to the overlay waiting for review by the ebuild's maintainers, their addition should be direct, with obviously the same name of the original package. It's usually better, if the patch is trivial, to open a bug and note that in the ChangeLog for the overlaid package just before adding the package to the overlay itself, unless the patches needs to be tested for a while before submitting them to the ebuild maintainer.

As soon as a patch is merged in the main tree, the ebuilds in the overlay need to go, to avoid having unneeded ebuilds there. It's also important to have the ebuilds be in sync with the main tree in case of revision bumps.

### Where

For developers, the overlay is available on Gentoo's Subversion repositories in svn+[ssh://username@svn.gentoo.org/var/svnroot/gentoo-alt/trunk/overlay](ssh://username@svn.gentoo.org/var/svnroot/gentoo-alt/trunk/overlay). It's not restricted, so please don't go over other's changes without notifying them first, unless they're causing a problem on all the systems. Always try to get a solution that does work for every project while changing the minimum quantity of code.

As the overlay is intended to be used by users, too, daily snapshots can be found on mirrors as /experimental/snapshots/portage-alt-overlay-latest.tar.bz2. The daily snapshot can be fetched and used as a portage overlay, and should be safe for use in both Gentoo/Alt and Gentoo/Linux systems. It might happen that the overlay interferes with Gentoo/Linux. In case that happens, remember to contact Gentoo/Alt developers so that the issues can be checked and resolved.

## Development

### Committing

One of the reasons why Subversion was chosen instead of CVS is that it supports atomic commits, does not expand keywords (like $Header: $) and so we can use it without using a two-step commit for manifests.

For this reason, the third line of ebuilds is safe to remain $Header: $ so that it will be safe when it's being moved to main portage.

When committing, it's important to use echangelog, also if it's an overlay to state the reasons why the ebuild is being changed. It's simple to write a bash function to get the commit done at one time:

Repoman still has a couple of issues when used in overlays, especially with Subversion, and with extra categories, as it's being done on Gentoo/Alt, but these things can be easily fixed in the future, and are just side problems.

echangelog still does not support Subversion-reposited ebuilds, so until it supports them fully, its output will be a bit limited, not telling when files are added or removed. It also returns a failure error to the caller, so it can't be checked against for failures.

### Distfiles

Because ebuilds in the overlay are public to the users, their distfiles should be present in the mirrors. As the overlay ebuilds are not parsed by mirroring scripts, the distfiles loaded directly into the local dev.gentoo.org distfiles directory will be removed when "dead" distfiles are removed.

To avoid having the distfiles removed, especially for tarballs that were created ad-hoc, it's important to whitelist the extra files. To do so, log into dev.gentoo.org and open the /space/distfiles-whitelist/current file. There, place one by line all the files you put or are going to put in the mirrors that should not be removed. The whitelist lasts for six months.
