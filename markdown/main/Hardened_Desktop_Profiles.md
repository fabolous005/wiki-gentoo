<!-- source: https://wiki.gentoo.org/wiki/Hardened_Desktop_Profiles | group: Gentoo Wiki (Main) | wiki-title: Hardened Desktop Profiles -->
---
title: Hardened Desktop Profiles
url: https://wiki.gentoo.org/wiki/Hardened_Desktop_Profiles
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-04-29"
fingerprint: c903207fa468aa8a
license: CC BY-SA 4.0
---

# Hardened Desktop Profiles

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Customized profiles**

A lot of people ask about combining the hardened and desktop profiles. Here's how!

## GNOME and KDE

For these desktop environments, please see [GNOME/Guide/Hardened\_GNOME\_Profiles](https://wiki.gentoo.org/wiki/GNOME/Guide/Hardened_GNOME_Profiles) and [KDE/Hardened\_KDE\_Plasma\_profile](https://wiki.gentoo.org/wiki/KDE/Hardened_KDE_Plasma_profile).

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

- /var/db/repos/local/profiles/hardened-desktop
- /var/db/repos/local/profiles/hardened-desktop-systemd
- /var/db/repos/local/profiles/hardened-desktop-split-usr

Use the following command:

`root #``mkdir -p /var/db/repos/local/profiles/{hardened-desktop,hardened-desktop-systemd,hardened-desktop-split-usr,}`

#### hardened-desktop

Create the following files:

**`/var/db/repos/local/profiles/hardened-desktop/eapi`**

**`/var/db/repos/local/profiles/hardened-desktop/parent`**

#### hardened-desktop-systemd

Create the following files:

**`/var/db/repos/local/profiles/hardened-desktop-systemd/eapi`**

**`/var/db/repos/local/profiles/hardened-desktop-systemd/parent`**

#### hardened-desktop-split-usr

Create the following files:

**`/var/db/repos/local/profiles/hardened-desktop-split-usr/eapi`**

**`/var/db/repos/local/profiles/hardened-desktop-split-usr/parent`**

## Selecting the profile

The new profiles should now appear in eselect profile list. Enjoy!
