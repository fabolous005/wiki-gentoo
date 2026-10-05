<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:Portage_fails_to_label_files_because_setfiles_does_not_work_anymore | group: Gentoo Knowledge | wiki-title: Knowledge_Base:Portage_fails_to_label_files_because_setfiles_does_not_work_anymore -->
---
title: Knowledge Base:Portage fails to label files because setfiles does not work anymore
url: https://wiki.gentoo.org/wiki/Knowledge_Base:Portage_fails_to_label_files_because_setfiles_does_not_work_anymore
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-04"
fingerprint: "3e79177a9a8e028c"
license: CC BY-SA 4.0
---

# Knowledge Base:Portage fails to label files because setfiles does not work anymore

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Synopsis

When trying to (re)install software, Portage breaks during the *Setting SELinux security labels* phase. The below error message is an example:

`root #``emerge ...`
\>>> Setting SELinux security labels
/usr/sbin/setfiles: error while loading shared libraries: libaudit.so.1: cannot
open shared object file: No such file or directory

## Environment

This article applies to Gentoo Linux installations with a *selinux* [profile](https://wiki.gentoo.org/wiki/Portage/Profiles):

`root #``eselect profile show`
Current /etc/make.profile symlink:
  hardened/linux/amd64/selinux

## Analysis

If for some reason setfiles no longer works, any install activity performed by Portage will fail since it calls setfilesduring each installation. Rebuilding [sys-apps/policycoreutils](https://packages.gentoo.org/packages/sys-apps/policycoreutils) thus fails.

## Resolution

Rebuild [sys-apps/policycoreutils](https://packages.gentoo.org/packages/sys-apps/policycoreutils) but temporarily disable SELinux support:

`root #``FEATURES="-selinux" emerge -1 policycoreutils`
Then, run rlpkg to fix the labels of the package:

`root #``rlpkg policycoreutils`
