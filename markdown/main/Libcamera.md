<!-- source: https://wiki.gentoo.org/wiki/Libcamera | group: Gentoo Wiki (Main) | wiki-title: Libcamera -->
---
title: Libcamera
url: https://wiki.gentoo.org/wiki/Libcamera
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-31"
fingerprint: "2c029d504b7431d4"
license: CC BY-SA 4.0
---

# Libcamera

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

**libcamera** is an open-source software library for image signal processors and embedded cameras.. The developers describe libcamera as a continuation of V4L2<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.

## Installation

### Kernel

The libcamera software ISP requires the operating system to provide access to either the DMA heap or udmabuf devices. [\[2\]](https://wiki.gentoo.org#cite_note-2)

**Enable support for DMA buffers>**

```
Device Drivers --->
  DMABUF options --->
    {*} DMA-BUF Userland Memory Heaps 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_DMABUF_HEAPS</code> to find this item.
### USE flags


| [+udev](https://packages.gentoo.org/useflags/+udev) | Enable virtual/udev integration (device discovery, power and storage device support, etc) | 
| [drm](https://packages.gentoo.org/useflags/drm) | Build with drm support for cam tool | 
| [elfutils](https://packages.gentoo.org/useflags/elfutils) | Build with improved debugging using dev-libs/elfutils | 
| [gstreamer](https://packages.gentoo.org/useflags/gstreamer) | Add support for media-libs/gstreamer (Streaming media) | 
| [gui](https://packages.gentoo.org/useflags/gui) | Build QCam extra tool | 
| [jpeg](https://packages.gentoo.org/useflags/jpeg) | Add JPEG image support | 
| [openssl](https://packages.gentoo.org/useflags/openssl) | Verify IPA modules signatures using dev-libs/openssl. Note:dev-libs/openssl is also required at build time to sign IPA modules. | 
| [sdl](https://packages.gentoo.org/useflags/sdl) | Build Cam extra tool with SDL sink support (requires USE=gui) | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [tiff](https://packages.gentoo.org/useflags/tiff) | Add support for the TIFF image format | 
| [tools](https://packages.gentoo.org/useflags/tools) | Build extra tools (including cam, qcam and lc-compliance). Note: 'qcam' requires also USE="gui" | 
| [trace](https://packages.gentoo.org/useflags/trace) | Build with tracing capabilities | 
| [unwind](https://packages.gentoo.org/useflags/unwind) | Build with improved debugging using sys-libs/libunwind (has no effect if USE=elfutils is set) | 
| [v4l](https://packages.gentoo.org/useflags/v4l) | Enable support for video4linux (using linux-headers or userspace libv4l libraries) | 

### Emerge

`root #``emerge --ask media-libs/libcamera`
### Additional software

Wireplumber offers support for capture devices using the libcamera stack via the Pipewire libcamera SPA (Single Plugin API) module. "libcamera" USE flag must be set for pipewire.

## Usage

### Invocation

`user $``cam --help````
Options:
  -c, --camera camera ...                               Specify which camera to operate on, by id or by index
  -h, --help                                            Display this help message
  -I, --info                                            Display information about stream(s)
  -l, --list                                            List all cameras
      --list-controls                                   List cameras controls
  -p, --list-properties                                 List cameras properties
  -m, --monitor                                         Monitor for hotplug and unplug camera events
Options valid in the context of --camera:
  -C, --capture[=count]                                 Capture until interrupted by user or until <count> frames captured
  -o, --orientation orientation                         Desired image orientation (rot0, rot180, mirror, flip)
  -D, --display[=connector]                             Display viewfinder through DRM/KMS on specified connector
  -F, --file[=filename]                                 Write captured frames to disk
                                                        If the file name ends with a '/', it sets the directory in which
                                                        to write files, using the default file name. Otherwise it sets the
                                                        full file path and name. The first '#' character in the file name
                                                        is expanded to the camera index, stream name and frame sequence number.
                                                        If the file name ends with '.dng', then the frame will be written to
                                                        the output file(s) in DNG format.
                                                        If the file name ends with '.ppm', then the frame will be written to
                                                        the output file(s) in PPM format.
                                                        The default file name is 'frame-#.bin'.
  -S, --sdl                                             Display viewfinder through SDL
  -s, --stream key=value[,key=value,...] ...            Set configuration of a camera stream
          colorspace=string                             Color space
          height=integer                                Height in pixels
          pixelformat=string                            Pixel format name
          role=string                                   Role for the stream (viewfinder, video, still, raw)
          width=integer                                 Width in pixels
      --strict-formats                                  Do not allow requested stream format(s) to be adjusted
      --metadata                                        Print the metadata for completed requests
      --script script                                   Load a capture session configuration script from a file
```
`user $``qcam --help````
Options:
  -c, --camera camera                                   Specify which camera to operate on
  -h, --help                                            Display this help message
  -r, --renderer renderer                               Choose the renderer type {qt,gles} (default: qt)
  -s, --stream key=value[,key=value,...] ...            Set configuration of a camera stream
          colorspace=string                             Color space
          height=integer                                Height in pixels
          pixelformat=string                            Pixel format name
          role=string                                   Role for the stream (viewfinder, video, still, raw)
          width=integer                                 Width in pixels
  -v, --verbose                                         Print verbose log messages
```
## Troubleshooting

### Failed to open /dev/udmabuf: Permission denied

This error indicates udmabuf special device provided by kernel is not accessible.

To fix this, add a custom udev rule:

**`/etc/udev/rules.d/99-uaccess.rules`**

**udmabuf udev rule**

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose media-libs/libcamera`
## See also

- [Wireplumber](https://wiki.gentoo.org/wiki/Wireplumber) — a modular session / policy manager for [PipeWire](https://wiki.gentoo.org/wiki/PipeWire)
- [Pipewire](https://wiki.gentoo.org/wiki/Pipewire) — low-latency, graph-based, processing engine and server, for interfacing with audio and video devices.
- [V4l-utils](https://wiki.gentoo.org/wiki/V4l-utils) — a set of utilities for handling media devices
- [Webcam](https://wiki.gentoo.org/wiki/Webcam) — information on setting up and using a **webcam** on Gentoo using [v4l-utils](https://wiki.gentoo.org/wiki/V4l-utils).
