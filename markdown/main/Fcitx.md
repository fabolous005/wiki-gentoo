<!-- source: https://wiki.gentoo.org/wiki/Fcitx | group: Gentoo Wiki (Main) | wiki-title: Fcitx -->
---
title: Fcitx
url: https://wiki.gentoo.org/wiki/Fcitx
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-18"
fingerprint: ced1fe1a2c8738e6
license: CC BY-SA 4.0
---

# Fcitx

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Fcitx** (**F**lexible **C**ontext-aware **I**nput **T**ool with e**X**tension support) \[ˈfaɪtɪks\] is an input method framework with support for many languages and scripts.

## Installation

### USE flags


| [+X](https://packages.gentoo.org/useflags/+X) | Add support for X11 | 
| [+autostart](https://packages.gentoo.org/useflags/+autostart) | Enable XDG-compatible autostart of Fcitx | 
| [+emoji](https://packages.gentoo.org/useflags/+emoji) | Enable emoji loading for CLDR | 
| [+enchant](https://packages.gentoo.org/useflags/+enchant) | Enable Enchant backend (using app-text/enchant) for spelling hinting | 
| [+keyboard](https://packages.gentoo.org/useflags/+keyboard) | Enable key event translation with XKB and build keyboard engine | 
| [+server](https://packages.gentoo.org/useflags/+server) | Build a fcitx as server, disable this option if you want to use fcitx as an embedded library | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [presage](https://packages.gentoo.org/useflags/presage) | Enable presage for word predication (not stable) | 
| [system-yoga](https://packages.gentoo.org/useflags/system-yoga) | Use the system-wide dev-libs/yogainstead of bundled. | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [wayland](https://packages.gentoo.org/useflags/wayland) | Enable dev-libs/wayland backend | 

### Emerge

`root #``emerge --ask app-i18n/fcitx`
## Using Fcitx

To use Fcitx under X, the appropriate input method environment variables must be set in the startup files.
In addition to `XMODIFIERS="@im=fcitx"`, `GTK_IM_MODULE` and `QT_IM_MODULE` must be set for [GTK](https://wiki.gentoo.org/wiki/GTK) and [Qt](https://wiki.gentoo.org/wiki/Qt) applications, respectively.

To enable native input method support for [GTK](https://wiki.gentoo.org/wiki/GTK) applications, install [app-i18n/fcitx-gtk](https://packages.gentoo.org/packages/app-i18n/fcitx-gtk) and set `GTK_IM_MODULE=fcitx`.
To enable native input method support for [Qt](https://wiki.gentoo.org/wiki/Qt) applications, install [app-i18n/fcitx-qt](https://packages.gentoo.org/packages/app-i18n/fcitx-qt) and set `QT_IM_MODULE=fcitx`.
In most cases, both modules should be installed and both variables set.

When using login managers such as [LightDM](https://wiki.gentoo.org/wiki/LightDM) to start the X server, add the following to the \~/.xprofile file.

**`~/.xprofile`**

```
export XMODIFIERS="@im=fcitx"
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
```
When starting X manually with the startx command or [SLiM](https://wiki.gentoo.org/wiki/SLiM), add the following to the \~/.xinitrc file.

**`~/.xinitrc`**

```
export XMODIFIERS="@im=fcitx"
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
pgrep -x fcitx5 > /dev/null || fcitx5 -d &
```
For KDE users, if these environment variables are not getting loaded either from \~/.xprofile or \~/.xinitrc, try adding them to a Plasma startup script, such as \~/.config/plasma-workspace/env/fcitx.sh.

Fcitx relies on the Dbus service to run. If it is a systemd init system, no additional configuration is generally required. Otherwise, you need to manually configure the operation of dbus. For details, refer to the [Dbus](https://wiki.gentoo.org/wiki/Dbus) entry.

## Configuration

Edit Fcitx's configuration file at \~/.config/fcitx5/config.

Configure Fcitx using graphical tools, install [app-i18n/fcitx-configtool](https://packages.gentoo.org/packages/app-i18n/fcitx-configtool). For KDE settings integration, enable [kcm](https://packages.gentoo.org/useflags/kcm) [USE flag.](https://wiki.gentoo.org/wiki/USE_flag)

## Specific language support

### Chinese

Fcitx itself has built-in pinyin support. Install [app-i18n/fcitx-chinese-addons](https://packages.gentoo.org/packages/app-i18n/fcitx-chinese-addons) to provide multiple table-based input methods such as WuBi and Ziranma.

Install [app-i18n/fcitx-chinese-addons](https://packages.gentoo.org/packages/app-i18n/fcitx-chinese-addons) with flag [cloudpinyin](https://packages.gentoo.org/useflags/cloudpinyin) [enabled to have better results in the candidate words list.](https://wiki.gentoo.org/wiki/USE_flag)

The built-in pinyin use a simple algorithm, and there are other pinyin input methods using other algorithms. Install [app-i18n/fcitx-rime](https://packages.gentoo.org/packages/app-i18n/fcitx-rime) to use them.

#### Bopomofo

Install [app-i18n/fcitx-chewing](https://packages.gentoo.org/packages/app-i18n/fcitx-chewing):

`root #``emerge --ask app-i18n/fcitx-chewing`
#### Cangjie & Boshiamy

Install [app-i18n/fcitx-table-extra](https://packages.gentoo.org/packages/app-i18n/fcitx-table-extra):

`root #``emerge --ask app-i18n/fcitx-table-extra`
### Japanese

Install [app-i18n/fcitx-anthy](https://packages.gentoo.org/packages/app-i18n/fcitx-anthy):

`root #``emerge --ask app-i18n/fcitx-anthy`
### Korean

Install [app-i18n/fcitx-hangul](https://packages.gentoo.org/packages/app-i18n/fcitx-hangul):

`root #``emerge --ask app-i18n/fcitx-hangul`
### Vietnamese

Install [app-i18n/fcitx-unikey](https://packages.gentoo.org/packages/app-i18n/fcitx-unikey):

`root #``emerge --ask app-i18n/fcitx-unikey`
## TroubleShooting

### Advanced features in some schemas of fcitx-rime doesn't work

Some third-party fcitx-rime schemas use [Lua](https://wiki.gentoo.org/wiki/Lua) to accomplish some advanced features. This could be identified by reading the corresponding schema definition files and the presence of Lua scripts in rime configuration path.

The configuration path of rime is located at `~/.local/share/fcitx5/rime` by default.

For these features to function properly, [app-i18n/fcitx-lua](https://packages.gentoo.org/packages/app-i18n/fcitx-lua) and [app-i18n/librime-lua](https://packages.gentoo.org/packages/app-i18n/librime-lua) have to be installed using matching `LUA_SINGLE_TARGET`  USE flags.

After the installation of these packages, restart fcitx and redeploy rime.

## See also

- [Dbus](https://wiki.gentoo.org/wiki/Dbus) — an interprocess communication (IPC) system for software applications.
- [IBus](https://wiki.gentoo.org/wiki/IBus) — an open source input framework for Linux and Unix.
