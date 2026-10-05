<!-- source: https://wiki.gentoo.org/wiki/StarFive_VisionFive_2/Catalyst | group: Gentoo Wiki (Main) | wiki-title: StarFive VisionFive 2/Catalyst -->
---
title: StarFive VisionFive 2/Catalyst
url: https://wiki.gentoo.org/wiki/StarFive_VisionFive_2/Catalyst
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-11-24"
fingerprint: "5cbe467926626aa2"
license: CC BY-SA 4.0
---

# StarFive VisionFive 2/Catalyst

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

#### Differents approach to build a Catalyst seed

##### Use the crossdev root (option 1)

It is possible directly use the crossdev root under /usr to build a rootfs; it is broadly similar to using Catalyst however instead of generating a seed tarball the rootfs is built by chrooting directly into the crossdev root. There are some advantages to this approach, particularly that it is possible to save a significant amount of time.

While this may be faster for quick development or building for a single device, if intending to target multiple configurations it is usually better to use Catalyst as a generic stage 1 image may be created and used as the base for multiple stage 3 images. Using Catalyst also provides better isolation between the host and target systems.

**The target environment must also match the merged/split /usr layout of the host environment:** if the host environment is a split /usr, then the Gentoo profile used in the target environment must also be one of the split /usr profile. In case a mismatch would happen, expect some failures when emerging some packages. No need for an exact match however: it is possible to use a OpenRC profile on the host and a Systemd profile on the target as long as their  /usr layout is the same.
