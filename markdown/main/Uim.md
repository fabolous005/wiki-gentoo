<!-- source: https://wiki.gentoo.org/wiki/Uim | group: Gentoo Wiki (Main) | wiki-title: Uim -->
---
title: Uim
url: https://wiki.gentoo.org/wiki/Uim
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-05-23"
fingerprint: eee3995cc9963944
license: CC BY-SA 4.0
---

# Uim

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**uim** is a multilingual input method library and environment.

## Installation

### USE flags


| [+anthy](https://packages.gentoo.org/useflags/+anthy) | Enable support for app-i18n/anthy-unicode or app-i18n/anthy if prior 1.9.0 | 
| [X](https://packages.gentoo.org/useflags/X) | Add support for X11 | 
| [curl](https://packages.gentoo.org/useflags/curl) | Add support for client-side URL transfer library | 
| [eb](https://packages.gentoo.org/useflags/eb) | Enable support for dev-libs/eb | 
| [emacs](https://packages.gentoo.org/useflags/emacs) | Add support for GNU Emacs | 
| [expat](https://packages.gentoo.org/useflags/expat) | Enable the use of dev-libs/expat for XML parsing | 
| [gtk](https://packages.gentoo.org/useflags/gtk) | Add support for x11-libs/gtk+ (The GIMP Toolkit) | 
| [gtk2](https://packages.gentoo.org/useflags/gtk2) | Enable support for x11-libs/gtk+:2 | 
| [libedit](https://packages.gentoo.org/useflags/libedit) | Use the libedit library (replacement for readline) | 
| [libffi](https://packages.gentoo.org/useflags/libffi) | Enable support for Foreign Function Interface library | 
| [libnotify](https://packages.gentoo.org/useflags/libnotify) | Enable desktop notification support | 
| [m17n-lib](https://packages.gentoo.org/useflags/m17n-lib) | Enable m17n-lib support | 
| [ncurses](https://packages.gentoo.org/useflags/ncurses) | Add ncurses support (console display library) | 
| [qt6](https://packages.gentoo.org/useflags/qt6) | Add support for the Qt 6 application and UI framework | 
| [skk](https://packages.gentoo.org/useflags/skk) | Enable support for app-i18n/skk-jisyo | 
| [sqlite](https://packages.gentoo.org/useflags/sqlite) | Add support for sqlite - embedded sql database | 
| [ssl](https://packages.gentoo.org/useflags/ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 
| [static-libs](https://packages.gentoo.org/useflags/static-libs) | Build static versions of dynamic libraries as well | 

### Emerge

`root #``emerge --ask app-i18n/uim`
## Configuration

### Bridge Configuration

#### X

To use uim as an input method on X, add the following lines to an X startup script:

**`~/.xprofile`**

```
# Set up environment variables for various applications
export GTK_IM_MODULE=uim    # Required for Gtk applications
export QT_IM_MODULE=uim     # Required for Qt applications
export XMODIFIERS=@im=uim   # Required for X applications without Gtk or Qt
# Start the XIM bridge (required for X applications without Gtk or Qt)
uim-xim &
```
#### uim-fep

uim can be used as an input method on a console environment via uim-fep bridge. No configurations are needed, so just run it on the console:

`user $``uim-fep`
#### uim.el

When uim is emerged with the [emacs](https://packages.gentoo.org/useflags/emacs) [USE flag, a Emacs package](https://wiki.gentoo.org/wiki/USE_flag) `uim.el` is also installed.

**`~/.emacs.d/init.el`**

```
;; read uim.el
(require 'uim)
;; uncomment next and comment out previous to load uim.el on-demand
;; (autoload 'uim-mode "uim" nil t)
;; set default IM (ex. use Anthy)
;; (setq uim-default-im-engine "anthy")
;; key-binding for activate uim (ex. C-o)
(global-set-key "\C-o" 'uim-mode)
```
### Preferences

User preferences are defined in two ways:

- via GUI Application (uim-pref-gtk3 or uim-pref-qt5);
- or editing \~/.uim file.

## See also

- [Input methods](https://wiki.gentoo.org/wiki/Input_methods)
- [IBus](https://wiki.gentoo.org/wiki/IBus) — an open source input framework for Linux and Unix.
- [Fcitx](https://wiki.gentoo.org/wiki/Fcitx) — an input method framework with support for many languages and scripts.
