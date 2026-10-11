<!-- source: https://wiki.gentoo.org/wiki/Swaybg | group: Gentoo Wiki (Main) | wiki-title: Swaybg -->
---
title: swaybg
url: https://wiki.gentoo.org/wiki/Swaybg
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-10"
fingerprint: "2a169f135481db84"
license: CC BY-SA 4.0
---

# swaybg

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Resources**

**swaybg** is a wallpaper utility for [Wayland](https://wiki.gentoo.org/wiki/Wayland) written in [C](https://wiki.gentoo.org/wiki/C).

## Installation

### USE Flags


### USE flags for
            [gui-apps/swaybg](https://packages.gentoo.org/packages/gui-apps/swaybg)
            
            A wallpaper utility for Wayland

| [gdk-pixbuf](https://packages.gentoo.org/useflags/gdk-pixbuf) | Support image types other than PNG | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

### Emerge

Emerge the [gui-apps/swaybg](https://packages.gentoo.org/packages/gui-apps/swaybg)

`root #``emerge --ask gui-apps/swaybg`
## Usage

### Options

- `-c --color RRGGBB`
- Set the background color.

- `-i --image <path>`
- Set the image to display.

- `-m --mode <mode>`
- Set the mode to use for the image.
- Modes : **stretch**, **fit**, **fill**, **center**, **tile**, or **solid\_color**

- `-o --output <name>`
- Set the output to operate on or \* for all.

### Command Usage

The usual usage for **swaybg** is shown below.

`user $``swaybg -o * -i <path> -m fill`
`-o *` - Puts the wallpaper on every screen.

`-i <path>` - Tells swaybg what image to use.

`-m fill` - Sets the mode for the wallpaper as **fill**.
