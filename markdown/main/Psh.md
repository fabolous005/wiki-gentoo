<!-- source: https://wiki.gentoo.org/wiki/Psh | group: Gentoo Wiki (Main) | wiki-title: Psh -->
---
title: psh
url: https://wiki.gentoo.org/wiki/Psh
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-06-16"
fingerprint: fffe3e6dc3b7b78d
license: CC BY-SA 4.0
---

# psh

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**psh** is a fast and and flexible shell with a [Perl 5](https://wiki.gentoo.org/wiki/Perl) syntax. Because it is written in Perl 5, psh has ready access to Perl's regular expression engine and flexible data structures. Additionally psh is very fast at mathematical operations, including floating point arithmetic which [Bash](https://wiki.gentoo.org/wiki/Bash) lacks entirely.

The Perl Shell is very "old school" in that it is not possible to configure it to use a Perl syntax newer than the interpreter's default. Thus, new features such as *say* are not available with a Perl 5.*x* interpreter, consistently psh has an early-90's Perl feel to it. Long standing Perl users may find this design decision "retro" but those coming from the perspective of either the modern Modern Perl movement or the [Raku](https://wiki.gentoo.org/wiki/Raku) programming language may find the experience to be jarring or limiting.

## Installation

### USE flags


### Emerge

`root #``emerge --ask app-shells/psh`
## Configuration

### Environment variables

### Files

- \~/.pshrc - the user's shell profile.

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose app-shell/psh`
## See also

- [Bash](https://wiki.gentoo.org/wiki/Bash) — the default shell on Gentoo systems and a popular [shell](https://wiki.gentoo.org/wiki/Shell) program found on many Linux systems.
- [Busybox](https://wiki.gentoo.org/wiki/Busybox) — a utility that combines tiny versions of many common UNIX utilities into a *single, small executable*.
- [Perl](https://wiki.gentoo.org/wiki/Perl) — a general purpose interpreted programming language with a powerful regular expression engine.
- [Raku](https://wiki.gentoo.org/wiki/Raku) — a high-level, general-purpose, and gradually typed programming language with low boilerplate objects, optionally immutable data structures, and an advanced macro system.
