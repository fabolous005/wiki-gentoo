<!-- source: https://wiki.gentoo.org/wiki/Dust | group: Gentoo Wiki (Main) | wiki-title: Dust -->
---
title: Dust
url: https://wiki.gentoo.org/wiki/Dust
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-05"
fingerprint: ab03916467e9df61
license: CC BY-SA 4.0
---

# Dust

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Dust is a command-line tool similar to du that displays file and directory usage with a bar displaying its percentage.

## Installing

### USE flags


### Emerge

`root #``emerge --ask sys-block/dust`
## Usage

### Displaying usage statistics

To display the storage use in a directory, run dust:

`user $``dust`
### Not displaying bars

To have the usage bars not show up, use the `-b` or `--no-percent-bars` arguments:

`user $``dust --no-percent-bars`
### Reverse output

To have the biggest directories show up on the top, use the `-r` or --reverse argument:

`user $``dust --reverse`
### Filter files

To filter the files searched, use `-e` or `--filter` arguments:

`user $``dust --filter "build\.log"`
## See also

- [ncdu](https://wiki.gentoo.org/wiki/Ncdu) — a disk usage analyzer with an ncurses interface
