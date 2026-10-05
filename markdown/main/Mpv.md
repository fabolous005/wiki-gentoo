<!-- source: https://wiki.gentoo.org/wiki/Mpv | group: Gentoo Wiki (Main) | wiki-title: Mpv -->
---
title: mpv
url: https://wiki.gentoo.org/wiki/Mpv
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-03-19"
fingerprint: ee00085c408a39d8
license: CC BY-SA 4.0
---

# mpv

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


mpv is a free and open source command-line media player. It is based on mplayer2, which in turn is based on the original [MPlayer](https://wiki.gentoo.org/wiki/MPlayer). Although there are still many similarities to its ancestors, mpv should generally be treated as a completely different program.

## Installation

### USE flags

Default USE configuration provides the following core features: CLI player, [Xorg](https://wiki.gentoo.org/wiki/Xorg) support, audio and video playback, On-Screen Display (OSD) and On-Screen Controller (OSC), and [Lua](https://wiki.gentoo.org/wiki/Lua) scripting interface.

Hardware video decoding is usually desired. mpv supports both [VAAPI](https://wiki.gentoo.org/wiki/VAAPI) and [VDPAU](https://wiki.gentoo.org/wiki/VDPAU) hardware decoding APIs via `vaapi` and `vdpau` USE flags respectively; manually enable the API available on your system. mpv also supports NVDEC hardware decoding API via `nvenc` USE flag (requires [proprietary nvidia-drivers](https://wiki.gentoo.org/wiki/NVIDIA/nvidia-drivers)).

For a complete set of features please refer to the list of USE flags below.


| [+X](https://packages.gentoo.org/useflags/+X) | Add support for X11 | 
| [+alsa](https://packages.gentoo.org/useflags/+alsa) | Add support for media-libs/alsa-lib (Advanced Linux Sound Architecture) | 
| [+cli](https://packages.gentoo.org/useflags/+cli) | Enable the command-line player | 
| [+curl](https://packages.gentoo.org/useflags/+curl) | Add support for client-side URL transfer library | 
| [+drm](https://packages.gentoo.org/useflags/+drm) | Enable Kernel Mode Setting / Direct Rendering Manager based video outputs | 
| [+egl](https://packages.gentoo.org/useflags/+egl) | Enable EGL (Embedded-System Graphics Library, interfacing between windowing system and OpenGL/GLES) support | 
| [+iconv](https://packages.gentoo.org/useflags/+iconv) | Enable support for the iconv character set conversion library | 
| [+libmpv](https://packages.gentoo.org/useflags/+libmpv) | Enable the shared library and headers (used by frontends / plugins) | 
| [+lua](https://packages.gentoo.org/useflags/+lua) | Enable Lua scripting, OSC (On Screen Controller) GUI, and net-misc/yt-dlp support | 
| [+uchardet](https://packages.gentoo.org/useflags/+uchardet) | Enable subtitles charset discovery via app-i18n/uchardet | 
| [+vulkan](https://packages.gentoo.org/useflags/+vulkan) | Add support for 3D graphics and computing via the Vulkan cross-platform API | 
| [aqua](https://packages.gentoo.org/useflags/aqua) | Include support for the Mac OS X Aqua (Carbon/Cocoa) GUI | 
| [archive](https://packages.gentoo.org/useflags/archive) | Enable support for various archive formats via app-arch/libarchive | 
| [bluray](https://packages.gentoo.org/useflags/bluray) | Enable playback of Blu-ray filesystems | 
| [cdda](https://packages.gentoo.org/useflags/cdda) | Add Compact Disk Digital Audio (Standard Audio CD) support | 
| [coreaudio](https://packages.gentoo.org/useflags/coreaudio) | Build the CoreAudio driver on Mac OS X systems | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [dvb](https://packages.gentoo.org/useflags/dvb) | Add support for DVB (Digital Video Broadcasting) | 
| [dvd](https://packages.gentoo.org/useflags/dvd) | Add support for DVDs | 
| [gamepad](https://packages.gentoo.org/useflags/gamepad) | Enable gamepad input support | 
| [jack](https://packages.gentoo.org/useflags/jack) | Add support for the JACK Audio Connection Kit | 
| [javascript](https://packages.gentoo.org/useflags/javascript) | Enable javascript support | 
| [jpeg](https://packages.gentoo.org/useflags/jpeg) | Add JPEG image support | 
| [lcms](https://packages.gentoo.org/useflags/lcms) | Add lcms support (color management engine) | 
| [libcaca](https://packages.gentoo.org/useflags/libcaca) | Add support for colored ASCII-art graphics | 
| [nvenc](https://packages.gentoo.org/useflags/nvenc) | Add support for NVIDIA Encoder/Decoder (NVENC/NVDEC) API for hardware accelerated encoding and decoding on NVIDIA cards (requires x11-drivers/nvidia-drivers) | 
| [openal](https://packages.gentoo.org/useflags/openal) | Add support for the Open Audio Library | 
| [pipewire](https://packages.gentoo.org/useflags/pipewire) | Enable sound support via native PipeWire backend | 
| [pulseaudio](https://packages.gentoo.org/useflags/pulseaudio) | Add sound server support via media-libs/libpulse (may be PulseAudio or PipeWire) | 
| [rubberband](https://packages.gentoo.org/useflags/rubberband) | Enable high quality pitch correction via media-libs/rubberband | 
| [sdl](https://packages.gentoo.org/useflags/sdl) | Enable media-libs/libsdl2 based video and audio outputs (Note: these outputs exist for compatibility reasons only, avoid if possible) | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [sixel](https://packages.gentoo.org/useflags/sixel) | Enable support for the sixel video backend using media-libs/libsixel | 
| [sndio](https://packages.gentoo.org/useflags/sndio) | Enable sound support via media-sound/sndio | 
| [soc](https://packages.gentoo.org/useflags/soc) | Use additional media-video/ffmpeg patches for efficient playback on some SoCs (e.g. ARM, RISC-V) | 
| [subrandr](https://packages.gentoo.org/useflags/subrandr) | Enable support for SRV3 and WebVTT subtitle formats using media-libs/subrandr | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [tools](https://packages.gentoo.org/useflags/tools) | Install extra tools: mpv\_identify.sh, mpv\_idet.sh, and umpv | 
| [vaapi](https://packages.gentoo.org/useflags/vaapi) | Enable Video Acceleration API for hardware decoding | 
| [vdpau](https://packages.gentoo.org/useflags/vdpau) | Enable the Video Decode and Presentation API for Unix acceleration interface | 
| [wayland](https://packages.gentoo.org/useflags/wayland) | Enable dev-libs/wayland backend | 
| [xv](https://packages.gentoo.org/useflags/xv) | Add in optional support for the Xvideo extension (an X API for video playback) | 
| [zimg](https://packages.gentoo.org/useflags/zimg) | Enable libzimg support (for vf\_fingerprint) | 
| [zlib](https://packages.gentoo.org/useflags/zlib) | Add support for zlib compression | 

### Emerge

`root #``emerge --ask media-video/mpv`
### Additional software

Install [net-misc/yt-dlp](https://packages.gentoo.org/packages/net-misc/yt-dlp) to directly play URLs from YouTube and other popular streaming websites:

`root #``emerge --ask net-misc/yt-dlp`
Install [media-video/celluloid](https://packages.gentoo.org/packages/media-video/celluloid) to have a [GTK](https://wiki.gentoo.org/wiki/GTK) GUI front-end:

`root #``emerge --ask media-video/celluloid`
## Configuration

mpv doesn't normally require any configuration. However, many aspects of the default behavior can be changed. Two most important parts of the configuration are player settings and key bindings. Both are briefly discussed below.

### Player settings

Player settings should be put into \~/.config/mpv/mpv.conf file using the syntax `option=value`.
Everything after the `#` is considered a comment.

Nearly all command line options can be player settings. In most cases the equivalent of `--option=value` command line argument is `option=value` setting. Options that work without values can be enabled by setting them to `yes` and disabled by setting them to `no`.

To ease working with different configurations, profiles can be defined in the configuration file. A profile starts with its name in square brackets, e.g. `[my-profile]`. All following options will be a part of this profile. To end the profile, start another one or use the profile name `default` to continue with normal options.

For a complete list of available options please refer to [the manual](http://mpv.io/manual/stable/#options).

**`~/.config/mpv/mpv.conf`**

**Player settings example**

```
# Always allow seeking, e.g. allow seeking within a local cache of HTTP stream
force-seekable=yes
# Always open a video window even with no video
force-window=yes
# Don't exit when the end of playlist is reached
keep-open=yes
# Always save the current playback position on exit
save-position-on-quit=yes
 
# Create 'high-quality' profile
[high-quality]
# Describe this profile
profile-desc="High quality rendering"
# Include all settings from the default 'opengl-hq' profile
profile=opengl-hq
# Disable debanding for better performance
deband=no
```
### Key bindings

Key bindings should be put into \~/.config/mpv/input.conf file using the syntax `key command`.
Everything after the `#` is considered a comment.

For a complete list of available commands please refer to [the manual](https://mpv.io/manual/stable/#list-of-input-commands).

**`~/.config/mpv/input.conf`**

**Key bindings example**

```
# Ctrl+q closes player
Ctrl+q quit
# D removes the current file from playlist
D playlist-remove current
# R shuffles the current playlist
R playlist-shuffle
```
### Examples

Advanced real-world configurations from mpv contributors: [\[1\]](https://github.com/pigoz/dotfiles/tree/master/mpv), [\[2\]](https://github.com/Argon-/mpv-config).

## Usage

Core mpv functionality can be extended with [Lua](https://wiki.gentoo.org/wiki/Lua) or [JavaScript](https://en.wikipedia.org/wiki/JavaScript) scripts or [C](<https://en.wikipedia.org/wiki/C_(programming_language)>) plugins.

All scripts and plugins from \~/.config/mpv/scripts/ directory are loaded automatically. Alternatively a script or plugin can be loaded manually via the command line, e.g. `--script=/path/to/script.file`.

Several Lua scripts are shipped with mpv and installed into /usr/share/mpv/lua/ directory. Upstream wiki also has [an extensive list of 3rd party scripts and plugins](https://github.com/mpv-player/mpv/wiki/User-Scripts).

mpv can be run on a directory.

`user $``cd ~/Music``user $``mpv --loop-playlist --shuffle --replaygain=track .`
Playing: ./IMANU and Kučka, It's Our Destiny.opus
 ● Audio  --aid=1  --alang=eng  (opus 2ch 48000 Hz)
AO: \[pipewire\] 48000Hz stereo 2ch floatp
A: 00:00:00 / 00:02:33 (0%)

Options and navigation keys are listed in the [browser reference](https://mpv.io/manual/master/) or [mpv(1)](https://man.archlinux.org/man/mpv.1.en)[.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

### Multiple monitors

If using a multi-monitor setup, the output to use can be specified via the `--drm-connector` option, e.g. `--drm-connector=O.DP-1` or, if using [Vulkan](https://wiki.gentoo.org/wiki/Vulkan), via the `--vulkan-display-display` option, e.g. `--vulkan-display-display=2`. Use `--drm-connector=help` to get a list of available connectors, and `--vulkan-display-display=help` to get a list of accessible monitors, e.g.:

`user $``mpv --vulkan-display-display=help`
## Troubleshooting

FFmpeg/Libav/libass are all known to, from time to time, introduce incompatible, ABI-breaking changes between minor releases. Usually SLOTs of these libraries remain the same between minor releases, i.e. an automatic mpv rebuild isn't triggered. This causes problems with mpv compiled against previous versions of these libraries. These problems vary from broken pieces of functionality to sudden crashes.

Upstream's answer in such cases is similar to "it's not our problem that others can't maintain proper ABI-compatibility, rebuild mpv". Please first report a [Gentoo bug](https://wiki.gentoo.org/wiki/Bugzilla/Bug_report_guide) to notify maintainers about the existing breakage, then rebuild mpv.

### "mpv was compiled against a different version of FFmpeg..." message

In the absence of any other problems with mpv, this message can be ignored. Otherwise, please follow [these instructions](https://wiki.gentoo.org/wiki/Mpv#Broken_playback_after_updating).

### Broken hardware video decoding/high CPU usage

Always consult the mpv log for any problems regarding hardware decoding. High CPU usage during video playback is a symptom of broken hardware decoding.

Firstly, make sure that the hardware supports the required video codec. The list of codecs supported by the GPU in use is available via the vainfo and vdpauinfo commands for the [VAAPI](https://wiki.gentoo.org/wiki/VAAPI) and [VDPAU](https://wiki.gentoo.org/wiki/VDPAU) decoding APIs, respectively. The codec used by a video file is available in the mpv log. If the GPU supports the required codec, but mpv doesn't do hardware decoding, try using the `--hwdec-codecs=all` option.

Secondly, make sure to use the latest non-live (not 9999) mpv version available. If updating to the latest version solves the problem, please report a Gentoo bug.

There are then several other things to try:

- Enable hardware decoder with copy-back via `--hwdec=auto-copy`.
- VAAPI only: use VAAPI output via `--vo=vaapi` with `--hwdec=vaapi` or `--hwdec=vaapi-copy`.
- VDPAU only: use VDPAU output via `--vo=vdpau` with `--hwdec=vdpau` or `--hwdec=vdpau-copy`.
- VDPAU only: use X11/GLX backend for OpenGL output via `--opengl-backend=x11`.
- Intel GPUs only: use the modesetting Xorg driver from [x11-base/xorg-server](https://packages.gentoo.org/packages/x11-base/xorg-server).
- Enable dumb-mode for OpenGL output via `--opengl-dumb-mode=yes`.
- Use Xv output via `--vo=xv` with `--hwdec=auto-copy`.

### Tearing

[Tearing](https://en.wikipedia.org/wiki/Screen_tearing) is a video playback defect when displayed objects aren't properly lined up horizontally.
Please refer to [the upstream FAQ](https://github.com/mpv-player/mpv/wiki/FAQ#Tearing).

Quote from upstream: "Libav is basically unsupported by mpv, except it still sort of compiles and many things work" [\[3\]](https://github.com/mpv-player/mpv/issues/3923#issuecomment-268120303). Patches are welcome for any issues regarding Libav.

## Update notes

Upstream maintains [a list of user-visible changes](https://github.com/mpv-player/mpv/blob/master/DOCS/interface-changes.rst).

## See also

- [MPlayer](https://wiki.gentoo.org/wiki/MPlayer) — a powerful command-line media player
- [VLC](https://wiki.gentoo.org/wiki/VLC) — a wildly popular, cross platform video player and streamer.
