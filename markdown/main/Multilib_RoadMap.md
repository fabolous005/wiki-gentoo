<!-- source: https://wiki.gentoo.org/wiki/Multilib/RoadMap | group: Gentoo Wiki (Main) | wiki-title: Multilib/RoadMap -->
---
title: Multilib/RoadMap
url: https://wiki.gentoo.org/wiki/Multilib/RoadMap
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-06-02"
fingerprint: "52316265e0c56547"
license: CC BY-SA 4.0
---

# Multilib/RoadMap

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Road map

| Stage 1 |  | 
|---|---|
| Enable multilib support in ebuilds |  | 
| Update binary packages to support multilib ebuilds |  | 
| Stage 2 |  | 
| Stabilize necessary multilib packages |  | 
| Stage 3 |  | 
| Enable multilib support on stable |  | 
| Mask emul-linux-x86 for removal |  | 

## Stage 1

### Enable multilib support in ebuilds

The first step involves enabling multilib support in ebuilds. Packages matching the following criteria need to be taken into consideration:

1. packages required by 32-bit packages,
2. dependencies of other multilib packages,
3. plugins that can be directly or indirectly used by 32-bit applications (gstreamer, nss, PAM).

During the initial setup, packages incorporated in emul-linux-x86 were converted first. However, since most of the commonly used packages are converted and not all packages in emul-linux-x86 were actually used by anything, it is recommended not to convert any more of the libraries provided by emul-linux-x86 unless they are actually needed by some other package.

Detailed status:

| Package type | Status | 
|---|---|
| [emul-linux-x86 incorporated packages](https://wiki.gentoo.org/wiki/Multilib_porting_status#Emul-.2A_Package_Porting_Overview) |  | 
| Multilib library dependencies |  | 
| Plugins |  | 
| gstreamer |  | 
| nss |  | 
| PAM |  | 

### Update binary packages to support multilib ebuilds

Once proper multilib dependencies are available, 32-bit packages need to be adjusted to support both emul-linux-x86 and multilib. Packages not intended for stable may switch directly to multilib.

The following dependency syntax is recommended:

See also: [dependency update status](https://wiki.gentoo.org/wiki/Multilib_porting_status#Dependency_update_status)
