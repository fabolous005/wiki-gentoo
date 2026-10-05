<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:Masking_a_package | group: Gentoo Knowledge | wiki-title: Knowledge_Base:Masking_a_package -->
---
title: Knowledge Base:Masking a package
url: https://wiki.gentoo.org/wiki/Knowledge_Base:Masking_a_package
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-04-05"
fingerprint: "9fa03e2d82a08f80"
license: CC BY-SA 4.0
---

# Knowledge Base:Masking a package

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Synopsis

On occasion it is necessary for Portage to **not** take into account a certain package or a specific version of a certain package. Reasons for such actions could be because the new version fails to work, a major upgrade breaks compatibility with the current system in production, drops a specific, etc.

## Environment

Any Gentoo installation.

## Analysis

Portage knows how to understand package masks. System administrators can mask any package they do not desire to have on their system. The scope of the package mask is left up to the administrator; a mask can be applied to all versions of a package, specific package versions, and versions of packages from specific [ebuild repositories](https://wiki.gentoo.org/wiki/Ebuild_repository).

## Resolution

#### Masking a specific package version

To mask versions *newer* than the version `ati-drivers-12.6_beta_pre897`:

If it does not already exist, create the /etc/portage/package.mask *file*:

`root #``echo ">x11-drivers/ati-drivers-12.6_beta_pre897" >> /etc/portage/package.mask`
Alternatively, when organizing masks under a directory of files, create the /etc/portage/package.mask/ *directory*:

`root #``mkdir -p /etc/portage/package.mask/`
Create a file for the mask operation for Portage to perform:

`root #``echo ">x11-drivers/ati-drivers-12.6_beta_pre897" > /etc/portage/package.mask/ati-drivers`
#### Masking a specific package from an ebuild repository

Make sure the package.mask directory has been created:

`root #``mkdir -p /etc/portage/package.mask/`
Create a file to mask packages of the name [www-client/firefox](https://packages.gentoo.org/packages/www-client/firefox) from an ebuild repository entitled "larry":

`root #``echo "www-client/firefox::larry" > /etc/portage/package.mask/firefox`
## See also

- [/etc/portage/package.mask](https://wiki.gentoo.org/wiki//etc/portage/package.mask) — a file, or a directory of files, controlled by the system administrator that can be used to prevent certain packages from being installed.
