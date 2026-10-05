<!-- source: https://wiki.gentoo.org/wiki/Terminal_emulator/Colors | group: Gentoo Wiki (Main) | wiki-title: Terminal emulator/Colors -->
---
title: Terminal emulator/Colors
url: https://wiki.gentoo.org/wiki/Terminal_emulator/Colors
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-21"
fingerprint: "66a29b3c5f158b2f"
license: CC BY-SA 4.0
---

# Terminal emulator/Colors

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Introduction

The [ECMA-48 (ISO/IEC 6429, ANSI X3.64) standard](https://ecma-international.org/publications-and-standards/standards/ecma-48), "Control functions for coded character sets", in part defines methods of using [Control Sequence Introducer (CSI)](<https://en.wikipedia.org/wiki/ANSI_escape_code#CSI_(Control_Sequence_Introducer)_sequences>) commands to set colors in terminals. The CSI sequence is `ESC [`, typically written in strings as `\e[` or `\033[`.

The CSI command for specifiying colors is known as [Select Graphic Rendition (SGR)](<https://en.wikipedia.org/wiki/ANSI_escape_code#SGR_(Select_Graphic_Rendition)_parameters>), the sequence `CSI` . In this sequence, `n` m`n` is a single number or a sequence of numbers separated by semicolons (';'), e.g. `1;2;3`. Setting `n` to `0`, or omitting a value for `n` entirely, instructs the terminal to reset all colors / set them to 'normal'. Thus, to reset all colors, use printf to send the appropriate CSI sequence to the terminal:

`user $``printf '\e[0m'`
or

`user $``printf '\e[m'`
More generally, a missing number is treated as `0`. For example, the sequence `;2;3` in `\e[;2;3` is treated as `0;2;3`.

## Specifying colors

The simplest method of specifying foreground and background colors in a terminal is via SGR parameters 30–37 (foreground) and 40–47 (background). For example, to specify a white foreground (37) on a black background (40):

`user $``printf '\e[37;40m'`
The following table lists the available values for foreground and background, together with their VGA and xterm (decimal) RGB mappings:

| FG | BG | Color name | VGA RGB | xterm RGB | 
|---|---|---|---|---|
| 30 | 40 | Black | 0, 0, 0 | 0, 0, 0 | 
| 31 | 41 | Red | 170, 0, 0 | 205, 0, 0 | 
| 32 | 42 | Green | 0, 170, 0 | 0, 205, 0 | 
| 33 | 43 | Yellow | 170, 85, 0 | 205, 205, 0 | 
| 34 | 44 | Blue | 0, 0, 170 | 0, 0, 238 | 
| 35 | 45 | Magenta | 170, 0, 170 | 205, 0, 205 | 
| 36 | 46 | Cyan | 0, 170, 170 | 0, 205, 205 | 
| 37 | 47 | White | 170, 170, 170 | 229, 229, 229 | 
| 90 | 100 | Bright Black (Gray) | 85, 85, 85 | 127, 127, 127 | 
| 91 | 101 | Bright Red | 255, 85, 85 | 255, 0, 0 | 
| 92 | 102 | Bright Green | 85, 255, 85 | 0, 255, 0 | 
| 93 | 103 | Bright Yellow | 255, 255, 85 | 255, 255, 0 | 
| 94 | 104 | Bright Blue | 85, 85, 255 | 92, 92, 255 | 
| 95 | 105 | Bright Magenta | 255, 85, 255 | 255, 0, 255 | 
| 96 | 106 | Bright Cyan | 85, 255, 255 | 0, 255, 255 | 
| 97 | 107 | Bright White | 255, 255, 255 | 255, 255, 255 | 

### 8-bit color

Terminals that support 8-bit color have two additional SGR sequences to set a color: `\e[38;5;` to select a foreground colour, and *n*m`\e[48;5;` to select a background color, where `n`m*n* can be:

- 0 - 7, for the 'standard' colors specified by the SGR sequences 30 to 37;
- 8 - 15, for the 'high intensity' colors specified by the SGR sequences 90 to 97;
- 16 - 231, for the colors in the 6 × 6 × 6 cube defined by 16 + 36 × r + 6 × g + b (0 ≤ r, g, b ≤ 5);

232-255: grayscale from dark to light in 24 steps

Wikipedia provides [a chart displaying the colors specified by the latter two sets of values](https://en.wikipedia.org/wiki/ANSI_escape_code#8-bit).

### 24-bit color / 'truecolor'

Terminals that support 24-bit color / 'truecolor' have two additional SGR sequences to set a color: `\e[38;2;` to select an RGB foreground color, and *r*;*g*;*b*m`\e[48;2;` to select an RGB background color.
*r*;*g*;*b*;m

Terminal emulators fully supporting 24-bit color include<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>:

| Emulator | Package | 
|---|---|
| [Alacritty](https://wiki.gentoo.org/wiki/Alacritty) | [x11-terms/alacritty](https://packages.gentoo.org/packages/x11-terms/alacritty) | 
| [Ghostty](https://wiki.gentoo.org/wiki/Ghostty) | [x11-terms/ghostty](https://packages.gentoo.org/packages/x11-terms/ghostty) | 
| GNOME Terminal | [x11-terms/gnome-terminal](https://packages.gentoo.org/packages/x11-terms/gnome-terminal) | 
| [Kitty](https://wiki.gentoo.org/wiki/Kitty) | [x11-terms/kitty](https://packages.gentoo.org/packages/x11-terms/kitty) | 
| [Konsole](https://wiki.gentoo.org/wiki/Konsole) | [kde-apps/konsole](https://packages.gentoo.org/packages/kde-apps/konsole) | 
| [rxvt-unicode](https://wiki.gentoo.org/wiki/Rxvt-unicode) ('urxvt') | [x11-terms/rxvt-unicode](https://packages.gentoo.org/packages/x11-terms/rxvt-unicode) | 
| [st](https://wiki.gentoo.org/wiki/St) | [x11-terms/st](https://packages.gentoo.org/packages/x11-terms/st) | 
| [XTerm](https://wiki.gentoo.org/wiki/XTerm) | [x11-terms/xterm](https://packages.gentoo.org/packages/x11-terms/xterm) | 

All emulators based on libvte ([dev-libs/libvterm](https://packages.gentoo.org/packages/dev-libs/libvterm)), such as GNOME Terminal, fully support 24-bit color.

### Linux console

The Linux console / Virtual Terminal ('VT') provides an OSC escape sequence (i.e. a sequence with the prefix `\e]`) to change the palette: `\e]P`, where:
*nrrggbb*

- *n* is a one-digit hexadecimal value specifying the color number to set, as per the table below; and
- *rrggbb* specifies the RGB value of the color to be associated with that color number, with each component - *rr*, *gg*, or *bb* - a two-digit hexadecimal value.

| Value | Color name | 
|---|---|
| 0 | Black | 
| 1 | Red | 
| 2 | Green | 
| 3 | Yellow | 
| 4 | Blue | 
| 5 | Magenta | 
| 6 | Cyan | 
| 7 | White | 
| 8 | Bright Black (Gray) | 
| 9 | Bright Red | 
| A | Bright Green | 
| B | Bright Yellow | 
| C | Bright Blue | 
| D | Bright Magenta | 
| E | Bright Cyan | 
| F | Bright White | 

For example, to set color "1" in the palette ("Red") to be the hex RGB tuple "FF,20,20",

`user $``printf '\e]P1ff2020'`
Or to set color "A" in the palette ("Bright Green") to be the hex RGB tuple "20,FF,20":

`user $``printf '\e]Pa20ff20'`
The palette can be reset with the OSC sequence `\e]R`.

For further information, refer to the [console\_codes(4)](https://man.archlinux.org/man/console_codes.4.en) [man page.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

## Configuring color

Some emulators, such as Kitty, Konsole, and libvte-based emulators, advertise truecolor support by setting the `COLORTERM` variable to the value `truecolor`.

Additionally, some emulators, such as Konsole and rxvt-unicode ('urxvt'), report the color scheme of the terminal via the `COLORFGBG` variable.

### Gentoo-specific configuration

By default, if the `NO_COLOR` variable is set, colorization will be disabled (e.g. via the /etc/bash/bashrc.d/10-gentoo-color.bash Bash configuration file).

The /etc/portage/color.map file, described in [color.map(5)](https://man.archlinux.org/man/color.map.5.en)[, contains variables that define color classes used by Portage.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

The emerge option `--color < y | n >` can be used to enable or disable color output:

This option will override NO\_COLOR and NOCOLOR (see make.conf(5)) and may also be used to force color output when stdout is not a tty (by default, color is disabled unless stdout is a tty).


[eix](https://wiki.gentoo.org/wiki/Eix) provides two options for controlling the use of ANSI color codes: `-n` / `--nocolor` to disable them, and `-F` / `--force-color` to force their use. This can also be specified in \~/.eixrc, via the `FORCE_COLORS` variable. Colors and color schemes can be specified in \~/.eixrc via the `BG0`, `BG1`, `BG2`, `BG3`, `COLORFGBG_DARK`, `COLORSCHEME?`, `COLORSCHEME0`, `COLORSCHEME1`, `COLORSCHEME2`, `COLORSCHEME3`, `SOLARIZED`, `TERM_ALT?`, `TERM_ALT1`, `TERM_ALT2`, `TERM_ALT3`, and `TERM_DARK` variables; refer to the eix(1) man page for details.

### Miscellaneous

OpenRC maintains an internal list of terminals that support colors, in libeinfo.c<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>; neither [foot](https://wiki.gentoo.org/wiki/Foot) nor [kitty](https://wiki.gentoo.org/wiki/Kitty) are included in OpenRC 0.54.2, with the result that output of commands like rc-status will not be colorized. However, both have been added to the list in OpenRC 0.55.1.

GNU [ls(1)](https://man.archlinux.org/man/ls.1.en) [uses the](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) `LS_COLORS` variable; the [dircolors(1)](https://man.archlinux.org/man/dircolors.1.en) [program can be used to generate a line setting that variable. Alternatively, the /etc/DIR\_COLORS file can be copied to \~/.dir\_colors and modified.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

## Software

- [ansifilter](https://manpages.debian.org/trixie/ansifilter/ansifilter.1.en.html) - ANSI escape code stripper and converter, provided by [app-text/ansifilter](https://packages.gentoo.org/packages/app-text/ansifilter).
- [pygmentize](https://pygments.org/docs/cmdline/) - Python script for syntax highlighting, provided by [dev-python/pygments](https://packages.gentoo.org/packages/dev-python/pygments).

## See also

- [Shell](https://wiki.gentoo.org/wiki/Shell) — command-line interpreter that provides a text-based interface to users

## External resources

### Man pages

- [dircolors(1)](https://man.archlinux.org/man/dircolors.1.en)- [showrgb(1)](https://man.archlinux.org/man/showrgb.1.en)- [color.map(5)](https://man.archlinux.org/man/color.map.5.en)- [color(7)](https://man.archlinux.org/man/color.7.en)- [console\_codes(7)](https://man.archlinux.org/man/console_codes.7.en)

### General

- [Wikipedia: "ANSI escape code"](https://en.wikipedia.org/wiki/ANSI_escape_code)
- [Terminal Colors](https://github.com/termstandard/colors) - Information about the level of color support provided by various emulators
- [256 colors cheat sheet](https://www.ditig.com/publications/256-colors-cheat-sheet)
- [Xterm Control Sequences](https://www.xfree86.org/current/ctlseqs.html)
