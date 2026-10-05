<!-- source: https://wiki.gentoo.org/wiki/List_of_audio_software | group: Gentoo Wiki (Main) | wiki-title: List of audio software -->
---
title: List of audio software
url: https://wiki.gentoo.org/wiki/List_of_audio_software
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-18"
fingerprint: "9c9fdffa4ba21e9b"
license: CC BY-SA 4.0
---

# List of audio software

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page gives brief lists of packages related to playing music/audio, such as mixers / volume controls, players, editors, and so on.

For packages related to music production, refer to the "[List of music production software](https://wiki.gentoo.org/wiki/List_of_music_production_software)" page.

For information about setting up audio output and input on Gentoo, refer to the [PipeWire](https://wiki.gentoo.org/wiki/PipeWire), [PulseAudio](https://wiki.gentoo.org/wiki/PulseAudio), [ALSA](https://wiki.gentoo.org/wiki/ALSA) and [JACK](https://wiki.gentoo.org/wiki/JACK) pages.

## Mixing/volume

| Name | Package | Description | 
|---|---|---|
| alsamixer | [media-sound/alsa-utils](https://packages.gentoo.org/packages/media-sound/alsa-utils) | Soundcard mixer for ALSA soundcard driver, with ncurses interface; [alsamixer(1)](https://man.archlinux.org/man/alsamixer.1.en)[.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)  | 
| amixer | [media-sound/alsa-utils](https://packages.gentoo.org/packages/media-sound/alsa-utils) | Command-line mixer for ALSA soundcard driver; [amixer(1)](https://man.archlinux.org/man/amixer.1.en)[.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)  | 
| [Easy Effects](https://wiki.gentoo.org/wiki/Easy_Effects) | [media-sound/easyeffects](https://packages.gentoo.org/packages/media-sound/easyeffects) | Limiter, auto volume and many other plugins for PipeWire applications. | 
| [pavucontrol](https://freedesktop.org/software/pulseaudio/pavucontrol/) | [media-sound/pavucontrol](https://packages.gentoo.org/packages/media-sound/pavucontrol) | GTK based mixer for Pulseaudio. | 
| [pipemixer](https://github.com/heather7283/pipemixer) | [media-sound/pipemixer::guru](https://github.com/gentoo-mirror/guru/tree/master/media-sound/pipemixer) | TUI mixer for PipeWire. | 
| [pulsemixer](https://github.com/GeorgeFilipkin/pulsemixer) | [media-sound/pulsemixer](https://packages.gentoo.org/packages/media-sound/pulsemixer) | CLI and curses mixer for PulseAudio. | 
| [pwvucontrol](https://github.com/saivert/pwvucontrol/) | [media-sound/pwvucontrol](https://packages.gentoo.org/packages/media-sound/pwvucontrol) | [GTK](https://wiki.gentoo.org/wiki/GTK)-based mixer GUI for PipeWire. | 
| [wiremix](https://wiki.gentoo.org/wiki/Wiremix) | [media-sound/wiremix](https://packages.gentoo.org/packages/media-sound/wiremix) | TUI  mixer for [PipeWire](https://wiki.gentoo.org/wiki/PipeWire) / [WirePlumber](https://wiki.gentoo.org/wiki/WirePlumber). | 

## Players

| Name | Package | Description | 
|---|---|---|
| aplay | [media-sound/alsa-utils](https://packages.gentoo.org/packages/media-sound/alsa-utils) | Command-line player for ALSA soundcard driver; [aplay(1)](https://man.archlinux.org/man/aplay.1.en)[.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)  | 
| [MPD](https://wiki.gentoo.org/wiki/MPD) | [media-sound/mpd](https://packages.gentoo.org/packages/media-sound/mpd) | The Music Player Daemon (mpd); [mpd(1)](https://man.archlinux.org/man/mpd.1.en)[.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)  | 
| [mpv](https://wiki.gentoo.org/wiki/Mpv) | [media-video/mpv](https://packages.gentoo.org/packages/media-video/mpv) | Media player for the command line; [mpv(1)](https://man.archlinux.org/man/mpv.1.en)[.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)  | 
| [Shortwave](https://apps.gnome.org/Shortwave/) | [media-sound/shortwave::guru](https://github.com/gentoo-mirror/guru/tree/master/media-sound/shortwave) | Internet radio player that provides access to a station database with over 50,000 stations. | 

## Patchbays / connection controllers

| Name | Package | Description | 
|---|---|---|
| [Helvum](https://gitlab.freedesktop.org/pipewire/helvum) | [media-sound/helvum](https://packages.gentoo.org/packages/media-sound/helvum) | GTK patchbay for [PipeWire](https://wiki.gentoo.org/wiki/PipeWire). | 
| [QjackCtl](https://qjackctl.sourceforge.io/) | [media-sound/qjackctl](https://packages.gentoo.org/packages/media-sound/qjackctl) | Qt GUI to control the [JACK](https://wiki.gentoo.org/wiki/JACK) Audio Connection Kit and [ALSA](https://wiki.gentoo.org/wiki/ALSA) sequencer connections; [qjackctl(1)](https://man.archlinux.org/man/qjackctl.1.en)[.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)  | 
| [qpwgraph](https://gitlab.freedesktop.org/rncbc/qpwgraph) | [media-sound/qpwgraph](https://packages.gentoo.org/packages/media-sound/qpwgraph) | PipeWire Graph Qt GUI Interface. | 

## MIDI support

| Name | Package | Description | 
|---|---|---|
| [amsynth](https://wiki.gentoo.org/wiki/Amsynth) | [media-sound/amsynth](https://packages.gentoo.org/packages/media-sound/amsynth) | Virtual analogue synthesizer; [amsynth(1)](https://man.archlinux.org/man/amsynth.1.en)[.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)  | 
| [FluidSynth](https://wiki.gentoo.org/wiki/FluidSynth) | [media-sound/fluidsynth](https://packages.gentoo.org/packages/media-sound/fluidsynth) | Software real-time synthesizer based on the Soundfont 2 specifications; [fluidsynth(1)](https://man.archlinux.org/man/fluidsynth.1.en)[.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)  | 
| [TiMidity++](https://wiki.gentoo.org/wiki/TiMidity%2B%2B) | [media-sound/timidity++](https://packages.gentoo.org/packages/media-sound/timidity++) | Handy MIDI to WAV converter with OSS and ALSA output support; [timidity(1)](https://man.archlinux.org/man/timidity.1.en)[.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)  | 

## MIDI controllers

| Name | Package | Description | 
|---|---|---|
| [VMPK](https://wiki.gentoo.org/wiki/VMPK) | [media-sound/vmpk](https://packages.gentoo.org/packages/media-sound/vmpk) | Virtual MIDI Piano Keyboard; [vmpk(1)](https://man.archlinux.org/man/vmpk.1.en)[.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)  | 

## Frontends

| Name | Package | Description | 
|---|---|---|
| [ncmpcpp](https://wiki.gentoo.org/wiki/Ncmpcpp) | [media-sound/ncmpcpp](https://packages.gentoo.org/packages/media-sound/ncmpcpp) | Featureful ncurses-based [MPD](https://wiki.gentoo.org/wiki/MPD) client inspired by ncmpc; [ncmpcpp(1)](https://man.archlinux.org/man/ncmpcpp.1.en)[.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)  | 
| [Qsynth](https://wiki.gentoo.org/wiki/Qsynth) | [media-sound/qsynth](https://packages.gentoo.org/packages/media-sound/qsynth) | Qt application to control FluidSynth; [qsynth(1)](https://man.archlinux.org/man/qsynth.1.en)[.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)  | 
| [Sonata](https://www.nongnu.org/sonata/) | [media-sound/sonata](https://packages.gentoo.org/packages/media-sound/sonata) | Lightweight [GTK+](https://wiki.gentoo.org/wiki/GTK) music client for the Music Player Daemon ([MPD](https://wiki.gentoo.org/wiki/MPD)). | 

## See also

- [Recommended applications](https://wiki.gentoo.org/wiki/Recommended_applications) — applications recommended for use in a graphical environment ([X11](https://wiki.gentoo.org/wiki/Xorg), [Wayland](https://wiki.gentoo.org/wiki/Wayland))
- [Recommended tools](https://wiki.gentoo.org/wiki/Recommended_tools) — lists system-administration related tools recommended for use in a **[shell](https://wiki.gentoo.org/wiki/Shell) environment** ([terminal/console](https://wiki.gentoo.org/wiki/Terminal_emulator))
- [Qt Desktop applications](https://wiki.gentoo.org/wiki/Qt_Desktop_applications) — a list of recommendations for a light-weight, non-[KDE](https://wiki.gentoo.org/wiki/KDE), [Qt](https://wiki.gentoo.org/wiki/Qt)-only desktop environment.
