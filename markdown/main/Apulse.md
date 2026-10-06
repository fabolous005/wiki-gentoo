<!-- source: https://wiki.gentoo.org/wiki/Apulse | group: Gentoo Wiki (Main) | wiki-title: Apulse -->
---
title: apulse
url: https://wiki.gentoo.org/wiki/Apulse
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-21"
fingerprint: dde8be5e6ef0fc36
license: CC BY-SA 4.0
---

# apulse

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**apulse** provides [PulseAudio](https://wiki.gentoo.org/wiki/PulseAudio) emulation for [ALSA](https://wiki.gentoo.org/wiki/ALSA). This allows sound from applications that use the Pulse API (e.g. [Firefox](https://wiki.gentoo.org/wiki/Firefox)) on a pure-ALSA system.

Internally, no separate sound mixing daemon is used. Instead, apulse relies on ALSA's `dmix`, `dsnoop`, and `plug` plugins to handle multiple sound sources and capture streams running at the same time. \[The\] `dmix` plugin muxes multiple playback streams; \[the\] `dsnoop` plugin allow multiple applications to capture from a single microphone; and \[the\] `plug` plugin transparently converts audio between various sample formats, sample rates and channel numbers.


## Installation

### USE flags


### USE flags for
            [media-sound/apulse](https://packages.gentoo.org/packages/media-sound/apulse)
            
            PulseAudio emulation for ALSA

| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [sdk](https://packages.gentoo.org/useflags/sdk) | Install PulseAudio headers and pkg-config files. Be aware apulse is not a full PulseAudio replacement by design and some functionality may be missing. | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

Emerge apulse:

`root #``emerge --ask media-sound/apulse`
