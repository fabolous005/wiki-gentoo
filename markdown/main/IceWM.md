<!-- source: https://wiki.gentoo.org/wiki/IceWM | group: Gentoo Wiki (Main) | wiki-title: IceWM -->
---
title: IceWM
url: https://wiki.gentoo.org/wiki/IceWM
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-09-22"
fingerprint: ae42499c19aa73cc
license: CC BY-SA 4.0
---

# IceWM

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**IceWM** is a free and open-source, lightweight, stacking [window manager](https://wiki.gentoo.org/wiki/Window_manager) for [X11](https://wiki.gentoo.org/wiki/X11). It is written in C++ and is designed to be easily customizable. It is also used by lightweight, beginner-friendly distributions like Puppy Linux.

## Installation

### USE flags


| [+alsa](https://packages.gentoo.org/useflags/+alsa) | Add support for media-libs/alsa-lib (Advanced Linux Sound Architecture) | 
| [+gdk-pixbuf](https://packages.gentoo.org/useflags/+gdk-pixbuf) | Enable gdk-pixbuf rendering | 
| [ao](https://packages.gentoo.org/useflags/ao) | Use libao audio output library for sound playback | 
| [bidi](https://packages.gentoo.org/useflags/bidi) | Enable bidirectional language support | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [imlib](https://packages.gentoo.org/useflags/imlib) | Add support for imlib, an image loading and rendering library | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [truetype](https://packages.gentoo.org/useflags/truetype) | Add support for FreeType and/or FreeType2 fonts | 
| [xinerama](https://packages.gentoo.org/useflags/xinerama) | Add support for querying multi-monitor screen geometry through the Xinerama API | 

### Emerge

Install [x11-wm/icewm](https://packages.gentoo.org/packages/x11-wm/icewm) with:

`root #``emerge --ask x11-wm/icewm`
