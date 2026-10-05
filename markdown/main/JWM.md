<!-- source: https://wiki.gentoo.org/wiki/JWM | group: Gentoo Wiki (Main) | wiki-title: JWM -->
---
title: JWM
url: https://wiki.gentoo.org/wiki/JWM
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2019-05-24"
fingerprint: a600c14e19821bc4
license: CC BY-SA 4.0
---

# JWM

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**JWM** (**J**oe's **W**indow **M**anager) is an extremely lightweight [window manager](https://wiki.gentoo.org/wiki/Window_manager) for the [X](https://wiki.gentoo.org/wiki/X) window system. Its installed size is less than 1.15 MB, regardless of the enabled USE flags.

## Installation

### USE flags


| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [iconv](https://packages.gentoo.org/useflags/iconv) | Enable support for the iconv character set conversion library | 
| [jpeg](https://packages.gentoo.org/useflags/jpeg) | Add JPEG image support | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [pango](https://packages.gentoo.org/useflags/pango) | Bidirectional text layout support with pango using xft backend. | 
| [png](https://packages.gentoo.org/useflags/png) | Add support for libpng (PNG images) | 
| [svg](https://packages.gentoo.org/useflags/svg) | Add support for SVG (Scalable Vector Graphics) | 
| [truetype](https://packages.gentoo.org/useflags/truetype) | Add support for FreeType and/or FreeType2 fonts | 
| [xinerama](https://packages.gentoo.org/useflags/xinerama) | Add support for querying multi-monitor screen geometry through the Xinerama API | 
| [xpm](https://packages.gentoo.org/useflags/xpm) | Add support for XPM graphics format | 

### Emerge

`root #``emerge --ask x11-wm/jwm`
## Configuration

### Files

Configuration files are in XML format.

- /etc/system.jwmrc - Global (system wide) configuration file.
- \~/.jwmrc - Local (per user) configuration file.

Additional information on configuring JWM can be found in the /usr/share/doc/jwm-2.3.6/ directory.

## Usage

### Invocation

`user $``jwm -h`
compiled options: confirm icons jpeg nls png shape svg xbm xft xinerama xrender system configuration: /etc/system.jwmrc usage: jwm \[ options \]

-display X  Set the X display to use
 -exit       Exit JWM (send \_JWM\_EXIT to the root)
 -f file     Use specified configuration file
 -h          Display this help message
 -p          Parse the configuration file and exit
 -reload     Reload menu (send \_JWM\_RELOAD to the root)
 -restart    Restart JWM (send \_JWM\_RESTART to the root)

-v Display version information

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose x11-wm/jwm`
## See also

- [I3](https://wiki.gentoo.org/wiki/I3) — a minimalist [tiling](https://en.wikipedia.org/wiki/Tiling_window_manager) [window manager](https://wiki.gentoo.org/wiki/Window_manager), completely written from scratch.
