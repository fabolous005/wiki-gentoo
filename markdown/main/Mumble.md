<!-- source: https://wiki.gentoo.org/wiki/Mumble | group: Gentoo Wiki (Main) | wiki-title: Mumble -->
---
title: Mumble
url: https://wiki.gentoo.org/wiki/Mumble
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-07"
fingerprint: de00081ce5ca7946
license: CC BY-SA 4.0
---

# Mumble

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Mumble is an open source, cross platform, low-latency, high quality voice over IP (VoIP) client. Mumble uses a client/server architecture and is primarily used by gamers, but can be used for any VoIP purpose.

## Installation

### Kernel

Many laptops have built-in USB microphones (almost always paired with USB web cameras). The kernel needs the `SND_USB_AUDIO` option enabled to support USB audio (input) devices.

**Enable support for USB microphones (`SND_USB_AUDIO`)**

### USE flags


| [+alsa](https://packages.gentoo.org/useflags/+alsa) | Add support for media-libs/alsa-lib (Advanced Linux Sound Architecture) | 
| [+rnnoise](https://packages.gentoo.org/useflags/+rnnoise) | Enable alternative noise suppression option based on RNNoise. | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [jack](https://packages.gentoo.org/useflags/jack) | Add support for the JACK Audio Connection Kit | 
| [multilib](https://packages.gentoo.org/useflags/multilib) | On 64bit systems, if you want to be able to compile 32bit and 64bit binaries | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [pipewire](https://packages.gentoo.org/useflags/pipewire) | Enable pipewire support for audio output. | 
| [portaudio](https://packages.gentoo.org/useflags/portaudio) | Add support for the crossplatform portaudio audio API | 
| [pulseaudio](https://packages.gentoo.org/useflags/pulseaudio) | Add sound server support via media-libs/libpulse (may be PulseAudio or PipeWire) | 
| [speech](https://packages.gentoo.org/useflags/speech) | Enable text-to-speech support | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [zeroconf](https://packages.gentoo.org/useflags/zeroconf) | Support for DNS Service Discovery (DNS-SD) | 

### Emerge

Install Mumble:

`root #``emerge --ask net-voip/mumble`
## Removal

### Unmerge

Uninstall Mumble by issuing:

`root #``emerge --ask --depclean --verbose net-voip/mumble`
