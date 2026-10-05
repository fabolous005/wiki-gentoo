<!-- source: https://wiki.gentoo.org/wiki/Mutter | group: Gentoo Wiki (Main) | wiki-title: Mutter -->
---
title: Mutter
url: https://wiki.gentoo.org/wiki/Mutter
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-02-06"
fingerprint: ce00d05c599638cc
license: CC BY-SA 4.0
---

# Mutter

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Mutter is a [Wayland](https://wiki.gentoo.org/wiki/Wayland) display server and [X11](https://wiki.gentoo.org/wiki/X11) [window manager](https://wiki.gentoo.org/wiki/Window_manager) and compositor library.

## Installation


| [+introspection](https://packages.gentoo.org/useflags/+introspection) | Add support for GObject based introspection | 
| [+wayland](https://packages.gentoo.org/useflags/+wayland) | Enable dev-libs/wayland backend | 
| [+xwayland](https://packages.gentoo.org/useflags/+xwayland) | Enable x11-base/xwayland integration for running X11 applications under Wayland | 
| [X](https://packages.gentoo.org/useflags/X) | Add support for X11 | 
| [bash-completion](https://packages.gentoo.org/useflags/bash-completion) | Enable bash-completion support | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [devkit](https://packages.gentoo.org/useflags/devkit) | Enable support for running a nested wayland session | 
| [elogind](https://packages.gentoo.org/useflags/elogind) | Rely on sys-auth/elogind as logind provider for Wayland sessions | 
| [gnome](https://packages.gentoo.org/useflags/gnome) | Add GNOME support | 
| [gtk-doc](https://packages.gentoo.org/useflags/gtk-doc) | Build and install gtk-doc based developer documentation for dev-util/devhelp, IDE and offline use | 
| [screencast](https://packages.gentoo.org/useflags/screencast) | Enable support for remote desktop and screen cast using PipeWire | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [sysprof](https://packages.gentoo.org/useflags/sysprof) | Enable profiling data capture support using dev-util/sysprof-capture | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [udev](https://packages.gentoo.org/useflags/udev) | Enable virtual/udev integration (device discovery, power and storage device support, etc) | 

### Merge

`root #``emerge --ask x11-wm/mutter`
## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose x11-wm/mutter`
