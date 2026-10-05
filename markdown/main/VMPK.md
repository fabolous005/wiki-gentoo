<!-- source: https://wiki.gentoo.org/wiki/VMPK | group: Gentoo Wiki (Main) | wiki-title: VMPK -->
---
title: VMPK
url: https://wiki.gentoo.org/wiki/VMPK
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-03-19"
fingerprint: "6212dc1a410b6c14"
license: CC BY-SA 4.0
---

# VMPK

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**VMPK**, **V**irtual **M**IDI **P**iano **K**eyboard, is a virtual [MIDI](https://wiki.gentoo.org/wiki/MIDI) controller for Linux, Windows and OSX.

## Installation

### USE flags


### Emerge

`root #``emerge --ask media-sound/vmpk`
## Usage

Launch the virtual keyboard by running vmpk:

`user $``vmpk`
By default, mouse input is disabled; it can be enabled via Edit -> Preferences -> Input.

Once VMPK is running, connect its output to a MIDI input on an [ALSA](https://wiki.gentoo.org/wiki/ALSA) client, using e.g. [aconnect(1)](https://man.archlinux.org/man/aconnect.1.en) [(provided by](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [media-sound/alsa-utils](https://packages.gentoo.org/packages/media-sound/alsa-utils)) or a GUI tool, such as [media-sound/helvum](https://packages.gentoo.org/packages/media-sound/helvum) or [media-sound/qjackctl](https://packages.gentoo.org/packages/media-sound/qjackctl). For example, to use aconnect to connect VMPK to a running instance of [amsynth](https://wiki.gentoo.org/wiki/Amsynth), first run `aconnect -l` to find the MIDI output port of VMPK and the MIDI input port of amsynth:

`user $``aconnect -l````
client 0: 'System' [type=kernel]
    0 'Timer           '
	Connecting To: 142:0
    1 'Announce        '
	Connecting To: 142:0
client 128: 'amsynth' [type=user,pid=16405]
    0 'MIDI IN         '
    1 'MIDI OUT        '
client 129: 'VMPK Input' [type=user,pid=16752]
    0 'in              '
client 130: 'VMPK Output' [type=user,pid=16752]
    0 'out             '
client 142: 'PipeWire-System' [type=user,pid=3553]
    0 'input           '
	Connected From: 0:1, 0:0
client 143: 'PipeWire-RT-Event' [type=user,pid=3553]
    0 'input           '
```
Then, to connect VMPK out (130:0) to amsynth MIDI in (128:0):

`user $``aconnect 130:0 128:0`
Use of the VMPK keyboard should now result in audio output from amsynth.
