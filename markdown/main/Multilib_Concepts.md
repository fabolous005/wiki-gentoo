<!-- source: https://wiki.gentoo.org/wiki/Multilib/Concepts | group: Gentoo Wiki (Main) | wiki-title: Multilib/Concepts -->
---
title: Multilib/Concepts
url: https://wiki.gentoo.org/wiki/Multilib/Concepts
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-03-09"
fingerprint: "6b31697e6265ef13"
license: CC BY-SA 4.0
---

# Multilib/Concepts

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page describes the concepts used in gx86 multilib project and the rationale for them.

## Multilib ABIs

Multilib ABIs are binary format 'flavors' that can be used on the same architecture. A particular ABI determines executable format, available CPU features, standard type sizes, etc.

For example, the amd64 architecture supports three multilib ABIs:

- The native *amd64* ABI that is used by the kernel and all non-multilib-capable packages.
- The 32-bit *x86* ABI that is compatible with applications and libraries written for plain x86 CPUs.
- The *x32* ABI that mostly aims to mix the advantages of *amd64* ABI with 32-bit pointers.

The *native ABI* is the ABI that is defined as default by a particular profile. It is used for all non-multilib packages, and also enabled by default for multilib packages. The remaining ABIs are considered *non-native ABIs*.

## Package flavors

The multilib-capable packages can be collected in two important groups:

- Library packages — packages that install both the libraries used by native ABI applications and multilib ABI applications.
- Application packages — packages that do not install public-use libraries. Since non-multilib packages don't depend on those, distinction between native ABI and non-native ABIs is not important in those packages.

## Supported implementations of multilib

### Library packages

Library packages are implemented using either or both of the two following methods:

- Emul-linux-x86 packages that contain pre-built 32-bit libraries for amd64.
- multilib-build.eclass that adds support for building regular ebuilds for multiple ABIs.

The latter method is preferred, and we are in process of converting packages to use it. However, until all packages are ready and stable, both methods are supported in parallel.

Due to inevitable file collisions, we support systems either:

- Using only emul-linux-x86 packages.
- Using multilib-build.eclass packages with optional fallback to emul-linux-x86 for libraries that were not converted yet.

### Application packages

Application packages can be implemented in variety of ways. The most common variants are:

- Ebuilds using ABI flags directly with upstream binary packages for closed-source applications.
- Ebuilds using package-specific build system quirks to support multilib (e.g. toolchain).
- Ebuilds using multilib-build.eclass to support building the application for multiple ABIs.

## Multilib ABI USE flags

Multilib-capable packages are using USE flags with `abi_` prefix in order to control the enabled ABIs. The following table lists all the currently supported flags:

| Architecture | Flag prefix | USE flag | ABI | Notes | 
|---|---|---|---|---|
| x86 | ABI\_X86 | abi\_x86\_32 | x86 | Native (and only) on x86. | 
| amd64 |  | abi\_x86\_32 | x86 |  | 
|  |  | abi\_x86\_64 | amd64 | Native on amd64. | 
|  |  | abi\_x86\_x32 | x32 | Available in experimental x32 sub-profile. | 
| mips | ABI\_MIPS | abi\_mips\_n32 | n32 |  | 
|  |  | abi\_mips\_n64 | n64 |  | 
|  |  | abi\_mips\_o32 | o32 |  | 

Availability and visibility of flags depends on the profile in use:

1. In multilib profiles, the relevant flag group is made visible and supported ABIs are unmasked. The native ABI is enabled by default and forced on on multilib library packages. For example, on amd64 you can notice `ABI_X86` flags on a number of packages.
2. In non-multilib profiles of architectures that support multilib, all the flags are hidden but the flag corresponding to the native ABI is enabled and forced to be always on. This allows packages to conveniently use the flags independently of whether multilib is enabled or not. For example, on x86 the flag `abi_x86_32` is always enabled on the multilib packages.
3. In profiles of architectures that do not support multilib, all multilib flags are hidden and disabled.

## Cross-package dependencies

Whenever a multilib package requires a multilib library built for the same ABI, USE flag dependencies shall be used to enforce the match. The multilib-build.eclass provides a convenience *${MULTILIB\_USEDEP}* variable that provides a proper USE dependency string.

## Installed multilib library components

### Libraries

Multilib packages should install the same set of public libraries for all multilib ABIs. Furthermore, all the ABIs should use the same set of features (USE flags).

This is mostly in order to avoid confusion and unnecessary complexity. It guarantees that same version ranges and USE flag dependencies can be used both for non-native and native ABI dependencies.

### Header files

By default, we assume that all ABIs use the same header files. Therefore, all headers are installed into the /usr/include directory. The eclasses additionally check whether each ABI installed the same header files, and prevent the installation from proceeding if it finds mismatched headers.

If necessary, the headers may be wrapped for a particular ABI. If the wrapping is in effect, the original headers are placed in /usr/include/${CHOST} sub-tree and a wrapper file is installed into the /usr/include directory. The wrapper includes proper file for ABI used by the compiler, or errors out if an unsupported ABI was requested.

### Executables

By default, we assume that the executables are equivalent in different ABIs and therefore the native executables are to be installed.

If possible, it is preferred to prevent building executables for other ABIs and build them for the native ABI only. However, it is acceptable to build (and therefore discard) the other ABI tools if the following conditions are met:

1. Disabling the tools would require patching or heavy hacking of the build system.
2. The tools are small and therefore do not take long to build.
3. The tools do not introduce additional multilib dependencies.

If there is a need to keep separate copies of the same executable for different ABIs (e.g. a *-config* tool), the extra tools are to be prefixed with *${CHOST}-' (e.g.* ${CHOST}-libgcrypt-config*). In place of the original name, a symlink to the native variant is to be placed.*

This utilizes the GNU build system behavior of preferring build tools with *${CHOST}-* prefix if available, and therefore makes it possible for multilib reverse dependencies to find the correct tools automatically. However, some build systems may require patching or hacking to use proper tools.
