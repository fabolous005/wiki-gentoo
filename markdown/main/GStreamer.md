<!-- source: https://wiki.gentoo.org/wiki/GStreamer | group: Gentoo Wiki (Main) | wiki-title: GStreamer -->
---
title: GStreamer
url: https://wiki.gentoo.org/wiki/GStreamer
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-13"
fingerprint: "4e038954c98639cc"
license: CC BY-SA 4.0
---

# GStreamer

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**GStreamer** is a library for constructing graphs of media-handling components.


## Installation


### USE flags


### USE flags for
            [media-libs/gstreamer](https://packages.gentoo.org/packages/media-libs/gstreamer)
            
            Open source multimedia framework

| [+caps](https://packages.gentoo.org/useflags/+caps) | Use Linux capabilities library to control privilege | 
| [+introspection](https://packages.gentoo.org/useflags/+introspection) | Add support for GObject based introspection | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [ptp](https://packages.gentoo.org/useflags/ptp) | Controls Precision Time Protocol (PTP) helper. Written in Rust. | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [unwind](https://packages.gentoo.org/useflags/unwind) | Enable sys-libs/libunwind usage for better backtrace support in leaks tracer module | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 


### Emerge

`root #``emerge --ask media-libs/gstreamer`
### Plugins

A number of plugins are available for GStreamer. The Gentoo repository provides a selection in the `media-plugins` category, where the name of each plugin package has the form `gst-plugins-<qualifier>`.

However, rather than installing plugins individually, users can instead install the [media-plugins/gst-plugins-meta](https://packages.gentoo.org/packages/media-plugins/gst-plugins-meta) package, which has the following USE flags:


### USE flags for
            [media-plugins/gst-plugins-meta](https://packages.gentoo.org/packages/media-plugins/gst-plugins-meta)
            
            Meta ebuild to pull in gst plugins for apps

| [X](https://packages.gentoo.org/useflags/X) | Add support for X11 | 
| [a52](https://packages.gentoo.org/useflags/a52) | Enable support for decoding ATSC A/52 streams used in DVD | 
| [aac](https://packages.gentoo.org/useflags/aac) | Enable support for MPEG-4 AAC Audio | 
| [alsa](https://packages.gentoo.org/useflags/alsa) | Add support for media-libs/alsa-lib (Advanced Linux Sound Architecture) | 
| [cdda](https://packages.gentoo.org/useflags/cdda) | Add Compact Disk Digital Audio (Standard Audio CD) support | 
| [dts](https://packages.gentoo.org/useflags/dts) | Enable DTS Coherent Acoustics decoder support | 
| [dv](https://packages.gentoo.org/useflags/dv) | Enable support for a codec used by many camcorders | 
| [dvb](https://packages.gentoo.org/useflags/dvb) | Add support for DVB (Digital Video Broadcasting) | 
| [dvd](https://packages.gentoo.org/useflags/dvd) | Add support for DVDs | 
| [ffmpeg](https://packages.gentoo.org/useflags/ffmpeg) | Enable ffmpeg/libav-based audio/video codec support | 
| [flac](https://packages.gentoo.org/useflags/flac) | Add support for FLAC: Free Lossless Audio Codec | 
| [http](https://packages.gentoo.org/useflags/http) | Enable http streaming via net-libs/libsoup | 
| [jack](https://packages.gentoo.org/useflags/jack) | Add support for the JACK Audio Connection Kit | 
| [lame](https://packages.gentoo.org/useflags/lame) | Add support for MP3 encoding using LAME | 
| [libass](https://packages.gentoo.org/useflags/libass) | SRT/SSA/ASS (SubRip / SubStation Alpha) subtitle support | 
| [libvisual](https://packages.gentoo.org/useflags/libvisual) | Enable visualization effects via media-libs/libvisual | 
| [modplug](https://packages.gentoo.org/useflags/modplug) | Add libmodplug support for playing SoundTracker-style music files | 
| [mp3](https://packages.gentoo.org/useflags/mp3) | Add support for reading mp3 files | 
| [mpeg](https://packages.gentoo.org/useflags/mpeg) | Add libmpeg3 support to various packages | 
| [ogg](https://packages.gentoo.org/useflags/ogg) | Add support for the Ogg container format (commonly used by Vorbis, Theora and flac) | 
| [opus](https://packages.gentoo.org/useflags/opus) | Enable Opus audio codec support | 
| [oss](https://packages.gentoo.org/useflags/oss) | Add support for OSS (Open Sound System) | 
| [pulseaudio](https://packages.gentoo.org/useflags/pulseaudio) | Add sound server support via media-libs/libpulse (may be PulseAudio or PipeWire) | 
| [taglib](https://packages.gentoo.org/useflags/taglib) | Enable tagging support with taglib | 
| [theora](https://packages.gentoo.org/useflags/theora) | Add support for the Theora Video Compression Codec | 
| [v4l](https://packages.gentoo.org/useflags/v4l) | Enable support for video4linux (using linux-headers or userspace libv4l libraries) | 
| [vaapi](https://packages.gentoo.org/useflags/vaapi) | Enable Video Acceleration API for hardware decoding | 
| [vcd](https://packages.gentoo.org/useflags/vcd) | Video CD support | 
| [vorbis](https://packages.gentoo.org/useflags/vorbis) | Add support for the OggVorbis audio codec | 
| [vpx](https://packages.gentoo.org/useflags/vpx) | Add support for VP8/VP9 codecs (usually via media-libs/libvpx) | 
| [wavpack](https://packages.gentoo.org/useflags/wavpack) | Add support for wavpack audio compression tools | 
| [x264](https://packages.gentoo.org/useflags/x264) | Enable H.264 encoding using x264 | 

## See also

- [PipeWire](https://wiki.gentoo.org/wiki/PipeWire) — low-latency, graph-based, processing engine and server, for interfacing with audio and video devices.
