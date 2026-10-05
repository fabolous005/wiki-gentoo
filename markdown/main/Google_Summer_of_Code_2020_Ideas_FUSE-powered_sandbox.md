<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2020/Ideas/FUSE-powered_sandbox | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2020/Ideas/FUSE-powered sandbox -->
---
title: Google Summer of Code/2020/Ideas/FUSE-powered sandbox
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2020/Ideas/FUSE-powered_sandbox
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2020-03-05"
fingerprint: "3ae9c8b09b2792f8"
license: CC BY-SA 4.0
---

# Google Summer of Code/2020/Ideas/FUSE-powered sandbox

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Gentoo sandbox is a cheap hack that aims to detect when ebuilds are accessing locations they aren't supposed to reach. It is implemented as LD\_PRELOAD library which generally makes it a bit of a hack. It is imperfect, sometimes requires hacks to stop breaking other software and sometimes need to be plain disabled.

I've proposed in the past to reimplement it as a overlay-alike filesystem using FUSE but never found time to work on it. The idea is rather simple — create a basic FUSE filesystem that wraps access to the root filesystem, add access control on top of it, add IPC to make it possible to edit access lists dynamically. Integrate everything into Portage, so it can be used in place of old sandbox.



| Contacts | Required Skills | 
|---|---|
|  |  |
