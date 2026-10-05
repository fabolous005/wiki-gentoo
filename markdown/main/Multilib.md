<!-- source: https://wiki.gentoo.org/wiki/Multilib | group: Gentoo Wiki (Main) | wiki-title: Multilib -->
---
title: Multilib
url: https://wiki.gentoo.org/wiki/Multilib
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-11-12"
fingerprint: "67f370ed3ac0b5de"
license: CC BY-SA 4.0
---

# Multilib

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Multilib is one of the solutions allowing users to run applications built for various [application binary interfaces](https://en.wikipedia.org/wiki/Application_binary_interface) (ABIs) of the same architecture. The most common use of multilib is to run 32-bit applications on **amd64**.

The multilib systems use separate library directories for non-native ABIs. This allows having the same library installed in variants for each ABI, as necessary to satisfy the dependencies of programs built for the ABI in question.

## Multilib library providers in Gentoo

There are currently three ways of providing multilib libraries in Gentoo:

- Using emul-linux-x86 packages (32-bit libraries for **amd64** only). (obsolete and removed)
- Using the eclasses provided by the [gx86-multilib](https://wiki.gentoo.org/wiki/Gx86-multilib) project. (Current implementation)
- Using the multilib-portage fork.

## Comparison of multilib approaches

|  | emul-linux | [gx86-multilib](https://wiki.gentoo.org/wiki/Multilib/gx86-multilib) | multilib-portage | 
|---|---|---|---|
| Supported ABIs |  |  |  | 
| Provision method |  |  |  | 
| Method of introducing | dedicated ebuilds | eclasses + changes in library ebuilds | changes in package manager | 
| Inter-package dependencies | explicit emul-linux package deps | explicit USE dependencies | implicit | 
| Cost of introduction |  |  |  | 
| Cost of adding libraries |  |  |  | 
| Cost of maintenance |  |  |  | 
| Cost of changing (fixing) implementation |  |  |  | 
| Security implications |  |  |  | 
| Supported libraries |  |  |  | 
| Support USE flags (choices) |  |  |  | 
| Extra fetching |  |  |  | 
| Extra build time |  |  |  | 
| Extra dependencies |  |  |  |
