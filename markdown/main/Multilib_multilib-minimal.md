<!-- source: https://wiki.gentoo.org/wiki/Multilib/multilib-minimal | group: Gentoo Wiki (Main) | wiki-title: Multilib/multilib-minimal -->
---
title: Multilib/multilib-minimal
url: https://wiki.gentoo.org/wiki/Multilib/multilib-minimal
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-06-02"
fingerprint: "7ad33b09e055cd63"
license: CC BY-SA 4.0
---

# Multilib/multilib-minimal

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The **multilib-minimal** is the eclass that is suggested to be used for most of the packages supporting multilib. It provides a friendly, sub-phase function-based API to build packages. It is built on top of [multilib-build](https://wiki.gentoo.org/wiki/Project:Multilib/multilib-build) eclass and therefore all [common APIs](https://wiki.gentoo.org/wiki/Project:Multilib/multilib-build#Common_API_reference) are provided.

## Description

The multilib-minimal eclass is built around the concept of sub-phase functions. It exports each of the following phase functions, and implements it in terms of sub-phase functions:

| Phase | Per-ABI sub-phase | Common sub-phase | 
|---|---|---|
| src\_configure |  | - | 
| src\_compile |  | - | 
| src\_test |  | - | 
| src\_install |  |  | 

Each of the *per-ABI sub-phase functions* is executed for each enabled multilib ABI, in a dedicated build directory (that is different from *${S}*), with an environment set up for building for the particular ABI.

By default, the per-ABI sub-phase performs the same task as the original phase function, e.g. *multilib\_src\_compile* calls *emake*. However, each of those functions can be overriden in ebuild to perform different code. When adding multilib support to existing packages, it is common to start by renaming phase functions to their respective multilib sub-phases.

The additional *common sub-phase* can be used to perform tasks that are irrelevant to multilib. It is run inside *${S}*, and defaults to installing documentation. When overriden in an ebuild, *einstalldocs* can be used to reproduce the original behavior.

Please note that the ebuild needs to explicitly ensure the correctness of commands run in build directory. This usually requires either:

- setting *ECONF\_SOURCE=${S}* for src\_configure() when using autotools so that an out-of-source build will be performed,
- calling [multilib\_copy\_sources](https://wiki.gentoo.org/wiki/Project:Multilib/multilib-build#multilib_copy_sources) in src\_prepare() to create separate copies of sources in build directories.
