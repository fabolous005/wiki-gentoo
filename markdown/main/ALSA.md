<!-- source: https://wiki.gentoo.org/wiki/ALSA | group: Gentoo Wiki (Main) | wiki-title: ALSA -->
---
title: ALSA
url: https://wiki.gentoo.org/wiki/ALSA
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-24"
fingerprint: "9e0c915179e33b91"
license: CC BY-SA 4.0
---

# ALSA

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

[checking over the content](https://wiki.gentoo.org/index.php?title=ALSA&action=edit)(

[how to get started](https://wiki.gentoo.org/wiki/Gentoo_Wiki:Contributor%27s_guide)).


ALSA, the **A**dvanced **L**inux **S**ound **A**rchitecture, is the Linux kernel's API for sound cards, together with an associated software framework. Sound servers such as [PipeWire](https://wiki.gentoo.org/wiki/PipeWire), [PulseAudio](https://wiki.gentoo.org/wiki/PulseAudio), and [JACK](https://wiki.gentoo.org/wiki/JACK) all function as a layer on top of ALSA. ALSA can be used directly, without a sound server; however, sound servers provide various additional conveniences and functionality.

As of 2026-01-14, desktop profiles enable the PipeWire sound server. This includes support for packages which make use of the PulseAudio interface (e.g. via the [pulseaudio](https://packages.gentoo.org/useflags/pulseaudio) [USE flag), by using](https://wiki.gentoo.org/wiki/USE_flag) [media-sound/libpulse](https://packages.gentoo.org/packages/media-sound/libpulse) and [pipewire-pulse(1)](https://man.archlinux.org/man/pipewire-pulse.1.en)[.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

Those using PipeWire as a sound server - whether via a profile or otherwise - should typically not need to manually configure ALSA. However, those with more complex hardware and software setups might need to do so.

This page assumes that no sound server is being used.


## Installation


### Hardware detection

To choose the right driver, detect the audio controller in use, via [lspci](https://wiki.gentoo.org/wiki/Hardware_detection):

`user $``lspci | grep -i audio`
The needed driver can then be found in the [ALSA sound card matrix](https://www.alsa-project.org/wiki/SoundCard-Matrix).


### Kernel

Activate the following kernel options:

General setup --->
   -\*- System V IPC [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SYSVIPC\</code> to find this item.
 Device Drivers --->
   \<\*> Sound card support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SOUND\</code> to find this item. --->
     \<\*> Advanced Linux Sound Architecture [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\</code> to find this item. --->
       \[\*\] PCI sound devices [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_PCI\</code> to find this item. --->
         \<Select the audio controller driver(s) for the system's audio controller(s)>
       HD-Audio --->
         \<Select a codec or enable all and let the generic parse choose the right one>
         \[\*\] ...
       \[\*\] USB sound devices [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_USB\</code> to find this item. --->
         \<Must have as some cards are presented as USB devices>
         \[\*\] USB Audio/MIDI driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_USB\_AUDIO\</code> to find this item.

If the system has more than eight sound outputs (note that each HDMI output on a GPU counts as an output), the maximum number of sound cards will need to be increased:

Device Drivers --->
   \<\*> Sound card support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SOUND\</code> to find this item. --->
     \<\*> Advanced Linux Sound Architecture [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\</code> to find this item. --->
       -\*- Dynamic device file minor numbers [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_DYNAMIC\_MINORS\</code> to find this item.
       (32) Max number of sound cards [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_MAX\_CARDS\</code> to find this item.


#### Kernel modules

For advanced configurations (e.g. involving multiple sound cards), it can be better to select all the ALSA-related kernel options as modules. Then, their use can be configured via two files:

- /etc/modules-load.d/alsa.conf - one line per module, normally only needed for virtual sound cards.
- /etc/modprobe.d/alsa.conf - for the modules' options.

For example, assume a laptop with two hda-intel sound cards and an external USB card. If the aloop and VirMIDI virtual sound cards are also required, together with a card order that persists across reboots:

1. the aloop virtual card
2. the hda-intel PCM device
3. the hda-intel HDMI device
4. the VirMIDI virtual device
5. the USB card

then the relevant modules should be loaded in that order:

**`/etc/modprobe.d/alsa.conf`**

```
options snd slots=snd-aloop,snd-hda-intel,snd-hda-intel,snd-virmidi,snd-usb-audio
options snd-hda-intel index=1,2 model=1002:1637,1022:15e3
```
Card indexes start from 0.

To get the model strings, use [lspci(1)](https://man.archlinux.org/man/lspci.1.en) [(or](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [lsusb(1)](https://man.archlinux.org/man/lsusb.1.en) [for USB cards):](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`root #``lspci -nn`
04:00.1 Audio device: Advanced Micro Devices, Inc. \[AMD/ATI\] Renoir Radeon High Definition Audio Controller \[1002:1637\]
04:00.6 Audio device: Advanced Micro Devices, Inc. \[AMD\] Family 17h/19h HD Audio Controller \[1022:15e3\]

Instruct the kernel to load the virtual sound cards:

**`/etc/modules-load.d/alsa.conf`**

```
snd-aloop
snd-virmidi
```

### Software

Enabling the [alsa](https://packages.gentoo.org/useflags/alsa) [global](https://wiki.gentoo.org/wiki/USE_flag) [USE flag](https://wiki.gentoo.org/wiki/USE_flag) will not only enable support for ALSA packages, but will also automatically pull in [media-libs/alsa-lib](https://packages.gentoo.org/packages/media-libs/alsa-lib). The [alsa](https://packages.gentoo.org/useflags/alsa) [USE flag is enabled by default in the](https://wiki.gentoo.org/wiki/USE_flag) **x86** and **amd64** desktop profiles.

After enabling the flag, update the system so the changes take effect:

`root #``emerge --ask --changed-use --deep @world`
The [media-sound/alsa-utils](https://packages.gentoo.org/packages/media-sound/alsa-utils) package provides some tools for troubleshooting and testing the sound system. It's a good idea to merge the package if it's not already installed:

`root #``emerge --ask media-sound/alsa-utils`
Commands provided by [media-sound/alsa-utils](https://packages.gentoo.org/packages/media-sound/alsa-utils) include:

| Name | Description | 
|---|---|
| [aconnect(1)](https://man.archlinux.org/man/aconnect.1.en) | ALSA sequencer connection manager | 
| [alsactl(1)](https://man.archlinux.org/man/alsactl.1.en) | Advanced controls for ALSA soundcard driver | 
| [alsamixer(1)](https://man.archlinux.org/man/alsamixer.1.en) | Soundcard mixer for ALSA soundcard driver, with ncurses interface | 
| [amixer(1)](https://man.archlinux.org/man/amixer.1.en) | Command-line mixer for ALSA soundcard driver | 
| [aplay(1)](https://man.archlinux.org/man/aplay.1.en) | Command-line player for ALSA soundcard driver | 
| [aplaymidi(1)](https://man.archlinux.org/man/aplaymidi.1.en) | Play Standard MIDI Files | 
| [arecord(1)](https://man.archlinux.org/man/arecord.1.en) | Command-line sound recorder for ALSA soundcard driver | 
| [aseqdump(1)](https://man.archlinux.org/man/aseqdump.1.en) | Show the events received at an ALSA sequencer port | 
| [speaker-test(1)](https://man.archlinux.org/man/speaker-test.1.en) | Command-line speaker test tone generator for ALSA | 


## Configuration

**Todo:**

- This section needs a subsection describing a basic dmix configuration, which is commonly wanted/needed on pure-ALSA setups.


### Files

The 'root' of ALSA configuration is the /usr/share/alsa/alsa.conf file. By default, this file specifies several locations for ALSA configuration files:

- /var/lib/alsa/conf.d/
- /usr/etc/alsa/conf.d/
- /etc/alsa/conf.d/
- /etc/asound.conf or /usr/etc/asound.conf
- \~/.asoundrc
- ${XDG\_CONFIG\_HOME}/alsa/asoundrc

Configuration locations later in the list override earlier ones. If the `XDG_CONFIG_HOME` environment variable is not set, it's given a default value of \~/.config/.

/etc/asound.conf is typically used as the global (system-wide) configuration file, while \~/.asoundrc is typically used as a local (per-user) configuration file, overriding ALSA configuration for a specific user. Neither file exists by default; each must be created manually as necessary.

This page refers to both /etc/asound.conf and \~/.asoundrc when describing configuration snippets, but the snippets can typically be added to either file (or to another location), depending on the specific configuration being sought. For example, one particular snippet might be most appropriately applied system-wide, or might be most appropriately applied per-user.

A description of the syntax for ALSA configuration files is available in /usr/share/doc/alsa-lib-\<version>/asoundrc.txt.gz.

Various configuration snippets are available in the /usr/share/alsa/, the global configuration directory. For example, snippets available in the /usr/share/alsa/pcm/ directory include dmix.conf, hdmi.conf, and surround51.conf.


### Creating a configuration

In general, ALSA should work "out of the box", and not require any manual configuration. To list the current ALSA configuration, including any configuration files being applied, use the command alsactl dump-cfg. Additionally, ALSA's current state can be listed by using the command alsactl dump-state; ALSA state is stored in, and restored from, /var/lib/alsa/asound.state.

If a system has special hardware, the defaults of card 0 and device 0 might not work. However, on a system with multiple sound cards, cards might be reordered across boots; thus, specifying a card by its index number might prevent sound from working properly.

Internally, the card number is a name, which can be listed by passing the `-L` option to [aplay(1)](https://man.archlinux.org/man/aplay.1.en)[, from the](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [media-sound/alsa-utils](https://packages.gentoo.org/packages/media-sound/alsa-utils) package:

`user $``aplay -L````
...
sysdefault:CARD=PCH
    HDA Intel PCH, ALC3246 Analog
    Default Audio Device
front:CARD=PCH,DEV=0
    HDA Intel PCH, ALC3246 Analog
    Front output / input
...
```
This information can then be used in an ALSA configuration file, e.g.:

**`~/.asoundrc`**

```
defaults.pcm.!card PCH
defaults.pcm.!device 0
defaults.ctl.!card PCH
```
This snippet demonstrates the use of `.!` to specify 'override' operation mode<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.

The default operation mode, 'merge+create', is indicated by either a bare `.` character before the final component of the node, or by `.+` in that position. In this mode, if a configuration node is not present, a new one is created; otherwise, if it passes type checking, the assignment is merged.

For example, in the node `defaults.pcm.card`, the final component is `card`, and there is a bare `.` preceding it, thus indicating that 'merge+create' mode is used for the node.

However, if `.!` is used instead, as in the snippet above, 'override' mode is used for the node: the old node value will be overwritten with the new value. Thus, the above snippet will overwrite the old value of each of the specified nodes with the value specified (e.g. `PCH`)

If the correct name for a device is unclear, a list of valid names can be obtained from [sysfs](https://wiki.gentoo.org/wiki/Sysfs):

`user $``cat /sys/class/sound/card*/id`
Q1U
HDMI
PCH
C930e

This shows the Q1U microphone is `Q1U`, the builtin HDMI is `HDMI`, the analog audio jacks are `PCH`, and a webcam's builtin microphone is `C930e`. Any of these are valid names for the card.


### PulseAudio emulation

Some software (e.g. [Firefox](https://wiki.gentoo.org/wiki/Firefox)) makes use of the Pulse API. For [PulseAudio](https://wiki.gentoo.org/wiki/PulseAudio) support on a pure-ALSA system (i.e. one not using a sound server such as PipeWire or Pulse), install [apulse](https://wiki.gentoo.org/wiki/Apulse).


### S/PDIF or HDMI

With a default ALSA installation, an S/PDIF or HDMI connection might work "out of the box", without requiring any manual configuration of ALSA. If this is not the case, use aplay -L to find the appropriate card name and device number, as described in the [the "Files" section](https://wiki.gentoo.org#Files) above, then use that information in the following snippet for the appropriate ALSA configuration file:

**`/etc/asound.conf`**

```
pcm.!spdif {
    type hw
    card PCH
    device 1
}
pcm.!default {
    type plug
    slave {
        pcm "spdif"
    }
}
```
The above configuration will not allow sound to be played from more than one sound application, or two sounds to be played simultaneously through the same sound card. In order to allow this, use the following configuration snippet for mixing (noting that doing so might result in interrupted output and/or a slight degradation in sound quality):

**`/etc/asound.conf`**

```
# Share a single card with multiple applications 
pcm.!default {
    type plug
    slave.pcm "dmix:CARD=PCH,DEVICE=1,RATE=48000"
}
```

### Cloning audio for two or more devices

Sometimes there's a need to clone audio: for example, using two cloned screens with different inputs, where the first screen is HDMI, and the second screen is DVI and analog audio.

The following snippet provides such a configuration. Adjust it as necessary for the system to which it's being applied.

Refer to [this forum discussion](https://forums.gentoo.org/viewtopic-t-902670-start-0.html) for more information.

**`/etc/asound.conf`**

```
ctl.!default {
    type hw
    # Set default card
    card PCH
}
 
pcm.!default both
 
pcm.both {
    type softvol
    slave {
        pcm {
            type plug
            slave {
                pcm {
                    type route
                    slave {
                        pcm {
                        type multi
                        slaves.a.pcm {
                            type dmix
                            ipc_key 2589455
                            ipc_perm 0666
                            slave {
                                pcm {
                                    # Add the default card - change to suit sound card #1
                                    type hw
                                    card PCH
                                    device 0
                                }
                                buffer_size 4096
                                channels 2
                            }
                        }
                        slaves.b.pcm {
                            type dmix
                            ipc_key 4855689
                            ipc_perm 0666
                            slave {
                                pcm {
                                    # Add the slave card - change to suit sound card #2
                                    type hw
                                    card PCH
                                    device 7
                                }
                                buffer_size 4096
                                channels 2
                            }
                        }
                        slaves.a.channels 2
                        slaves.b.channels 2
                        bindings.0.slave a
                        bindings.0.channel 0
                        bindings.1.slave a
                        bindings.1.channel 1
                        bindings.2.slave b
                        bindings.2.channel 0
                        bindings.3.slave b
                        bindings.3.channel 1
                    }
                }
                ttable.0.0 1
                ttable.1.1 1
                ttable.0.2 1
                ttable.1.3 1
            }
        }
    }
}
control {
    # Define volume control name
    name PCM
    # Set to the default card
    card PCH
}
```

### A/52 / Dolby AC-3 / Dolby Digital and DTS

Sound cards providing S/PDIF output can do lossy compressed multichannel audio passthrough. An S/PDIF or HDMI connection is required for passthrough of advanced encoding; analog connections (RCA and minijacks) don't support any advanced encoding.

If a sound card states it has S/PDIF, it will also likely pass A/52 (also known as Dolby AC-3 and Dolby Digital) and/or DTS, even if neither is specifically listed as a feature. When a card lists one or both, it's because the vendor provides software for upmixing, or provides the decoded signal through analog output.

For A/52 support, install the [media-plugins/alsa-plugins](https://packages.gentoo.org/packages/media-plugins/alsa-plugins) package with the [ffmpeg](https://packages.gentoo.org/useflags/ffmpeg) [USE Flag enabled.](https://wiki.gentoo.org/wiki/USE_flag)

Audio/multimedia players might need multichannel codecs specified in their configuration, to allow the provision of hardware passthrough rather than downmixing.


#### Encoding to A/52

To simulate A/52-encoded audio from standard one- or two-channel audio streams or files:

**`~/.asoundrc`**

```
pcm.!default {
    type plug:surroundaudio
}
 
ctl.!default {
    type plug:surroundaudio
}
 
pcm.a52encode {
    type a52
    format S16_LE
    channels 6
    rate 48000
    bitrate 448
}
 
pcm.surroundaudio a52encode
 
ctl.surroundaudio {
    type hw
    card PCH
    device 1
}
```
Test the configuration with [speaker-test(1)](https://man.archlinux.org/man/speaker-test.1.en)[:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`user $``speaker-test -Dsurroundaudio -c 6`
The preceding snippet might not work with PCM 5.1 streams. Additionally, such streams might also require downmixing from S24\_LE to S16\_LE format: 24-bit is a common format for most media, but receivers can sometimes only decode 16- or 32-bit audio.

The following configuration snippet will encode a PCM 5.1 24-bit stream into a 16-bit A/52 stream, for streaming over S/PDIF. Note that this is not required when using HDMI, as HDMI can handle the bandwidth of a PCM 5.1 audio stream.

**`~/.asoundrc`**

```
# Make it possible to use A/52 with PulseAudio out of 
# the box. May also be useful for other use-cases.
 
pcm.a52 {
    @args [ CARD ]
    @args.CARD {
        type string
        default 0
    }
    type plug
    slave {
        pcm {
            type a52
            card $CARD
            bitrate 640 # Default 448
        }
        # Convert to S16 bit format, per S/PDIF spec
        format S16_LE
    }
}
```
Test the configuration with [aplay(1)](https://man.archlinux.org/man/aplay.1.en)[:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`user $``aplay -D pcm.a52 /home/me/Music/Led_Zeppelin/Celegration_Day/PCM51-24bit/*.wav`

### JACK Audio Connection Kit

It is possible to interconnect JACK and ALSA. For more information, refer to [this section of the "JACK" page](https://wiki.gentoo.org/wiki/JACK#ALSA).


### Permissions

If the [acl](https://packages.gentoo.org/useflags/acl) [USE flag is enabled globally, and a login daemon (e.g.](https://wiki.gentoo.org/wiki/USE_flag) [systemd-logind(8)](https://man.archlinux.org/man/systemd-logind.8.en) [or](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [elogind](https://wiki.gentoo.org/wiki/Elogind)) is being used, such as on systems using a *desktop* [profile](https://wiki.gentoo.org/wiki/Portage/Profiles), sound card permissions will be handled automatically. Permissions can be checked using [getfacl(1)](https://man.archlinux.org/man/getfacl.1.en)[:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`user $``getfacl /dev/snd/controlC0 | grep larry`
user:larry:rw-

A broader solution is to add the user accessing the sound card to the `audio` group.

`root #``gpasswd -a larry audio`
To confirm that the user has been added to the `audio` group, check the contents of /etc/group:

`user $``grep audio /etc/group`
audio::18:larry

To apply this change generally, log out and back in again. To immediately apply this change within a current shell session, use [newgrp(1)](https://man.archlinux.org/man/newgrp.1.en)[:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`user $``newgrp audio`

### Service


#### OpenRC

The `alsasound` service saves and restores mixer levels. To start it manually:

`root #``rc-service alsasound start`
To have the service start at boot, add it the `boot` runlevel using rc-update:

`root #``rc-update add alsasound boot`

#### systemd

On systemd systems, ALSA state is preserved and restored automatically across system restarts.

The current ALSA state can be checked using the systemctl command:

`root #``systemctl status alsa-restore`

## Usage

This section assumes that the [media-sound/alsa-utils](https://packages.gentoo.org/packages/media-sound/alsa-utils) package has been installed, as recommended in the "[Software](https://wiki.gentoo.org/wiki/ALSA#Software)" section.


### Test speakers

Use [speaker-test(1)](https://man.archlinux.org/man/speaker-test.1.en) [to test speakers:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`user $``speaker-test -t wav`
Press `Ctrl`+`C` to interrupt the test.

Various options are available to specify the test output, e.g. the number of channels and the test type; refer to the man page for details.

For systems using 5.1 surround sound:

`user $``speaker-test -t wav -c 6 -D surround51`
If speaker-test returns an error about [IPC](https://en.wikipedia.org/wiki/Inter-process_communication) semaphore not being implemented, ensure the kernel's "System V IPC" option is enabled, as described in [the "Kernel" section](https://wiki.gentoo.org/wiki/ALSA#Kernel).


#### HDMI/SPDIF 5.1 and 7.1 speaker testing

The [speaker-test(1)](https://man.archlinux.org/man/speaker-test.1.en) [utility doesn't test audio with more than two channels over HDMI or SPDIF, but](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [MPlayer](https://wiki.gentoo.org/wiki/MPlayer) and [VLC](https://wiki.gentoo.org/wiki/VLC) can be used instead.

To do so, download a [5.1 channel (FLAC)](https://github.com/sfiera/flac-test-files/raw/master/surround51.flac) or [7.1 channel (FLAC)](https://github.com/sfiera/flac-test-files/raw/master/surround71.flac) speaker test file.

Then, play the provided sound file, specifying the HDMI or SPDIF device, the PCM file, and the number of channels (which will likely be either 6, for 5.1 surround, or 8, for 7.1 channel surround).

To test a 5.1 channel file using [mplayer(1)](https://man.archlinux.org/man/mplayer.1.en)[:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`user $``mplayer -channels 6 "/path/to/downloaded/flac_file.flac" -channels 6`
To test a 7.1 channel file:

`user $``mplayer -channels 8 "/path/to/downloaded/flac_file.flac" -channels 8`
To test a FLAC file using [cvlc(1)](https://man.archlinux.org/man/cvlc.1.en)[:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`user $``cvlc /path/to/downloaded/flac_file.flac`

### Test microphone

[arecord(1)](https://man.archlinux.org/man/arecord.1.en) [can be used to test a microphone.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

The following will record until `CTRL`+`C` is pressed, and by default will provide a mono recording as a WAV file in U8 format:

`user $``arecord /tmp/test.wav`
Use [aplay(1)](https://man.archlinux.org/man/aplay.1.en) [to play the saved file.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

The file type, number of channels, and file format can be specified via the `-t`/`--file-type`, `-c`/`--channels`, and `-f`/`--format` options, respectively.

If a default audio device is not set, specify a device using arecord's `--device` option. using the `-l`/`--list-devices` option to find the relevant card and device number:

`user $``arecord -l`
$ arecord -l
\*\*\*\* List of CAPTURE Hardware Devices \*\*\*\*
card 0: PCH \[HDA Intel PCH\], device 0: ALC3246 Analog \[ALC3246 Analog\]
  Subdevices: 1/1
  Subdevice #0: subdevice #0

`user $``arecord --device=hw:0,0 /tmp/test.wav`

## Troubleshooting

Refer to the [ALSA/troubleshooting](https://wiki.gentoo.org/wiki/ALSA/troubleshooting) page.


## See also

- [Power management/Soundcard](https://wiki.gentoo.org/wiki/Power_management/Soundcard) — describes the setup of [power management](https://wiki.gentoo.org/wiki/Power_management) of [sound devices](https://wiki.gentoo.org/wiki/Category:Sound_devices).
- [PipeWire](https://wiki.gentoo.org/wiki/PipeWire) — low-latency, graph-based, processing engine and server, for interfacing with audio and video devices.
- [PulseAudio](https://wiki.gentoo.org/wiki/PulseAudio) — a multi-platform, open source, *sound server* that provides a number of features on top of the low-level audio interface [ALSA]
- [JACK](https://wiki.gentoo.org/wiki/JACK) — describes the setup of a playing sound with **JACK** (**J**ACK **A**udio **C**onnection **K**it).


## External resources

- [ALSA project - the C library reference](https://www.alsa-project.org/alsa-doc/alsa-lib/) - home page with links to official information about ALSA configuration and the ALSA API.
- ALSA Project - the C library reference, [Configuration syntax](https://www.alsa-project.org/alsa-doc/alsa-lib/conf.html) - Overview of the syntax of ALSA configuration files.
- ALSA Project - the C library reference, [PCM (digital audio) plugins](https://www.alsa-project.org/alsa-doc/alsa-lib/pcm_plugins.html) - ALSA PCM (digital audio) plugin descriptions and configuration file examples.
- [A close look at ALSA](https://www.volkerschatz.com/noise/alsa.html) - introductory tutorial.
- [PaulBredbury's asoundrc file](https://gist.github.com/thanley11/100754cc911442901867) - possibly very out of date, given mention of mplayer2, but might have useful snippets.


## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) [ALSA project - the C library reference: Configuration files](https://www.alsa-project.org/alsa-doc/alsa-lib/conf.html#https://www.alsa-project.org/alsa-doc/alsa-lib/conf.html#https://www.alsa-project.org/alsa-doc/alsa-lib/conf.html): Operation modes for parsing nodes
