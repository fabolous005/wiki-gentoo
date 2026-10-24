<!-- source: https://wiki.gentoo.org/wiki/Where_to_go_next/Desktop | group: Gentoo Wiki (Main) | wiki-title: Where to go next/Desktop -->
---
title: Where to go next/Desktop
url: https://wiki.gentoo.org/wiki/Where_to_go_next/Desktop
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-06"
fingerprint: "9eea095a4cb32375"
license: CC BY-SA 4.0
---

# Where to go next/Desktop

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Desktop system should already be have installed from desktop stage file already, if this was not the case then make sure to follow this quick conversion guide otherwise this process could take anywhere from 6 hours to 24 hours depending on the machine being used. (TODO - write this article)

## Graphical environment

Linux systems have many different graphical environment / GUI options, for different needs. Options generally fall into two categories:

- [Desktop environments](https://wiki.gentoo.org/wiki/Desktop_environment) (DEs)

- A desktop environment is a complete ecosystem of software and resources providing a homogeneous graphical user experience. Generally based on specific graphical widgets, configuration system, root window with desktop background, taskbar with window list and menu, icons, window manager etc.

- [Window managers](https://wiki.gentoo.org/wiki/Window_manager) (WMs)

- A window manager (WM) manages the creation, manipulation, and destruction of on-screen windows and window decorations in a GUI environment.

If wondering whether to use [Wayland](https://wiki.gentoo.org/wiki/Wayland) or [Xorg](https://wiki.gentoo.org/wiki/Xorg), it's wise to first determine the specific applications wanted/needed, and then pick a GUI on that basis.

### Desktop environments

A list of desktop environments can be found in the ["Desktop environment"](https://wiki.gentoo.org/wiki/Desktop_environment) article.

If struggling to decide, two of the most popular choices on Linux are [GNOME](https://wiki.gentoo.org/wiki/GNOME) and [KDE](https://wiki.gentoo.org/wiki/KDE).

### Window manager

A list of window managers which support [Xorg](https://wiki.gentoo.org/wiki/Xorg) can be found on the ["Window managers"](https://wiki.gentoo.org/wiki/Window_manager) page.

A list of window managers which support [Wayland](https://wiki.gentoo.org/wiki/Wayland) can be found on the ["List of software for Wayland"](https://wiki.gentoo.org/wiki/List_of_software_for_Wayland#Compositors) page.

## Audio / sound

By default, desktop [profiles](https://wiki.gentoo.org/wiki/Profile) are configured for the PipeWire sound server; instructions for configuring and using both PipeWire and its associated session manager, WirePlumber, can be found on the ["PipeWire"](https://wiki.gentoo.org/wiki/PipeWire) and ["WirePlumber"](https://wiki.gentoo.org/wiki/WirePlumber) pages, respectively. OpenRC users will need to first set up [user services](https://wiki.gentoo.org/wiki/OpenRC#User_services) and enable the [`dbus` user service](https://wiki.gentoo.org/wiki/D-Bus#OpenRC_2).

Gentoo's PipeWire package provides the `pipewire-pulse` user service to emulate the [PulseAudio](https://wiki.gentoo.org/wiki/PulseAudio) sound system, so installing PulseAudio is not required. It also provides emulation for the [JACK](https://wiki.gentoo.org/wiki/JACK) sound system; refer to [this section of the "PipeWire" page](https://wiki.gentoo.org/wiki/PipeWire#Replacing_JACK) for further information.

PulseAudio can be used to provide a sound server instead of PipeWire; refer to the ["PulseAudio"](https://wiki.gentoo.org/wiki/PulseAudio) page for configuration information.

Finally, PipeWire, PulseAudio and JACK are all built on top of ALSA, the Advanced Linux Sound Architecture, [the Linux kernel's audio API](https://docs.kernel.org/sound/kernel-api/index.html). It's possible to use ALSA directly, rather than using sound servers on top of it; refer to the ["ALSA"](https://wiki.gentoo.org/wiki/ALSA) page for details.

If unsure which setup to use, start with PipeWire, as it's most likely to provide a "just works out of the box" experience. The system audio configuration can be changed to directly use PulseAudio, or JACK, or pure ALSA, at a later time if desired.

## Web browser

### Firefox and Gecko-based browsers

Firefox is based on the [Gecko](<https://en.wikipedia.org/wiki/Gecko_(software)>) browser engine.

For information about installing and configuring Firefox, refer to the ["Firefox"](https://wiki.gentoo.org/wiki/Firefox)
page, which includes [a list of Firefox forks](https://wiki.gentoo.org/wiki/Firefox#Firefox_forked_projects).

### Chrome, and Blink-based browsers

Chrome is based on the [Blink](<https://en.wikipedia.org/wiki/Blink_(browser_engine)>) browser engine.

For information about installing the closed-source Chrome browser, refer to the ["Google Chrome"](https://wiki.gentoo.org/wiki/Google_Chrome) page.

## Gaming

### Linux games

The ["Games"](https://wiki.gentoo.org/wiki/Games) page lists various free and paid Linux-native games, e.g. [OpenRCT2](https://wiki.gentoo.org/wiki/Games/simulation#OpenRCT2)

### Steam

For information about installing and configuring Steam on Gentoo, refer to the ["Steam"](https://wiki.gentoo.org/wiki/Steam) page.

### Non-Steam games

Lutris is a popular [WINE](https://en.wikipedia.org/wiki/WINE) frontend to allow installing games from places like GOG. Refer to the ["Lutris"](https://wiki.gentoo.org/wiki/Lutris) page for further information.

Users can also directly use WINE, as described on the ["Wine"](https://wiki.gentoo.org/wiki/Wine) page.

### Emulation

A selection of emulators to play older games can be found on the ["Games/emulation"](https://wiki.gentoo.org/wiki/Games/emulation) page.

[RetroArch](https://wiki.gentoo.org/wiki/RetroArch) is a popular multi-system emulator.

## Office Suite

The [Recommended applications](https://wiki.gentoo.org/wiki/Recommended_applications) page has  [a list of productivity suites](https://wiki.gentoo.org/wiki/Recommended_applications#Productivity_software).

If stuck making a choice, [LibreOffice](https://wiki.gentoo.org/wiki/LibreOffice) is popular suite in the style of Microsoft Office.
