<!-- source: https://wiki.gentoo.org/wiki/Audacity | group: Gentoo Wiki (Main) | wiki-title: Audacity -->
---
title: Audacity
url: https://wiki.gentoo.org/wiki/Audacity
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-27"
fingerprint: "600431c4997784c"
license: CC BY-SA 4.0
---

# Audacity

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Audacity** is a cross-platform audio editing application.

## Installation

### USE flags


### USE flags for
            [media-sound/audacity](https://packages.gentoo.org/packages/media-sound/audacity)
            
            Free crossplatform audio editor

| [+flac](https://packages.gentoo.org/useflags/+flac) | Add support for FLAC: Free Lossless Audio Codec | 
| [+ladspa](https://packages.gentoo.org/useflags/+ladspa) | Enable the ability to support ladspa plugins | 
| [+lv2](https://packages.gentoo.org/useflags/+lv2) | Add support for Ladspa V2 | 
| [+ogg](https://packages.gentoo.org/useflags/+ogg) | Add support for the Ogg container format (commonly used by Vorbis, Theora and flac) | 
| [+portmixer](https://packages.gentoo.org/useflags/+portmixer) | Enable the internal portmixer feature | 
| [+vorbis](https://packages.gentoo.org/useflags/+vorbis) | Add support for the OggVorbis audio codec | 
| [alsa](https://packages.gentoo.org/useflags/alsa) | Add support for media-libs/alsa-lib (Advanced Linux Sound Architecture) | 
| [audiocom](https://packages.gentoo.org/useflags/audiocom) | Enable integrated uploading to audio.com | 
| [ffmpeg](https://packages.gentoo.org/useflags/ffmpeg) | Enable ffmpeg/libav-based audio/video codec support | 
| [id3tag](https://packages.gentoo.org/useflags/id3tag) | Enable ID3 tagging via media-libs/libid3tag | 
| [mad](https://packages.gentoo.org/useflags/mad) | Add support for mad (high-quality mp3 decoder library and cli frontend) | 
| [mpg123](https://packages.gentoo.org/useflags/mpg123) | Use media-sound/mpg123-base instead of media-libs/libmad for MPEG decoding | 
| [opus](https://packages.gentoo.org/useflags/opus) | Enable Opus audio codec support | 
| [sbsms](https://packages.gentoo.org/useflags/sbsms) | Enable slower, more accurate pitch and tempo changing via media-libs/libsbsms | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [twolame](https://packages.gentoo.org/useflags/twolame) | Enable twolame support (MPEG Audio Layer 2 encoder) via media-sound/twolame | 
| [vamp](https://packages.gentoo.org/useflags/vamp) | Enable vamp plugins support (Audio analysing plugins) | 
| [wavpack](https://packages.gentoo.org/useflags/wavpack) | Add support for wavpack audio compression tools | 

### Emerge

`root #``emerge --ask media-sound/audacity`
## Troubleshooting

### JACK missing as a host

If the `jack` USE flag is enabled for [media-libs/portaudio](https://packages.gentoo.org/packages/media-libs/portaudio), and JACK still isn't available, it's likely because JACK isn't running. [PipeWire](https://wiki.gentoo.org/wiki/PipeWire)-based systems can use PipeWire as a JACK replacement by enabling the `jack` USE flag on [media-libs/portaudio](https://packages.gentoo.org/packages/media-libs/portaudio); otherwise, refer to the [JACK](https://wiki.gentoo.org/wiki/JACK) article for information on setting up JACK.

### No audio output

On a OpenRC+PipeWire system absence of audio output might indicate that the `elogind` USE flag needs to be enabled on the [media-video/pipewire](https://packages.gentoo.org/packages/media-video/pipewire) package.

If you're using [media-video/pipewire](https://packages.gentoo.org/packages/media-video/pipewire) in a SystemD system, make sure the `pipewire-alsa` USE flag is enabled for [media-video/pipewire](https://packages.gentoo.org/packages/media-video/pipewire), and the `alsa` USE flag is enabled for [media-libs/portaudio](https://packages.gentoo.org/packages/media-libs/portaudio)

## See also

- [Music production](https://wiki.gentoo.org/wiki/Music_production) — Gentoo can be a good platform for **music production**.
