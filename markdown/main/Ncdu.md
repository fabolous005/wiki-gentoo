<!-- source: https://wiki.gentoo.org/wiki/Ncdu | group: Gentoo Wiki (Main) | wiki-title: Ncdu -->
---
title: Ncdu
url: https://wiki.gentoo.org/wiki/Ncdu
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-05"
fingerprint: "6edf16e6d5c9d9e3"
license: CC BY-SA 4.0
---

# Ncdu

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Ncdu is a disk usage analyzer with an ncurses interface. It is designed to find space hogs on a remote server where you don’t have an entire graphical setup available, but it is a useful tool even on regular desktop systems. Ncdu aims to be fast, simple and easy to use, and should be able to run in any minimal POSIX-like environment with ncurses installed.

## Installation

### USE flags



### Emerge

`root #``emerge --ask sys-fs/ncdu``root #``emerge --ask sys-fs/ncdu-bin`
## Usage

### Directory usage statistics

To get basic directory usage statistics, run ncdu or ncdu-bin:

`user $``ncdu`
### Extended information

To enable reading additional information, such as ownership, permissions, and last modification time, use the `-e` argument:

`user $``ncdu -e`
## See also

- [Dust](https://wiki.gentoo.org/wiki/Dust) — a command-line tool similar to du that displays file and directory usage with a bar displaying its percentage.
