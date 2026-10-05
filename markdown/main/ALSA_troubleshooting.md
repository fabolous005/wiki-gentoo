<!-- source: https://wiki.gentoo.org/wiki/ALSA/troubleshooting | group: Gentoo Wiki (Main) | wiki-title: ALSA/troubleshooting -->
---
title: ALSA/troubleshooting
url: https://wiki.gentoo.org/wiki/ALSA/troubleshooting
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-30"
fingerprint: f40ec81d38427799
license: CC BY-SA 4.0
---

# ALSA/troubleshooting

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## ALSA troubleshooting

For in-depth information about a program's usage of ALSA, such as its PID (`owner_pid`) and sample rate (`rate`), use the /proc interface. This can be done by substituting the relevant card/device details into the following command. Note that the /proc interface only lists physical devices, not virtual devices<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.

`user $``cat /proc/asound/card2/pcm0p/sub0/*`
access: RW\_INTERLEAVED
format: S16\_LE
subformat: STD
channels: 2
rate: 44100 (44100/1)
period\_size: 5513
buffer\_size: 22050
card: 2
device: 0
subdevice: 0
stream: PLAYBACK
id: USB Audio
name: USB Audio
subname: subdevice #0
class: 0
subclass: 0
subdevices\_count: 1
subdevices\_avail: 0
state: RUNNING
owner\_pid   : 934
trigger\_time: 86393.193574796
tstamp      : 86540.250594985
delay       : 17714
avail       : 4602
avail\_max   : 7379
-----
hw\_ptr      : 6485052
appl\_ptr    : 6502500
tstamp\_mode: NONE
period\_step: 1
avail\_min: 5513
start\_threshold: 2147483647
stop\_threshold: 22050
silence\_threshold: 0
silence\_size: 0
boundary: 6206523236469964800


### No sound

If there's no sound, output channels may be muted. Unmute the channels, either by using the GUI environment's mixer, or by using alsamixer (from the [media-audio/alsa-utils](https://packages.gentoo.org/packages/media-audio/alsa-utils) package), selecting the appropriate channels and pressing the `M` key to mute or unmute:

`user $``alsamixer`

### Custom ALSA config results in no sound in browsers

Try explicitly specifying defaults in the configuration:

**`~/.asoundrc`**

Note that, since Firefox 52 (released in 2017), support for direct output to ALSA has been dropped, and PulseAudio has been made a hard requirement. To address this, enable Firefox's [pulseaudio](https://packages.gentoo.org/useflags/pulseaudio) [flag and install](https://wiki.gentoo.org/wiki/USE_flag) [apulse](https://wiki.gentoo.org/wiki/Apulse), which provides Pulse emulation for ALSA


### Sound card only available for one application

Sometimes one app essentially takes over all sound devices, e.g. for performance reasons.

To force the use of dmix instead of direct audio output (which is what most things, such as [Wine](https://wiki.gentoo.org/wiki/Wine), use by default), when the device is card 1 device 7:

**`~/.asoundrc`**

Use of \~/.asoundrc is immediate: as long as use of a specific device is not being forced by any applications, applications will either begin to produce audio output immediately, or will require a restart. One of the best tests is to open a browser, go to YouTube, open a terminal, and use an audio or video player to try to play an audio or video file: success is indicated by an absence of errors (e.g. "Device or resource busy").


### Missing dialogue/sound with 4.0 speakers

If using a 4.0 sound card (like an old SB Live), or 4.0 speakers in general, the dialogue in some games or movies might be very quiet or even missing. This is because most applications and movies support only either 2.0 (stereo) or 5.1 output. In order to achieve surround sound, the 5.1 audio track is used, but two channels are discarded: the center channel (which usually carries dialogue), and the subwoofer channel.

This issue can be circumvented by creating a virtual device which downmixes 5.1 to 4.0, mixing the center and subwoofer channels with other audio channels.

**`~/.asoundrc`**


### HDMI output from aplay has incorrect speaker channels

If MPlayer or VLC correctly plays a file in a 5.1 or 7.1 configuration over HDMI, but aplay doesn't, this might be caused by the snd\_hda\_intel HDMI audio module/driver, which is used by vendors other than Intel (e.g. Nvidia). Additionally, aplay might refuse to play the file if its format is 24-bit PCM 2.0/5.1 WAV.

To address these issues with minimal alterations to the PCM streams, remap the speaker channels. The following configuration is for both 5.1 and 7.1 audio. (If audio from a 7.1 stream should not be omitted, further map/copy the two extra channels to the 5.1 channels.) Additionally, as most HDMI-to Stereo-receiver connections only stream 16- and 32- bit formats, skipping 24-bit, the configuration up-mixes any PCM stream using the pcm.myHDMI profile to 32 bits.

**`/etc/asound.conf`**


### Weak center channel on PCM 5.1 live music

If a multi-channel soundtrack or piece of music has an apparently weak center channel, and the sound track is a live recording, it might be possible to map the center channel to the rear channels, e.g. when using [mplayer](https://wiki.gentoo.org/wiki/Mplayer):

`user $``mplayer -ao alsa:device=hw=1.7  Music/MyAlbum/PCM51-24bit/01.MyMusic.wav -channels 6 -format s32le -af channels=6:6:0:0:1:1:4:2:4:3:4:4:5:5`
The above incantation of [mplayer](https://wiki.gentoo.org/wiki/Mplayer) specifies:

- an HDMI device of `hw:1.7`;
- the PCM 5.1 audio file;
- the number of channels;
- the format (not needed if the receiver can natively handle 24 bit; receivers that can only natively handle 16- or 32-bit audio need to be upmixed); and
- the mapping.

The mapping specifies:

- a 6 channel audio stream, with 6 mappings immediately following, then to copy:
- the left front channel to left speaker;
- the right channel to right speaker;
- the center channel to left rear speaker;
- the center channel to right rear speaker;
- the center channel to center speaker; and
- the subwoofer channel to the subwoofer speaker.

Note that the rear channels on live recordings usually contain only the audience screaming, with very little music.

For further details, refer to the [mplayer(1)](https://man.archlinux.org/man/mplayer.1.en) [man page.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)


### Laptops with HDMI audio output

Some laptops with HDMI audio output will map HDMI as /proc/asound/card0, making it the default output device for applications. To change the device order, refer to [the "Kernel modules" section](https://wiki.gentoo.org/wiki/ALSA#Kernel_modules).


### Headset jack not working

Sometimes, to get a headset jack working, additional model information needs to be passed to the audio driver. For example, in case of a Dell Latitude E7470 laptop with snd-hda-intel driver, the following needs to be added to /etc/modprobe.d/alsa.conf:

**`/etc/modprobe.d/alsa.conf`**

More information can be found in [this section of the Linux kernel documentation](https://www.kernel.org/doc/html/latest/sound/hd-audio/models.html).


### udev/alsactl errors on boot

Due to partitioning, encryption, or having a [split /usr](https://wiki.gentoo.org/wiki/Split_/usr) system, these errors may appear on boot:

`root #``journalctl -b | grep alsa`
(udev-worker)\[2594\]: controlC2: Process '/usr/sbin/alsactl restore 2' failed with exit code 2.
 
(udev-worker)\[2611\]: controlC0: Process '/usr/sbin/alsactl restore 0' failed with exit code 2.
 
(udev-worker)\[2579\]: controlC1: Process '/usr/sbin/alsactl restore 1' failed with exit code 2.

To fix the issue, add `TEST=="@sbindir@/alsactl"` to /lib/udev/rules.d/90-alsa-restore.rules:

**`/lib/udev/rules.d/90-alsa-restore.rules`**

For further details and discussion, refer to [this discussion on alsa-devel](https://patchwork.kernel.org/project/alsa-devel/patch/1482964275.11185.34.camel@users.sourceforge.net/) and [this discussion on bugs.debian.org](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=636437).


### No sound after rebooting following a system update

If, after a system update followed by a reboot, sound is not working, resulting in e.g. [speaker-test(1)](https://man.archlinux.org/man/speaker-test.1.en) [producing an error like:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

ALSA lib /var/tmp/portage/media-libs/alsa-lib-1.2.14/work/alsa-lib-1.2.14/src/pcm/pcm\_dmix.c:1000:(snd\_pcm\_dmix\_open) unable to open slave
 Playback open error: -2,No such file or directory

and [alsaplayer(1)](https://man.archlinux.org/man/alsaplayer.1.en) [producing an error like:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

/usr/lib64/alsaplayer/output/libalsa\_out.so failed to load
 NOTE: THIS IS THE NULL PLUGIN.      YOU WILL NOT HEAR SOUND!!

It might be that a stale /var/lib/alsa/asound.state file is present (e.g. due to the format of that file changing between kernel versions). Remove that file and reboot.
