<!-- source: https://wiki.gentoo.org/wiki/Qsynth | group: Gentoo Wiki (Main) | wiki-title: Qsynth -->
---
title: Qsynth
url: https://wiki.gentoo.org/wiki/Qsynth
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-13"
fingerprint: c73bd5904c359c3b
license: CC BY-SA 4.0
---

# Qsynth

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Qsynth** is a GUI frontend to [FluidSynth](https://wiki.gentoo.org/wiki/FluidSynth).

## Installation

### Emerge

`root #``emerge --ask media-sound/qsynth`
### Usage

To start Qsynth from the command line:

`user $``qsynth`
A soundfont will need to be specified for [FluidSynth](https://wiki.gentoo.org/wiki/FluidSynth) to use. Two soundfonts are provided by the [media-sound/fluid-soundfont](https://packages.gentoo.org/packages/media-sound/fluid-soundfont) package, FluidR3\_GM.sf2 and FluidR3\_SM.sf2.

In Qsynth, the soundfont can be specified via Settings -> Soundfonts; once the [media-sound/fluid-soundfont](https://packages.gentoo.org/packages/media-sound/fluid-soundfont) package is installed, FluidR3\_GM.sf2 can be be found in /usr/share/sounds/sf2/.

To use Qsynth with PipeWire, specify PipeWire as the audio driver via Settings -> Audio.

## See also

- [FluidSynth](https://wiki.gentoo.org/wiki/FluidSynth) — a real-time software synthesizer based on the SoundFont 2 specifications
- [MIDI controller guide](https://wiki.gentoo.org/wiki/MIDI_controller_guide) — musical equipment including keyboards, pads, pot/fader controls and much more
- [TiMidity++](https://wiki.gentoo.org/wiki/TiMidity%2B%2B) — software synthesizer that can interpret MIDI information
