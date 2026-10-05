<!-- source: https://wiki.gentoo.org/wiki/KWin | group: Gentoo Wiki (Main) | wiki-title: KWin -->
---
title: KWin
url: https://wiki.gentoo.org/wiki/KWin
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-02-06"
fingerprint: ce40d00c58d6384e
license: CC BY-SA 4.0
---

# KWin

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

KWin is an [Xorg](https://wiki.gentoo.org/wiki/Xorg) [window manager](https://wiki.gentoo.org/wiki/Window_manager) and [Wayland](https://wiki.gentoo.org/wiki/Wayland) compositor.

## Installation


| [+filecaps](https://packages.gentoo.org/useflags/+filecaps) | Use Linux file capabilities to control privilege rather than set\*id (this is orthogonal to USE=caps which uses capabilities at runtime e.g. libcap) | 
| [+handbook](https://packages.gentoo.org/useflags/+handbook) | Enable handbooks generation for packages by KDE | 
| [+shortcuts](https://packages.gentoo.org/useflags/+shortcuts) | Enable global shortcuts support via kde-plasma/kglobalacceld | 
| [X](https://packages.gentoo.org/useflags/X) | Enable ability to support native X11 applications via x11-base/xwayland | 
| [accessibility](https://packages.gentoo.org/useflags/accessibility) | Add support for accessibility (eg 'at-spi' library) | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [gamepad](https://packages.gentoo.org/useflags/gamepad) | Support using game controllers as input devices | 
| [gles2-only](https://packages.gentoo.org/useflags/gles2-only) | Use GLES 2.0 (OpenGL for Embedded Systems) or later instead of full OpenGL (see also: gles2) | 
| [lock](https://packages.gentoo.org/useflags/lock) | Enable screen locking via kde-plasma/kscreenlocker | 
| [screencast](https://packages.gentoo.org/useflags/screencast) | Enable screencast portal using media-video/pipewire | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Merge

`root #``emerge --ask kde-plasma/kwin`
## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose kde-plasma/kwin`
