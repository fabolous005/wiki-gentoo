<!-- source: https://wiki.gentoo.org/wiki/Configuration_file_management | group: Gentoo Wiki (Main) | wiki-title: Configuration file management -->
---
title: Configuration file management
url: https://wiki.gentoo.org/wiki/Configuration_file_management
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-07"
fingerprint: b50c5f6e8ee79b81
license: CC BY-SA 4.0
---

# Configuration file management

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Managing [configuration files](https://en.wikipedia.org/wiki/Configuration_file) is a key skill in system administration. Updated configuration files for core system software suites occasionally ship with new software releases. When available via a package upgrade, the newly updated configuration files from upstream will need to be reconciled with existing local configuration files in a sane manner.

If the system administrator has not modified the existing local configuration files, then generally newer files can be 'clobbered' (overwritten) over older versions. Things become a little more complicated when existing configuration files are modified beyond their original content, and a newer file contains changes that should be merged into the original. That is where configuration management systems become helpful.

## Available software

### Included with Portage

[Portage](https://wiki.gentoo.org/wiki/Portage) ships with one tool in order to help system administrators manage software on the system.

- [dispatch-conf](https://wiki.gentoo.org/wiki/Dispatch-conf)
- dispatch-conf is the primary and default configuration file management tool in Gentoo. Usage detail can be found on in [Portage tools#dispatch-conf](https://wiki.gentoo.org/wiki/Handbook:AMD64/Portage/Tools#dispatch-conf) section of the handbook.

### Alternative software

- [cfg-update](https://wiki.gentoo.org/wiki/Cfg-update)
- An alternative to the default config file management tools deployed with Portage. cfg-update includes an integrated backup system which does not require an external version control system.

## See also

- [CONFIG\_PROTECT](https://wiki.gentoo.org/wiki/CONFIG_PROTECT)
- [savedconfig](https://wiki.gentoo.org/wiki/Savedconfig) — a USE flag that preserves the saved configuration files upon package updates.
