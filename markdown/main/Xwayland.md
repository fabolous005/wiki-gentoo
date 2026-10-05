<!-- source: https://wiki.gentoo.org/wiki/Xwayland | group: Gentoo Wiki (Main) | wiki-title: Xwayland -->
---
title: Xwayland
url: https://wiki.gentoo.org/wiki/Xwayland
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-03-27"
fingerprint: df9375596ae02cc9
license: CC BY-SA 4.0
---

# Xwayland

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Xwayland is an X server for running X clients under [Wayland](https://wiki.gentoo.org/wiki/Wayland). It is built from a collection of patches to the X.Org server codebase, created and maintained by Wayland developers.

Not all software supports, or plans to support, running under Wayland. Wayland compositors can use Xwayland to allow use of such software within their environment.

## Installation

### USE flags


| [libei](https://packages.gentoo.org/useflags/libei) | Enable emulated input using dev-libs/libei | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [unwind](https://packages.gentoo.org/useflags/unwind) | Enable libunwind usage for backtraces | 
| [xcsecurity](https://packages.gentoo.org/useflags/xcsecurity) | Build Security extension | 

Certain packages need to be built with certain USE flags in order to utilize Xwayland:

| Package | USE flag | 
|---|---|
| [dev-libs/weston](https://packages.gentoo.org/packages/dev-libs/weston) | `xwayland` | 
| [gui-libs/wlroots](https://packages.gentoo.org/packages/gui-libs/wlroots) | `X` | 
| [gui-wm/sway](https://packages.gentoo.org/packages/gui-wm/sway) | `X` | 
| [x11-wm/enlightenment](https://packages.gentoo.org/packages/x11-wm/enlightenment) | `xwayland` | 

### Emerge

`root #``emerge --ask x11-base/xwayland`
## Usage

Normally Xwayland shouldn't need to be started manually; it should be started on-demand by the running Wayland compositor.

Once started, there should be an "Xwayland" entry in the list of running processes, e.g.:

Xwayland :0 -rootless -core -terminate -listenfd 26 -listenfd 27 -displayfd 60 -wm 57

[Xwayland(1)](https://man.archlinux.org/man/Xwayland.1.en) [accepts the same command-line arguments as](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [Xserver(1)](https://man.archlinux.org/man/Xserver.1.en)[, followed by various arguments specifically handled by Xwayland. In the above example, the following arguments are](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [Xserver(1)](https://man.archlinux.org/man/Xserver.1.en) [arguments:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

- `:0`: Request "0" as the display number for the server. Refer to the discussion about the `-displayfd` argument, below, regarding situations where a different display number is required.

- `-core`: "\[C\]auses the server to generate a core dump on fatal errors."

- `-displayfd 60`: "Rather than specify a display number, the X server will attempt to listen on successively higher display numbers, and upon finding a free one, will write the display number back on this file descriptor as a newline-terminated string."

- `-terminate`: "\[C\]auses the server to terminate at server reset, instead of continuing to run."

The remaining arguments are handled by [Xwayland(1)](https://man.archlinux.org/man/Xwayland.1.en)[:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

- `-rootless`: "Run Xwayland rootless, so that X clients integrate seamlessly with Wayland clients in a Wayland desktop. That requires the Wayland server to be an X window manager as well."

- `-listenfd 26`, `-listenfd 27`: "Add given fd as a listen socket. This option is used by the Wayland server to pass Xwayland the socket where X clients connect."

- `-wm 57`: "\[U\]sed by the Wayland server to pass Xwayland the socket where the X window manager client connects".

### The DISPLAY environment variable

In general, as the compositor is typically responsible for starting an Xwayland instance, it also needs to set the X `DISPLAY` environment variable appropriately - such as at the start of a session - for use by client applications. If a particular compositor does not seem to be doing so, please refer to the compositor's documentation and/or support communities.

## See also

- [Xserver(1)](https://man.archlinux.org/man/Xserver.1.en)- [Xwayland(1)](https://man.archlinux.org/man/Xwayland.1.en)
