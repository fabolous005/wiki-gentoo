<!-- source: https://wiki.gentoo.org/wiki/WxFormBuilder | group: Gentoo Wiki (Main) | wiki-title: WxFormBuilder -->
---
title: WxFormBuilder
url: https://wiki.gentoo.org/wiki/WxFormBuilder
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-08-16"
fingerprint: ca99177fb8ea98a7
license: CC BY-SA 4.0
---

# WxFormBuilder

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**wxFormBuilder** is an application that can literally speed up GUI development, being Python either C++ the programming language of choice.

## Installation

To install it, first add the *gentoo-zh* [ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository):

`root #``eselect repository enable gentoo-zh``root #``emerge --sync gentoo-zh`
Then, since premake-3.7 is currently an ebuild prerequisite, though the core system is already providing premake-4, you have to directly specify it:

`root #``emerge --ask dev-util/premake:3`
You may now proceed and install [dev-util/wxFormBuilder](https://packages.gentoo.org/packages/dev-util/wxFormBuilder) itself:

`root #``emerge --ask wxformbuilder`
