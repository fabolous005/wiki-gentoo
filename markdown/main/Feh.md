<!-- source: https://wiki.gentoo.org/wiki/Feh | group: Gentoo Wiki (Main) | wiki-title: Feh -->
---
title: Feh
url: https://wiki.gentoo.org/wiki/Feh
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-24"
fingerprint: "7884185a708059cc"
license: CC BY-SA 4.0
---

# Feh

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**feh** is an open-source image viewer that is mainly aimed at command-line users. It can be used to view images on disk or, more often, for setting the desktop background for tiling [window managers](https://wiki.gentoo.org/wiki/Window_managers) like [i3](https://wiki.gentoo.org/wiki/I3). It has several command line options to change the behavior for either of these uses, mostly entered around how to resize the image for the view.

## Installation

### USE flags


| [curl](https://packages.gentoo.org/useflags/curl) | Add support for client-side URL transfer library | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [exif](https://packages.gentoo.org/useflags/exif) | Add support for reading EXIF headers from JPEG and TIFF images | 
| [inotify](https://packages.gentoo.org/useflags/inotify) | Enable inotify filesystem monitoring support | 
| [magic](https://packages.gentoo.org/useflags/magic) | Use libmagic from sys-apps/file to filter unsupported file formats | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [xinerama](https://packages.gentoo.org/useflags/xinerama) | Add support for querying multi-monitor screen geometry through the Xinerama API | 

### Emerge

Install [media-gfx/feh](https://packages.gentoo.org/packages/media-gfx/feh):

`root #``emerge --ask media-gfx/feh`
## Usage

### Setting backgrounds

To use it to set one's desktop background to a scaled version of a wallpaper:

`user $``feh --bg-scale /path/to/wallpaper`
where /path/to/wallpaper is the path to the wallpaper one wishes to use as desktop background. Likewise to set the desktop background to a tiled version of the wallpaper:

`user $``feh --bg-tile /path/to/wallpaper`
Other options for wallpapers include:

`--bg-center`

`--bg-fill`

`--bg-max`

Using `--no-xinerama` will set the wallpaper across all monitors, rather than individually for each monitor. `--bg-tile` does not support Xinerama at all.

Using `--no-fehbg` will prevent the creation of the \~/.fehbg file, although this file can be used to simplify wallpaper setting in \~/.xinitrc by adding `~/.fehbg &` instead of the full wallpaper setting command.

Adding these commands to \~/.xsession is an easy way to have the wallpaper set when X is started. Alternatively cron can be used to change the background in a time interval:

`user $``crontab -e`
This will run feh every 5 minutes, and every time randomly chooses a picture from the directory /path/to/wallpaper\_library. The environment variable `DISPLAY` must be set inside of cron because cron was started before X. For more help with automated tasks on intervals, see the page for [cron](https://wiki.gentoo.org/wiki/Cron).

### Viewing images

feh can also be used as a standalone image viewer. feh can accept a single image, a number of images, or a directory as input.

`user $``feh /path/to/image``user $``feh /path/to/directory`
| Control | Action | 
|---|---|
| `↑` | Zoom In | 
| `↓` | Zoom Out | 
| `←` | Previous Image | 
| `→` | Next Image | 
| Left Mouse Drag | Pans the Image | 
| Right Mouse Click | Secondary Menu | 
| `+` | Zoom In | 
| `-` | Zoom Out | 
| Scroll Up | Previous Picture | 
| Scroll Down | Next Picture | 

For further details on its usage see [The ArchWiki's article on feh](https://wiki.archlinux.org/index.php/feh#Usage).
