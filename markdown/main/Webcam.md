<!-- source: https://wiki.gentoo.org/wiki/Webcam | group: Gentoo Wiki (Main) | wiki-title: Webcam -->
---
title: Webcam
url: https://wiki.gentoo.org/wiki/Webcam
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-05"
categories: ['media-video']
fingerprint: "16076c54290a3bcc"
license: CC BY-SA 4.0
---

# Webcam

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article will provide information on setting up and using a **webcam** on Gentoo using [v4l-utils](https://wiki.gentoo.org/wiki/V4l-utils).

## Installation

### Kernel

**Enable Webcam support**

```
Device Drivers --->
  LED Support --->
    {*} LED Class Support 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_LEDS_CLASS</code> to find this item.
    <*> LED Flash Class Support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_LEDS_CLASS_FLASH</code> to find this item.
  <*> Multimedia support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_MEDIA_SUPPORT</code> to find this item. --->
    [*] Filter media drivers [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_MEDIA_SUPPORT_FILTER</code> to find this item.
    Media device types --->
      [*] Cameras and video grabbers [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_CAMERA_SUPPORT</code> to find this item.
    Video4Linux options --->
      <M> V4L2 flash API for LED flash class devices [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_V4L2_FLASH_LED_CLASS</code> to find this item.
    Media drivers  ---> 
      [*] Media USB Adapters [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_MEDIA_USB_SUPPORT</code> to find this item. ---> 
        <M> USB Video Class (UVC) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_USB_VIDEO_CLASS</code> to find this item.
        [*] UVC input events device support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_USB_VIDEO_CLASS_INPUT_EVDEV</code> to find this item.
### USE flags

Recommended USE flags for v4l include: `qt6` - at least `qt6` will be needed to follow on with this article.


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
### Additional software

There are several webcam viewer applications in the [Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository), a few examples are:

| Name | Package | Description | 
|---|---|---|
| Cheese | [media-video/cheese](https://packages.gentoo.org/packages/media-video/cheese) | Cheesy program to take pictures and videos from a webcam. | 
| guvcview | [media-video/guvcview](https://packages.gentoo.org/packages/media-video/guvcview) | Simple Qt6 or GTK+3 interface for capturing and viewing video from v4l2 devices. | 
| kamoso | [kde-apps/kamoso](https://packages.gentoo.org/packages/kde-apps/kamoso) | Application to take pictures and videos from a webcam, by KDE. | 
| [Motion](https://wiki.gentoo.org/wiki/Motion) | [media-video/motion](https://packages.gentoo.org/packages/media-video/motion) | Software motion detector. | 

Media players like [VLC](https://wiki.gentoo.org/wiki/VLC) or [mpv](https://wiki.gentoo.org/wiki/Mpv) may be used to view or sometimes record video from a webcam. Most webcams are recognized and accessible through a device in /dev/video0, wherein the number in the name indicates which webcam it is, in this case the first one. Thus reading from the /dev/video0 device with the video player of your choice, will give you the image of your webcam.

See [media-video](https://packages.gentoo.org/categories/media-video) for more video software.

## Configuration

v4l-utils is used here to check/set the camera's preferences.

### Configuration GUI and settings test

Launch the v4l GUI:

`user $``qv4l2`
### Get settings from video device

Example of using the video4linux control application to display supported video formats:

`user $``v4l2-ctl --list-formats-ext`
### Settings example

Some cheap USB cameras (like integrated models in ThinkPad laptops) default to a 640x480 resolution even when they are capable of higher resolutions. It is possible to override this with v4l2-ctl:

`user $``v4l2-ctl --set-parm=30 --set-fmt-video=width=1280,height=720,pixelformat=MJPG --device /dev/video0`
**Explanation**

`--set-parm` = Framerate (integer)

`--set-fmt-video` = Resolution + Compression

#### Check settings

Launch the v4l2 video capture viewer to test if the settings are working:

`user $``qvidcap`
#### Make options persistent

Getting device info:

`root #``lsusb | grep Cam`
Bus 003 Device 007: ID 5986:0268 Acer, Inc SunplusIT INC. Integrated Camera

Creating a rule:

**`/etc/udev/rules.d/99-v4linux.rules`**

**Example udev rule**

## See also

- [v4l-utils](https://wiki.gentoo.org/wiki/V4l-utils) — a set of utilities for handling media devices
