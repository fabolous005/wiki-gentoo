<!-- source: https://wiki.gentoo.org/wiki/Multilib/Adding_new_ABIs | group: Gentoo Wiki (Main) | wiki-title: Multilib/Adding new ABIs -->
---
title: Multilib/Adding new ABIs
url: https://wiki.gentoo.org/wiki/Multilib/Adding_new_ABIs
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-21"
fingerprint: "4a10f1e40ce19d25"
license: CC BY-SA 4.0
---

# Multilib/Adding new ABIs

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article outlines the steps necessary to add a new ABI to gx86-multilib framework.

## Adding a new ABI group (arch)

Once multilib support is introduced in a new architecture (or group of architectures), a new ABI group needs to be introduced. The group name serves as the USE flag prefix and needs to be the same for all arches involved in particular multilib. Preferably, it should match the most generic name for the arch group.

Examples of ABI groups are: *abi\_x86* (x86 and amd64), *abi\_ppc* (ppc and ppc64), or *abi\_mips* (mips).

The following steps need to be taken in order to add a new ABI group:

1. Create a profiles/desc/abi\_xxx.desc file for the new ABI.
2. In the profiles/base/make.defaults file:
  1. Add *ABI\_XXX* (an uppercase variant of ABI group name) into the `USE_EXPAND` variable.
  2. Add *ABI\_XXX* into the `USE_EXPAND_HIDDEN` variable.
3. In the profiles/base/use.mask file:
  1. Mask all *abi\_xxx\_\** flags.
4. In profiles/arch/\* for arches involved in the multilib:
  1. Set the default value for *ABI\_XXX* in the make.defaults file.
  2. Add the native ABI entry (*abi\_xxx\_yyy*) to the use.force file.
  3. Unmask the native ABI (*-abi\_xxx\_yyy*) in the use.mask file.
  4. Set the native ABI as only ABI in *IUSE\_IMPLICIT* in the make.defaults file.
5. In profiles/arch/\* for multilib profiles of the above arches:
  1. Add *-ABI\_XXX* to `USE_EXPAND_HIDDEN` in make.defaults to make the flags visible.
  2. Unmask other supported ABIs (*-abi\_xxx\_yyy*) in the use.mask file.
6. In eclass/multilib-build.eclass:
  1. Add all new ABIs to `_MULTILIB_FLAGS` variable.
  2. Add conditionals appropriate for the new ABIs to the header template in `multilib_prepare_wrappers()` function (it is important that the *#error* contains full ABI flag since the substitution relies on it).
  3. Add ABI->USE flag mapping to `multilib_prepare_wrappers()` function.

**Before those changes are committed, they need to be tested and a full `pkgcheck scan` needs to be done in order to ensure that the new flags do not introduce dependency errors.**

## Adding new ABIs to existing groups

If a new ABI is to be added to an existing group, the following steps need to be taken:

1. Add the new ABI to the profiles/desc/abi\_xxx.desc file.
2. In profiles/base/use.mask
  1. Mask the new *abi\_xxx\_yyy* flag.
3. In profiles/arch/\* for multilib profiles.
  1. Unmask the new ABI (*-abi\_xxx\_yyy*) in the use.mask file.
4. In the eclass/multilib-build.eclass:
  1. Add the new ABI to the `_MULTILIB_FLAGS` variable.
  2. Add conditional appropriate for the new ABI to the header template in `multilib_prepare_wrappers()` function (it is important that the *#error* contains full ABI flag since the substitution relies on it).
  3. Add ABI->USE flag mapping to `multilib_prepare_wrappers()` function.

**Before those changes are committed, they need to be tested and a full `pkgcheck scan` needs to be done in order to ensure that the new flag does not introduce dependency errors.**
