<!-- source: https://wiki.gentoo.org/wiki/CONFIG_PROTECT_MASK | group: Gentoo Wiki (Main) | wiki-title: CONFIG PROTECT MASK -->
---
title: CONFIG PROTECT MASK
url: https://wiki.gentoo.org/wiki/CONFIG_PROTECT_MASK
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-04-21"
fingerprint: "14279e78a2f2088a"
license: CC BY-SA 4.0
---

# CONFIG PROTECT MASK

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The `CONFIG_PROTECT_MASK` variable contains a list of files or subdirectories which will be *excluded* from the overwrite protection offered by the `[CONFIG_PROTECT](https://wiki.gentoo.org/wiki/CONFIG_PROTECT)` variable. This is to say that files or subdirectories mentioned in this variable will be *overwritten* by Portage upon (re)installation.

"Masking" is useful when locations within a certain *parent* directory are protected from automatic overwrites - *excluded* from the package manager's control, but a certain *child* location should be *included* in the package manager's control.

## See also

- [CONFIG\_PROTECT](https://wiki.gentoo.org/wiki/CONFIG_PROTECT)
- [savedconfig](https://wiki.gentoo.org/wiki/Savedconfig) — a USE flag that preserves the saved configuration files upon package updates.
- [/etc/portage/make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf) — the main configuration file used to customize the [Portage](https://wiki.gentoo.org/wiki/Portage) environment on a global level., the location [Portage](https://wiki.gentoo.org/wiki/Portage) keeps binary packages.
