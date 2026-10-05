<!-- source: https://wiki.gentoo.org/wiki/OBS_Studio | group: Gentoo Wiki (Main) | wiki-title: OBS Studio -->
---
title: OBS Studio
url: https://wiki.gentoo.org/wiki/OBS_Studio
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-26"
fingerprint: fe128d5449b4b995
license: CC BY-SA 4.0
---

# OBS Studio

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**OBS Studio** is free software for video recording and live streaming.

Built with [Qt](https://wiki.gentoo.org/wiki/Qt), [C](https://wiki.gentoo.org/wiki/C), and [C++](https://wiki.gentoo.org/wiki/C%2B%2B), and maintained by the OBS Project, the software provides real-time device capture, scene composition, recording, broadcasting and source capture functions with presets for streaming to popular services such as YouTube, Twitch, Instagram and Facebook<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.

In 2014, development started on a rewrite of the software, known as OBS Multiplatform, which included a larger feature set, multi-platform support and broader plugin support<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>. As of 2016, the software was rebranded as OBS Studio, with the older OBS Classic being deprecated.[\[3\]](https://wiki.gentoo.org#cite_note-3)

## Installation

### USE flags


| [+alsa](https://packages.gentoo.org/useflags/+alsa) | Add support for media-libs/alsa-lib (Advanced Linux Sound Architecture) | 
| [browser](https://packages.gentoo.org/useflags/browser) | Enable browser source support via (precompiled) CEF. | 
| [decklink](https://packages.gentoo.org/useflags/decklink) | Build the Decklink plugin. | 
| [fdk](https://packages.gentoo.org/useflags/fdk) | Build with LibFDK AAC support. | 
| [jack](https://packages.gentoo.org/useflags/jack) | Add support for the JACK Audio Connection Kit | 
| [lua](https://packages.gentoo.org/useflags/lua) | Enable Lua scripting support | 
| [mpegts](https://packages.gentoo.org/useflags/mpegts) | Enable native SRT/RIST mpegts output. | 
| [nvenc](https://packages.gentoo.org/useflags/nvenc) | Add support for NVIDIA Encoder/Decoder (NVENC/NVDEC) API for hardware accelerated encoding and decoding on NVIDIA cards (requires x11-drivers/nvidia-drivers) | 
| [pulseaudio](https://packages.gentoo.org/useflags/pulseaudio) | Add sound server support via media-libs/libpulse (may be PulseAudio or PipeWire) | 
| [python](https://packages.gentoo.org/useflags/python) | Build with scripting support for Python 3. | 
| [qsv](https://packages.gentoo.org/useflags/qsv) | Build with Intel Quick Sync Video support. | 
| [screencast](https://packages.gentoo.org/useflags/screencast) | Build with screen/video device capture support via PipeWire. | 
| [sndio](https://packages.gentoo.org/useflags/sndio) | Build with sndio support. | 
| [speex](https://packages.gentoo.org/useflags/speex) | Build with Speex noise suppression filter support. | 
| [test-input](https://packages.gentoo.org/useflags/test-input) | Build and install input sources used for testing. | 
| [truetype](https://packages.gentoo.org/useflags/truetype) | Add support for FreeType and/or FreeType2 fonts | 
| [v4l](https://packages.gentoo.org/useflags/v4l) | Enable support for video4linux (using linux-headers or userspace libv4l libraries) | 
| [vlc](https://packages.gentoo.org/useflags/vlc) | Build with VLC media source support. | 
| [wayland](https://packages.gentoo.org/useflags/wayland) | Enable dev-libs/wayland backend | 
| [websocket](https://packages.gentoo.org/useflags/websocket) | Build with WebSocket API support. | 

As an example, for a streaming setup with an [NVIDIA](https://wiki.gentoo.org/wiki/NVIDIA) graphics card, a [webcam](https://wiki.gentoo.org/wiki/Webcam), and [PulseAudio](https://wiki.gentoo.org/wiki/PulseAudio), which integrates with major streaming services, the following should be added to /etc/portage/package.use:

**`/etc/portage/package.use/obs-studio`**

### Emerge

To use the **testing** version of OBS Studio, specify it in an [ACCEPT\_KEYWORDS](https://wiki.gentoo.org/wiki/ACCEPT_KEYWORDS) file:

**`/etc/portage/package.accept_keywords/obs-studio`**

**obs-studio**

To install [media-video/obs-studio](https://packages.gentoo.org/packages/media-video/obs-studio):

`root #``emerge --ask media-video/obs-studio`
### Additional software

#### Audio

OBS Studio can be paired with [JACK](https://wiki.gentoo.org/wiki/JACK), [PulseAudio](https://wiki.gentoo.org/wiki/PulseAudio), or [PipeWire](https://wiki.gentoo.org/wiki/PipeWire) for audio.

#### VLC

OBS Studio supports integration with services provided by the [VLC](https://wiki.gentoo.org/wiki/VLC) media player. VLC support behaves much like the ordinary media source; however, it also accepts a list of files to play, and provides a way to play every path, URL or media source supported by VLC.

## Usage

### Invocation

OBS Studio can be invoked from the command line as follows:

`user $``obs --help`
--help, -h: Get list of available commands.
 
--startstreaming: Automatically start streaming.
--startrecording: Automatically start recording.
--startreplaybuffer: Start replay buffer.
--startvirtualcam: Start virtual camera (if available).
 
--collection \<string>: Use specific scene collection.
--profile \<string>: Use specific profile.
--scene \<string>: Start with specific scene.
 
--studio-mode: Enable studio mode.
--minimize-to-tray: Minimize to system tray.
--portable, -p: Use portable mode.
--multi, -m: Don't warn when launching multiple instances.
 
--verbose: Make log more verbose.
--always-on-top: Start in 'always on top' mode.
 
--unfiltered\_log: Make log unfiltered.
 
--disable-updater: Disable built-in updater (Windows/Mac only)
 
--disable-high-dpi-scaling: Disable automatic high-DPI scaling
 
--version, -V: Get current version.

## Removal

### Unmerge

To remove [media-video/obs-studio](https://packages.gentoo.org/packages/media-video/obs-studio):

`root #``emerge --ask --depclean --verbose media-video/obs-studio`
## Troubleshooting

### PipeWire audio

For PipeWire audio capture support , install [the "PipeWire Audio Capture" plugin](https://obsproject.com/forum/resources/pipewire-audio-capture.1458/).

### Wayland

#### Shortcuts

**Todo:**

- Is this still accurate?

Some Wayland [desktop environments](https://wiki.gentoo.org/wiki/Desktop_environment) (e.g. KDE Plasma) don't have good support for shortcuts with Wayland.

If OBS Studio has been built with the [websocket](https://packages.gentoo.org/useflags/websocket) [USE flag enabled, it can be controlled by sending commands to its WebSocket interface.](https://wiki.gentoo.org/wiki/USE_flag)

Check [this project](https://github.com/wordhater/obs-wayland-shortcuts-kde) for details.

#### Video capturing

To capture windows or the full screen when using PipeWire with a Wayland compositor, enable the [pipewire](https://packages.gentoo.org/useflags/pipewire) [USE flag on](https://wiki.gentoo.org/wiki/USE_flag) [media-video/obs-studio](https://packages.gentoo.org/packages/media-video/obs-studio), and the [dbus](https://packages.gentoo.org/useflags/dbus) [USE flag on](https://wiki.gentoo.org/wiki/USE_flag) [media-video/pipewire](https://packages.gentoo.org/packages/media-video/pipewire):

**`/etc/portage/package.use`**

Re-emerge these packages to apply the USE flag changes:

`root #``emerge --ask media-video/obs-studio``root #``emerge --ask --oneshot media-video/pipewire`
Additionally, an appropriate xdg-desktop-portal should be installed; refer to the [XDG/xdg-desktop-portal](https://wiki.gentoo.org/wiki/XDG/xdg-desktop-portal) for further information.

Adding the [screencast](https://packages.gentoo.org/useflags/screencast)[,](https://wiki.gentoo.org/wiki/USE_flag) [gstreamer](https://packages.gentoo.org/useflags/gstreamer)[, and](https://wiki.gentoo.org/wiki/USE_flag) [gles2](https://packages.gentoo.org/useflags/gles2) [global USE flags to](https://wiki.gentoo.org/wiki/USE_flag) [/etc/portage/make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf) might be required for some desktop environments.

Finally, the [dev-qt/qtwebkit](https://packages.gentoo.org/packages/dev-qt/qtwebkit) package might also be required:

`root #``emerge --ask dev-qt/qtwebkit`
### Virtual camera

For virtual camera support within OBS Studio, emerge the v4l2loopback kernel module:

`root #``emerge --ask media-video/v4l2loopback`
**Todo:**

- Surely running OBS Studio as root should be avoided? If so, concrete information about a non-root-based configuration should be described here.

For OBS Studio to initialize the kernel module, give it the appropriate permissions. This can be done by running it as root or using a [polkit](https://wiki.gentoo.org/wiki/Polkit) agent.

## External Resources

- [OBS Tutorials](https://obstutorials.com/) - Tips and tricks for OBS Studio.
- [OBS Project](https://obsproject.com/) - The OBS Project main site.
- [OBS Documentation](https://obsproject.com/docs/) - Detailed information for developers and users alike.
