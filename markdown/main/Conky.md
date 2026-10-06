<!-- source: https://wiki.gentoo.org/wiki/Conky | group: Gentoo Wiki (Main) | wiki-title: Conky -->
---
title: Conky
url: https://wiki.gentoo.org/wiki/Conky
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-09-24"
fingerprint: de42531c489e38ec
license: CC BY-SA 4.0
---

# Conky

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Conky** is an advanced and highly configurable system monitor for the [Xorg window system](https://wiki.gentoo.org/wiki/Xorg), which "can display arbitrary information (such as the date, CPU temperature from [I2C](https://wiki.gentoo.org/wiki/I2C), [MPD](https://wiki.gentoo.org/wiki/MPD) info, and anything else you desire) to the root window in X. Conky normally does this by drawing to the root window, however Conky can also be run in windowed mode (though this is not how Conky was meant to be used)."[\[1\]](https://wiki.gentoo.org#cite_note-1)

## Installation

### USE flags


### USE flags for
            [app-admin/conky](https://packages.gentoo.org/packages/app-admin/conky)
            
            An advanced, highly configurable system monitor for X

| [+portmon](https://packages.gentoo.org/useflags/+portmon) | Enable support for tcp (ip4) port monitoring | 
| [X](https://packages.gentoo.org/useflags/X) | Add support for X11 | 
| [apcupsd](https://packages.gentoo.org/useflags/apcupsd) | Enable support for sys-power/apcupsd | 
| [bundled-toluapp](https://packages.gentoo.org/useflags/bundled-toluapp) | Enable support for bundled toluapp. This only makes sense in combination with the lua-\* flags | 
| [cmus](https://packages.gentoo.org/useflags/cmus) | Enable monitoring of music played by media-sound/cmus | 
| [colour-name-map](https://packages.gentoo.org/useflags/colour-name-map) | Include mappings of colour name | 
| [curl](https://packages.gentoo.org/useflags/curl) | Add support for client-side URL transfer library | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [extras](https://packages.gentoo.org/useflags/extras) | Enable syntax highlighting for app-editors/nanoand app-editors/vim | 
| [hddtemp](https://packages.gentoo.org/useflags/hddtemp) | Enable monitoring of hdd temperature (app-admin/hddtemp) | 
| [ical](https://packages.gentoo.org/useflags/ical) | Enable support for events from iCalendar (RFC 5545) files using dev-libs/libical | 
| [iconv](https://packages.gentoo.org/useflags/iconv) | Enable support for the iconv character set conversion library | 
| [imlib](https://packages.gentoo.org/useflags/imlib) | Add support for imlib, an image loading and rendering library | 
| [intel-backlight](https://packages.gentoo.org/useflags/intel-backlight) | Enable support for Intel backlight | 
| [iostats](https://packages.gentoo.org/useflags/iostats) | Enable support for per-task I/O statistics | 
| [irc](https://packages.gentoo.org/useflags/irc) | Enable support for displaying everything from an irc channel using net-libs/libircclient | 
| [lua-cairo](https://packages.gentoo.org/useflags/lua-cairo) | Enable if you want Lua Cairo bindings | 
| [lua-cairo-xlib](https://packages.gentoo.org/useflags/lua-cairo-xlib) | Enable support for Cairo and Xlib interoperability for Lua | 
| [lua-imlib](https://packages.gentoo.org/useflags/lua-imlib) | Enable if you want Lua Imlib2 bindings | 
| [lua-rsvg](https://packages.gentoo.org/useflags/lua-rsvg) | Enable if you want Lua RSVG bindings | 
| [math](https://packages.gentoo.org/useflags/math) | Enable support for glibc's libm math library | 
| [moc](https://packages.gentoo.org/useflags/moc) | Enable monitoring of music played by media-sound/moc | 
| [mouse-events](https://packages.gentoo.org/useflags/mouse-events) | Enable support for mouse events" | 
| [mpd](https://packages.gentoo.org/useflags/mpd) | Enable monitoring of music controlled by media-sound/mpd | 
| [mysql](https://packages.gentoo.org/useflags/mysql) | Add mySQL Database support | 
| [ncurses](https://packages.gentoo.org/useflags/ncurses) | Add ncurses support (console display library) | 
| [nvidia](https://packages.gentoo.org/useflags/nvidia) | Enable Nvidia variables with NvCtrl via x11-drivers/nvidia-drivers | 
| [nvidia-nvml](https://packages.gentoo.org/useflags/nvidia-nvml) | Enable Nvidia variables with NVML via x11-drivers/nvidia-drivers | 
| [pulseaudio](https://packages.gentoo.org/useflags/pulseaudio) | Add sound server support via media-libs/libpulse (may be PulseAudio or PipeWire) | 
| [rss](https://packages.gentoo.org/useflags/rss) | Enable support for RSS feeds | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [thinkpad](https://packages.gentoo.org/useflags/thinkpad) | Enable support for IBM/Lenovo notebooks | 
| [truetype](https://packages.gentoo.org/useflags/truetype) | Add support for FreeType and/or FreeType2 fonts | 
| [wayland](https://packages.gentoo.org/useflags/wayland) | Enable dev-libs/wayland backend | 
| [webserver](https://packages.gentoo.org/useflags/webserver) | Enable support to act as a webserver serving conkys output using net-libs/libmicrohttpd | 
| [wifi](https://packages.gentoo.org/useflags/wifi) | Enable wireless network functions | 
| [xinerama](https://packages.gentoo.org/useflags/xinerama) | Add support for querying multi-monitor screen geometry through the Xinerama API | 
| [xinput](https://packages.gentoo.org/useflags/xinput) | Enable support for Xinput 2 (slow) | 
| [xmms2](https://packages.gentoo.org/useflags/xmms2) | Enable monitoring of music played by media-sound/xmms2 | 

### Emerge

Install [app-admin/conky](https://packages.gentoo.org/packages/app-admin/conky):

`root #``emerge --ask app-admin/conky`
## Configuration

After installing Conky, create a default configuration as a starting point:

`user $``mkdir -p ~/.config/conky && conky -C > ~/.config/conky/conky.conf` Open the configuration file with a text editor of choice and edit away. Enabling lua syntax highlighting may help.

Users of modern composited desktop environments will probably want to use conky in own window mode with true transparency:

FILE **`~/.config/conky/conky.conf`**

```
own_window = true,
own_window_class = 'conky',
own_window_argb_visual = true,
own_window_argb_value = 80,
own_window_hints = 'undecorated,below,sticky,skip_taskbar,skip_pager',
own_window_colour = '101010',
own_window_type = 'desktop'
```
## See also

- [Conky/Guide](https://wiki.gentoo.org/wiki/Conky/Guide) — describes how to install and configure the system monitor known as Conky.
