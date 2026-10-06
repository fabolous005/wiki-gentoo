<!-- source: https://wiki.gentoo.org/wiki/GNOME/Guide/Hardened_GNOME_Profiles | group: Gentoo Wiki (Main) | wiki-title: GNOME/Guide/Hardened GNOME Profiles -->
---
title: GNOME/Guide/Hardened GNOME Profiles
url: https://wiki.gentoo.org/wiki/GNOME/Guide/Hardened_GNOME_Profiles
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-04-29"
fingerprint: d941207fa465a88e
license: CC BY-SA 4.0
---

# GNOME/Guide/Hardened GNOME Profiles

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Customized profiles**

A lot of people ask about combining the hardened and GNOME profiles. Here's how!

## Create a local repository

A [local repository](https://wiki.gentoo.org/wiki/Creating_an_ebuild_repository) is needed for the custom profile to be created.

First, install [app-eselect/eselect-repository](https://packages.gentoo.org/packages/app-eselect/eselect-repository):

`root #``emerge --ask app-eselect/eselect-repository`
Create a local repository:

`root #``eselect repository create local`
### Set up the repository layout

It's recommended to make use of a [Portage extension](https://wiki.gentoo.org/wiki/Repository_format/metadata/layout.conf#profile-formats) for the repository as it simplifies configuration.

FILE **`/var/db/repos/local/metadata/layout.conf`**

```
masters = gentoo
thin-manifests = true
# Needed for profiles parent with repo syntax
profile-formats = portage-2
```
## Create the profile

### profiles.desc

profiles.desc provides a list of profiles for eselect profile list to consume:

FILE **`/var/db/repos/local/profiles/profiles.desc`**

```
# Adjust the list below as needed, no need to make them all
amd64 hardened-gnome stable
amd64 hardened-gnome-systemd stable
amd64 hardened-gnome-split-usr stable
```
### The profile itself

Create the following directories (adjust as needed):

- /var/db/repos/local/profiles/hardened-gnome
- /var/db/repos/local/profiles/hardened-gnome-systemd
- /var/db/repos/local/profiles/hardened-gnome-split-usr

Use the following command:

`root #``mkdir -p /var/db/repos/local/profiles/{hardened-gnome,hardened-gnome-systemd,hardened-gnome-split-usr,}`

#### hardened-gnome

Create the following files:

FILE **`/var/db/repos/local/profiles/hardened-gnome/eapi`**

```
8
```
FILE **`/var/db/repos/local/profiles/hardened-gnome/parent`**

```
gentoo:default/linux/amd64/23.0/hardened
gentoo:targets/desktop/gnome
```
#### hardened-gnome-systemd

Create the following files:

FILE **`/var/db/repos/local/profiles/hardened-gnome-systemd/eapi`**

```
8
```
FILE **`/var/db/repos/local/profiles/hardened-gnome-systemd/parent`**

```
gentoo:default/linux/amd64/23.0/hardened
gentoo:targets/desktop/gnome
gentoo:targets/systemd
```
#### hardened-gnome-split-usr

Create the following files:

FILE **`/var/db/repos/local/profiles/hardened-gnome-split-usr/eapi`**

```
8
```
FILE **`/var/db/repos/local/profiles/hardened-gnome-split-usr/parent`**

```
gentoo:default/linux/amd64/23.0/hardened
gentoo:features/split-usr
gentoo:targets/desktop/gnome
```
## Selecting the profile

The new profiles should now appear in eselect profile list. Enjoy!
