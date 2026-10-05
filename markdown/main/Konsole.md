<!-- source: https://wiki.gentoo.org/wiki/Konsole | group: Gentoo Wiki (Main) | wiki-title: Konsole -->
---
title: Konsole
url: https://wiki.gentoo.org/wiki/Konsole
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-09-08"
fingerprint: "8620413e2a9879e8"
license: CC BY-SA 4.0
---

# Konsole

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Konsole** is [KDE](https://wiki.gentoo.org/wiki/KDE)'s [terminal emulator](https://wiki.gentoo.org/wiki/Terminal_emulator).

## Installation

### USE flags


| [+handbook](https://packages.gentoo.org/useflags/+handbook) | Enable handbooks generation for packages by KDE | 
| [X](https://packages.gentoo.org/useflags/X) | Add support for X11 | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [ssh](https://packages.gentoo.org/useflags/ssh) | Enable net-libs/libsshsupport for resolving user SSH configuration | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

`root #``emerge --ask kde-apps/konsole`
## Configuration

To make the title of a process running in Konsole (e.g. emerge) visible in the tab title or window titlebar, add `%w` (window title set by shell) to the `Tab title format`, under 'Tabs' in the Konsole profile.

## See also

- [Terminal emulator](https://wiki.gentoo.org/wiki/Terminal_emulator) — emulates a video terminal within another display architecture (e.g. in [X](https://wiki.gentoo.org/wiki/X_server)).
