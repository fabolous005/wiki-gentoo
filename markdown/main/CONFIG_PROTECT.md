<!-- source: https://wiki.gentoo.org/wiki/CONFIG_PROTECT | group: Gentoo Wiki (Main) | wiki-title: CONFIG PROTECT -->
---
title: CONFIG_PROTECT
url: https://wiki.gentoo.org/wiki/CONFIG_PROTECT
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-23"
fingerprint: "3c359e7886a6178a"
license: CC BY-SA 4.0
---

# CONFIG\_PROTECT

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The `CONFIG_PROTECT` variable contains a space-delimited list of files and directories that Portage will protect from automatic modification. Proposed changes to protected configuration locations will require manual merges from the system administrator (see [dispatch-conf](https://wiki.gentoo.org/wiki/Dispatch-conf) or similar merge tools).

A current list of presently protected locations can be displayed with [portageq](https://wiki.gentoo.org/wiki/Portageq):

`user $``portageq envvar CONFIG_PROTECT`
/etc /usr/share/config /usr/share/gnupg/qualified.txt

Using portageq is a short hand alternative to running a regular expression search on verbose, informational output from the emerge command:

`user $``emerge --verbose --info | grep -E '^CONFIG_PROTECT='`
CONFIG\_PROTECT="/etc /usr/share/config /usr/share/gnupg/qualified.txt"

Files or subdirectories defined within the `CONFIG_PROTECT` can be *excluded* from protection through the `[CONFIG_PROTECT_MASK](https://wiki.gentoo.org/wiki/CONFIG_PROTECT_MASK)` variable. Masking is useful when a parent directory should be protected, but a certain child file or directory beneath it should not.

The variable has a sane default setting handled by the Portage installation and the user's Gentoo [profile](https://wiki.gentoo.org/wiki/Portage/Profiles). It can be extended through the system environment (which is often used by applications that update the variable through their /etc/env.d file) and the user's [/etc/portage/make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf) setting.

**`/etc/portage/make.conf`**

**Example`CONFIG_PROTECT` definitions**

```
CONFIG_PROTECT="/var/bind"
```
See also the [Environment variables](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/EnvVar) chapter in the Gentoo Handbook.

- [Configuration file management](https://wiki.gentoo.org/wiki/Configuration_file_management)
- [CONFIG\_PROTECT\_MASK](https://wiki.gentoo.org/wiki/CONFIG_PROTECT_MASK) — contains a list of files or subdirectories which will be *excluded* from the overwrite protection offered by the `[CONFIG_PROTECT]` variable.
- [savedconfig](https://wiki.gentoo.org/wiki/Savedconfig) — a USE flag that preserves the saved configuration files upon package updates.
- [/etc/portage/make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf) — the main configuration file used to customize the [Portage](https://wiki.gentoo.org/wiki/Portage) environment on a global level., the location [Portage](https://wiki.gentoo.org/wiki/Portage) keeps binary packages.
