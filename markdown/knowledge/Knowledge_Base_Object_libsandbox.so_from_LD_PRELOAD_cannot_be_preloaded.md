<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:Object_libsandbox.so_from_LD_PRELOAD_cannot_be_preloaded | group: Gentoo Knowledge | wiki-title: Knowledge_Base:Object_libsandbox.so_from_LD_PRELOAD_cannot_be_preloaded -->
---
title: Knowledge Base:Object libsandbox.so from LD PRELOAD cannot be preloaded
url: https://wiki.gentoo.org/wiki/Knowledge_Base:Object_libsandbox.so_from_LD_PRELOAD_cannot_be_preloaded
hostname: gentoo.org
sitename: Knowledge Base:Object libsandbox.so from LD PRELOAD cannot be preloaded
date: "2026-02-04"
fingerprint: ec1d1ef2c6c301cc
license: CC BY-SA 4.0
---

# Knowledge Base:Object libsandbox.so from LD PRELOAD cannot be preloaded

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Synopsis

During installation of a package, the following error message appears:

`root #``emerge ...`
\>>> Setting SELinux security labels
ERROR: ld.so: object 'libsandbox.so' from LD\_PRELOAD cannot be preloaded: ignored.

## Environment

This article is applicable to Gentoo Linux systems with a *selinux* [profile](https://wiki.gentoo.org/wiki/Portage/Profiles) set:

`root #``eselect profile show`
Current /etc/make.profile symlink:
  hardened/linux/amd64/selinux

A SELinux profile always ends with `/selinux`

## Analysis

This message should *only* occur after the *Setting SELinux security labels* message. It happens because SELinux tells glibc to disable `LD_PRELOAD` (and other environment variables that are considered potentially harmful) during domain transitions. Here, Portage calls the setfiles command (part of a SELinux installation) and as such transitions from portage\_t to setfiles\_t, which clears the environment variable.

Gentoo recommends it is safe to trust the SELinux policy here (since setfiles runs in its own confined domain anyhow) rather than updating the policy to allow transitioning between portage\_t to setfiles\_t without clearing these environment variables.

## Resolution

The error is cosmetic and can be ignored but sadly not hidden.
