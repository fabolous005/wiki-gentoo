<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:SELinux_module_not_found_during_emerge | group: Gentoo Knowledge | wiki-title: Knowledge_Base:SELinux_module_not_found_during_emerge -->
---
title: Knowledge Base:SELinux module not found during emerge
url: https://wiki.gentoo.org/wiki/Knowledge_Base:SELinux_module_not_found_during_emerge
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-04"
fingerprint: "7637367a8ca2898e"
license: CC BY-SA 4.0
---

# Knowledge Base:SELinux module not found during emerge

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Synopsis

During any `emerge` operation, Portage bails out with the following error:

`root #``emerge ...`
!!! SELinux module not found. Please verify that it was installed.

## Environment

This article is applicable to Gentoo Linux installations using a *selinux* [profile](https://wiki.gentoo.org/wiki/Portage/Profiles):

`root #``eselect profile show`
Current /etc/make.profile symlink:
  hardened/linux/amd64/selinux

## Analysis

This indicates that the Portage SELinux module is missing or damaged. Recent Portage versions provide this module out-of-the-box, but the security contexts of the necessary files might be wrong on the system.

## Resolution

Relabel the files offered by the Portage package:

`root #``rlpkg portage`
