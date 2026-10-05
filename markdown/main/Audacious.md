<!-- source: https://wiki.gentoo.org/wiki/Audacious | group: Gentoo Wiki (Main) | wiki-title: Audacious -->
---
title: Audacious
url: https://wiki.gentoo.org/wiki/Audacious
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-11-07"
fingerprint: "7e438b5cc986399c"
license: CC BY-SA 4.0
---

# Audacious

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Audacious** is a media player and library manager, similar to XMMS, and Winamp.

## Installation

### USE flags

#### Audacious


#### Audacious plugins


| [+alsa](https://packages.gentoo.org/useflags/+alsa) | Add support for media-libs/alsa-lib (Advanced Linux Sound Architecture) | 
| [+mp3](https://packages.gentoo.org/useflags/+mp3) | Add support for reading mp3 files | 
| [X](https://packages.gentoo.org/useflags/X) | Add support for X11 | 
| [aac](https://packages.gentoo.org/useflags/aac) | Enable support for MPEG-4 AAC Audio | 
| [ampache](https://packages.gentoo.org/useflags/ampache) | Support controlling audacious via ampache | 
| [bs2b](https://packages.gentoo.org/useflags/bs2b) | Enable Bauer Bauer stereophonic-to-binaural headphone filter | 
| [cdda](https://packages.gentoo.org/useflags/cdda) | Add Compact Disk Digital Audio (Standard Audio CD) support | 
| [cue](https://packages.gentoo.org/useflags/cue) | Support CUE sheets using the libcue library | 
| [ffmpeg](https://packages.gentoo.org/useflags/ffmpeg) | Enable ffmpeg/libav-based audio/video codec support | 
| [flac](https://packages.gentoo.org/useflags/flac) | Add support for FLAC: Free Lossless Audio Codec | 
| [fluidsynth](https://packages.gentoo.org/useflags/fluidsynth) | Support FluidSynth as MIDI synth backend | 
| [gme](https://packages.gentoo.org/useflags/gme) | Support various gaming console music formats | 
| [gtk](https://packages.gentoo.org/useflags/gtk) | Add support for x11-libs/gtk+ (The GIMP Toolkit) | 
| [http](https://packages.gentoo.org/useflags/http) | Support HTTP streams through neon | 
| [jack](https://packages.gentoo.org/useflags/jack) | Add support for the JACK Audio Connection Kit | 
| [lame](https://packages.gentoo.org/useflags/lame) | Add support for MP3 encoding using LAME | 
| [libnotify](https://packages.gentoo.org/useflags/libnotify) | Enable desktop notification support | 
| [libsamplerate](https://packages.gentoo.org/useflags/libsamplerate) | Build with support for converting sample rates using libsamplerate | 
| [lirc](https://packages.gentoo.org/useflags/lirc) | Add support for lirc (Linux's Infra-Red Remote Control) | 
| [mms](https://packages.gentoo.org/useflags/mms) | Support for Microsoft Media Server (MMS) streams | 
| [modplug](https://packages.gentoo.org/useflags/modplug) | Add libmodplug support for playing SoundTracker-style music files | 
| [mpris](https://packages.gentoo.org/useflags/mpris) | Enable support for MPRIS2 interface | 
| [opengl](https://packages.gentoo.org/useflags/opengl) | Add support for OpenGL (3D graphics) | 
| [openmpt](https://packages.gentoo.org/useflags/openmpt) | Add support for OpenMPT | 
| [opus](https://packages.gentoo.org/useflags/opus) | Enable Opus audio codec support | 
| [pipewire](https://packages.gentoo.org/useflags/pipewire) | Build the PipeWire output plugin | 
| [pulseaudio](https://packages.gentoo.org/useflags/pulseaudio) | Add sound server support via media-libs/libpulse (may be PulseAudio or PipeWire) | 
| [qt6](https://packages.gentoo.org/useflags/qt6) | Add support for the Qt 6 application and UI framework | 
| [qtmedia](https://packages.gentoo.org/useflags/qtmedia) | Enable playback via dev-qt/qtmultimedia | 
| [scrobbler](https://packages.gentoo.org/useflags/scrobbler) | Build with scrobbler/LastFM submission support | 
| [sdl](https://packages.gentoo.org/useflags/sdl) | Add support for Simple Direct Layer (media library) | 
| [sid](https://packages.gentoo.org/useflags/sid) | Enable SID (Commodore 64 audio) file support | 
| [sndfile](https://packages.gentoo.org/useflags/sndfile) | Add support for libsndfile | 
| [soxr](https://packages.gentoo.org/useflags/soxr) | Build with SoX Resampler support | 
| [streamtuner](https://packages.gentoo.org/useflags/streamtuner) | Build the streamtuner plugin | 
| [vorbis](https://packages.gentoo.org/useflags/vorbis) | Add support for the OggVorbis audio codec | 
| [wavpack](https://packages.gentoo.org/useflags/wavpack) | Add support for wavpack audio compression tools | 
| [wayland](https://packages.gentoo.org/useflags/wayland) | Enable dev-libs/wayland backend | 

### Emerge

Install [media-sound/audacious](https://packages.gentoo.org/packages/media-sound/audacious) and [media-plugins/audacious-plugins](https://packages.gentoo.org/packages/media-plugins/audacious-plugins):

`root #``emerge --ask media-sound/audacious media-plugins/audacious-plugins`
## Usage

- To put Audacious in classic Winamp mode, go to *View > Interface > Winamp Classic Interface*.
- To insert classical Winamp EQ presets:

`user $``wget -O $HOME/.config/audacious/eq.preset https://gist.github.com/666threesixes666/6017524/raw/6f92831829453dd659f063299ba1bdf775a893ac/wapresets`
## Troubleshooting

### PipeWire

When running Audacious with PipeWire, you may experience the following error:

`user $``audacious`
ERROR ../src/pipewire/pipewire.cc:341 \[init\_core\]: PipeWireOutput: unable to initialize loop

Switching to PulseAudio or ALSA in settings should resolve this.

### Context Menu opens wrong application

When right-clicking a song from the playlist, "Open Containing Folder" does not respect the preferred file manager. This can be fixed by changing the default application in the desktop environment (e. g. [Xfce](https://wiki.gentoo.org/wiki/Xfce)). The corresponding MIME-Type is called **inode/directory**.

### No option shown for CD playback

If [CDROM](https://wiki.gentoo.org/wiki/CDROM) is configured correctly, "Play CD" should appear beneath the "Services" button. If not, the USE-Flag setting for *media-plugins/audacious-plugins*:<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> may be misconfigured.



**`/etc/portage/package.use`**

**Enabling cdda locally**

```
 cdda
```
