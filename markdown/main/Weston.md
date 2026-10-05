<!-- source: https://wiki.gentoo.org/wiki/Weston | group: Gentoo Wiki (Main) | wiki-title: Weston -->
---
title: Weston
url: https://wiki.gentoo.org/wiki/Weston
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-05-31"
fingerprint: ed22d114d89939dc
license: CC BY-SA 4.0
---

# Weston

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Weston** is a reference implementation of a [Wayland compositor](https://wiki.gentoo.org/wiki/Wayland_compositor). It is part of the Wayland project and can run as an [X](https://wiki.gentoo.org/wiki/X) client or under Linux Kernel Mode Setting (KMS).

## Installation

### USE flags


| [+X](https://packages.gentoo.org/useflags/+X) | Add support for X11 | 
| [+desktop](https://packages.gentoo.org/useflags/+desktop) | Enable the desktop shell | 
| [+drm](https://packages.gentoo.org/useflags/+drm) | Enable drm compositor support | 
| [+gles2](https://packages.gentoo.org/useflags/+gles2) | Enable the GLESv2 renderer, not just the x11-libs/pixman-based software fallback | 
| [+resize-optimization](https://packages.gentoo.org/useflags/+resize-optimization) | Increase performance, allocate more RAM. Recommended to disable on Raspberry Pi | 
| [+suid](https://packages.gentoo.org/useflags/+suid) | Enable setuid root program(s) | 
| [editor](https://packages.gentoo.org/useflags/editor) | Install wayland-editor example application | 
| [examples](https://packages.gentoo.org/useflags/examples) | Install examples, usually source code | 
| [fullscreen](https://packages.gentoo.org/useflags/fullscreen) | Enable fullscreen shell | 
| [headless](https://packages.gentoo.org/useflags/headless) | Headless backend and a noop renderer, mainly for testing purposes | 
| [ivi](https://packages.gentoo.org/useflags/ivi) | Enable the IVI shell | 
| [jpeg](https://packages.gentoo.org/useflags/jpeg) | Add JPEG image support | 
| [kiosk](https://packages.gentoo.org/useflags/kiosk) | Enable the kiosk shell | 
| [lcms](https://packages.gentoo.org/useflags/lcms) | Add lcms support (color management engine) | 
| [lua](https://packages.gentoo.org/useflags/lua) | Enable Lua scripting support | 
| [pipewire](https://packages.gentoo.org/useflags/pipewire) | Enable virtual remote output with Pipewire on DRM backend | 
| [rdp](https://packages.gentoo.org/useflags/rdp) | Enable Remote Desktop Protocol compositor support | 
| [remoting](https://packages.gentoo.org/useflags/remoting) | Enable plugin to stream output to remote hosts using media-libs/gstreamer | 
| [screen-sharing](https://packages.gentoo.org/useflags/screen-sharing) | Enable screen-sharing through RDP | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [vnc](https://packages.gentoo.org/useflags/vnc) | Enable VNC (remote desktop viewer) support | 
| [vulkan](https://packages.gentoo.org/useflags/vulkan) | Add support for 3D graphics and computing via the Vulkan cross-platform API | 
| [wayland-compositor](https://packages.gentoo.org/useflags/wayland-compositor) | Enable Wayland compositor support | 
| [webp](https://packages.gentoo.org/useflags/webp) | Add support for the WebP image format | 
| [xwayland](https://packages.gentoo.org/useflags/xwayland) | Enable ability support native X11 applications | 

### Emerge

`root #``emerge --ask dev-libs/weston`
## Usage

The Weston compositor is a minimal and fast compositor and is suitable for many embedded and mobile use cases.

Enable the `examples` USE flag for building example applications like weston-image or weston-view.

Weston is configured on a local level with the \~/.config/weston.ini file (cf. man 5 weston.ini).

The *environment variable* can be defined in the usual configuration files. For example, if  [Larry the cow (Larry)](https://wiki.gentoo.org/wiki/User:Larry)  sets `XDG_RUNTIME_DIR` variable in his [Bash](https://wiki.gentoo.org/wiki/Bash) shell's configuration file and he has chosen that the directory will be in /tmp.

**`/home/larry/.bash_profile`**

**Set`XDG_RUNTIME_DIR`**

```
#!/bin/bash
if test -z "${XDG_RUNTIME_DIR}"; then
    export XDG_RUNTIME_DIR=/tmp/${UID}-runtime-dir
    if ! test -d "${XDG_RUNTIME_DIR}"; then
        mkdir "${XDG_RUNTIME_DIR}"
        chmod 0700 "${XDG_RUNTIME_DIR}"
    fi
fi
```
To launch the compositor as a standalone display server (i) enable systemd session support for weston-launch (by USE=systemd), (ii) or users without systemd are referred to the section [#weston-launch without systemd](https://wiki.gentoo.org#weston-launch_without_systemd) below.

On a VT (outside of X), launch Weston with the DRM backend:

`user $``weston-launch`
Ditto, with XWayland support:

`user $``weston-launch -- --xwayland`
Nest a weston instance "wayland-1" in another Weston "wayland-0":

`user $``WAYLAND_DISPLAY=wayland-0 weston -Swayland-1` From an X terminal, launch Weston with the x11 backend:

`user $``weston`
### weston without systemd (weston-9.0.0-r1)

As of Nov 2021 (weston-9.0.0-r1), users without systemd need the workaround below. As solved in [bug #479468](https://bugs.gentoo.org/show_bug.cgi?id=479468), weston-9999 introduced the USE flag "seatd", enabling elogind as a substitute of systemd.

You have to create the group named "weston-launch", and add the user to that group:

`root #````
groupadd weston-launch
```
`root #``usermod -a -G weston-launch` *user-name*
Notice: This might be unbelievable, but true: The group name "weston-launch" is hardcoded, and the command weston-launch checks if the user belongs to it. It is *not* relevant e.g. a device file is writable to a user.

### weston without systemd (weston-10.0.0)

Users without systemd can use either [seatd](https://wiki.gentoo.org/wiki/Seatd) or [elogind](https://wiki.gentoo.org/wiki/Elogind). Either service must be running before starting weston.

For [seatd](https://wiki.gentoo.org/wiki/Seatd) the user running weston must be a member of the video group, otherwise the unix-socket */run/seatd.sock* for seatd access is not available

## See also

- [Wayland](https://wiki.gentoo.org/wiki/Wayland) — a [communication protocol](https://en.wikipedia.org/wiki/communication_protocol) between a [display server](https://en.wikipedia.org/wiki/display_server) and its clients
- [Xorg](https://wiki.gentoo.org/wiki/Xorg) — an open source implementation of the [X server](https://wiki.gentoo.org/wiki/X_server).
