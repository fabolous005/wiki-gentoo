<!-- source: https://wiki.gentoo.org/wiki/Gdu | group: Gentoo Wiki (Main) | wiki-title: Gdu -->
---
title: Gdu
url: https://wiki.gentoo.org/wiki/Gdu
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-10-21"
fingerprint: "5f939cffd173784c"
license: CC BY-SA 4.0
---

# Gdu

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Gdu**, or go DiskUsage(), is a disk usage analyzer written in the [Go](https://wiki.gentoo.org/wiki/Go) programming language.

## Installation

### Emerge

Gdu is a package from [GURU](https://wiki.gentoo.org/wiki/GURU), to enable the GURU repository:

`root #````
eselect repository enable guru
```
`root #````
emaint sync -r guru
```
`root #``emerge --ask sys-fs/gdu`
## Usage

### Scan a directory

To scan a directory to find the size of the files and directories inside, simply run gdu in the desired directory.

`user $``gdu /home/larry`
