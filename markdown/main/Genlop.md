<!-- source: https://wiki.gentoo.org/wiki/Genlop | group: Gentoo Wiki (Main) | wiki-title: Genlop -->
---
title: genlop
url: https://wiki.gentoo.org/wiki/Genlop
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-05-02"
fingerprint: f6059e4994a26103
license: CC BY-SA 4.0
---

# genlop

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

**genlop** is a utility for extracting information about emerged ebuilds from Portage log files (/var/log/emerge.log). Genlop is written in Perl.

## Features

Detailed features include:

- Colorful output.
- Full Portage merge and unmerge history.
- Display date, time, and build time of every merge.
- Display total and average build time of selected ebuilds.
- Estimate upgrade time.
- Watching current merge progress.
- Use alternate portage logfile(s).
- Compressed logfiles (gzip, bzip2) are supported
- Match ebuild names using regular expressions.
- Log corruption detection.
- Display build specific USE and CFLAGS variables.
- GMT/UTC or localized time and date.
- Full portage rsync history.

## Installation

### Emerge

`root #``emerge --ask app-portage/genlop`
## Usage

### Invocation

`user $``genlop -h````
Usage: genlop  [options] [-f logfile] [category/package]
Options:
  -c  display the currently compiling packages (if any)
  -e  display package history; default if any option is used.
  -f  read emerge log information from "logfile" instead of ""
  -h  print this help
  -i  extra infos for the selected package (build specific USE, CFLAGS,
      CXXFLAGS and LDFLAGS variables, average build time, etc)
  -g  display GMT/UTC, not localized time.
  -l  show full merge history.
  -n  no color in output
  -p  estimate build time from a piped "emerge -p" output
  -q  query gentoo.linuxhowtos.org database if no local emerge was found
  -r  search for portage tree sync/rsync history.
  -s  use (case insensitive) regular expressions to match package names
  -S  use case sensitive regular expressions to match package names
  -t  calculate merge time for the specific package(s).
  -u  show when packages have been unmerged.
  -v  display genlop version and exit.
  --date datestring1 [--date datestring2] only shows results between
    datestring1 and datestring2.
    datestring2 defaults to "now" if not explicitly set.
    (e.g. genlop --list --date 3 days ago) shows packages emerged since this
    time three days ago.
This program is licensed under the GPL v2. See COPYING.
For further info about genlop please read the man page.
```
See the man page for full option explanation and usage examples.

### example: Refresh status every 10 seconds

`user $``watch -cn 10 genlop -ci````
Every 10.0s: genlop -ci                            mypc: Tue May 30 03:43:20 2017
 Currently merging 2 out of 3
 * www-client/firefox-53.0.3 
       current merge time: 6 minutes and 30 seconds.
       ETA: 11 minutes and 24 seconds.
```
## See also

- [Portage-utils](https://wiki.gentoo.org/wiki/Portage-utils) — a collection of small, fast [Portage](https://wiki.gentoo.org/wiki/Portage) **q**uery utilities written in C.
