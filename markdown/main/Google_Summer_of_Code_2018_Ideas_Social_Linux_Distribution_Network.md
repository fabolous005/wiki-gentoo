<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2018/Ideas/Social_Linux_Distribution_Network | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2018/Ideas/Social Linux Distribution Network -->
---
title: Google Summer of Code/2018/Ideas/Social Linux Distribution Network
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2018/Ideas/Social_Linux_Distribution_Network
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-11-11"
fingerprint: "812b3e34e17de610"
license: CC BY-SA 4.0
---

# Google Summer of Code/2018/Ideas/Social Linux Distribution Network

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Gentoo is a Meta Distribution, and it's binary instantiation usually does emerge on the users machine(s).

So users actually do have their private "Linux Distribution" - either with or without caching (and redistributing) the binary packages.

Of course there is chance that users do have identical profile setups (USE flags, optimization flags, etc.), which is where some build service (OpenBuildService or similar) may be useful.

But rather than sharing binary packages, the idea is to share Gentoo user's profile setups - with the cache for binary packages to be optional (when powered by some build service). Note that some USE flags disallow binary packaging at all.

The idea came up first in [https://archives.gentoo.org/gentoo-dev/message/e7880d4edb2a250e14b7677675f89931](https://archives.gentoo.org/gentoo-dev/message/e7880d4edb2a250e14b7677675f89931), and there is nothing more than that yet.

The profile sharing mechanism may fit the GSoC scope, either with or without sharing user's binary packages.



| Contacts | Required Skills | 
|---|---|
|  |  |
