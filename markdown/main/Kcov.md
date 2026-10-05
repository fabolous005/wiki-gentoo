<!-- source: https://wiki.gentoo.org/wiki/Kcov | group: Gentoo Wiki (Main) | wiki-title: Kcov -->
---
title: kcov
url: https://wiki.gentoo.org/wiki/Kcov
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-10-12"
fingerprint: c7f129deecc5dafe
license: CC BY-SA 4.0
---

# kcov

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**kcov** is a [code coverage](https://en.wikipedia.org/wiki/Code_coverage) tool.

## Installation

### USE flags


### Emerge

`root #``emerge --ask dev-util/kcov`
Versions below 42 require the following patch to be applied on musl-based systems:

FILE **`/etc/portage/patches/dev-util/kcov-40/ptrace.patch`**
