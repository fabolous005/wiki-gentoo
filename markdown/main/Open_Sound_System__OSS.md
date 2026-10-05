<!-- source: https://wiki.gentoo.org/wiki/Open_Sound_System_(OSS) | group: Gentoo Wiki (Main) | wiki-title: Open Sound System (OSS) -->
---
title: Open Sound System (OSS)
url: https://wiki.gentoo.org/wiki/Open_Sound_System_(OSS)
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-08-16"
fingerprint: e780015aedb39947
license: CC BY-SA 4.0
---

# Open Sound System (OSS)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Open Sound System (OSS) v4 is an alternative to [ALSA](https://wiki.gentoo.org/wiki/ALSA) as a layer between programs and sound hardware. Users of v3 are encouraged to use the not so maintained v4 version which has a BSD and GPL-2 licences.

## Installation

Currently there is no maintained ebuild in portage for this software.

### Kernel

**Sound card support**

### Ebuild

You will first need [eselect-repository](https://wiki.gentoo.org/wiki/Eselect-repository)

`root #``emerge --ask dev-vcs/git app-eselect/eselect-repository`
#### Bug Report

Ebuilds can also be found in [bug #184123](https://bugs.gentoo.org/show_bug.cgi?id=184123), which would need to be added to the local overlay

### Install

Build the modules and add /etc/init.d/oss to the default runlevel:

`root #````
emerge oss
```
`root #``rc-update add oss default`
## Configuration

General configuration after install.

### Compatibility

Many applications only provide support for ALSA, so it is a good idea to keep both USE flags enabled.

**`/etc/portage/make.conf`**

OSS will create ALSA devices for those applications.

- To be able to use OSS4 with [GNOME](https://wiki.gentoo.org/wiki/GNOME) or other gstreamer-dependent desktop environments like xfce4: [media-plugins/gst-plugins-oss4](https://packages.gentoo.org/packages/media-plugins/gst-plugins-oss4) (included in [media-libs/gst-plugins-good](https://packages.gentoo.org/packages/media-libs/gst-plugins-good)) was dropped from the official tree, so making an ebuild is necessary (quite easy for this one) or getting one from somewhere (bar-overlay, for example).

`root #``emerge --ask media-plugins/gst-plugins-oss4`
### CMI878x

Unlike ALSA, OSS4 has fewer complex options to configure, so you won't need any \`soundon.user'. Especially if your setup isn't complex or are using more than one sound card. In the latter case, you may want to keep a \`soundon.user' file to be able to choose your first card (default card). And then, insert a few lines correcting devices nodes which can be problematic.

```
#!/bin/sh
# $ID: /usr/lib/oss/soundon.user, 2012/07/27 12:57:40 -tclover Exp $
modprobe -a oss_{cmi878x,hdaudio}
/usr/sbin/ossdetect -diu
/usr/sbin/ossdevlinks 
# Remapping ossdevlinks links
if [ -d /dev/oss/oss_cmi878x ]; then
	ln -sf /dev/oss/oss_cmi878x0/pcm1 /dev/dsp
	ln -sf /dev/oss/oss_cmi878x0/pcm1 /dev/dsp_ac3
	ln -sf /dev/oss/oss_cmi878x0/pcm1 /dev/dsp_in
	ln -sf /dev/oss/oss_cmi878x0/pcm1 /dev/dsp_mmap
	ln -sf /dev/oss/oss_cmi878x0/pcm1 /dev/dsp_multich
	ln -sf /dev/oss/oss_cmi878x0/pcm1 /dev/dsp_out
	vmixctl attach /dev/oss/oss_cmi878x0/pcm1 /dev/oss/oss_cmi878x0/pcm1
else
	ln -sf /dev/oss/oss_hdaudio0/pcm0 /dev/dsp
	ln -sf /dev/oss/oss_hdaudio0/pcmin0 /dev/dsp_in
#	ln -sf /dev/oss/oss_hdaudio0/spdout0 /dev/dsp
	ln -sf /dev/oss/oss_hdaudio0/spdout0 /dev/dsp_ac3
#	ln -sf /dev/oss/oss_hdaudio0/spdin0 /dev/dsp_in
	ln -sf /dev/oss/oss_hdaudio0/spdout0 /dev/dsp_mmap
	ln -sf /dev/oss/oss_hdaudio0/spdout0 /dev/dsp_multich
	ln -sf /dev/oss/oss_hdaudio0/spdout0 /dev/dsp_out
#	vmixctl attach /dev/oss/oss_hdaudio0/pcm0 /dev/oss/oss_hdaudio0/pcmin0
fi
# vim:fenc=utf-8:ft=sh:ci:pi:sts=0:sw=2:ts=2:
```
The previous script will check if there are devices nodes, so a sound card, with CMI878x driver and then relink almost everything to it or else use HDAudio.

**`/etc/portage/make.conf`**

Or else, create a file like \`\~/.scripts/soundon.user' and edit \`/etc/init.d/oss' to your liking.

**`/etc/init.d/oss`**

```
 -L
```
Now you can enjoy Open Sound System. I found the sound with OSS4 to be better than what I can get with ALSA. There was a time when my CMI878x sound works only with OSS4 but a few things changed.

OSS4 has its own mixer so using extra sound daemons like pulseaudio, esd etc. is not necessary.

## Usage

OSS4 has several comandline tools and one gtk-GUI to configure the devices.

### osstest

Test all audio devices with the following command:

`user $``osstest`
### ossxmix

ossxmix is the graphical tool to configure devices:
