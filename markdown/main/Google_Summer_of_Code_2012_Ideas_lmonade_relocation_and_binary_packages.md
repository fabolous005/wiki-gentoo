<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/lmonade_relocation_and_binary_packages | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2012/Ideas/lmonade relocation and binary packages -->
---
title: Google Summer of Code/2012/Ideas/lmonade relocation and binary packages
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/lmonade_relocation_and_binary_packages
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-11-09"
fingerprint: "8165186ca65d23d8"
license: CC BY-SA 4.0
---

# Google Summer of Code/2012/Ideas/lmonade relocation and binary packages

[Google Summer of Code](https://wiki.gentoo.org/wiki/Google_Summer_of_Code) |

[2012](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012) |

[Ideas](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## [lmonade relocation and binary packages]

The [lmonade](http://www.lmona.de) project is a distribution of mathematical software based on [Gentoo prefix](http://www.gentoo.org/proj/en/gentoo-alt/prefix/). It can be installed without admistrative rights on different linux distributions or OSX. The main aim is to make scientific software available across platforms.

For a user not familiar with Unix and the shell, the quickest solution to get a working copy of some software is to download a binary. However, binary distribution for the all different flavors of GNU/Linux and OSX is too much of a burden for most software developers.  A solution is provided by Gentoo Prefix and Portage's binary package feature.  Gentoo Prefix can handle basic rewriting of absolute paths encoded in binaries present in these packages. Though there are still [quite a few rough edges](https://wiki.gentoo.org#Improved_binary_package_support) that require attention before this can be used in production.

The goal of this project is to bring all these pieces together to form an easy to use solution that will be useful to many software projects. The focus is on making things "just work" for both users and software projects. This task would involve

- modifying eclasses to generate relocatable files,
- writing scripts to fix absolute paths in binaries (e.g., [1](http://hg.sagemath.org/scripts-main/file/78a0b42fab1f/sage-make_relative) [2](http://hg.sagemath.org/scripts-main/file/78a0b42fab1f/sage-location)),
- packaging in a user friendly format and
- finding creative solutions to platform dependent quirks.

Please e-mail us for more information.



| Contacts | Required Skills | 
|---|---|
|  |  |
