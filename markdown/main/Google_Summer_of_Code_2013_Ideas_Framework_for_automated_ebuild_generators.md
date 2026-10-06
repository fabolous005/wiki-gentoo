<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2013/Ideas/Framework_for_automated_ebuild_generators | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2013/Ideas/Framework for automated ebuild generators -->
---
title: Google Summer of Code/2013/Ideas/Framework for automated ebuild generators
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2013/Ideas/Framework_for_automated_ebuild_generators
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-11-09"
fingerprint: f328796f9dfd03f2
license: CC BY-SA 4.0
---

# Google Summer of Code/2013/Ideas/Framework for automated ebuild generators

[Google Summer of Code](https://wiki.gentoo.org/wiki/Google_Summer_of_Code) |

[2013](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2013) |

[Ideas](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2013/Ideas)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## [Framework for automated ebuild generators]

**Completed in 2013**

Lots of ebuild generators were already created by Gentoo users, developers, GSoC Students, etc., to generate ebuilds for a large number of 3rd party software providers, like octave-forge (Octave), pypi (Python), cran (R), cpan (Perl), and others, but each one tries to solve the very same problems on its own unique and "innovative" way.

This project wants to implement a solid base framework to be used by these tools, implementing all the basic algorithms needed to resolve dependencies, create ebuilds, etc.

Each software provider should be a backend, that implements a common interface, defined by the framework, with all the required provider-only stuff needed by the framework to create the ebuilds, either in runtime, calling a package manager just after the ebuild generation, or creating a big overlay with all the packages and dependencies.



| Contacts | Required Skills | 
|---|---|
|  |  |
