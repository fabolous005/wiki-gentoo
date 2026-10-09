<!-- source: https://wiki.gentoo.org/wiki/Rosegarden | group: Gentoo Wiki (Main) | wiki-title: Rosegarden -->
---
title: Rosegarden
url: https://wiki.gentoo.org/wiki/Rosegarden
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-13"
fingerprint: de807319c9a77cbe
license: CC BY-SA 4.0
---

# Rosegarden

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Rosegarden** is a music composition and editing environment based around a MIDI sequencer that features a rich understanding of music notation and includes basic support for digital audio.

## Installation

### Emerge

`root #``emerge --ask media-sound/rosegarden`
## Configuration

### Kernel

**Enable sequencer support**

Device Drivers --->
  \<\*> Sound card support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SOUND\</code> to find this item. --->
    \<\*> Advanced Linux Sound Architecture [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\</code> to find this item. --->
      \<\*> Sequencer support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_SEQUENCER\</code> to find this item.

Each of the above kernel configuration options can instead be built as a module (`<M>`), but in that case the system must be configured to load the `snd-seq-midi` module and its dependencies at boot. To do so, add a file in the /etc/modules-load.d/ directory, creating the directory if necessary:

**`sequencer.conf`**

```
snd-seq-midi
```
### Software synth

Install a software synth that can be used by Rosegarden. [Qsynth](https://wiki.gentoo.org/wiki/Qsynth) ([media-sound/qsynth](https://packages.gentoo.org/packages/media-sound/qsynth)) is a [Qt](https://wiki.gentoo.org/wiki/Qt) GUI frontend to [FluidSynth](https://wiki.gentoo.org/wiki/FluidSynth) ([media-sound/fluidsynth](https://packages.gentoo.org/packages/media-sound/fluidsynth)).

### Usage

Before starting Rosegarden, ensure the software synth (e.g. Qsynth) is running:

`user $``qsynth`
Start Rosegarden:

`user $``rosegarden`
If not using [JACK](https://wiki.gentoo.org/wiki/JACK), but instead using e.g. PipeWire, ensure that the following Rosegarden settings, available via Edit -> Audio, are **disabled**:

- Make default JACK connections for audio outputs
- Make default JACK connections for audio inputs
- Start JACK automatically

#### Exporting to MP3

Rosegarden can export a composition to a MIDI file, which can then be converted to an MP3 via the use of [TiMidity++](https://wiki.gentoo.org/wiki/TiMidity%2B%2B) and [ffmpeg](https://wiki.gentoo.org/wiki/Ffmpeg) (see those respective articles for installation instructions).

In Rosegarden's main window, select File -> Export -> Export MIDI file, and specify an appropriate filename, e.g. composition.mid.

See the [TiMidity++ article section on converting MIDI to mp3](https://wiki.gentoo.org/wiki/TiMidity%2B%2B#Convert_a_MIDI_file_to_mp3) for instructions on how to convert this exported MIDI file to mp3.

## See also

- [JACK](https://wiki.gentoo.org/wiki/JACK) — a sound server for professional audio production that provides low-latency communication for applications that implement the JACK API
- [MIDI](https://wiki.gentoo.org/wiki/MIDI) — a set of technical specifications that enable devices to interoperate in order to work with a digital representation of music
- [PipeWire](https://wiki.gentoo.org/wiki/PipeWire) — low-latency, graph-based, processing engine and server, for interfacing with audio and video devices.
