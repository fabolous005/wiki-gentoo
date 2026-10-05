<!-- source: https://wiki.gentoo.org/wiki/Multilib/multilib-build | group: Gentoo Wiki (Main) | wiki-title: Multilib/multilib-build -->
---
title: Multilib/multilib-build
url: https://wiki.gentoo.org/wiki/Multilib/multilib-build
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-06-02"
fingerprint: "2ef31b1ced69c320"
license: CC BY-SA 4.0
---

# Multilib/multilib-build

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

*multilib-build* is the common eclass that is used to build all multilib building logic in Gentoo. This eclass is seldom inherited directly. Instead, its API is exposed via other eclasses such as *multilib-minimal*.

The *multilib-build* eclass adds multilib-specific USE flags to *IUSE*. It does not export any phase functions or dependencies. However, [MULTILIB\_USEDEP](https://wiki.gentoo.org#MULTILIB_USEDEP) variable is provided to help construct proper multilib dependencies.

## Common API reference

### Variables set by ebuilds

#### MULTILIB\_COMPAT

Optional. Must be set above the *inherit* line.

If defined, specifies the list of ABI flags corresponding to the ABIs supported by ebuild. The eclass will emit flags and enable support only for the specified ABIs. Note that the variable affects all architectures — i.e. if you specify flags for *abi\_x86* only, the ebuild will cease to support non-x86 arches.

This option can be used for binary packages where upstream provides variants only for some of the ABIs. For example, on x86 binary packages would commonly restrict to *abi\_x86\_32* and *abi\_x86\_64* (i.e. leave out unsupported *abi\_x86\_x32*).

Please note that MULTILIB\_COMPAT causes REQUIRED\_USE to be defined, requiring at least one of the supported flags to be enabled. The ebuild will reject to run on any profile (and architecture) not supporting one of the specified ABIs. You will need to package.mask the ebuild on the profiles that do not support ABIs required.

This also effectively restricts the ebuild from running on non-multilib architectures. If you need to do that, please report an enhancement requests against the eclass.

### Variables exported by eclass

#### MULTILIB\_USEDEP

Contains a USE dependency string that can be used to enforce matching multilib ABIs on package dependencies.

### Generic helper functions

#### multilib\_copy\_sources

Usage: *multilib\_copy\_sources*

Create a separate copy of package sources for each of enabled ABIs. The sources will be obtained from initial BUILD\_DIR (or S, if BUILD\_DIR is unset), and they will be copied to implementation-specific build directories.

This function is usually necessary to use custom build systems that do not support out-of-source builds. Whenever possible, out-of-source builds are preferred.

### Multilib phase-scope variables

Those variables are available in multilib-enabled functions, e.g. called via [multilib\_foreach\_abi](https://wiki.gentoo.org#multilib_foreach_abi) or in [multilib-minimal](https://wiki.gentoo.org/wiki/Project:Multilib/multilib-minimal) sub-phases.

#### MULTILIB\_ABI\_FLAG

The USE flag name corresponding to the ABI for which the build is currently performed.

Please note that this variable can be unset if no USE flag is in effect, e.g. when performing build on non-multilib architecture, or when using multilib-portage. In this case, the phase will be run only once, and the ebuild should assume it's using the native ABI.

### Native ABI querying functions

#### multilib\_is\_native\_abi

Usage: *multilib\_is\_native\_abi*

Determines whether the current ABI is considered the native ABI, and therefore all the features that are limited for native ABI are to be enabled. This function is guaranteed to return true for exactly one ABI. It can be used to avoid building program executables and documentation multiple times, and to disable the dependencies that do not support multilib.

Technically, *multilib\_is\_native\_abi* returns true in one of the following cases:

1. The current *ABI* is equal to the *DEFAULT\_ABI*.
2. The *MULTILIB\_COMPLETE* variable is set (it is used by multilib-portage, and must not be set by users directly).

#### multilib\_native\_use\_with

Usage: *multilib\_native\_use\_with \<use> \[\<opt-name> \[\<opt-value>\]\]*

A wrapper on top of *use\_with* EAPI function. Outputs *--with-* option if the flag is enabled and *multilib\_is\_native\_abi* is true, *--without-* otherwise.

#### multilib\_native\_use\_enable

Usage: *multilib\_native\_use\_enable \<use> \[\<opt-name> \[\<opt-value>\]\]*

A wrapper on top of *use\_enable* EAPI function. Outputs *--enable-* option if the flag is enabled and *multilib\_is\_native\_abi* is true, *--disable-* otherwise.

#### multilib\_native\_usex

Usage: *multilib\_native\_usex \<use> \[\<true1> \[\<false1> \[\<true2> \[\<false2>\]\]\]\]*

A wrapper on top of *usex* EAPI function. Requires EAPI 5 or *eutils* eclass inherit.

Outputs the concatenation of *\<true1>* (or *yes* if not provided) and *\<true2>* if the flag is enabled and *multilib\_is\_native\_abi* is true, the concatenation of *\<false1>* (or *no*) and *\<false2>* otherwise.
