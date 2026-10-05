<!-- source: https://wiki.gentoo.org/wiki/Blue_Yeti_Pro | group: Gentoo Wiki (Main) | wiki-title: Blue Yeti Pro -->
---
title: Blue Yeti Pro
url: https://wiki.gentoo.org/wiki/Blue_Yeti_Pro
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-07-04"
fingerprint: "5743367c61b3392c"
license: CC BY-SA 4.0
---

# Blue Yeti Pro

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The Blue Yeti Pro is a high quality, USB condenser microphone. It is popular among streaming.

Getting the Blue Yeti Pro operational in Gentoo requires the USB Audio/MIDI driver to be built-in to the kernel or, at minimum, snd-usb-audio built as a module.

## Hardware

### Standard

| Device | Make/model | Status | Bus ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| USB microphone | Blue Yeti Pro |  | `074d:0002` | snd-usb-audio (when built as a module) | 4.4.1 | Enable kernel option `SND_USB_AUDIO` in the kernel. | 

## Installation

### Kernel

**Enable support for`SND_USB_AUDIO`**

## Configuration

Simply use the application of choice to select the Blue Yeti microphone as the system's input device.

## See also

- [lsusb](https://wiki.gentoo.org/wiki/Usbutils) - A utility for listing devices attached to system via the USB bus.

## External resources

- [https://forums.gentoo.org/viewtopic-t-797843-start-0.html](https://forums.gentoo.org/viewtopic-t-797843-start-0.html) - A Gentoo Forums post concerning the correct operation of a webcam.
- [http://www.wolfmanzbytes.com/audio-gear/189-yeti-usb-microphone.html](http://www.wolfmanzbytes.com/audio-gear/189-yeti-usb-microphone.html) - A Yeti USB Microphone review.
- [http://www.linux-hardware-guide.com/2014-01-06-blue-microphones-yeti-usb-microphone](http://www.linux-hardware-guide.com/2014-01-06-blue-microphones-yeti-usb-microphone)
