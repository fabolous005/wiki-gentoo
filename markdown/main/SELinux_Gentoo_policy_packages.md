<!-- source: https://wiki.gentoo.org/wiki/SELinux/Gentoo_policy_packages | group: Gentoo Wiki (Main) | wiki-title: SELinux/Gentoo policy packages -->
---
title: SELinux/Gentoo policy packages
url: https://wiki.gentoo.org/wiki/SELinux/Gentoo_policy_packages
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-24"
fingerprint: dfbd8b2eccf24910
license: CC BY-SA 4.0
---

# SELinux/Gentoo policy packages

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Gentoo packages SELinux policy modules into their own, individual policy packages. This allows for a model where only those policy modules are loaded that are needed, instead of loading all possible policies.

## Introduction

### SELinux policy modules

[SELinux policy modules](https://wiki.gentoo.org/wiki/SELinux/Policy) provide individual SELinux policy parts which can be loaded and unloaded at will, similar to Linux kernel modules.

As a module contains all SELinux rules related to one application (or a coherent set of applications), it makes sense to have these applications depend the SELinux policy module(s) they need.

### Gentoo policy

The policy that Gentoo uses is provided through the [hardened-refpolicy](https://gitweb.gentoo.org/proj/hardened-refpolicy.git/) repository. This repository is kept up to date with the upstream commits (which is done manually, but does not take up that much effort) and contains the Gentoo-provided updates (as long as these updates aren't immediately pushed upstream).

## Ebuilds

The SELinux policy package ebuilds have the following structure:

Most of the real work is done by the selinux-policy-2 eclass.

### Eclass variables

Two important variables are used by the eclass to handle the build process of a SELinux policy packages.

The `MODS` variable contains one (or more) SELinux module names to build. These names represent the file name within the policy repository which reflects the policy to build. For instance, `MODS="screen"` will look for the screen.if, screen.te and screen.fc files that provide all information of a single policy module. It can also be an array.

The `BASEPOL` variable contains the base version on which the policy is to be based. This is because SELinux policies call interfaces of other policies, and the content or format of these interfaces might change across versions (this backwards incompatibility is reduced as much as possible, but cannot be guaranteed). As policy modules are part of the entire repository, we take snapshots of the repository from time to time and give it the proper version (starting with the upstream version, followed by specific revisions). All policy modules from the same snapshot need to use the same `BASEPOL` version. `BASEPOL` if unset is determined by the package version.

Two other interesting variables to use are:

- `SELINUX_GIT_REPO` to point to a different Git repository which contains a refpolicy-like SELinux policy. This allows users or developers to use a different repository than the Gentoo one.
- `SELINUX_GIT_BRANCH` is to use a different Git branch (the default is "master"), which allows users or developers to test changes before merging into the main branch.

### IUSE support

Support for `IUSE` has been added so that policies can be optimized based on USE flags. For instance, modules that optionally have ALSA support can enable/disable this support in the policies through the `USE="alsa"` support.
