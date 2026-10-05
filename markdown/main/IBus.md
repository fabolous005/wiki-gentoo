<!-- source: https://wiki.gentoo.org/wiki/IBus | group: Gentoo Wiki (Main) | wiki-title: IBus -->
---
title: IBus
url: https://wiki.gentoo.org/wiki/IBus
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-06-24"
fingerprint: "6e4aba5845cf385c"
license: CC BY-SA 4.0
---

# IBus

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**IBus** (**I**ntelligent Input **Bus**) is an open source input framework for Linux and Unix.

## Installation

### USE flags


| [+emoji](https://packages.gentoo.org/useflags/+emoji) | Enable support for Emoji | 
| [+gtk3](https://packages.gentoo.org/useflags/+gtk3) | Enable the GTK-3 input method module | 
| [+gtk4](https://packages.gentoo.org/useflags/+gtk4) | Enable the GTK-4 input method module | 
| [+gui](https://packages.gentoo.org/useflags/+gui) | Enable support for a graphical user interface | 
| [+introspection](https://packages.gentoo.org/useflags/+introspection) | Add support for GObject based introspection | 
| [+python](https://packages.gentoo.org/useflags/+python) | Add optional support/bindings for the Python language | 
| [+unicode](https://packages.gentoo.org/useflags/+unicode) | Enable support for Unicode choice | 
| [X](https://packages.gentoo.org/useflags/X) | Add support for X11 | 
| [appindicator](https://packages.gentoo.org/useflags/appindicator) | Build in support for notifications using the libindicate or libappindicator plugin | 
| [gtk2](https://packages.gentoo.org/useflags/gtk2) | Enable the GTK-2 input method module | 
| [libnotify](https://packages.gentoo.org/useflags/libnotify) | Enable desktop notification support | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [vala](https://packages.gentoo.org/useflags/vala) | Enable bindings for dev-lang/vala | 
| [wayland](https://packages.gentoo.org/useflags/wayland) | Enable dev-libs/wayland backend | 

### Emerge

As well as the basic IBus framework, one or more engines should be installed. For example:

`root #``emerge --ask app-i18n/ibus ibus-libpinyin`
Have a look at the [eix](https://wiki.gentoo.org/wiki/Eix) output for more available engines:

`user $``eix -c -S engine app-i18n/ibus`
For graphical toolkit integration, [app-i18n/ibus](https://packages.gentoo.org/packages/app-i18n/ibus) offers `gtk2`, `gtk3` and `gtk4` flags.

### KDE

For IBus to work with [Qt](https://wiki.gentoo.org/wiki/Qt) 5 / [KDE](https://wiki.gentoo.org/wiki/KDE) 5 / Plasma, the [ibus](https://packages.gentoo.org/useflags/ibus) [USE flag](https://wiki.gentoo.org/wiki/USE_flag) needs to be set on at least [dev-qt/qtgui](https://packages.gentoo.org/packages/dev-qt/qtgui) and [kde-plasma/plasma-desktop](https://packages.gentoo.org/packages/kde-plasma/plasma-desktop):

**`/etc/portage/package.use/ibus`**

```
 ibus
kde-plasma/plasma-desktop ibus
```
And those packages rebuilt to reflect that change:

`root #``emerge --ask --oneshot --newuse dev-qt/qtgui kde-plasma/plasma-desktop`
## Configuration

The [app-i18n/im-chooser](https://packages.gentoo.org/packages/app-i18n/im-chooser) package can be helpful for selecting the preferred input method.

If nothing starts (no ibus daemon) when a user logs in, [add](https://wiki.gentoo.org/wiki/Knowledge_Base:Configuring_environment_variables) the following environment variables and log out:


```
export XMODIFIERS=@im=ibus
export GTK_IM_MODULE=ibus
export QT_IM_MODULE=ibus
# Use `xim` in case some Electron apps (like Chromium) refuse to work with IBus
# export GTK_IM_MODULE=xim
# export QT_IM_MODULE=xim
ibus-daemon -drx
```
The installation can be finely tuned by running:

`user $``ibus-setup`
ibus-setup can be used to set the preferred input methods, the system keyboard, or to add an icon in the taskbar.

The command-line ibus utility may also be used, in place of ibus-setup:

`user $``ibus list-engine``user $``ibus engine m17n:t:unicode      # For example`
To inform Qt ibus is now the input method, run:

`user $``qtconfig`
### Plasma

On Plasma, the daemon should be started with ibus-daemon -dr --panel=/usr/libexec/kimpanel-ibus-panel instead.

This line should be placed in a login script, **not** a pre-startup script. See [Knowledge Base:Configuring environment variables](https://wiki.gentoo.org/wiki/Knowledge_Base:Configuring_environment_variables) for more information.

## See also

- [How to read and write in Japanese](https://wiki.gentoo.org/wiki/How_to_read_and_write_in_Japanese) — how to read and write in Japanese on a non-Japanese system.
- [How\_to\_read\_and\_write\_in\_Chinese](https://wiki.gentoo.org/wiki/How_to_read_and_write_in_Chinese) — how to read and write in Chinese on a non-Chinese system.
- [Ibus-libpinyin](https://wiki.gentoo.org/wiki/Ibus-libpinyin) — a Chinese input engine
