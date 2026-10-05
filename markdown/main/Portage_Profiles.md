<!-- source: https://wiki.gentoo.org/wiki/Portage/Profiles | group: Gentoo Wiki (Main) | wiki-title: Portage/Profiles -->
---
title: Portage/Profiles
url: https://wiki.gentoo.org/wiki/Portage/Profiles
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-09"
fingerprint: "819339b7e251b70e"
license: CC BY-SA 4.0
---

# Portage/Profiles

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Customized profiles**

**Profiles** are a core [Portage](https://wiki.gentoo.org/wiki/Portage) feature that allow the highly flexible Gentoo metadistribution to be primed for use on target systems. Profiles provide minimal workable baselines to fit system usage-requirements, and allow for a high-degree of customization. A profile is chosen [at installation time](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Choosing_the_right_profile), though profiles can often be changed if requirements evolve.

Profiles determine compatible subsets of the metadistribution to provide the user with composable features. By compartmentalizing mutually exclusive components and configurations, they provide one of the pillars of Gentoo's extreme flexibility: 14 supported architectures, many subarchitectures, several platforms, a choice of libc, init systems, toolchains, optional hardening, combined with all the supported Gentoo use-cases - from server, to workstation, to embedded - require such a robust architecture as provided by profiles.

Technically, profiles define core low-level parameters on different system architectures, they can determine package availability, specify default states for [USE flags](https://wiki.gentoo.org/wiki/USE_flag), set default values for [/etc/portage/make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf) variables, adjust the [set of system packages](<https://wiki.gentoo.org/wiki/System_set_(Portage)>), determine default toolchains and core system libraries, amongst other things.

New profiles are [made available](https://wiki.gentoo.org/wiki/Upgrading_Gentoo) when there are fundamental changes to the way Gentoo works, though such releases can be years apart: when the 23.0 profile was released, the previous profile (17.1) was nearly 6 years old. Be aware that many profiles are experimental, and thus can require involved and sometimes difficult work, so stable profiles should be used unless there is a specific requirement not to.

Some profiles can be switched easily when required, though certain profile changes will require more steps than just switching the profile, and a few specific profiles can be extremely difficult to switch between, and it is not possible to change to profiles with a different [ABI](https://wiki.gentoo.org/wiki/Application_binary_interface) (e.g. pure LLVM or musl profiles) without a reinstall.

Profiles are defined on an [ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository) basis; the ones from [the main repository](https://wiki.gentoo.org/wiki//var/db/repos/gentoo/profiles) are maintained by the Gentoo developers, but users can [define their own](https://wiki.gentoo.org/wiki/Portage/Profiles/Custom_profiles).

## Profile stability

General-usage profiles are marked stable, only use other profiles if fully-cognizant of non-stable profile usage.

### Stable (stable)

Stable profiles are fully tested and should provide a *just works* experience. Gentoo's CI checks that stable profiles have a sound dependency graph. All stable profiles are marked with "*(stable)*".

### In-development (dev)

During the development process that aims to create stable profiles, work in progress profiles are marked as "*(dev)*". Gentoo's CI checks these profiles for issues in the dependency graph, but issues only a warning and not an error.

Users running these should only be doing so if they have a certain need that can only be resolved by using them.

### Experimental (exp)

An experimental profile is, as the name suggests, an experiment. It may or may not result in a new "permanent" profile. These profiles are marked "*(exp)*".

Many profiles of exotic architectures are marked as experimental.

Experimental profiles should work, as they have undergone some level of testing. However, the fixes to make it run are normally included in testing keyword packages so a user of these should either be running `~ARCH` globally or at the very least be prepared to use [/etc/portage/package.accept\_keywords](https://wiki.gentoo.org/wiki//etc/portage/package.accept_keywords) to pull in the latest versions and fill a bug to [stable request](https://wiki.gentoo.org/wiki/Stable_request) the package if found to be safe by the user.

In short, expect to have random issues when using these profiles.

## Available profiles

This section explains what some commonly encountered profiles are used for.

The full list of profiles for all architectures is [available in the Gentoo repository](https://gitweb.gentoo.org/repo/gentoo.git/tree/profiles/profiles.desc). As evident in the profile names from that list, the current profiles version for most architectures is version 23.0.

The most basic profiles available are `default/linux/<architecture>/23.0` and `default/linux/<architecture>/23.0/systemd`. These can for example be used for server systems, in container installations, or for machines that will have only a command line interface.

### OpenRC and systemd profiles

Profiles are split depending on which [init system](https://wiki.gentoo.org/wiki/Init_system) they use, into [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) and [systemd](https://wiki.gentoo.org/wiki/Systemd) variants. systemd profiles include the *systemd* qualifier, whereas OpenRC profiles are the ones that include no init system qualifier.

### Desktop profiles

Desktop profiles include the *desktop* qualifier, and are to be used for any graphical installation, whether using a [desktop environment](https://wiki.gentoo.org/wiki/Desktop_environment), [window manager](https://wiki.gentoo.org/wiki/Window_manager), [xorg](https://wiki.gentoo.org/wiki/Xorg), [wayland](https://wiki.gentoo.org/wiki/Wayland), etc.

The desktop profiles provide [GNOME](https://wiki.gentoo.org/wiki/GNOME) and [Plasma](https://wiki.gentoo.org/wiki/KDE) variants - see the corresponding articles for details of setup using the correct profile.

These profiles only provide the bare minimum for desktop system usage, and are still as flexible as the less qualified profiles.

### Hardened profiles

Hardened profiles are available for [Hardened Gentoo](https://wiki.gentoo.org/wiki/Hardened_Gentoo) installations, though these require a more technical setup, and such systems have usage-constraints. These aren't used as frequently as non-hardened profiles.

### amd64 multilib and no-multilib profiles

For the **amd64** architecture, Gentoo provides support for the x86 architecture and it's x86\_64 instruction set extension. For very specific use cases, users fully-cognizant of the constraints of constricting support to x86\_64 only have the possibility to use the no-multilib profiles.

Note that no-multilib profiles are only provided for very specific use-cases, and are not advised for general use. Using these profiles needlessly can result in complex and time-consuming issues whenever still-common non x86\_64 software needs to be used. Moving from no-multilib to standard profiles is practically impossible when building from source, and will usually require a full-system re-installation unless binpkgs can be used, so it's particularly important to never use these profiles unless they are specifically required.

### split-usr profiles

[Split /usr](https://wiki.gentoo.org/wiki/Split_/usr) profiles exist to support systems from before all data from /bin, /sbin, /lib, and /lib64 was merged into the /usr/bin, /usr/lib, and /usr/lib64 directories.

### Experimental and in-development profiles

These profiles are often used by [testers](https://wiki.gentoo.org/wiki/Project:AMD64_Arch_Testers). They should only be used when specifically required, as stated above, for general-use pick a *stable* profile.

## Switching profiles

Sometimes when the usage of a system changes, or when realizing that another profile is a better fit, it can be necessary to [switch profiles](https://wiki.gentoo.org/wiki/Portage/Profiles/Switching_profiles).

After the release of a new profile, profiles can need to be upgraded - see the [upgrading profiles](https://wiki.gentoo.org#Upgrading_profiles) section.

## Upgrading profiles

The user is informed of the availability of a profile update by the publication of a [news item](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Reading_news_items), which will be reported after a Gentoo ebuild repository sync. This will include detailed instructions that should be properly followed. A profile upgrade is a non trivial operation and, as always, systems should be regularly [backed up](https://wiki.gentoo.org/wiki/Backup) - particularly before system changes.

See the article on [upgrading Gentoo](https://wiki.gentoo.org/wiki/Upgrading_Gentoo#Keeping_up_with_new_profiles) for information on profile updates.

## See also

- [/var/db/repos/gentoo/profiles](https://wiki.gentoo.org/wiki//var/db/repos/gentoo/profiles) — a directory that contains global [profiles](<https://wiki.gentoo.org/wiki/Profile_(Portage)>) that are controlled by developers of the Gentoo ebuild repository (gentoo.git)
- [Multilib layout](https://wiki.gentoo.org/wiki/Project:AMD64/Multilib_layout) for **amd64** architecture.
- [How to change to SELinux](https://wiki.gentoo.org/wiki/SELinux/Installation)
- [Upgrading Gentoo](https://wiki.gentoo.org/wiki/Upgrading_Gentoo#Updating_to_23.0_profile)

## External resources

- [Gentoo news: Profile upgrade to version 23.0 available](https://www.gentoo.org/support/news-items/2024-03-22-new-23-profiles.html)
- [Gentoo: profiles and keywords rather than releases](https://blogs.gentoo.org/mgorny/2024/08/20/gentoo-profiles-and-keywords-rather-than-releases/)

For developers:

- [Profiles section of Gentoo devmanual](https://devmanual.gentoo.org/profiles/index.html)
- [Profiles section](https://projects.gentoo.org/pms/8/pms.html#x1-410005) of [Package Manager Specification](https://wiki.gentoo.org/wiki/Package_Manager_Specification)
- [Profiles directory section](https://projects.gentoo.org/pms/8/pms.html#x1-320004.4) of Package Manager Specification
