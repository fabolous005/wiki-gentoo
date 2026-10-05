<!-- source: https://wiki.gentoo.org/wiki/Easy_Effects | group: Gentoo Wiki (Main) | wiki-title: Easy Effects -->
---
title: Easy Effects
url: https://wiki.gentoo.org/wiki/Easy_Effects
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-03"
fingerprint: "2e04924e4907f062"
license: CC BY-SA 4.0
---

# Easy Effects

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Easy Effects** is a limiter, compressor, convolver, equalizer, auto volume, and many other plugins, for [PipeWire](https://wiki.gentoo.org/wiki/PipeWire) applications.

Easy Effects was:

formerly known as PulseEffects, but ... was renamed to Easy Effects after it started to use GTK4 and GStreamer usage was replaced by native PipeWire filters. And eventually the whole application was ported from GTK4 to a combination of Qt, QML and KDE/Kirigami frameworks.


## Installation

### USE flags


| [+doc](https://packages.gentoo.org/useflags/+doc) | Install packages needed to display built-in user documentation | 
| [calf](https://packages.gentoo.org/useflags/calf) | Enable use of media-plugins/calf for adding various FX | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [mda-lv2](https://packages.gentoo.org/useflags/mda-lv2) | Enable use of media-plugins/mda-lv2 for the loudness FX | 
| [webengine](https://packages.gentoo.org/useflags/webengine) | Read documentation inside the application with dev-qt/qtwebengine | 
| [zamaudio](https://packages.gentoo.org/useflags/zamaudio) | Enable use of media-plugins/zam-plugins for the maximizer FX | 

### Emerge

`root #``emerge --ask media-sound/easyeffects`
EasyEffects is available as a [Flatpak](https://wiki.gentoo.org/wiki/Flatpak) application that can be downloaded and installed automatically from Flathub:

`user $``flatpak install flathub com.github.wwmm.easyeffects`
To start:

`user $``flatpak run com.github.wwmm.easyeffects`
## Usage

To start Easy Effects and launch its GUI:

`user $``easyeffects`
To start Easy Effects without launching the GUI, use:

`user $``easyeffects -w`
or

`user $``easyeffects --hide-window`
To list available presets:

`user $``easyeffects -p`
To load a preset at startup:

`user $``easyeffects -l` *<preset>*
where "\<preset>" should be replaced by the name of the preset.

The `-p` and `-l` options have long forms, `--presets` and `--load-preset`, respectively.
