<!-- source: https://wiki.gentoo.org/wiki/Racket | group: Gentoo Wiki (Main) | wiki-title: Racket -->
---
title: Racket
url: https://wiki.gentoo.org/wiki/Racket
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-01-14"
fingerprint: "5f507c5b003eb23a"
license: CC BY-SA 4.0
---

# Racket

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

**Racket** is a general-purpose programming language designed for language-oriented programming, that is, the ability to implement other programming languages and DSLs. Racket is a descendant of [Scheme](https://wiki.gentoo.org/index.php?title=Scheme&action=edit&redlink=1) which is itself a dialect of [Lisp](https://wiki.gentoo.org/wiki/Lisp) that began development in 1995 and continues to this day.

## Features

Racket is considered to be an excellent language for learning computer science, specifically the discipline of compiler design. The language has both a runtime and a JIT (just in time) compiler. It has a very expressive macro engine that goes well beyond the typical text-replacement macros common to the [C](https://wiki.gentoo.org/wiki/C) preprocessor. Rather, racket macros are baked into Racket parser itself and can be used to extend the language and add new features on the fly.

Racket has its own package manager, raco.

## Installation

### USE flags


| [+chez](https://packages.gentoo.org/useflags/+chez) | Build Racket on Chez (Racket CS) | 
| [+doc](https://packages.gentoo.org/useflags/+doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [+futures](https://packages.gentoo.org/useflags/+futures) | Enable racket/future library for fine-grained hardware parallelism | 
| [+jit](https://packages.gentoo.org/useflags/+jit) | Enable just-in-time compilation for improved performance. May prevent use of some PaX memory protection features in Gentoo Hardened. | 
| [+places](https://packages.gentoo.org/useflags/+places) | Enable racket/place library for share-nothing parallelism and message-passing communication. Compared to futures, places are heavyweight, but they have a simpler performance model. | 
| [+threads](https://packages.gentoo.org/useflags/+threads) | Enable support for green threads | 
| [iconv](https://packages.gentoo.org/useflags/iconv) | Enable support for the iconv character set conversion library | 
| [minimal](https://packages.gentoo.org/useflags/minimal) | Install a very minimal build (disables, for example, plugins, fonts, most drivers, non-critical features) | 
| [ncurses](https://packages.gentoo.org/useflags/ncurses) | Add ncurses support for expeditor (REPL expression editor) | 

### Emerge

`root #``emerge --ask dev-scheme/racket`
## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose dev-scheme/racket`
### Cleanup

If manually installing any Racket packages in the installation scope, then they might have created some files inside /usr/share/racket/pkgs.

Those files can be cleaned up by removing them with rm as follows:

`root #``rm -r /usr/share/racket/pkgs`
## Installing Racket packages

### Natively via Portage

To install Racket packages in the installation scope in a more controlled way, users may wish to use Portage for this task.
Currently this is one of the aims of the "[Gentoo-Racket](https://gentoo-racket.gitlab.io/)" project.

Gentoo-Racket is maintaining the "[Gentoo-Racket overlay](https://gitlab.com/gentoo-racket/gentoo-racket-overlay/)" on the official GitLab instance.

The overlay can be installed by using the "[Eselect/Repository eselect-repository](https://wiki.gentoo.org/wiki/Eselect/Repository)" tool:

`root #``eselect repository enable racket-overlay`
### Via Racket's Package Manager, Raco

Raco is the built-in package manager of Racket, it can install packages via the following:

`user $``raco pkg install --user`
Please use the switch --user to install packages in the *user scope*. Installation in user scope should be the default but it can be also configured to install packages globally, in the *installation scope*.

## See Also

- [Raku](https://wiki.gentoo.org/wiki/Raku) — a high-level, general-purpose, and gradually typed programming language with low boilerplate objects, optionally immutable data structures, and an advanced macro system.

## External resources

### Racket Community

- The [Racket Subreddit](https://www.reddit.com/r/Racket/)

### Resources for Learning Racket

- [Learn Racket in Y Minutes](https://learnxinyminutes.com/docs/racket/)
- [Beautiful Racket](https://beautifulracket.com/) by Matthew Butterick
