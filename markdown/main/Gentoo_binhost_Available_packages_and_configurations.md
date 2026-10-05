<!-- source: https://wiki.gentoo.org/wiki/Gentoo_binhost/Available_packages_and_configurations | group: Gentoo Wiki (Main) | wiki-title: Gentoo binhost/Available packages and configurations -->
---
title: Gentoo binhost/Available packages and configurations
url: https://wiki.gentoo.org/wiki/Gentoo_binhost/Available_packages_and_configurations
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-09"
fingerprint: "3a332686175f709"
license: CC BY-SA 4.0
---

# Gentoo binhost/Available packages and configurations

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Gentoo binhost**

Which packages to build and provide on the [Gentoo binhost](https://wiki.gentoo.org/wiki/Gentoo_Binary_Host_Quickstart) is dependent on system architecture. Packages for a given architecture are compiled for specific profiles, and with specific *CFLAGS*. These are the **packages** that the Gentoo binhost currently provides, and the **settings** for which they are built, for each system architecture:

## amd64 x86-64

### Selected packages

Each profile has a world file as well, listing the packages which will be built (along with their build dependencies). These can be found in the subdirectories of [https://gitweb.gentoo.org/proj/binhost.git/tree/builders/milou](https://gitweb.gentoo.org/proj/binhost.git/tree/builders/milou)

There is also a "lottery" that will randomly try to build a few packages each day, per profile. Those additional packages may disappear over time as they are version-bumped, and be replaced by entirely different packages.

### Profiles

This binhost is built with USE flags of the following profiles:

- `default/linux/amd64/23.0/no-multilib`
- `default/linux/amd64/23.0/desktop/gnome`
- `default/linux/amd64/23.0/desktop/gnome/systemd`
- `default/linux/amd64/23.0/desktop/plasma/systemd`

This will cover most of the USE flag combinations needed for both a OpenRC and systemd system. This list is what the builders use, but it doesn't mean users must run one of the listed profiles to get binpkgs. Several builders run to get coverage across different common USE flags.

### CFLAGS

The packages are built with `CFLAGS="-O2 -pipe -march=x86-64 -mtune=generic"`, which means all x86-64 CPUs are supported.

## amd64 x86-64-v3 variant

Most configuration is identical between x86-64 and x86-64-v3, but there are two tweaks:

- The packages are built with `CFLAGS="-O2 -pipe -march=x86-64-v3"` instead.
- Additionally, package.use contains `*/* CPU_FLAGS_X86: avx avx2 f16c fma3 mmx mmxext popcnt sse sse2 sse3 sse4_1 sse4_2 ssse3`.

## arm64

### Selected packages

Each profile has a world file as well, listing the packages which will be built (along with their build dependencies). These can be found in the subdirectories of [https://gitweb.gentoo.org/proj/binhost.git/tree/builders/dola](https://gitweb.gentoo.org/proj/binhost.git/tree/builders/dola)

There is also a "lottery" that will randomly try to build a few packages each day, per profile. Those additional packages may disappear over time as they are version-bumped, and be replaced by entirely different packages.

### Profiles

This binhost is built with USE flags of the following profiles:

- `default/linux/arm64/23.0`
- `default/linux/arm64/23.0/desktop/gnome/systemd`
- `default/linux/arm64/23.0/desktop/plasma/systemd`

This will cover most of the USE flag combinations needed for both a OpenRC and systemd system.

### CFLAGS

The packages are built with `CFLAGS="-O2 -pipe"`, which means all arm64 CPUs are supported.

## Other arches and experimental profiles on amd64 and arm64

### Profiles

Listing a few examples of how this would work for a user's system.

amd64 using llvm as system compiler:

- `default/linux/amd64/23.0/clang`

arm64 using musl as the libc:

- `default/linux/arm64/23.0/musl`

ppc using the standard profile:

- `default/linux/ppc/23.0`

m68k using systemd:

- `default/linux/m68k/23.0/systemd`

USE flags will be the defaults set by the profile itself.

### CFLAGS

The packages are built with `CFLAGS="-O2 -pipe"`, which means all CPUs are supported in their respect profile architects.
