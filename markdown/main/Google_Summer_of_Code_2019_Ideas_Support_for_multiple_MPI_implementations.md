<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2019/Ideas/Support_for_multiple_MPI_implementations | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2019/Ideas/Support for multiple MPI implementations -->
---
title: Google Summer of Code/2019/Ideas/Support for multiple MPI implementations
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2019/Ideas/Support_for_multiple_MPI_implementations
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-08-16"
fingerprint: "3afcd70ecf334cd7"
license: CC BY-SA 4.0
---

# Google Summer of Code/2019/Ideas/Support for multiple MPI implementations

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

There are numerous MPI (Message Passing Interface) implementations available, but most of them can't be installed together as is. In HPC world it is often mandatory to provide for users different MPI implementations and/or versions of the same implementation, e.g. due to binary package dependencies, codebase or performance issues. The only way to do this now is by using empi from the [science overlay](https://wiki.gentoo.org/wiki/Project:Science/Overlay). Empi has its shortcomings such as lack of multilib support and was not ported to the main tree.

Your goal will be to enable Gentoo to use multiple MPI versions at once. There are several possible approaches.

1\. You may implement new eclass taking into account empi experience. The key idea is to make compatible MPI implementations selectable from the ebuild similar to they way how multiple python implementations are supported, and allow to build multiple versions of the same package for different MPI implementations.

There was [a previous attempt](https://github.com/gilroy/gentoo-mpi) to solve this task. While it was not completed and oversimplified, it may give some hints on what to do.

2\. You may use Gentoo prefix technologies to enable simultaneous use of several versions in the tree.

Different MPI implementations should be selectable by users without any special privileges. A good start will be to use [modules](http://modules.sourceforge.net/) to control environment variables. You will also need to port existing MPI applications to the new framework.



| Contacts | Required Skills | 
|---|---|
|  |  |
