<!-- source: https://wiki.gentoo.org/wiki/MIDI_controller_guide | group: Gentoo Wiki (Main) | wiki-title: MIDI controller guide -->
---
title: MIDI controller guide
url: https://wiki.gentoo.org/wiki/MIDI_controller_guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-13"
fingerprint: fb11c61ce60b7c48
license: CC BY-SA 4.0
---

# MIDI controller guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

[checking over the content](https://wiki.gentoo.org/index.php?title=MIDI_controller_guide&action=edit)(

[how to get started](https://wiki.gentoo.org/wiki/Gentoo_Wiki:Contributor%27s_guide)).

**MIDI controllers** include a vast assortment of musical equipment including keyboards, pads, pot/fader controls and much more.

These devices can be connected to a computer (e.g. via [USB](https://wiki.gentoo.org/wiki/USB)) to trigger and control sound and video for live performances, recording, etc.

There is a variety of [MIDI](https://wiki.gentoo.org/wiki/MIDI) software available for Gentoo, including software synthesizers that can be "played" in real time by a MIDI controller, such as [TiMidity++](https://wiki.gentoo.org/wiki/TiMidity%2B%2B) and [FluidSynth](https://wiki.gentoo.org/wiki/FluidSynth).

This article will explain how to set up MIDI controller hardware with a Gentoo system.

## Installation

### Kernel

Kernel drivers are usually required for external MIDI controller support, such as interfacing with a MIDI keyboard or MIDI drum pads.

The configuration below are examples for the legacy PC game/MIDI port and through USB, for other more exotic hardware try to find if the specific hardware is in the kernel module list.

**Enable support for MIDI devices**

Device Drivers --->
   \<\*> Sound card support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SOUND\</code> to find this item. --->
     \<\*> Advanced Linux Sound Architecture [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\</code> to find this item. --->
       \<M> HR-timer backend support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_HRTIMER\</code> to find this item.
       \<\*> Sequencer support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_SEQUENCER\</code> to find this item.
         \<M> Sequencer dummy client [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_SEQ\_DUMMY\</code> to find this item.
         \[\*\]  Use HR-Timer as default sequencer timer [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_SEQ\_HRTIMER\_DEFAULT\</code> to find this item.

(For the PC MIDI/game port)

\[\*\] Generic sound devices [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_DRIVERS\</code> to find this item. --->
         \<M> Generic MPU-401 UART Driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_MPU401\</code> to find this item.
         (enable any special modules here)

(USB)

\[\*\] USB sound devices [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_USB\</code> to find this item. --->
         \<M> USB Audio/MIDI driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_USB\_AUDIO\</code> to find this item.
         (enable any special modules here)

## Configuration

### Connecting MIDI ports

Various software exists to connect MIDI ports, some with graphical interfaces, and often larger pieces of software that interact with MIDI have this functionality built-in.

#### ALSA

A MIDI input port can be connected to an output port with the [ALSA](https://wiki.gentoo.org/wiki/ALSA) [aconnect(1)](https://man.archlinux.org/man/aconnect.1.en) [command.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) 

To see which input ports are available with aconnect:

`user $``aconnect -li` ```
client 0: 'System' [type=kernel]
    0 'Timer           '
    1 'Announce        '
client 14: 'Midi Through' [type=kernel]
    0 'Midi Through Port-0'
client 28: 'KOMPLETE KONTROL A49' [type=kernel,card=3]
    0 'KOMPLETE KONTROL A49 MIDI 1'
```
In this example output, the MIDI controller is a "Native Instruments Komplete Kontrol A49".

To see which output ports are available:

`user $``aconnect -lo` ```
client 14: 'Midi Through' [type=kernel]
    0 'Midi Through Port-0'
client 28: 'KOMPLETE KONTROL A49' [type=kernel,card=3]
    0 'KOMPLETE KONTROL A49 MIDI 1'
client 128: 'TiMidity' [type=user,pid=5294]
    0 'TiMidity port 0 '
    1 'TiMidity port 1 '
    2 'TiMidity port 2 '
    3 'TiMidity port 3 '
```
A MIDI controller could for example be connected directly to [TiMidity++](https://wiki.gentoo.org/wiki/TiMidity%2B%2B), to allow the controller to play music live:

`user $``aconnect 28:0 128:0`
The connection can be verified using:

`user $``aconnect -l` ```
client 0: 'System' [type=kernel]
    0 'Timer           '
    1 'Announce        '
client 14: 'Midi Through' [type=kernel]
    0 'Midi Through Port-0'
client 28: 'KOMPLETE KONTROL A49' [type=kernel,card=3]
    0 'KOMPLETE KONTROL A49 MIDI 1'
        Connecting To: 128:0
client 128: 'TiMidity' [type=user,pid=5294]
    0 'TiMidity port 0 '
        Connected From: 28:0
    1 'TiMidity port 1 '
    2 'TiMidity port 2 '
    3 'TiMidity port 3 '
```
If all went well, pressing a key on the MIDI controller should play a sound.

To remove all connections, use:

`user $``aconnect -x`
##### Testing MIDI controller keys

If MIDI controller input is not producing any output, physical device failure can be ruled out using [aseqdump(1)](https://man.archlinux.org/man/aseqdump.1.en) [from](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [media-sound/alsa-utils](https://packages.gentoo.org/packages/media-sound/alsa-utils).

First, identify the port of the MIDI controller:

`user $``aconnect -l`
Then run the following, `PORT` being the port of the MIDI controller. :

`user $``aseqdump -p PORT`
Pressing any key on the controller should result in the key appearing in the output. If no output is displayed, assuming the correct port was used, physical hardware failure may be to blame.

## See also

- [MIDI](https://wiki.gentoo.org/wiki/MIDI) — a set of technical specifications that enable devices to interoperate in order to work with a digital representation of music
- [Project:Sound/How to Enable Realtime for Multimedia Applications](https://wiki.gentoo.org/wiki/Project:Sound/How_to_Enable_Realtime_for_Multimedia_Applications)
- [TiMidity++](https://wiki.gentoo.org/wiki/TiMidity%2B%2B) — software synthesizer that can interpret MIDI information
- [FluidSynth](https://wiki.gentoo.org/wiki/FluidSynth) — a real-time software synthesizer based on the SoundFont 2 specifications
