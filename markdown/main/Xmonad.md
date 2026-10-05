<!-- source: https://wiki.gentoo.org/wiki/Xmonad | group: Gentoo Wiki (Main) | wiki-title: Xmonad -->
---
title: xmonad
url: https://wiki.gentoo.org/wiki/Xmonad
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-24"
fingerprint: e6900fec40b71bd6
license: CC BY-SA 4.0
---

# xmonad

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**xmonad** is a fast and lightweight tiling [window manager](https://wiki.gentoo.org/wiki/Window_manager) for [X11](https://wiki.gentoo.org/wiki/X11), written, configured, and extended in the purely-functional programming language [Haskell](https://wiki.gentoo.org/wiki/Haskell).

## Installation

### USE flags


| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [hscolour](https://packages.gentoo.org/useflags/hscolour) | Include coloured haskell sources to generated documentation (dev-haskell/hscolour) | 
| [no-autorepeat-keys](https://packages.gentoo.org/useflags/no-autorepeat-keys) | Allow ignoring of keyboard autorepeat. | 
| [profile](https://packages.gentoo.org/useflags/profile) | Add support for software performance analysis (will likely vary from ebuild to ebuild) | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

There are two ways to install XMonad. The Gentoo recommended method is to use Portage so that the package will be integrated into the system's package database.

### Emerge

Merge the [x11-wm/xmonad](https://packages.gentoo.org/packages/x11-wm/xmonad) package:

`root #``emerge --ask x11-wm/xmonad`
It may be preferable to install XMonad (and other haskell packages) from the Gentoo Haskell repository. Instructions for enabling this can be found on the [Haskell page](https://wiki.gentoo.org/wiki/Haskell#Getting_started).

### Cabal (unsupported)

It is possible to install using [cabal](https://wiki.gentoo.org/wiki/Haskell#Cabal), although it is not the Gentoo recommended method for installation system-wide packages.

`user $``cabal install xmonad`
## Configuration

### Starting

Start XMonad using a [display manager](https://wiki.gentoo.org/wiki/Display_manager) or the startx command.

To use startx with [elogind](https://wiki.gentoo.org/wiki/Elogind) support, add the following to \~/.xinitrc:

**`~/.xinitrc`**

```
exec dbus-launch --sh-syntax --exit-with-session xmonad
```
### Basic configuration

XMonad itself can be configured in \~/.config/xmonad/xmonad.hs, which is written in the Haskell programming language.

Minimal configuration file with default configuration:

**`~/.config/xmonad/xmonad.hs`**

```
import XMonad
main = xmonad $ def
```
After editing the configuration file, XMonad must be recompiled and restarted to apply the changes. This can be done by running:

`user $````
xmonad --recompile && xmonad --restart
```
In most cases, to write a config file, additional features provided by the xmonad-contrib library are necessary. It can be installed from [x11-wm/xmonad-contrib](https://packages.gentoo.org/packages/x11-wm/xmonad-contrib):

`root #``emerge --ask xmonad-contrib`
Or using [cabal](https://wiki.gentoo.org/wiki/Haskell#Cabal) (not recommended):

`user $``cabal install xmonad-contrib`
Further configuration information can be found in the [official XMonad tutorial](https://xmonad.org/TUTORIAL.html).

### Adding status bars

Unlike other window managers, XMonad does not have a built-in status bar. Instead, the required information can be piped to an external program. Xmobar is a good choice for XMonad, but it will also work with other window managers. Dzen is also compatible with XMonad.

#### xmobar

Install [x11-misc/xmobar](https://packages.gentoo.org/packages/x11-misc/xmobar):

`root #``emerge --ask x11-misc/xmobar`
Information on configuring xmobar can also be found on the XMonad website, as linked in the configuration section.

#### dzen

Install [x11-misc/dzen](https://packages.gentoo.org/packages/x11-misc/dzen):

`root #``emerge --ask x11-misc/dzen`
### Tray programs

Xmonad does not include a tray program (for application icons, volume controls, etc.), but both [x11-misc/trayer](https://packages.gentoo.org/packages/x11-misc/trayer) and [x11-misc/trayer-srg](https://packages.gentoo.org/packages/x11-misc/trayer-srg) are compatible. Both can also be configured to have various size and position options, allowing for seamless integration with a status bar.

`root #``emerge --ask x11-misc/trayer``root #``emerge --ask x11-misc/trayer-srg`
