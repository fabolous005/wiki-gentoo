<!-- source: https://wiki.gentoo.org/wiki/LilyPond | group: Gentoo Wiki (Main) | wiki-title: LilyPond -->
---
title: LilyPond
url: https://wiki.gentoo.org/wiki/LilyPond
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-17"
fingerprint: b6401ade71bf3624
license: CC BY-SA 4.0
---

# LilyPond

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**LilyPond** is a music engraving program, devoted to producing the highest-quality sheet music possible.

## Installation

### USE flags


| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [emacs](https://packages.gentoo.org/useflags/emacs) | Add support for GNU Emacs | 
| [profile](https://packages.gentoo.org/useflags/profile) | Add support for software performance analysis (will likely vary from ebuild to ebuild) | 

### Emerge

`root #``emerge --ask media-sound/lilypond`
### Usage

LilyPond's documentation [notes](https://lilypond.org/doc/v2.24/Documentation/usage/normal-usage):

Most users run LilyPond through a GUI; if you have not done so already, please read the Tutorial. If you use an alternate editor to write LilyPond files, see the documentation for that program.


[Frescobaldi](https://wiki.gentoo.org/index.php?title=Frescobaldi&action=edit&redlink=1), [media-sound/frescobaldi](https://packages.gentoo.org/packages/media-sound/frescobaldi), is one such GUI: a LilyPond sheet music text editor with MIDI support. Refer to [the Frescobaldi site](https://www.frescobaldi.org/) for further information.

However, LilyPond can be run directly from the command line. For example, to create a PDF from a LilyPond input file, such as the "Welcome to LilyPond" file:

`user $``lilypond /usr/share/lilypond/2.24.4/ly/Welcome_to_LilyPond.ly`
Output of PNG, PS, SVG, and MIDI files is also supported; for information about MIDI output, refer to [the "MIDI block" section of the LilyPond Notation Reference](https://lilypond.org/doc/v2.25/Documentation/notation/the-midi-block).
