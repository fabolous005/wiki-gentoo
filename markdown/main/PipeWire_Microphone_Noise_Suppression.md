<!-- source: https://wiki.gentoo.org/wiki/PipeWire/Microphone_Noise_Suppression | group: Gentoo Wiki (Main) | wiki-title: PipeWire/Microphone Noise Suppression -->
---
title: PipeWire/Microphone Noise Suppression
url: https://wiki.gentoo.org/wiki/PipeWire/Microphone_Noise_Suppression
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-05-18"
fingerprint: f6a3480a2153aa3f
license: CC BY-SA 4.0
---

# PipeWire/Microphone Noise Suppression

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

During video meetings or other voice recordings, background noise can make it harder for people to understand you. Fans, mechnicaly keyboards, construction and more can make it harder for others to hear you.

Using a LADSPA plugin for PipeWire that you can automatically filter background noise when you're using your microphone.


Microphone noise can be reduced using noise-suppression-for-voice.

## Installation

### Emerge

`root #``emerge --ask media-libs/noise-suppression-for-voice`


## Configuration

**`~/.config/pipewire/pipewire.conf.d/99-input-denoising.conf`**

### Service

#### Systemd

Restart PipeWire:

`user $``systemctl restart --user pipewire.service`
