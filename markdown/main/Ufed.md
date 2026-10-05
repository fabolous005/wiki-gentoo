<!-- source: https://wiki.gentoo.org/wiki/Ufed | group: Gentoo Wiki (Main) | wiki-title: Ufed -->
---
title: Ufed
url: https://wiki.gentoo.org/wiki/Ufed
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-11-13"
fingerprint: e3e7317ca6fa09b9
license: CC BY-SA 4.0
---

# Ufed

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**ufed** is an ncurses-based USE flag editor designed to simplify configuration of the USE flags set in [make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf).

## Installation

### USE flags

There are currently no available USE flags for [app-portage/ufed](https://packages.gentoo.org/packages/app-portage/ufed) itself. The irony!

### Emerge

## Usage

![](https://wiki.gentoo.org/images/thumb/8/8e/Ufed-0.92_screenshot.png/200px-Ufed-0.92_screenshot.png)

Start the program from a command-line with the ufed command. It may take a few seconds for the program to open; do not be alarmed if it seems like nothing is happening, just wait a little longer.

`root #``ufed`
Hot keys include:

| Key | Description | 
|---|---|
| `↑ ↓`, or `Page Up Page Down`, or `Home End` | Move between flags. | 
| `← →` | Scroll though the USE flag descriptions. This is helpful if the description text is concatenated by the screen or terminal resolution. | 
| `Space` | Toggles the flag on or off. | 
| `/`, or start typing the flag name | Searches the listed flags. | 
| `?` | Displays the in-program help screen. | 
| `F5` | Toggles the display of local, global, or all flag descriptions. | 
| `F6` | Toggles display of flags supported by at least one installed package, supported by no installed package, or all flags. | 
| `F7` | Toggles display of masked and forced flags, flags that are neither masked nor forced, or all flags. | 
| `F10` | Changes the description to show full or reduced descriptions. | 
| `F11` | Changes the display to wrap long lines into multiple lines. | 
| `Esc` | Cancels changes. | 
| `Ctrl`+`C`, or `Q`+`Y` | Closes (exits) the program. | 

## See also

- [/etc/portage/make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf) — the main configuration file used to customize the [Portage](https://wiki.gentoo.org/wiki/Portage) environment on a global level., the location [Portage](https://wiki.gentoo.org/wiki/Portage) keeps binary packages.
- [Portage](https://wiki.gentoo.org/wiki/Portage) — the official [package manager](https://en.wikipedia.org/wiki/Package_manager) and [distribution system](https://www.gentoo.org/get-started/about/) for Gentoo.

## External resources

- ufed's man page locally (man ufed).
