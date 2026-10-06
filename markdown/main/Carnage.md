<!-- source: https://wiki.gentoo.org/wiki/Carnage | group: Gentoo Wiki (Main) | wiki-title: Carnage -->
---
title: carnage
url: https://wiki.gentoo.org/wiki/Carnage
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-04-27"
fingerprint: be8175b079cfa8d3
license: CC BY-SA 4.0
---

# carnage

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**carnage** is a terminal user interface front-end (TUI) for [Portage](https://wiki.gentoo.org/wiki/Portage) and [eix](https://wiki.gentoo.org/wiki/Eix). Its goal is to centralize common Gentoo package management tasks in a unified, efficient, and user-friendly interface.

carnage is not meant to compete in feature completeness with the command line. It is dedicated to providing an intuitive browsing and inspection environment rather than replacing [emerge](https://wiki.gentoo.org/wiki/Emerge) or eix directly.

## Installation

### Emerge

Enable the [GURU](https://wiki.gentoo.org/wiki/GURU) repository as described in [Project:GURU/Information for End Users](https://wiki.gentoo.org/wiki/Project:GURU/Information_for_End_Users) then install [app-portage/carnage::guru](https://gpo.zugaina.org/Overlays/guru/app-portage/carnage):

`root #``emerge --ask app-portage/carnage`
## Configuration

### Files

- \~/.config/carnage/carnage.toml - Principal configuration file.
- \~/.config/carnage/commands.toml - Advanced command overrides configuration file.

## Usage

carnage can be started directly:

`user $``carnage`
### Invocation

`user $``carnage --help`
usage: carnage \[-h\] \[-V\] \[-c FILE\] \[--css FILE\]
TUI front-end for Portage and eix
options:
  -h, --help         show this help message and exit
  -V, --version      Show version information and exit
  -c, --config FILE  Path to configuration file
  --css FILE         Path to custom CSS file

## Troubleshooting

### Command fails with missing privileges

Certain commands may or may not require [superuser privileges](https://wiki.gentoo.org/wiki/Sudo) depending on the user. Privilege can be requested always using a command override:

FILE **`~/.config/carnage/commands.toml`****Requesting privilege for reading news**

```
[news.read]
# command = ["eselect", "news", "read", "--quiet", "$1"]
privilege = true
```
## See also

- [eix](https://wiki.gentoo.org/wiki/Eix) — a set of utilities for searching and diffing local [ebuild repositories](https://wiki.gentoo.org/wiki/Ebuild_repository) using a binary cache.
