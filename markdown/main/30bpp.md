<!-- source: https://wiki.gentoo.org/wiki/30bpp | group: Gentoo Wiki (Main) | wiki-title: 30bpp -->
---
title: "30bpp"
url: https://wiki.gentoo.org/wiki/30bpp
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-08-14"
fingerprint: d6f8b34d6ac52f97
license: CC BY-SA 4.0
---

# 30bpp

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

As of 2018, it is possible to run Xorg in 30bpp mode (sometimes branded as “Deep Color”) with radeon or amdgpu open drivers, and a capable monitor or two. The extra precision is mainly beneficial for graphical editing work, but can also improve general UI quality by reducing gradient banding artifacts. This feature comes with a number of caveats though; see below for known software compatibility issues.

## Prerequisites

A digital connector (DVI, HDMI, DisplayPort) is needed - although VGA might work, the extra precision gets lost in the analog noise. Consult your monitor's documentation to ensure it is 30bpp-capable first; it should be safe to attempt this in any case, but if yours lacks support expect to see “signal out of range” errors.

First, test that the basic server works by running it from the console (without X already running):

`user $``Xorg -depth 30 -retro`
You should see the traditional X11 black and white stipple pattern if everything's working. To get out of this screen, `Ctrl`+`Alt`+`F1` to switch back to the console and `Ctrl`+`c` there to kill the server. If it works, you can move on to making the change more permanent.

## Configuring the X server for 30bpp

Users of startx can append the option to their command line:

`user $``startx -- -depth 30`
If you have a display manager, find the X server arguments in its configuration files and append `-depth 30`.

To set 30bpp in xorg.conf instead, using slightly longer code:

**`/etc/X11/xorg.conf.d/30bpp.conf`**

**Sample xorg.conf fragment**

```
Section "Screen"
  Identifier "Screen Name"
  DefaultDepth 30
EndSection
```
## Compatibility

While most GUI toolkits have no problem with 30bpp, individual software may have graphical issues or crash outright. Anything that uses [media-libs/imlib2](https://packages.gentoo.org/packages/media-libs/imlib2) or [media-libs/libsdl](https://packages.gentoo.org/packages/media-libs/libsdl) will either crash, not display anything, or misrender. This includes quite a long list of old games, as well as tools that interact with the screen.

- [dev-java/icedtea](https://packages.gentoo.org/packages/dev-java/icedtea) - Applications using AWT will crash immediately on startup, with a `sun.java2d.InvalidPipeException` stating that 30bpp is unsupported. This can be *partially* fixed by setting `_JAVA_OPTIONS='-Dsun.java2d.opengl=true'` in your environment.
- [games-engines/odamex](https://packages.gentoo.org/packages/games-engines/odamex) - black screen
- [games-fps/ut2004](https://packages.gentoo.org/packages/games-fps/ut2004) - crashes at startup
- [media-gfx/scrot](https://packages.gentoo.org/packages/media-gfx/scrot) - captures black screenshots
- [media-video/mpv](https://packages.gentoo.org/packages/media-video/mpv) - `--vo=vdpau` produces garbled output. `--vo=opengl --hwdec=vdpau` works fine, and is the default.
- [media-video/obs-studio](https://packages.gentoo.org/packages/media-video/obs-studio) - crashes at startup
- [x11-misc/slim](https://packages.gentoo.org/packages/x11-misc/slim) - crashes at startup
- [x11-wm/openbox](https://packages.gentoo.org/packages/x11-wm/openbox) - mostly works, but window decoration backgrounds are drawn too dark (i.e. `#0FF0FF0FF` instead of `#FFFFFF`).
- [net-im/slack](https://packages.gentoo.org/packages/net-im/slack) - render in wrong colors, almost all is pink, fonts are unreadable
- [net-im/skypeforlinux](https://packages.gentoo.org/packages/net-im/skypeforlinux) - render in wrong colors, almost all is pink, fonts are unreadable
- [Steam](https://wiki.gentoo.org/wiki/Steam) - updater progress box renders wrong, main app crashes at startup
