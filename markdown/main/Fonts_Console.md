<!-- source: https://wiki.gentoo.org/wiki/Fonts/Console | group: Gentoo Wiki (Main) | wiki-title: Fonts/Console -->
---
title: Fonts/Console
url: https://wiki.gentoo.org/wiki/Fonts/Console
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-01"
fingerprint: "8652135d18871bd1"
license: CC BY-SA 4.0
---

# Fonts/Console

[Fonts](https://wiki.gentoo.org/wiki/Fonts)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page describes configuration of fonts on the Linux console. For information about configuring fonts in a GUI context, such as when using a desktop environment or window manager - and including running terminal emulators in that context - please refer to the [Fontconfig](https://wiki.gentoo.org/wiki/Fontconfig) article.

## Kernel

If a manual kernel configuration is created, a font can also be selected in the kernel itself. This is particularly useful when a 4K monitor is in use. Currently the largest font available is a Terminus 16x32, but larger fonts will be available in kernel version 6.9.

**Enable Terminus 16x32 font**

```
Device Drivers  --->
    Graphics support  --->
        Console display driver support  --->
            [*] Framebuffer Console support
Library routines  --->
    [*] Select compiled-in fonts
        [*] Terminus 16x32 font (not supported by all drivers)
```
## OpenRC

In order to use a specific font in the console, set the `consolefont` variable in /etc/conf.d/consolefont to the name of a file found in /usr/share/consolefonts/ (without the .psfu.gz suffix).

**`/etc/conf.d/consolefont`**

**Setting the console font**

```
consolefont="lat9w-16"
```
Next add consolefont init script to boot runlevel:

`root #``rc-update add consolefont boot`
[media-fonts/terminus-font](https://packages.gentoo.org/packages/media-fonts/terminus-font) is a good option; It has fonts with sizes from 12 to 32 pixels making it suitable for HD screens or providing an option for larger console fonts.

## systemd

Configuration of the systemd virtual console is done using the /etc/vconsole.conf configuration file.

To set a specific font set the `FONT` to the name of a file found in /usr/share/consolefonts/ (without the .psfu.gz suffix). The `FONT_MAP` is set to the name of a file found in /usr/share/unimaps/ (also without any suffix).

**`/etc/vconsole.conf`**

**Setting the console font**

```
FONT=lat9w-16
FONT_MAP=8859-15
```
New configuration can be reloaded by:

`root #``systemctl restart systemd-vconsole-setup.service`
## Specifying a font manually

A font can be set directly on the command line via [setfont(8)](https://man.archlinux.org/man/setfont.8.en)[:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`user $``setfont cyr-sun16`
