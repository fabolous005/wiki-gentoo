<!-- source: https://wiki.gentoo.org/wiki/Fiio_K11 | group: Gentoo Wiki (Main) | wiki-title: Fiio K11 -->
---
title: Fiio K11
url: https://wiki.gentoo.org/wiki/Fiio_K11
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-07"
fingerprint: "3e4b0227db43f3d0"
license: CC BY-SA 4.0
---

# Fiio K11

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The FiiO K11 is a Desktop DAC and Headphone Amplifier that can be used with Linux. It acts as a USB Audio Class 2.0 (UAC2) device, enabling effectively plug-and-play usage.

## Configuration

While the device will work out-of-the-box provided that sound and USB Audio Class drivers are enabled. It is worth ensuring that the device is set to UAC2 mode. Long-press the knob to access the device menu, then scroll to **UAC** and ensure that it is set to **2**.

Further configuration of filters, RGB, etc. through the on-device controls are down to personal preference and connected equipment and are beyond the scope of this article.

### PipeWire

While the device is perfectly functional without configuration, there are some steps that enable the use of more of the capabilities of the device.

First, configure PipeWire to enable additional rates:

**`~/.config/pipewire/pipewire.conf.d/fiio-k11-rates.conf`**

Then configure WirePlumber to open the device at its full 32-bit depth:

**`~/.config/wireplumber/wireplumber.conf.d/51-fiio-k11.conf`**

Finally, reload the daemons:

`user $``systemctl --user daemon-reload``user $``systemctl --user restart pipewire pipewire-pulse wireplumber`
pw-top can be used to confirm the format and bitrate. Play a 96k FLAC (example available from Sony's help pages<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>) - the device should change to a Yellow FiiO logo, display 96k on the front display, and pw-top should display something like:

`I   56   4096  96000   4.0us   9.3us  0.00  0.00    0    S32LE 2 96000 alsa_output.usb-FIIO_FiiO_K11-01.analog-stereo`
