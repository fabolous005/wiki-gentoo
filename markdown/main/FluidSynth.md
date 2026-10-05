<!-- source: https://wiki.gentoo.org/wiki/FluidSynth | group: Gentoo Wiki (Main) | wiki-title: FluidSynth -->
---
title: FluidSynth
url: https://wiki.gentoo.org/wiki/FluidSynth
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-23"
fingerprint: "7e42c35cdd0639e0"
license: CC BY-SA 4.0
---

# FluidSynth

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**FluidSynth** is a real-time software synthesizer based on the SoundFont 2 specifications.

## Installation

### USE flags


| [+readline](https://packages.gentoo.org/useflags/+readline) | Enable support for libreadline, a GNU line-editing library that almost everyone wants | 
| [+sndfile](https://packages.gentoo.org/useflags/+sndfile) | Add support for libsndfile | 
| [alsa](https://packages.gentoo.org/useflags/alsa) | Add support for media-libs/alsa-lib (Advanced Linux Sound Architecture) | 
| [dbus](https://packages.gentoo.org/useflags/dbus) | Enable dbus support for anything that needs it (gpsd, gnomemeeting, etc) | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [ipv6](https://packages.gentoo.org/useflags/ipv6) | Add support for IP version 6 | 
| [jack](https://packages.gentoo.org/useflags/jack) | Add support for the JACK Audio Connection Kit | 
| [ladspa](https://packages.gentoo.org/useflags/ladspa) | Enable the ability to support ladspa plugins | 
| [network](https://packages.gentoo.org/useflags/network) | enable network support (requires BSD sockets) | 
| [openmp](https://packages.gentoo.org/useflags/openmp) | Build support for the OpenMP (support parallel computing), requires >=sys-devel/gcc-4.2 built with USE="openmp" | 
| [oss](https://packages.gentoo.org/useflags/oss) | Add support for OSS (Open Sound System) | 
| [pipewire](https://packages.gentoo.org/useflags/pipewire) | enable media-video/pipewire support | 
| [portaudio](https://packages.gentoo.org/useflags/portaudio) | Add support for the crossplatform portaudio audio API | 
| [pulseaudio](https://packages.gentoo.org/useflags/pulseaudio) | Add sound server support via media-libs/libpulse (may be PulseAudio or PipeWire) | 
| [sdl](https://packages.gentoo.org/useflags/sdl) | Add support for Simple Direct Layer (media library) | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [threads](https://packages.gentoo.org/useflags/threads) | Add threads support for various packages. Usually pthreads | 

### Emerge

`root #``emerge --ask media-sound/fluidsynth`
## Configuration

### Files

FluidSynth looks for a configuration file at \~/.fluidsynth. For example:

**`~/.fluidsynth`**

To list available settings and their current values, use `-o help`:

`user $``fluidsynth -o help`
In particular, available values for `audio.driver` as of FluidSynth 2.5.2 are `alsa`,`file`, `jack`, `pipewire`, `pulseaudio`.

### Service

#### OpenRC

The OpenRC service provided by the [media-sound/fluidsynth](https://packages.gentoo.org/packages/media-sound/fluidsynth) package is a system service; it will be run as root. This might not be appropriate for setups in which the sound server, such as [PipeWire](https://wiki.gentoo.org/wiki/PipeWire), is run as a user.

To manually start FluidSynth as a server, and without starting an interactive session, use the `-s` and `-i` options, respectively, using one of the SoundFonts provided by the [media-sound/fluid-soundfont](https://packages.gentoo.org/packages/media-sound/fluid-soundfont) package:

`user $``fluidsynth -s -i /usr/share/sounds/sf2/FluidR3_GM.sf2`
### Usage

Although FluidSynth can be used directly from the command line, GUI front ends are available, such as [Qsynth](https://wiki.gentoo.org/wiki/Qsynth).

#### PipeWire

On [PipeWire](https://wiki.gentoo.org/wiki/PipeWire)-based systems, FluidSynth can be combined with pw-midiplay to play MIDI files.

With FluidSynth running, run pw-midiplay on the MIDI file to be played:

`user $``pw-midiplay --target 0 track.midi &`
Run pw-link to find the node IDs of the FluidSynth input port and the pw-midiplay output port:

`user $``pw-link -ioI | grep 'FLUID\|midiplay'`
124 Midi-Bridge:FLUID Synth (10855):(playback\_0) Synth input port (10855:0)
133 pw-midiplay:output\_1

Link pw-midiplay's output to FluidSynth's input:

`user $``pw-link 133 124`
The track should now be played.

## See also

- [MIDI controller guide](https://wiki.gentoo.org/wiki/MIDI_controller_guide) — musical equipment including keyboards, pads, pot/fader controls and much more
- [Qsynth](https://wiki.gentoo.org/wiki/Qsynth) — a GUI frontend to [FluidSynth]
- [TiMidity++](https://wiki.gentoo.org/wiki/TiMidity%2B%2B) — software synthesizer that can interpret MIDI information
