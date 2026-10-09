<!-- source: https://wiki.gentoo.org/wiki/Wiremix | group: Gentoo Wiki (Main) | wiki-title: Wiremix -->
---
title: wiremix
url: https://wiki.gentoo.org/wiki/Wiremix
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-27"
fingerprint: b200dd300922d8ef
license: CC BY-SA 4.0
---

# wiremix

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**wiremix** is a simple TUI audio mixer for [PipeWire](https://wiki.gentoo.org/wiki/PipeWire) / [WirePlumber](https://wiki.gentoo.org/wiki/WirePlumber):

\[U\]se it to adjust volumes, route audio between devices and applications, and configure audio device settings like input/output ports and profiles.
wiremix's interface is more or less a clone of the wonderful ncpamixer which was itself inspired by [pavucontrol(1)](https://man.archlinux.org/man/pavucontrol.1.en)[, so users of either should find it familiar.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)


## Installation

### USE flags


### USE flags for
            [media-sound/wiremix](https://packages.gentoo.org/packages/media-sound/wiremix)
            
            A TUI mixer for PipeWire

| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 

### Emerge

Emerge wiremix:

`root #``emerge --ask media-sound/wiremix`
## Configuration

Refer to the project's [annotated wiremix.toml file](https://github.com/tsowell/wiremix/blob/main/wiremix.toml):

It is recommended to start with an empty configuration file and to use this file only as a reference. Anything specified in the configuration file will be merged with wiremix's defaults.


## Usage

To start wiremix from the command line:

`user $``wiremix`
Refer to the output of wiremix --help for command-line options, such as `-t` / `--theme` and `-v` / `--tab`.

Once wiremix has started, press `?` for help.
