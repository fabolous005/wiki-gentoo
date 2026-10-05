<!-- source: https://wiki.gentoo.org/wiki/Htop | group: Gentoo Wiki (Main) | wiki-title: Htop -->
---
title: htop
url: https://wiki.gentoo.org/wiki/Htop
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-03-19"
fingerprint: e70319ded18778c4
license: CC BY-SA 4.0
---

# htop

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**htop** is a cross-platform interactive process viewer. It is a text-mode application (for console or X terminals) and requires ncurses.

## Installation

### USE flags


| [+filecaps](https://packages.gentoo.org/useflags/+filecaps) | Use Linux file capabilities to control privilege rather than set\*id (this is orthogonal to USE=caps which uses capabilities at runtime e.g. libcap) | 
| [bfd](https://packages.gentoo.org/useflags/bfd) | With USE=unwind, demangle symbol names (like for C++) in backtraces via sys-libs/binutils-libs | 
| [caps](https://packages.gentoo.org/useflags/caps) | Use Linux capabilities library to control privilege | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [delayacct](https://packages.gentoo.org/useflags/delayacct) | Enable Linux delay accounting support via dev-libs/libnl | 
| [hwloc](https://packages.gentoo.org/useflags/hwloc) | Use sys-apps/hwloc for CPU affinity support | 
| [llvm-libunwind](https://packages.gentoo.org/useflags/llvm-libunwind) | Use llvm-runtimes/libunwind instead of sys-libs/libunwind | 
| [lm-sensors](https://packages.gentoo.org/useflags/lm-sensors) | Add linux lm-sensors (hardware sensors) support | 
| [openvz](https://packages.gentoo.org/useflags/openvz) | Enable openvz support | 
| [unicode](https://packages.gentoo.org/useflags/unicode) | Add support for Unicode | 
| [unwind](https://packages.gentoo.org/useflags/unwind) | Add support for call stack unwinding and function name resolution | 
| [vserver](https://packages.gentoo.org/useflags/vserver) | Enable vserver support | 

### Emerge

`root #``emerge --ask sys-process/htop`
## Usage

**htop** can be used with one command:

`user $``htop`
### Invocation

`user $``htop --help`
htop \_version\_
(C) 2004-2019 Hisham Muhammad. (C) 2020 htop dev team.
Released under the GNU GPLv2.
-C --no-color                   Use a monochrome color scheme
-d --delay=DELAY                Set the delay between updates, in tenths of seconds
-F --filter=FILTER              Show only the commands matching the given filter
-h --help                       Print this help screen
-H --highlight-changes\[=DELAY\]  Highlight new and old processes
-M --no-mouse                   Disable the mouse
-p --pid=PID\[,PID,PID...\]       Show only the given PIDs
-s --sort-key=COLUMN            Sort by COLUMN in list view (try --sort-key=help for a list)
-t --tree                       Show the tree view (can be combined with -s)
-u --user\[=USERNAME\]            Show only processes for a given user (or $USER)
-U --no-unicode                 Do not use unicode but plain ASCII
-V --version                    Print version info
Long options may be passed with a single dash.
Press F1 inside htop for online help.
See 'man htop' for more information.

## See also

- [ps](https://wiki.gentoo.org/wiki/Ps) — a tool for reporting on a system's active processes
- [Recommended tools](https://wiki.gentoo.org/wiki/Recommended_tools) — lists system-administration related tools recommended for use in a **[shell](https://wiki.gentoo.org/wiki/Shell) environment** ([terminal/console](https://wiki.gentoo.org/wiki/Terminal_emulator))

## External resources

- [https://peteris.rocks/blog/htop](https://peteris.rocks/blog/htop) - htop utility output explained in quite good detail.
