<!-- source: https://wiki.gentoo.org/wiki/SuperCollider | group: Gentoo Wiki (Main) | wiki-title: SuperCollider -->
---
title: SuperCollider
url: https://wiki.gentoo.org/wiki/SuperCollider
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-17"
fingerprint: "8e225d5fe9a2f8d6"
license: CC BY-SA 4.0
---

# SuperCollider

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

SuperCollider is a platform for audio synthesis and algorithmic composition.

## Installation

### USE flags


| [+fftw](https://packages.gentoo.org/useflags/+fftw) | Use FFTW library for computing Fourier transforms | 
| [+gpl3](https://packages.gentoo.org/useflags/+gpl3) | Build GPL-3 licensed code (recommended) | 
| [+sndfile](https://packages.gentoo.org/useflags/+sndfile) | Add support for libsndfile | 
| [+zeroconf](https://packages.gentoo.org/useflags/+zeroconf) | Support for DNS Service Discovery (DNS-SD) | 
| [X](https://packages.gentoo.org/useflags/X) | Add support for X11 | 
| [ableton-link](https://packages.gentoo.org/useflags/ableton-link) | Enable support for Ableton Link | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [emacs](https://packages.gentoo.org/useflags/emacs) | Enable the SCEL user interface | 
| [jack](https://packages.gentoo.org/useflags/jack) | Add support for the JACK Audio Connection Kit | 
| [qt6](https://packages.gentoo.org/useflags/qt6) | Add support for the Qt 6 application and UI framework | 
| [server](https://packages.gentoo.org/useflags/server) | Build with internal server | 
| [static-libs](https://packages.gentoo.org/useflags/static-libs) | Build static versions of dynamic libraries as well | 
| [vim](https://packages.gentoo.org/useflags/vim) | Enable the SCVIM user interface | 
| [webengine](https://packages.gentoo.org/useflags/webengine) | Enable the internal help system using dev-qt/qtwebengine | 

### Emerge

`root #``emerge --ask media-sound/supercollider`
## Usage

The [media-sound/supercollider](https://packages.gentoo.org/packages/media-sound/supercollider) package provides three binaries:

- **sclang**, the interpreter for the SuperCollider language. An interactive session can be started from the command line via sclang; a server can be started via sclang -D.
- **scide**, the IDE. An introduction to its use can be found in the " [Getting started with SC](https://doc.sccode.org/Tutorials/Getting-Started/00-Getting-Started-With-SC.html)" tutorial.
- **scsynth**, a SuperCollider synthesizer.

There are no man pages for these binaries, but command-line options for **sclang** and **scsynth** can be listed by passing the `-help` option to either.

### Emacs

To make **scel** available in Emacs:
