<!-- source: https://wiki.gentoo.org/wiki/Ed | group: Gentoo Wiki (Main) | wiki-title: Ed -->
---
title: ed
url: https://wiki.gentoo.org/wiki/Ed
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-02-05"
fingerprint: f72f0f0a47b71a8d
license: CC BY-SA 4.0
---

# ed

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**ed** (pronounced *E.D.*) is a  [line-based](https://en.wikipedia.org/wiki/Line_editor) text editor with support for regular expressions.

ed is one of the oldest text editors that still sees use today, and has its place deep in the history of UNIX(like) operating systems. The line-based nature of ed is a result of the fact that it was written at a time when teletype machines were the common user interface of the day, and CRT monitors — then often called "glass teletypes" — were rare and expensive.

ed inspired the clone  [ex](<https://en.wikipedia.org/wiki/Ex_(text_editor)>), which served as the basis for [vi](https://wiki.gentoo.org/wiki/Vi). vi still has an ex mode, analogous to ed.

In current practice it is uncommon to use ed interactively, but it sees occasional use as an embedded means to automate edits to files from within a shell script.

The ed utility is [specified by the](https://pubs.opengroup.org/onlinepubs/9799919799/utilities/ed.html) [POSIX](https://wiki.gentoo.org/wiki/POSIX) standard.

## Installation

### Emerge

`root #``emerge --ask sys-apps/ed`
### Files

- ./ed.hup or $HOME/ed.hup - where ed dumps the current write buffer if the terminal hangs up unexpectedly.

### Invocation

`user $``ed --help`
GNU ed is a line-oriented text editor. It is used to create, display,
modify and otherwise manipulate text files, both interactively and via
shell scripts. A restricted version of ed, red, can only edit files in
the current directory and cannot execute shell commands. Ed is the
'standard' text editor in the sense that it is the original editor for
Unix, and thus widely available. For most purposes, however, it is
superseded by full-screen editors such as GNU Emacs or GNU Moe.
Usage: ed \[options\] \[file\]
Options:
  -h, --help                 display this help and exit
  -V, --version              output version information and exit
  -E, --extended-regexp      use extended regular expressions
  -G, --traditional          run in compatibility mode
  -l, --loose-exit-status    exit with 0 status even if a command fails
  -p, --prompt=STRING        use STRING as an interactive prompt
  -r, --restricted           run in restricted mode
  -s, --quiet, --silent      suppress diagnostics, byte counts and '!' prompt
  -v, --verbose              be verbose; equivalent to the 'H' command
Start edit by reading in 'file' if given.
If 'file' begins with a '!', read output of shell command.
Exit status: 0 for a normal exit, 1 for environmental problems (file
not found, invalid flags, I/O errors, etc), 2 to indicate a corrupt or
invalid input file, 3 for an internal consistency error (eg, bug) which
caused ed to panic.
Report bugs to bug-ed@gnu.org
Ed home page: http://www.gnu.org/software/ed/ed.html
General help using GNU software: http://www.gnu.org/gethelp

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose sys-apps/ed`
## See also

- [sed](https://wiki.gentoo.org/wiki/Sed) — a program that uses regular expressions to programmatically modify streams of text
- [grep](https://wiki.gentoo.org/wiki/Grep) — a tool for searching text files with regular expressions
- [awk](https://wiki.gentoo.org/wiki/Awk) — a scripting language for data extraction
- [Perl](https://wiki.gentoo.org/wiki/Perl) — a general purpose interpreted programming language with a powerful regular expression engine.
- [Raku](https://wiki.gentoo.org/wiki/Raku) — a high-level, general-purpose, and gradually typed programming language with low boilerplate objects, optionally immutable data structures, and an advanced macro system.
