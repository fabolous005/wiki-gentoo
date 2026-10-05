<!-- source: https://wiki.gentoo.org/wiki/X_resources | group: Gentoo Wiki (Main) | wiki-title: X resources -->
---
title: X resources
url: https://wiki.gentoo.org/wiki/X_resources
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-07-04"
fingerprint: "74c1c2596697fb9c"
license: CC BY-SA 4.0
---

# X resources

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Also known as the \~/.Xresources file.

## Introduction

X resources are configuration options for X applications such as the [rxvt-unicode](https://wiki.gentoo.org/wiki/Rxvt-unicode) terminal emulator. They can also be used for setting the [cursor theme](https://wiki.gentoo.org/wiki/Cursor_themes). X resources can be set in \~/.Xresources.

While most display managers will automatically load this configuration file on startup, it is possible to load the configuration manually by running:

`user $``xrdb ~/.Xresources`
The xrdb command is provided by the [x11-apps/xrdb](https://packages.gentoo.org/packages/x11-apps/xrdb) package.

## Syntax

Configuration options in the \~/.Xresources file should respect the following component syntax:

*name*.*Class*.*resource*: *value*

Examples of existing configuration options:

- `Xcursor.theme: redglass`
- `xscreensaver.Dialog.background: #111111`

### Comments

Comments start with an exclamation mark or with double slashes. For example:

### Wildcards

It is possible to use `?` and `*` wildcards to apply a single rule to multiple configuration options. The `?` matches any single component. The `*` matches zero or more components. For example:

`*background: #000000` - Set the given value to all programs/classes which contain a component named *background*.

### Constants

Constants can be defined in the following way:

`#define`  (for example *name value*`#define black #000000`).

### Includes

The main \~/.Xresources file can be composed of multiple sub-files (e.g. on a per-application basis). The includes can be defined as:

`#include "`
*file\_name*"

Example of including application sub-files:

**`~/.Xresources`**

## See also

- [Cursor themes](https://wiki.gentoo.org/wiki/Cursor_themes) — provides instructions for cursor theme management on an X11-based system.
- [Rxvt-unicode](https://wiki.gentoo.org/wiki/Rxvt-unicode) — a fast and lightweight [terminal emulator](https://wiki.gentoo.org/wiki/Terminal_emulator) with [Xft](https://en.wikipedia.org/wiki/Xft) and [Unicode](https://en.wikipedia.org/wiki/Unicode) support.
