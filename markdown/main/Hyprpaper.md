<!-- source: https://wiki.gentoo.org/wiki/Hyprpaper | group: Gentoo Wiki (Main) | wiki-title: Hyprpaper -->
---
title: Hyprpaper
url: https://wiki.gentoo.org/wiki/Hyprpaper
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-29"
fingerprint: ac763442f8b57aa1
license: CC BY-SA 4.0
---

# Hyprpaper

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**hyprpaper** is a blazing fast [Wayland](https://wiki.gentoo.org/wiki/Wayland) wallpaper utility with IPC controls.

## Installation

### USE flags

Currently this package has no use flags.

### Emerge

It is available in the [GURU](https://wiki.gentoo.org/wiki/Project:GURU/Information_for_End_Users) repository<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>. After enabling that repo, run

`root #``emerge --ask gui-apps/hyprpaper`
## Configuration

For a simple wallpaper that loads when Hyprland starts, place `hl.on("hyprland.start", function() hl.exec_cmd("hyprpaper") end)` into the Hyprland config file. Then create `~/.config/hypr/hyprpaper.conf` with the following lines:

**`~/.config/hypr/hyprpaper.conf`**

```
wallpaper {
    monitor = eDP-1
    path = /home/username/Pictures/wallpaper.jpg
    fit_mode = cover
}
```
Replace eDP-1 to denote the monitor being used. This preloads wallpaper.jpg and sets it as the default wallpaper. Refer to the official documentation link above for more advanced configuration options. Use of \~ in the wallpaper file paths may cause issues, so it is safer to default to spelling out the full path.

### Alternatives

See [https://wiki.hypr.land/Useful-Utilities/Wallpapers/](https://wiki.hypr.land/Useful-Utilities/Wallpapers/) for other Hyprland wallpaper utility options.

## See also

- [Hyprland](https://wiki.gentoo.org/wiki/Hyprland) — an open-source [Wayland compositor](https://wiki.gentoo.org/wiki/Wayland_compositor) written in C++.
- [List of software for Wayland](https://wiki.gentoo.org/wiki/List_of_software_for_Wayland) — various desktop related packages for Wayland
