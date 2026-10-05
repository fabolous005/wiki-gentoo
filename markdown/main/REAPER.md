<!-- source: https://wiki.gentoo.org/wiki/REAPER | group: Gentoo Wiki (Main) | wiki-title: REAPER -->
---
title: REAPER
url: https://wiki.gentoo.org/wiki/REAPER
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-04"
fingerprint: fcc39c9cc9e5397c
license: CC BY-SA 4.0
---

# REAPER

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**REAPER** is a complete digital audio production application for computers, offering a full multitrack audio and MIDI recording, editing, processing, mixing and mastering toolset.

Although REAPER is proprietary, Cockos offers a fully functional indefinite evaluation period.

## Installation

### USE flags


| [+jack](https://packages.gentoo.org/useflags/+jack) | Add support for the JACK Audio Connection Kit | 
| [ffmpeg](https://packages.gentoo.org/useflags/ffmpeg) | Enable ffmpeg/libav-based audio/video codec support | 
| [mp3](https://packages.gentoo.org/useflags/mp3) | Add support for reading mp3 files | 
| [pulseaudio](https://packages.gentoo.org/useflags/pulseaudio) | Add sound server support via media-libs/libpulse (may be PulseAudio or PipeWire) | 

### License

To install [media-sound/reaper-bin](https://packages.gentoo.org/packages/media-sound/reaper-bin), Portage requires accepting the Cockos license agreement:

**`/etc/portage/package.license/media-sound`**

**Accepting REAPER's license**

### Emerge

Install REAPER:

`root #``emerge --ask media-sound/reaper-bin`
### Additional software

See the page on [music production](https://wiki.gentoo.org/wiki/Music_production) for additional software that can be used in conjunction with REAPER, including controllers, plugins, synths etc...

## Usage

See below for information and resources on how to get started with REAPER

### Invocation

REAPER can be found in the application launcher of your desktop environment. It can also be started with the reaper command.

`user $``reaper`
