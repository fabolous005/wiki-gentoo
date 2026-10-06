<!-- source: https://wiki.gentoo.org/wiki/Dedicated_Build_Machine-Single_ARCH | group: Gentoo Wiki (Main) | wiki-title: Dedicated Build Machine-Single ARCH -->
---
title: Dedicated Build Machine-Single ARCH
url: https://wiki.gentoo.org/wiki/Dedicated_Build_Machine-Single_ARCH
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2020-09-20"
fingerprint: "4e881c5b0e97519c"
license: CC BY-SA 4.0
---

# Dedicated Build Machine-Single ARCH

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article has some todo items:

- A lot of the packages will be broken, explain where to report


This page shows how to create a single dedicated build machine to compile software for multiple target devices of a single **ARCH**.

The main target **ARCH** for this is the *amd64* architecture due to the amount of resources needed to handle the building process, but the process itself is agnostic to what **ARCH** is being used.

Throughout this article **BUILD** will refer to the build machine on which we are doing the compilation of the software and **HOST** will refer to the machine which will finally be running the compiled software.
