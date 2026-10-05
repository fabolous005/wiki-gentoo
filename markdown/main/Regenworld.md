<!-- source: https://wiki.gentoo.org/wiki/Regenworld | group: Gentoo Wiki (Main) | wiki-title: Regenworld -->
---
title: regenworld
url: https://wiki.gentoo.org/wiki/Regenworld
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-11-13"
fingerprint: "3830e22daeeeb3af"
license: CC BY-SA 4.0
---

# regenworld

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


The regenworld script, which is distributed with [Portage](https://wiki.gentoo.org/wiki/Portage), regenerates the Portage world file by checking the Portage log file for all actions performed in the past. It ignores any arguments except the `--help` option.

## Usage

### Invocation

`root #``regenworld --help`
This script regenerates the portage world file by checking the portage
logfile for all actions that you've done in the past. It ignores any
arguments except --help. It is recommended that you make a backup of
your existing world file (/var/lib/portage/world) before using this tool.
