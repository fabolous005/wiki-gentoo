<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2017/Ideas/Support_for_multiple_MPI_implementations | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2017/Ideas/Support for multiple MPI implementations -->
---
title: Google Summer of Code/2017/Ideas/Support for multiple MPI implementations
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2017/Ideas/Support_for_multiple_MPI_implementations
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-03-30"
fingerprint: "3becd32cd7130cc7"
license: CC BY-SA 4.0
---

# Google Summer of Code/2017/Ideas/Support for multiple MPI implementations

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

There are numerous MPI (Message Passing Interface) implementations available, but most of them can't be installed together as is. In HPC world it is often mandatory to provide for users different MPI implementations and/or versions of the same implementation, e.g. due to binary package dependencies, codebase or performance issues. The only way to do this now is by using empi from the [science overlay](https://wiki.gentoo.org/wiki/Project:Science/Overlay). Empi has its shortcomings such as lack of multilib support and was not ported to the main tree.

Your goal will be to implement new eclass taking into account empi experience. The key idea is to make compatible MPI implementations selectable from the ebuild similar to they way how multiple python implementations are supported, and allow to build multiple versions of the same package for different MPI implementations.

Different MPI implementations should be selectable by users without any special privileges. A good start will be to use [modules](http://modules.sourceforge.net/) to control environment variables. You will also need to port existing MPI applications to the new framework.



| Contacts | Required Skills | 
|---|---|
|  |  |
