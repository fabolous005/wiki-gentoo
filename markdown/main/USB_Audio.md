<!-- source: https://wiki.gentoo.org/wiki/USB_Audio | group: Gentoo Wiki (Main) | wiki-title: USB Audio -->
---
title: USB Audio
url: https://wiki.gentoo.org/wiki/USB_Audio
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-31"
fingerprint: fd9a0a28681bbd14
license: CC BY-SA 4.0
---

# USB Audio

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article details the necessary system configuration to support **speakers** and **microphones** connected to the system via USB. Laptops commonly include microphones connected to the USB as part of the integrated webcam hardware whereas many USB headsets include both microphones and speakers over USB for sound input and output.

## Configuration

### Kernel

#### SND\_USB\_AUDIO

The USB audio/MIDI kernel symbol must be enabled for USB speakers and microphones to work properly.

**Enable support for`SND_USB_AUDIO`**

## Testing

Many desktop suites include a dedicated application for which to test the functionality webcam and/or voice recording software. Systems running the [Gnome](https://wiki.gentoo.org/wiki/Gnome) desktop environment can utilize the [media-video/cheese](https://packages.gentoo.org/packages/media-video/cheese) application.

The CLI tool [media-sound/sox](https://packages.gentoo.org/packages/media-sound/sox) can also be used to record and playback sound for users not using Gnome.

## Troubleshooting

### Extremely high volume

When testing the microphone, it is common for the output to be very loud and grainy. This is often caused by the input volume of the microphone being too high. This can be solved by decreasing the volume using the audio server:

For PulseAudio:

`user $``pactl set-source-volume $(pactl get-default-source) 50%`
For Pipewire:

`user $``wpctl set-volume @DEFAULT_AUDIO_SOURCE@ 20%`
## See also

- [USB to Ethernet Adapter](https://wiki.gentoo.org/wiki/USB_to_Ethernet_Adapter)
- [Kernel](https://wiki.gentoo.org/wiki/Kernel) — a central part of the Gentoo [operating system (OS)](https://en.wikipedia.org/wiki/operating_system)
