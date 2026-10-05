<!-- source: https://wiki.gentoo.org/wiki/Portage/Profiles/Structure | group: Gentoo Wiki (Main) | wiki-title: Portage/Profiles/Structure -->
---
title: Portage/Profiles/Structure
url: https://wiki.gentoo.org/wiki/Portage/Profiles/Structure
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-03-19"
fingerprint: c3a318762722a702
license: CC BY-SA 4.0
---

# Portage/Profiles/Structure

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Customized profiles**

## What is a profile

/var/db/repos/gentoo/profiles store *profile* subdirectory configuration files. The symbolic link /etc/portage/make.profile describes the current profile's running architecture, default [USE flags](https://wiki.gentoo.org/wiki/USE_flag), and [*@system* package](<https://wiki.gentoo.org/wiki/System_set_(Portage)>) ebuilds.

The [Package Manager Specification](https://wiki.gentoo.org/wiki/Package_Manager_Specification) defines how profiles set or unset USE flags (globally or per package); unconditionally force set or unset USE flags (globally for the architecture's stable branch or per package); and mask packages or specified versions.

Obsoleted profiles place a deprecated file in their directory naming the new profile to upgrade to, automatically warning administrators through Portage.

/var/db/repos/gentoo/profiles/default/linux/amd64/23.0 is the default **amd64** 23.0 profile. The files in the parent directories are part of the profile as well (and are therefore shared by different subprofiles). This is why profiles are said to be *cascaded*. [Eselect profile](https://wiki.gentoo.org/wiki/Eselect#Profile) may set the profile.

There are various reasons that a new profile may be created: the release of new versions of core packages (such as [sys-apps/baselayout](https://packages.gentoo.org/packages/sys-apps/baselayout), [sys-devel/gcc](https://packages.gentoo.org/packages/sys-devel/gcc), or [sys-libs/glibc](https://packages.gentoo.org/packages/sys-libs/glibc)) that are incompatible with previous versions, a change in the default USE flags or in the virtual mappings, or maybe a change in system-wide settings.

## Profile Structure

Profiles are defined in directories contained in the profiles subdirectory of an ebuild repository, which also contains the mandatory repo\_name file that specifies the name of the ebuild repository. These are some of the files that can be present in a directory that defines a profile:

- eapi, which specifies the [EAPI](https://wiki.gentoo.org/wiki/EAPI) to use when handling the directory in question.
- packages, which specifies packages that are members of the [system](<https://wiki.gentoo.org/wiki/System_set_(Portage)>) or [profile](<https://wiki.gentoo.org/wiki/Profile_set_(Portage)>) set.
- package.mask, which specifies masked packages. The administrator can unmask a package masked by the profile using [/etc/portage/package.unmask](https://wiki.gentoo.org/wiki//etc/portage/package.unmask).
- make.defaults, with [make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf)-like variable assignments, including USE flag global default state (set or unset) and profile variables with special meaning (like `ARCH`, `USE_EXPAND`, `CONFIG_PROTECT`, `IUSE_IMPLICIT`, etc.). Administrator settings in /etc/portage/make.conf and [/etc/portage/package.use](https://wiki.gentoo.org/wiki//etc/portage/package.use) override profile settings in make.defaults.
- package.use, which defines default USE flag state on a per package basis. Administrator settings in /etc/portage/make.conf and /etc/portage/package.use override profile settings in package.use.
- use.force and use.mask, which unconditionally set or unset USE flags, overriding administrator settings in /etc/portage/make.conf and /etc/portage/package.use. If a flag is both masked and forced, the mask takes precedence.
- package.use.force and package.use.mask, which unconditionally set or unset USE flags on a per package basis, overriding administrator settings in /etc/portage/make.conf and /etc/portage/package.use, and profile settings in use.force and use.mask.
- use.stable.force, use.stable.mask, package.use.stable.force and package.use.stable.mask, which work like use.force, use.mask, package.use.force and package.use.mask, but override them for packages in the stable branch of the current architecture.

USE flag settings in profile use.\* and package.use.\* files can be overridden by the administrator with files of the same name in directory /etc/portage/profile. For complete information about profile directory structure please consult the [Package Manager Specification](https://wiki.gentoo.org/wiki/Package_Manager_Specification). A summary is also contained in [the Devmanual](https://devmanual.gentoo.org) and man portage.
