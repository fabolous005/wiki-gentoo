<!-- source: https://wiki.gentoo.org/wiki/Cursor_themes | group: Gentoo Wiki (Main) | wiki-title: Cursor themes -->
---
title: Cursor themes
url: https://wiki.gentoo.org/wiki/Cursor_themes
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-02"
fingerprint: "6f4fe75b07bf13de"
license: CC BY-SA 4.0
---

# Cursor themes

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article provides instructions for cursor theme management on an X11-based system.

## Installation

### Emerge

Before installing any new packages, check to see what cursors are available on the system. Cursors can be found beneath the /usr/share/cursors/ directory.

[x11-themes/xcursor-themes](https://packages.gentoo.org/packages/x11-themes/xcursor-themes) comes with the whiteglass and the redglass themes:

`root #``emerge --ask x11-themes/xcursor-themes`
### Additional software

Other packages with some Gentoo themed cursors is available as well.

[x11-themes/gentoo-xcursors](https://packages.gentoo.org/packages/x11-themes/gentoo-xcursors) comes with gentoo, gentoo-blue, and gentoo-silver cursor themes:

`root #``emerge --ask x11-themes/gentoo-xcursors`
To install all of the cursor themes available from Portage:

`root #``emerge --ask x11-themes/blueglass-xcursors x11-themes/chameleon-xcursors x11-themes/comix-xcursors x11-themes/gentoo-xcursors x11-themes/golden-xcursors x11-themes/haematite-xcursors x11-themes/neutral-xcursors x11-themes/obsidian-xcursors x11-themes/pearlgrey-xcursors x11-themes/silver-xcursors x11-themes/vanilla-dmz-aa-xcursors x11-themes/vanilla-dmz-xcursors x11-themes/xcursor-themes`
## Configuration

Edit the user's \~/.Xresources file:

**`~/.Xresources`**

**Choose a cursor theme**

```
Xcursor.theme: redglass
```
Cursor size can be optionally chosen as well:

**`~/.Xresources`**

**Choose the cursor size**

```
Xcursor.size: 16
```
### Testing themes

To test some themes on the fly, run a command like:

`user $````
for i in /usr/share/icons/gentoo*; do th=$(basename "${i}"); echo theme = $th; XCURSOR_THEME=${th} XCURSOR_SIZE=64 urxvt; done
```
`user $````
for i in /usr/share/icons/*; do th=$(basename "${i}"); echo theme = $th; XCURSOR_THEME=${th} XCURSOR_SIZE=32 urxvt; done
```
The former will test the gentoo cursor themes with a size of 64, the later will test all the installed themes with a size of 32. They run a console because it's a commonly used software on gentoo.

It work for scalable themes and may work with or without adjustments for non scalable themes, theirs directory structure is different.

The available sizes depend on the theme. When a theme cannot do the specified size, it will go down to the highest related size it can do.

## Troubleshooting

### Cursor theme is not loaded at all

For [window managers](https://wiki.gentoo.org/wiki/Window_managers) it might be necessary to load the cursor theme before starting the window manager using xrdb via \~/.xinitrc:

**`~/.xinitrc`**

```
xrdb -merge ~/.Xresources
eval $(dbus-launch --sh-syntax --exit-with-session <window_manager>)
```
Restart *X* to apply the changes:

`user $````
pkill X
```
`user $````
startx
```
### Cursor keeps going back to default X cursors

In some desktop environments the mouse theme can change when hovering outside of a window. Inside the window the mouse cursor respects the user's choice. But outside the window the mouse cursor changes into the default X cursor.

Create the symlink to the user's home path:

`user $``ln -s /usr/share/cursors/xorg-x11 ~/.icons`
An alternative is to create an icon theme file:

**`~/.icons/default/index.theme`**

```
Name=Default
Comment=Default Cursor Theme
Inherits=gentoo-silver
Size=64
```
This file must follow the [freedesktop Icon Theme Specification](https://specifications.freedesktop.org/icon-theme/latest/).

### GTK 2 and GTK 3

With some GTK applications the mouse cursor changes to default settings when it is inside the GTK application window. Users can fix this problem by setting the `gtk-theme-name` and `gtk-cursor-theme-name` variables in the \~/.config/gtk-3.0/settings.ini and \~/.gtkrc-2.0 configuration files. In the examples below the `gentoo` cursor theme has been chosen.

For GTK 3 applications:

**`~/.config/gtk-3.0/settings.ini`**

```
[Settings]
gtk-theme-name = gentoo
gtk-icon-theme-name = gnome
gtk-cursor-theme-name = gentoo
gtk-cursor-theme-size = 16
```
For GTK 2 applications:

**`~/.gtkrc-2.0`**

```
gtk-theme-name = gentoo
gtk-icon-theme-name = gnome
gtk-cursor-theme-name = gentoo
```
## See also

- [X resources](https://wiki.gentoo.org/wiki/X_resources) — configuration options for X applications
- [Xorg](https://wiki.gentoo.org/wiki/Xorg) — an open source implementation of the [X server](https://wiki.gentoo.org/wiki/X_server).
