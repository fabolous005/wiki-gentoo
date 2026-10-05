<!-- source: https://wiki.gentoo.org/wiki/V4l-utils | group: Gentoo Wiki (Main) | wiki-title: V4l-utils -->
---
title: v4l-utils
url: https://wiki.gentoo.org/wiki/V4l-utils
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-10"
fingerprint: "2681680c928a3bd4"
license: CC BY-SA 4.0
---

# v4l-utils

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**v4l-utils** is a set of utilities for handling media devices contained in the Video4Linux library package which can be installed by enabling the [utils](https://packages.gentoo.org/useflags/utils) [USE flag](https://wiki.gentoo.org/wiki/USE_flag).

## Installation

### USE flags


| [+utils](https://packages.gentoo.org/useflags/+utils) | Build the v4l-utils collection of utilities | 
| [bpf](https://packages.gentoo.org/useflags/bpf) | Enable support for IR BPF decoders | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [dvb](https://packages.gentoo.org/useflags/dvb) | Add support for DVB (Digital Video Broadcasting) | 
| [jpeg](https://packages.gentoo.org/useflags/jpeg) | Add JPEG image support | 
| [qt6](https://packages.gentoo.org/useflags/qt6) | Add support for the Qt 6 application and UI framework | 
| [tracer](https://packages.gentoo.org/useflags/tracer) | Build the v4l2-tracer tool and library | 

### Emerge

Install [media-libs/libv4l](https://packages.gentoo.org/packages/media-libs/libv4l):

`root #``emerge --ask media-libs/libv4l`
### Additional packages

#### gtk-v4l

The GTK+ package [media-tv/gtk-v4l](https://packages.gentoo.org/packages/media-tv/gtk-v4l) is available for controlling webcam v4l preferences.


#### v4l-dvb-saa716x

A driver for saa716x-based dvb cards is available at [media-tv/v4l-dvb-saa716x](https://packages.gentoo.org/packages/media-tv/v4l-dvb-saa716x).


| [+strip](https://packages.gentoo.org/useflags/+strip) | Allow symbol stripping to be performed by the ebuild for special files | 
| [dist-kernel](https://packages.gentoo.org/useflags/dist-kernel) | Enable subslot rebuilds on Distribution Kernel upgrades | 
| [modules-compress](https://packages.gentoo.org/useflags/modules-compress) | Install compressed kernel modules (if kernel config enables module compression) | 
| [modules-sign](https://packages.gentoo.org/useflags/modules-sign) | Cryptographically sign installed kernel modules (requires CONFIG\_MODULE\_SIG=y in the kernel) | 

#### v4l2loopback

[media-video/v4l2loopback](https://packages.gentoo.org/packages/media-video/v4l2loopback) provides a v4l2 loopback device whose output is its own input.


| [+strip](https://packages.gentoo.org/useflags/+strip) | Allow symbol stripping to be performed by the ebuild for special files | 
| [dist-kernel](https://packages.gentoo.org/useflags/dist-kernel) | Enable subslot rebuilds on Distribution Kernel upgrades | 
| [examples](https://packages.gentoo.org/useflags/examples) | Install examples, usually source code | 
| [modules-compress](https://packages.gentoo.org/useflags/modules-compress) | Install compressed kernel modules (if kernel config enables module compression) | 
| [modules-sign](https://packages.gentoo.org/useflags/modules-sign) | Cryptographically sign installed kernel modules (requires CONFIG\_MODULE\_SIG=y in the kernel) | 

#### gst-plugins-v4l2

Finally, a gstreamer plugin package is available: [media-plugins/gst-plugins-v4l2](https://packages.gentoo.org/packages/media-plugins/gst-plugins-v4l2).


## Usage

### Invocation

`user $``v4l2-ctl --help````
General/Common options:
  --all              display all information available
  -C, --get-ctrl <ctrl>[,<ctrl>...]
                     get the value of the controls [VIDIOC_G_EXT_CTRLS]
  -c, --set-ctrl <ctrl>=<val>[,<ctrl>=<val>...]
                     set the value of the controls [VIDIOC_S_EXT_CTRLS]
  -D, --info         show driver info [VIDIOC_QUERYCAP]
  -d, --device <dev> use device <dev> instead of /dev/video0
                     if <dev> starts with a digit, then /dev/video<dev> is used
                     Otherwise if -z was specified earlier, then <dev> is the entity name
                     or interface ID (if prefixed with 0x) as found in the topology of the
                     media device with the bus info string as specified by the -z option.
  -e, --out-device <dev> use device <dev> for output streams instead of the
                     default device as set with --device
                     if <dev> starts with a digit, then /dev/video<dev> is used
                     Otherwise if -z was specified earlier, then <dev> is the entity name
                     or interface ID (if prefixed with 0x) as found in the topology of the
                     media device with the bus info string as specified by the -z option.
  -E, --export-device <dev> use device <dev> for exporting DMA buffers
                     if <dev> starts with a digit, then /dev/video<dev> is used
                     Otherwise if -z was specified earlier, then <dev> is the entity name
                     or interface ID (if prefixed with 0x) as found in the topology of the
                     media device with the bus info string as specified by the -z option.
  -z, --media-bus-info <bus-info>
                     find the media device with the given bus info string. If set, then
                     -d, -e and -E options can use the entity name or interface ID to refer
                     to the device nodes.
  -h, --help         display this help message
  --help-all         all options
  --help-io          input/output options
  --help-meta        metadata format options
  --help-misc        miscellaneous options
  --help-overlay     overlay format options
  --help-sdr         SDR format options
  --help-selection   crop/selection options
  --help-stds        standards and other video timings options
  --help-streaming   streaming options
  --help-subdev      sub-device options
  --help-tuner       tuner/modulator options
  --help-vbi         VBI format options
  --help-vidcap      video capture format options
  --help-vidout      vidout output format options
  --help-edid        edid handling options
  -k, --concise      be more concise if possible.
  -l, --list-ctrls   display all controls and their values [VIDIOC_QUERYCTRL]
  -L, --list-ctrls-menus
		    display all controls and their menus [VIDIOC_QUERYMENU]
  -r, --subset <ctrl>[,<offset>,<size>]+
                     the subset of the N-dimensional array to get/set for control <ctrl>,
                     for every dimension an (<offset>, <size>) tuple is given.
  -w, --wrapper      use the libv4l2 wrapper library.
  --list-devices     list all v4l devices. If -z was given, then list just the
                     devices of the media device with the bus info string as
                     specified by the -z option.
  --log-status       log the board status in the kernel log [VIDIOC_LOG_STATUS]
  --get-priority     query the current access priority [VIDIOC_G_PRIORITY]
  --set-priority <prio>
                     set the new access priority [VIDIOC_S_PRIORITY]
                     <prio> is 1 (background), 2 (interactive) or 3 (record)
  --silent           only set the result code, do not print any messages
  --sleep <secs>     sleep <secs>, call QUERYCAP and close the file handle
  --verbose          turn on verbose ioctl status reporting
```
### libcamera

media-libs/libcamera is available in Gentoo repository.

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose media-libs/libv4l`
## See also

- [darktable](https://wiki.gentoo.org/wiki/Darktable) — a photography workflow application and [RAW](https://en.wikipedia.org/wiki/Raw_image_format) developer.
- [Droidcam](https://wiki.gentoo.org/wiki/Droidcam) — a tool to use a smartphone's cameras as webcam on a computer
- [Ffmpeg](https://wiki.gentoo.org/wiki/Ffmpeg) — a cross platform, free, open source media encoder/decoder toolkit.
- [gPhoto](https://wiki.gentoo.org/wiki/GPhoto)
- [Jellyfin](https://wiki.gentoo.org/wiki/Jellyfin) — installation and management of the **jellyfin** media server
- [Kodi](https://wiki.gentoo.org/wiki/Kodi) — an open source home theater application.
- [Motion](https://wiki.gentoo.org/wiki/Motion)
- [MythTV](https://wiki.gentoo.org/wiki/MythTV) — a powerful media center and video recording software system.
- [OBS Studio](https://wiki.gentoo.org/wiki/OBS_Studio) — free software for video recording and live streaming.
- [OpenShot](https://wiki.gentoo.org/wiki/OpenShot) — an open source, cross-platform, video editor, written in Python and C++
- [Streamlink](https://wiki.gentoo.org/wiki/Streamlink) — a command-line utility that can extract video streams and pipe them into a video player.
- [TV Tuner](https://wiki.gentoo.org/wiki/TV_Tuner) — configuring and using **television (TV) tuners** with Gentoo Linux
- [Libcamera](https://wiki.gentoo.org/wiki/Libcamera) — an open-source software library for image signal processors and embedded cameras.

## External resources

- [Part I - Video for Linux API](https://www.kernel.org/doc/html/latest/userspace-api/media/v4l/v4l2.html) - the V4L2 API kernel specification.
- [Video for Linux version 2 (V4L2) examples](https://github.com/kmdouglass/v4l2-examples)
