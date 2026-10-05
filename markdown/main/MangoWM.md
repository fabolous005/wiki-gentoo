<!-- source: https://wiki.gentoo.org/wiki/MangoWM | group: Gentoo Wiki (Main) | wiki-title: MangoWM -->
---
title: MangoWM
url: https://wiki.gentoo.org/wiki/MangoWM
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-07"
fingerprint: fe72577bce80f989
license: CC BY-SA 4.0
---

# MangoWM

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

MangoWM is a [Wayland](https://wiki.gentoo.org/wiki/Wayland) Compositor based on [wlroots](https://wiki.gentoo.org/wiki/Wlroots) and scenefx.

## Installation

First, unmask MangoWM and SceneFX:

**`/etc/portage/package.accept_keywords/mango`**

Emerge MangoWM:

`root #``emerge --ask gui-wm/mangowm`
## Usage

### Invocation

To start Mango on systemd:

`user $``mango`
To start Mango under [OpenRC](https://wiki.gentoo.org/wiki/OpenRC):

`user $``dbus-run-session mango`
### Configuration

Copy the default configuration file to the user directory:

`user $````
mkdir -p ~/.config/mango/
```
`user $````
cp /etc/mango/config.conf ~/.config/mango/config.conf
```
### Terminal emulator

By default, the MangoWM configuration file uses [gui-apps/foot](https://packages.gentoo.org/packages/gui-apps/foot) as the [terminal emulator](https://wiki.gentoo.org/wiki/Terminal_emulator). It is recommended to install this terminal emulator to ensure a terminal will be available once MangoWM is running:

`root #``emerge --ask gui-apps/foot`
### Launcher

By default, the MangoWM configuration file uses [x11-misc/rofi](https://packages.gentoo.org/packages/x11-misc/rofi) as the application launcher. It is recommended to install this launcher to ensure that programs can be opened when MangoWM is running.

`root #``emerge --ask x11-misc/rofi`
### Essential Keybindings

- `Alt`+`Return` = Open Terminal (defaults to [gui-apps/foot](https://packages.gentoo.org/packages/gui-apps/foot)).
- `Alt`+`Space` = Open Launcher (defaults to [x11-misc/rofi](https://packages.gentoo.org/packages/x11-misc/rofi)).
- `Alt`+`Q` = Close (Kill) the active window.
- `Super`+`M` = Quit MangoWC.
- `Super`+`F` = Toggle Fullscreen.
- `Ctrl`+`1-9` = Switch to Tag 1-9.
- `Alt`+`1-9` = Move window to Tag 1-9.

## See also

- [Wayland](https://wiki.gentoo.org/wiki/Wayland) — a [communication protocol](https://en.wikipedia.org/wiki/communication_protocol) between a [display server](https://en.wikipedia.org/wiki/display_server) and its clients
- [List of software for Wayland](https://wiki.gentoo.org/wiki/List_of_software_for_Wayland) — various desktop related packages for Wayland
