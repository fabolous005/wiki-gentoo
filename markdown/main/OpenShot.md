<!-- source: https://wiki.gentoo.org/wiki/OpenShot | group: Gentoo Wiki (Main) | wiki-title: OpenShot -->
---
title: OpenShot
url: https://wiki.gentoo.org/wiki/OpenShot
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-20"
fingerprint: "26cf1974c12b5845"
license: CC BY-SA 4.0
---

# OpenShot

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is

**deprecated (obsolete)**. Contents are

<u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

**OpenShot** is an open source, cross-platform, video editor, written in Python and C++. It is dedicated to delivering high quality video editing and animation solutions.

## Installation

### USE flags

Note that OpenShot uses [FFmpeg](https://wiki.gentoo.org/wiki/FFmpeg) and [ALSA](https://wiki.gentoo.org/wiki/ALSA) for audio output when previewing a video. If using [PipeWire](https://wiki.gentoo.org/wiki/PipeWire) as the sound server, ensure that it has been built with the pipewire-alsa `USE` flag.

### Emerge

`root #``emerge --ask media-video/openshot`
### Overview

The OpenShot Video Editor consists of three main components:

- C++ video library ([media-libs/libopenshot](https://packages.gentoo.org/packages/media-libs/libopenshot))
- C++ audio library ([media-libs/libopenshot-audio](https://packages.gentoo.org/packages/media-libs/libopenshot-audio))
- Python and PyQt user interface video editor ([openshot-qt](https://github.com/OpenShot/openshot-qt))

OpenShot has many features, and covering them all would go beyond the scope of this wiki. Nonetheless, a brief summary of some important features is provided here, although certainly not exhaustive. For further, more detailed functions and instructions, end users should consult the [OpenShot User Guide](https://www.openshot.org/user-guide/) as a helpful resource.

#### System Requirements

The process of video editing can place high demands on computational resources and requires a substantial amount of processing power to complete efficiently. To efficiently process videos using OpenShot, the following system requirements are recommended:[\[1\]](https://wiki.gentoo.org#cite_note-:0-1)

|  | CPU Cores | Threads | Turbo Clock speed | RAM | Disk space | 
|---|---|---|---|---|---|
| minimum | 2 | 4 | 2.7 Ghz | 4 GB | 1 GB (install & useage) | 
| recommended | 6+ | 6+ | 3.4+ Ghz | 16+ GB | 50+ GB (incl. media) | 

Furthermore, the system should have a CPU with 64-bit support and a 64-bit operating system installed.

Optional components are:

- A [SSD](https://wiki.gentoo.org/wiki/SSD), if utilizing disk-caching the add of additional 10 GB hard-disk space are required.<sup>[\[1\]](https://wiki.gentoo.org#cite_note-:0-1)</sup>
- If enabling Hardware Acceleration (experimental), a GPU with the appropriate features listed in the Hardware Acceleration section should be used. Hardware Acceleration is optional and experimental.

### Video library libopenshot

The OpenShot video library [libopenshot](https://github.com/OpenShot/libopenshot) provides high quality video editing and animation with support for all [FFmpeg](https://wiki.gentoo.org/wiki/FFmpeg) formats and Codecs (video, audio and images). This allows the rendering of videos in many codecs and formats.

The video library supports, but is not limited to, the following points:

- Multi-Layer Compositing (the combining of multiple layers of visual elements into a single final image or video sequence)
- Image overlays and watermarks
- Audio Plug-ins and Drivers
- Additional Video and Audio effects, color adjustment, chroma key, etc. \[...\]
- Animation Curves
- Time Mapping, speed control features, time direction (reverse time etc.)
- Multi-Processor Support
- Frame accuracy, allows to step through the video frame by frame
- Digital video effects, including brightness, gamma, hue, greyscale and many more
- Clip manipulation (resizing, x,y-adjustment, alpha, scaling, trimming, snapping, rotation, cutting ... )
- Powerful curve-based Key frame animations, with an unlimited number of keyframes ( quadratic bezier curves, linear, or constant)
- Over 400 video transitions with real-time previews for fading

#### Hardware Acceleration

Experimental hardware acceleration is available for one or more GPUs, which can significantly speed up encoding and decoding if the hardware meets the corresponding power and feature requirements. However, hardware acceleration can also slow down performance if it is not set up properly, if the hardware does not have enough power, or if the necessary features are not supported by the hardware.

The supported features for hardware acceleration with OpenShot and instructions how to enable, disable and control them, can be found in the [Hardware Acceleration section of OpenShot](https://github.com/OpenShot/libopenshot/blob/develop/doc/HW-ACCEL.md). Some of the supported hardware acceleration features are [VAAPI](https://wiki.gentoo.org/wiki/VAAPI), [VDPAU](https://wiki.gentoo.org/wiki/VDPAU) and NVDEC. To enable hardware acceleration in general, the [Gentoo Hardware Acceleration Guide](https://wiki.gentoo.org/wiki/Xorg/Hardware_3D_acceleration_guide) provides detailed instructions for doing so.

#### API

A full C++ API to the libopenshot library is provided.<sup>[\[2\]](https://wiki.gentoo.org#cite_note-:1-2)</sup> The following examples are a rough excerpt from the official documentation.[\[2\]](https://wiki.gentoo.org#cite_note-:1-2)

To use the API the header file must be included:

The basic components of the API consist of four parts:[\[2\]](https://wiki.gentoo.org#cite_note-:1-2)

- **Readers** are used to read multimedia files, streams and return openshot::Frame objects.
- **Writers** are consuming openshot::Frame objects and are used to write / create video, audio, image files or streams.
- **Timeline** allows many openshot::Clip objects to be trimmed, arranged, and layered together.
- **Keyframes** are used to change values of properties over time on the timeline.

This example from the documentation shows, how to use a reader to access frames of a video file:[\[2\]](https://wiki.gentoo.org#cite_note-:1-2)

For further implementation details and use of the API, the [OpenShot documentation](https://openshot.org/files/libopenshot/) should be consulted.

### Audio library libopenshot-audio

The [libopenshot-audio](https://github.com/OpenShot/libopenshot-audio) provides high quality audio editing and playback. [\[3\]](https://wiki.gentoo.org#cite_note-:2-3)

Some features, according to the GitHub project page are:[\[3\]](https://wiki.gentoo.org#cite_note-:2-3)

- Audio Drivers (ASIO, WASAPI, DirectSound, CoreAudio, iPhone Audio, ALSA, JACK, and Android)
- Audio Plug-ins (VST & AU)
- Audio Mixing & Resampling
- Audio Playback

### openshot-qt

[openshot-qt](https://github.com/OpenShot/openshot-qt) provides several features, some of them are:[\[4\]](https://wiki.gentoo.org#cite_note-4)

- Unlimited tracks / layers
- Advanced timeline control (drag & drop, zoom, snapping, panning, scroling ... )
- 3D animated titles and effects, powered by [Blender](https://wiki.gentoo.org/wiki/Blender), with render properties
- Desktop integration (drag and drop support)
- Title templates, support of custom title creation for svg vector titles, sub-titles
- 2D animation support (image sequences)
- SVG friendly, to create and include vector titles and credits
- Scrolling motion picture credits
- Widely supported export & import formats

## Troubleshooting

Bugs and Issues to the corresponding parts of OpenShot can easily reported and looked up on their related GitHub issue pages.

For libopenshot the reports can be found [here](https://github.com/OpenShot/libopenshot/issues). For problems related with libopenshot-audio the reports can be found [here](https://github.com/OpenShot/libopenshot-audio/issues).

For problems not related to a specific library, but rather to OpenShot itself, the issue section can be found [here](https://github.com/OpenShot/openshot-qt/issues).
