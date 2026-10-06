<!-- source: https://wiki.gentoo.org/wiki/Gentoo_Alt/Contributor%27s_Guide/Policy | group: Gentoo Wiki (Main) | wiki-title: Gentoo Alt/Contributor's Guide/Policy -->
---
title: Gentoo Alt/Contributor's Guide/Policy
url: https://wiki.gentoo.org/wiki/Gentoo_Alt/Contributor%27s_Guide/Policy
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-01-02"
fingerprint: "8759fd60ddb0cff7"
license: CC BY-SA 4.0
---

# Gentoo Alt/Contributor's Guide/Policy

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Deprecated article**

This article is **deprecated (obsolete)**. Contents are <u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

This document explains policies for Gentoo/Alt developers.

## Patches

Patches added to the overlay (and to portage) should follow some basic policies, meant to simplify the process of merging them upstream, and without breaking stuff. This allows to drop the patches when new versions are released.

The first important step is to make sure that the patch applies unconditionally. This means that after applying the patch, the sources work fine on every system, and not just the one you're patching for, and also that when adding code to workaround system problems, it should be protected with the right checks (preprocessor or autoconf) so that they don't get in the way when they are not needed.

Patches that change the entire building system of a package are usually discouraged, try to find a compromise with upstream developers, even if that would mean having an unusable package in the time being.

## Behavior changes

All the behavior changes that might affect Gentoo Linux users must always be announced on gentoo-alt at least, and on gentoo-dev if they might affect development practices.

The behavior changes should also be tested on the gentoo-alt overlay so that they don't hit the normal (Gentoo/Linux) users before testing.
