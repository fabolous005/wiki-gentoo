<!-- source: https://wiki.gentoo.org/wiki/LLVM/Clang/Desktop_profile | group: Gentoo Wiki (Main) | wiki-title: LLVM/Clang/Desktop profile -->
---
title: LLVM/Clang/Desktop profile
url: https://wiki.gentoo.org/wiki/LLVM/Clang/Desktop_profile
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-04-30"
fingerprint: c903207f2468aaa2
license: CC BY-SA 4.0
---

# LLVM/Clang/Desktop profile

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Customized profiles**

Some users may wish to use LLVM as a desktop system. No official profiles for these exist (see [bug #913225](https://bugs.gentoo.org/show_bug.cgi?id=913225)), however it is still possible to create them locally using the following steps.

The custom LLVM profile described below, which is not the same as just using Clang and friends, is **experimental**. A 'GCC fallback' cannot be used as-is (though it is possible to use it with -stdlib=libc++ added in that case).

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

- /var/db/repos/local/profiles/llvm-desktop
- /var/db/repos/local/profiles/llvm-plasma-systemd
- /var/db/repos/local/profiles/llvm-plasma-split-usr
- /var/db/repos/local/profiles/llvm-gnome-systemd
- /var/db/repos/local/profiles/llvm-gnome-split-usr

Use the following command:

`root #``mkdir -p /var/db/repos/local/profiles/{llvm-desktop,llvm-plasma-systemd,llvm-plasma-split-usr,llvm-gnome-systemd,llvm-gnome-split-usr,}`

#### llvm-desktop

Create the following files:

**`/var/db/repos/local/profiles/llvm-desktop/eapi`**

**`/var/db/repos/local/profiles/llvm-desktop/parent`**

#### llvm-plasma-systemd

Create the following files:

**`/var/db/repos/local/profiles/llvm-plasma-systemd/eapi`**

**`/var/db/repos/local/profiles/llvm-plasma-systemd/parent`**

#### llvm-plasma-split-usr

Create the following files:

**`/var/db/repos/local/profiles/llvm-plasma-split-usr/eapi`**

**`/var/db/repos/local/profiles/llvm-plasma-split-usr/parent`**

#### llvm-gnome-systemd

Create the following files:

**`/var/db/repos/local/profiles/llvm-gnome-systemd/eapi`**

**`/var/db/repos/local/profiles/llvm-gnome-systemd/parent`**

#### llvm-gnome-split-usr

Create the following files:

**`/var/db/repos/local/profiles/llvm-gnome-split-usr/eapi`**

**`/var/db/repos/local/profiles/llvm-gnome-split-usr/parent`**

## Selecting the profile

The new profiles should now appear in eselect profile list. Enjoy!
