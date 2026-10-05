<!-- source: https://wiki.gentoo.org/wiki/KDE/Hardened_KDE_Plasma_profile | group: Gentoo Wiki (Main) | wiki-title: KDE/Hardened KDE Plasma profile -->
---
title: KDE/Hardened KDE Plasma profile
url: https://wiki.gentoo.org/wiki/KDE/Hardened_KDE_Plasma_profile
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-04-29"
fingerprint: c903107fa46cab8a
license: CC BY-SA 4.0
---

# KDE/Hardened KDE Plasma profile

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Customized profiles**

A lot of people ask about combining the hardened and KDE Plasma profiles. Here's how!

## Create a local repository

A [local repository](https://wiki.gentoo.org/wiki/Creating_an_ebuild_repository) is needed for the custom profile to be created.

First, install [app-eselect/eselect-repository](https://packages.gentoo.org/packages/app-eselect/eselect-repository):

`root #``emerge --ask app-eselect/eselect-repository`
Create a local repository:

`root #``eselect repository create local`
### Set up the repository layout

It's recommended to make use of a [Portage extension](https://wiki.gentoo.org/wiki/Repository_format/metadata/layout.conf#profile-formats) for the repository as it simplifies configuration.

**`/var/db/repos/local/metadata/layout.conf`**

## Create the profile

### profiles.desc

profiles.desc provides a list of profiles for eselect profile list to consume:

**`/var/db/repos/local/profiles/profiles.desc`**

### The profile itself

Create the following directories (adjust as needed):

- /var/db/repos/local/profiles/hardened-plasma
- /var/db/repos/local/profiles/hardened-plasma-systemd
- /var/db/repos/local/profiles/hardened-plasma-split-usr

Use the following command:

`root #``mkdir -p /var/db/repos/local/profiles/{hardened-plasma,hardened-plasma-systemd,hardened-plasma-split-usr,}`

#### hardened-plasma

Create the following files:

**`/var/db/repos/local/profiles/hardened-plasma/eapi`**

**`/var/db/repos/local/profiles/hardened-plasma/parent`**

#### hardened-plasma-systemd

Create the following files:

**`/var/db/repos/local/profiles/hardened-plasma-systemd/eapi`**

**`/var/db/repos/local/profiles/hardened-plasma-systemd/parent`**

#### hardened-plasma-split-usr

Create the following files:

**`/var/db/repos/local/profiles/hardened-plasma-split-usr/eapi`**

**`/var/db/repos/local/profiles/hardened-plasma-split-usr/parent`**

## Selecting the profile

The new profiles should now appear in eselect profile list. Enjoy!
