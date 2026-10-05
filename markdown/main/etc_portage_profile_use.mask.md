<!-- source: https://wiki.gentoo.org/wiki//etc/portage/profile/use.mask | group: Gentoo Wiki (Main) | wiki-title: /etc/portage/profile/use.mask -->
---
title: "/etc/portage/profile/use.mask"
url: https://wiki.gentoo.org/wiki//etc/portage/profile/use.mask
hostname: gentoo.org
sitename: "/etc/portage/profile/use.mask"
date: "2023-01-01"
fingerprint: "712239da64893a74"
license: CC BY-SA 4.0
---

# /etc/portage/profile/use.mask

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Masking

This file can be used for system wide masking of [USE flags](https://wiki.gentoo.org/wiki/USE_flag).  As an example, if [doc](https://packages.gentoo.org/useflags/doc) support is unwanted generally it can be masked by adding it here like:

**`/etc/portage/profile/use.mask`**

**use.mask example**

## Unmasking

Also this file can be used for unmasking of USE flags which were masked by the developers in the Gentoo repository. Those masks can be found either in: /var/db/repos/gentoo/profiles/arch/base/use.mask or in arch-specific file: /var/db/repos/gentoo/profiles/arch/\<Arch name>/use.mask

For unmasking such masked USE flag needs to be added here with a leading minus sign (-).

**`/etc/portage/profile/use.mask`**

**use.mask example**

## Format

- Comments begin with `#` (no inline comments).
- One `USE` flag value per line.
