<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Cross_Container_Support | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2012/Ideas/Cross Container Support -->
---
title: Google Summer of Code/2012/Ideas/Cross Container Support
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Cross_Container_Support
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-04-02"
fingerprint: ab6c941e2900a9a1
license: CC BY-SA 4.0
---

# Google Summer of Code/2012/Ideas/Cross Container Support

[Google Summer of Code](https://wiki.gentoo.org/wiki/Google_Summer_of_Code) |

[2012](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012) |

[Ideas](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## [Cross Container Support]

It is already possible to create fully working chroot using [qemu-user](http://www.gentoo.org/proj/en/base/embedded/handbook/?part=1&chap=5) and build quickly packages through it, the natural step further is to make it work as a normal container, providing a similar interface to manage it. This way build for arm targets can be done on faster systems and sidetracks also issues about python and perl not supporting proper cross compilation or widespread build systems such waf and cmake failing completely at the task.

The project aims in providing initscripts, management tools and canned recipes to generate container-like chroot, let developers easily import and export system images and further integrate it with crossdev.

An additional task is to support layered systems so native userspace can be used to further speed up the process (hybrid chroot).




| Contacts | Required Skills | 
|---|---|
| [Luca Barbato](mailto:lu_zero@gentoo.org) jing.huang.pku@×××××.com Jing Huang |  |
