<!-- source: https://wiki.gentoo.org/wiki/Apulse | group: Gentoo Wiki (Main) | wiki-title: Apulse -->
---
title: apulse
url: https://wiki.gentoo.org/wiki/Apulse
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-24"
fingerprint: "9ee87e5e6ee8bd32"
license: CC BY-SA 4.0
---

# apulse

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
### Troubleshooting

#### Errors when using pulse-rust backend in Firefox

If using the pulse-rust backend in Firefox results in errors from apulse:

\[apulse\] \[error\] do\_connect\_pcm: can't open capture device "default". Error code -22 (Invalid argument)
\[apulse\] \[error\] do\_connect\_pcm: failed to open ALSA device. Apulse does no resampling or format conversion, leaving that task to ALSA plugins. Ensure that selected device is capable of playing a particular sample format at a particular rate. They have to be supported by either hardware directly, or by "plug" and "dmix" ALSA plugins which will perform required conversions on CPU.

this might be able to be addressed by setting the `APULSE_CAPTURE_DEVICE` environment variable when calling Firefox, e.g.:

`user $``APULSE_CAPTURE_DEVICE=null firefox`
modifying the name of the binary if necessary (e.g. to firefox-bin).

If a permanent solution is required, modify [the appropriate ALSA configuration file](https://wiki.gentoo.org/wiki/ALSA#Files), e.g. \~/.asoundrc or /etc/asound.conf:

```
# Existing working playback setup (e.g. using dmix)
pcm.dmixer {
    type dmix
    ipc_key 1024
    slave {
        pcm "hw:0,0"
    }
}
# Combine working playback with a completely silent capture device
pcm.silent_duplex {
    type asym
    playback.pcm "plug:dmixer"
    capture.pcm "null" # Produces/absorbs pure silence
}
# Set this asymmetric layout as system default
pcm.!default {
    type plug
    slave.pcm "silent_duplex"
}
ctl.!default {
    type hw
    card 0
}
```
## See also

- [PulseAudio](https://wiki.gentoo.org/wiki/PulseAudio) — a multi-platform, open source, sound server that provides a number of features on top of the low-level audio interface [ALSA](https://wiki.gentoo.org/wiki/ALSA)
