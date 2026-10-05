<!-- source: https://wiki.gentoo.org/wiki/MIDI | group: Gentoo Wiki (Main) | wiki-title: MIDI -->
---
title: MIDI
url: https://wiki.gentoo.org/wiki/MIDI
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-11-30"
fingerprint: c412423c4abf6e3e
license: CC BY-SA 4.0
---

# MIDI

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**MIDI** (Musical Instrument Digital Interface) is a set of technical specifications that enable devices to interoperate in order to work with a digital representation of music.

MIDI controllers (often in the form of a MIDI keyboard) are used to create music in the form a MIDI signal that can be sent to a number of other MIDI-compatible devices, such as a MIDI instrument to convert the MIDI signal to sound ("play" it), a MIDI sequencer to record the MIDI data, or to a computer.

Computers allow great flexibility in the processing of MIDI data, such as playing the MIDI signal on virtual MIDI instruments, recording the MIDI information to file, editing the MIDI data, representing the MIDI signal as musical notes in notation software, etc.

## Available software

These packages can interpret MIDI information to produce audio output:

| Name | Package | Description | 
|---|---|---|
| [FluidSynth](https://wiki.gentoo.org/wiki/FluidSynth) | [media-sound/fluidsynth](https://packages.gentoo.org/packages/media-sound/fluidsynth) | Software real-time synthesizer based on the Soundfont 2 specifications | 
| [Rosegarden](https://wiki.gentoo.org/wiki/Rosegarden) | [media-sound/rosegarden](https://packages.gentoo.org/packages/media-sound/rosegarden) | A music composition and editing environment based around a MIDI sequencer | 
| [TiMidity++](https://wiki.gentoo.org/wiki/TiMidity%2B%2B) | [media-sound/timidity++](https://packages.gentoo.org/packages/media-sound/timidity++) | Handy MIDI to WAV converter with OSS and ALSA output support | 

### Packages with MIDI support

Several packages use the [midi](https://packages.gentoo.org/useflags/midi) [USE flag](https://wiki.gentoo.org/wiki/USE_flag). These packages may be of interest to anyone looking to use MIDI on Gentoo.

To see a list of some of the packages that provide USE flags containing the keyword `midi`, [quse](https://wiki.gentoo.org/wiki/Q_applets) can help:

`user $``quse -ve midi`
## See also

- [MIDI controller guide](https://wiki.gentoo.org/wiki/MIDI_controller_guide) — musical equipment including keyboards, pads, pot/fader controls and much more
- [Music production](https://wiki.gentoo.org/wiki/Music_production) — Gentoo can be a good platform for **music production**.
- [Project:Sound/How to Enable Realtime for Multimedia Applications](https://wiki.gentoo.org/wiki/Project:Sound/How_to_Enable_Realtime_for_Multimedia_Applications)
