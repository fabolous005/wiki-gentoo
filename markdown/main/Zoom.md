<!-- source: https://wiki.gentoo.org/wiki/Zoom | group: Gentoo Wiki (Main) | wiki-title: Zoom -->
---
title: Zoom
url: https://wiki.gentoo.org/wiki/Zoom
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-08-03"
fingerprint: a4cd8559b10bb904
license: CC BY-SA 4.0
---

# Zoom

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Zoom Meetings** also known as **Zoom** is a proprietary videotelephony software program developed by Zoom Video Communications.

**Zoom** is commonly used in remote work and educational environments.

## Installation

Installation is pretty fairly straightforward; just set `USE` flags as appropriate and then emerge the package. See the sections below about `USE` flag requirements for specific features (video and audio).


| [+zoom-symlink](https://packages.gentoo.org/useflags/+zoom-symlink) | Install a zoom symlink in /usr/bin | 
| [opencl](https://packages.gentoo.org/useflags/opencl) | Use OpenCL for virtual background support (virtual/opencl) | 
| [pulseaudio](https://packages.gentoo.org/useflags/pulseaudio) | Add sound server support via media-libs/libpulse (may be PulseAudio or PipeWire) | 
| [wayland](https://packages.gentoo.org/useflags/wayland) | Enable dev-libs/wayland backend | 

First, accept the all-rights-reserved license (you can read it at '/var/db/repos/gentoo/licenses/all-rights-reserved')

`root #``echo "net-im/zoom all-rights-reserved" >> /etc/portage/package.license`
then

`root #``emerge --ask net-im/zoom`
### Enabling audio

If the system uses Pulseaudio, enable the `pulseaudio` `USE` flag. This should rectify issues of not being able to speak to people.

### Enabling video

Normally, if the system has a webcam available, it should just work. If the webcam video feed is not displayed in Zoom, here are some things to try:

1. Use a different program to make sure that the webcam itself is working (see [Webcam](https://wiki.gentoo.org/wiki/Webcam))
2. Ensure that Zoom is upgraded to `5.14.10.3738-r1` or later, *or* that the `opencl` `USE` flag is enabled. (see [bug #833951](https://bugs.gentoo.org/show_bug.cgi?id=833951) for the context)
3. Ensure that the `bundled-libjpeg-turbo` `USE` flag is enabled.
