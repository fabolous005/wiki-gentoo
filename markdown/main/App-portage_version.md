<!-- source: https://wiki.gentoo.org/wiki/App-portage/version | group: Gentoo Wiki (Main) | wiki-title: App-portage/version -->
---
title: app-portage/version
url: https://wiki.gentoo.org/wiki/App-portage/version
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-11-13"
fingerprint: bfb40cadcce1a6c1
license: CC BY-SA 4.0
---

# app-portage/version

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


This article is **deprecated (obsolete)**. Contents are <u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

Version command is a package that is used for the following tasks:

1. get version information of a package.
2. operate with version of a package.


Make the program available by adding guru repository:

`root #``eselect repository enable guru``root #``emaint sync -r guru`
Install the package:

`root #``emerge --ask app-portage/version`
Display available options:

`user $``version -h`
For example, use the following command to the version of 'gentoolkit' package:

`user $``version -g app-portage/gentoolkit`
0.5.0

Use 's' option to check if the version is smaller than the specified version (in this case 1.0):

`user $``version -c app-portage/gentoolkit s= 1.0` true

or (if the shell accepts a more friendly syntax):

`user $``version -c app-portage/gentoolkit <= 1.0` true

Descriptive outputs can be enabled with 'i' option:

`user $``version -ci app-portage/gentoolkit s= 1.0` app-portage/gentoolkit : pass(0.5.0)

There is also an option to get the current platform:

`user $``version -p` Portage
