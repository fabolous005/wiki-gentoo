<!-- source: https://wiki.gentoo.org/wiki/FVWM | group: Gentoo Wiki (Main) | wiki-title: FVWM -->
---
title: FVWM
url: https://wiki.gentoo.org/wiki/FVWM
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-07-23"
fingerprint: d203615cd19a18ec
license: CC BY-SA 4.0
---

# FVWM

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**FVWM** (**F V**irtual **W**indow **M**anager) is a stacking [window manager](https://wiki.gentoo.org/wiki/Window_manager) for [Xorg](https://wiki.gentoo.org/wiki/Xorg). It is designed to minimize memory consumption, provide a 3D look to window frames, and to provide a virtual desktop. It is also possible to extend FVWM using C, M4 and Perl preprocessing or scripts, in the case of Perl.

## Installation

### USE flags


### USE flags for
            [x11-wm/fvwm3](https://packages.gentoo.org/packages/x11-wm/fvwm3)
            
            A multiple large virtual desktop window manager derived from fvwm

| [+go](https://packages.gentoo.org/useflags/+go) | Enable building dev-lang/go code (FvwmPrompt) | 
| [bidi](https://packages.gentoo.org/useflags/bidi) | Enable bidirectional language support | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [readline](https://packages.gentoo.org/useflags/readline) | Enable support for libreadline, a GNU line-editing library that almost everyone wants | 
| [svg](https://packages.gentoo.org/useflags/svg) | Add support for SVG (Scalable Vector Graphics) | 

### Emerge

After flags have been set issue an emerge command to install FVWM:

`root #``emerge --ask x11-wm/fvwm3`
## Configuration

FVWM's main configuration file is \~/.fvwm/config.

### Starting

To start FVWM use a [display manager](https://wiki.gentoo.org/wiki/Display_manager) or the startx command.

When using startx with [elogind](https://wiki.gentoo.org/wiki/Elogind) support, setup elogind and create the following file:

FILE **`~/.xinitrc`**

```
exec dbus-launch --sh-syntax --exit-with-session fvwm
```
## See also

- [FVWM-Crystal](https://wiki.gentoo.org/wiki/FVWM-Crystal) — an easy to use, powerful and pretty desktop environment.
