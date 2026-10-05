<!-- source: https://wiki.gentoo.org/wiki/TiMidity%2B%2B | group: Gentoo Wiki (Main) | wiki-title: TiMidity++ -->
---
title: TiMidity++
url: https://wiki.gentoo.org/wiki/TiMidity%2B%2B
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-03-17"
fingerprint: be06895dc106b986
license: CC BY-SA 4.0
---

# TiMidity++

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**TiMidity++** is a software synthesizer that can interpret MIDI information.

## Installation

### USE flags


| [X](https://packages.gentoo.org/useflags/X) | Add support for X11 | 
| [Xaw3d](https://packages.gentoo.org/useflags/Xaw3d) | Add support for the 3d athena widget set | 
| [alsa](https://packages.gentoo.org/useflags/alsa) | Add support for media-libs/alsa-lib (Advanced Linux Sound Architecture) | 
| [ao](https://packages.gentoo.org/useflags/ao) | Use libao audio output library for sound playback | 
| [emacs](https://packages.gentoo.org/useflags/emacs) | Add support for GNU Emacs | 
| [flac](https://packages.gentoo.org/useflags/flac) | Add support for FLAC: Free Lossless Audio Codec | 
| [gtk](https://packages.gentoo.org/useflags/gtk) | Add support for x11-libs/gtk+ (The GIMP Toolkit) | 
| [jack](https://packages.gentoo.org/useflags/jack) | Add support for the JACK Audio Connection Kit | 
| [motif](https://packages.gentoo.org/useflags/motif) | Add support for the Motif toolkit | 
| [nas](https://packages.gentoo.org/useflags/nas) | Add support for network audio sound | 
| [ncurses](https://packages.gentoo.org/useflags/ncurses) | Add ncurses support (console display library) | 
| [ogg](https://packages.gentoo.org/useflags/ogg) | Add support for the Ogg container format (commonly used by Vorbis, Theora and flac) | 
| [oss](https://packages.gentoo.org/useflags/oss) | Add support for OSS (Open Sound System) | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [slang](https://packages.gentoo.org/useflags/slang) | Add support for the slang text display library (it's like ncurses, but different) | 
| [speex](https://packages.gentoo.org/useflags/speex) | Add support for the speex audio codec (used for speech) | 
| [tk](https://packages.gentoo.org/useflags/tk) | Add support for Tk GUI toolkit | 
| [vorbis](https://packages.gentoo.org/useflags/vorbis) | Add support for the OggVorbis audio codec | 

### Emerge

Emerge [media-sound/timidity++](https://packages.gentoo.org/packages/media-sound/timidity++):

`root #``emerge --ask media-sound/timidity++`
Optionally, install [media-sound/fluid-soundfont](https://packages.gentoo.org/packages/media-sound/fluid-soundfont):

`root #``emerge --ask media-sound/fluid-soundfont`
## Configuration

Once the [eselect-timidity](https://packages.gentoo.org/packages/eselect-timidity) package is installed, a patchset needs to be specified. The Gentoo repository provides two patchsets: [media-sound/timidity-eawpatches](https://packages.gentoo.org/packages/media-sound/timidity-eawpatches) and [media-sound/timidity-freepats](https://packages.gentoo.org/packages/media-sound/timidity-freepats). The list of installed patchsets can be queried using [eselect](https://wiki.gentoo.org/wiki/Eselect):

`user $``eselect timidity list` Available TiMidity++ patchsets:
  \[1\]   eawpatches
  \[2\]   freepats

To set `eawpatches` as the default patchset for the current user:

`user $``eselect timidity set 1`
### Starting the daemon

The [media-sound-timidity++](https://packages.gentoo.org/packages/media-sound-timidity++) package provides system services for both OpenRC and systemd.

#### OpenRC

Start the `timidity` service by running:

`root #``rc-service timidity start`
To add the service to the default run level:

`root #``rc-update add timidity default`
#### Running the daemon in user mode

The TiMidity++ daemon need not be run as a system service, but instead can be run in user mode:

`user $``timidity -iAD -Os`
Timidity can be used with the [JACK](https://wiki.gentoo.org/wiki/JACK) audio server. It must be started by the same user that is running JACK, e.g.:

`user $``timidity -iA -B2,8 -Oj -EFreverb=0 -s 48000`
## Usage

### Convert a MIDI file to mp3

TiMidity++ can be used to "play" a MIDI file to stdout, which can be piped to [ffmpeg](https://wiki.gentoo.org/wiki/Ffmpeg) (install if necessary) to create an mp3 file.

On the command line, run:

`user $``timidity -Ow -o - <file_to_convert.mid> | ffmpeg -i - <output_file.mp3>`
This should create a new .mp3 file.

The `-Ow` option to timidity specifies that output should be in WAV format, the `-o -` specifies the output should be sent to stdout. The `-i -` switch for ffmpeg makes it take input from stdin.

### Emacs

The [media-sound/timidity++](https://packages.gentoo.org/packages/media-sound/timidity++) package includes timidity.el, for controlling TiMidity++ from within Emacs. The file is installed into site-lisp/timidity++/, and should therefore be available automatically; the interface can be started from within Emacs via `M-x timidity`.

## Troubleshooting

### Pulseaudio

Unfortunately TiMidity++ has difficulties with [PulseAudio](https://wiki.gentoo.org/wiki/PulseAudio).

Two possibilities to overcome the situation:

1. Remove [media-sound/pulseaudio](https://packages.gentoo.org/packages/media-sound/pulseaudio) and disable PulseAudio support if installed, by removing the [pulseaudio](https://packages.gentoo.org/useflags/pulseaudio)2. Emerge [media-sound/timidity++](https://packages.gentoo.org/packages/media-sound/timidity++) with Libao support by activating the [ao](https://packages.gentoo.org/useflags/ao)`-Oo`

### Sound output monopolized by TiMidity++

Even with PulseAudio removed, sound may be blocked when TiMidity++ is run as a system service. The solution seems to be that some soundcards require a dmix parameter in /etc/asound.conf:

**`/etc/asound.conf`**

## See also

- [MIDI](https://wiki.gentoo.org/wiki/MIDI) — a set of technical specifications that enable devices to interoperate in order to work with a digital representation of music
- [MIDI controller guide](https://wiki.gentoo.org/wiki/MIDI_controller_guide) — musical equipment including keyboards, pads, pot/fader controls and much more
- [Project:Sound/How to Enable Realtime for Multimedia Applications](https://wiki.gentoo.org/wiki/Project:Sound/How_to_Enable_Realtime_for_Multimedia_Applications)
