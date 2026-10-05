<!-- source: https://wiki.gentoo.org/wiki/MPD | group: Gentoo Wiki (Main) | wiki-title: MPD -->
---
title: MPD
url: https://wiki.gentoo.org/wiki/MPD
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-28"
fingerprint: fe005b58599e39e6
license: CC BY-SA 4.0
---

# MPD

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

**MPD** (**M**usic **P**layer **D**aemon) is a flexible, server-side application for playing music. Through plugins and libraries it can play a variety of sound files while being controlled by a network protocol.


## Installation


### USE flags


| [+alsa](https://packages.gentoo.org/useflags/+alsa) | Add support for media-libs/alsa-lib (Advanced Linux Sound Architecture) | 
| [+curl](https://packages.gentoo.org/useflags/+curl) | Enable web stream listening support via net-misc/curl | 
| [+dbus](https://packages.gentoo.org/useflags/+dbus) | Enable dbus support for anything that needs it (gpsd, gnomemeeting, etc) | 
| [+eventfd](https://packages.gentoo.org/useflags/+eventfd) | Use the eventfd function in MPD's event loop | 
| [+ffmpeg](https://packages.gentoo.org/useflags/+ffmpeg) | Enable ffmpeg/libav-based audio/video codec support | 
| [+icu](https://packages.gentoo.org/useflags/+icu) | Enable ICU (Internationalization Components for Unicode) support, using dev-libs/icu | 
| [+id3tag](https://packages.gentoo.org/useflags/+id3tag) | Enable support for ID3 tags via media-libs/libid3tag | 
| [+inotify](https://packages.gentoo.org/useflags/+inotify) | Use the Linux kernel inotify subsystem to notice changes to mpd music library | 
| [+io-uring](https://packages.gentoo.org/useflags/+io-uring) | Enable the use of io\_uring for efficient asynchronous IO and system requests | 
| [+mpg123](https://packages.gentoo.org/useflags/+mpg123) | Enable support for mp3 decoding over media-sound/mpg123 | 
| [ao](https://packages.gentoo.org/useflags/ao) | Use libao audio output library for sound playback | 
| [audiofile](https://packages.gentoo.org/useflags/audiofile) | Add support for libaudiofile where applicable | 
| [bzip2](https://packages.gentoo.org/useflags/bzip2) | Enable bzip2 compression support | 
| [cdio](https://packages.gentoo.org/useflags/cdio) | Enable ISO9660 parsing via dev-libs/libcdio-paranoia | 
| [chromaprint](https://packages.gentoo.org/useflags/chromaprint) | Enable ChromaPrint / AcoustID support | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [expat](https://packages.gentoo.org/useflags/expat) | Enable the use of dev-libs/expat for XML parsing | 
| [faad](https://packages.gentoo.org/useflags/faad) | Use media-libs/faad2 for AAC decoding | 
| [flac](https://packages.gentoo.org/useflags/flac) | Add support for FLAC: Free Lossless Audio Codec | 
| [fluidsynth](https://packages.gentoo.org/useflags/fluidsynth) | Enable MIDI support with media-sound/fluidsynth (discouraged) | 
| [gme](https://packages.gentoo.org/useflags/gme) | Enable video game music formats support via media-libs/game-music-emu | 
| [httpd](https://packages.gentoo.org/useflags/httpd) | Enable built-in stream server | 
| [jack](https://packages.gentoo.org/useflags/jack) | Add support for the JACK Audio Connection Kit | 
| [lame](https://packages.gentoo.org/useflags/lame) | Support for MP3 streaming via Icecast2 | 
| [libmpdclient](https://packages.gentoo.org/useflags/libmpdclient) | Enable support for remote mpd databases | 
| [libsamplerate](https://packages.gentoo.org/useflags/libsamplerate) | Build with support for converting sample rates using libsamplerate | 
| [libsoxr](https://packages.gentoo.org/useflags/libsoxr) | Enable resampling with media-libs/soxr | 
| [mad](https://packages.gentoo.org/useflags/mad) | Add support for mad (high-quality mp3 decoder library and cli frontend) | 
| [mikmod](https://packages.gentoo.org/useflags/mikmod) | Add libmikmod support to allow playing of SoundTracker-style music files | 
| [mms](https://packages.gentoo.org/useflags/mms) | Support for Microsoft Media Server (MMS) streams | 
| [modplug](https://packages.gentoo.org/useflags/modplug) | Add libmodplug support for playing SoundTracker-style music files | 
| [musepack](https://packages.gentoo.org/useflags/musepack) | Enable support for the musepack audio codec | 
| [nfs](https://packages.gentoo.org/useflags/nfs) | Enable support for the Network File System | 
| [openal](https://packages.gentoo.org/useflags/openal) | Add support for the Open Audio Library | 
| [openmpt](https://packages.gentoo.org/useflags/openmpt) | OpenMPT decoder plugin | 
| [opus](https://packages.gentoo.org/useflags/opus) | Enable Opus audio codec support | 
| [oss](https://packages.gentoo.org/useflags/oss) | Add support for OSS (Open Sound System) | 
| [pipewire](https://packages.gentoo.org/useflags/pipewire) | Enable support for PipeWire audio backend | 
| [pulseaudio](https://packages.gentoo.org/useflags/pulseaudio) | Add sound server support via media-libs/libpulse (may be PulseAudio or PipeWire) | 
| [qobuz](https://packages.gentoo.org/useflags/qobuz) | Build plugin to access qobuz | 
| [recorder](https://packages.gentoo.org/useflags/recorder) | Enables output plugin for recording radio streams | 
| [samba](https://packages.gentoo.org/useflags/samba) | Add support for SAMBA (Windows File and Printer sharing) | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [shout](https://packages.gentoo.org/useflags/shout) | Enable ShoutCast/IceCast plugin using media-libs/libshout | 
| [sid](https://packages.gentoo.org/useflags/sid) | Enable SID (Commodore 64 audio) file support | 
| [signalfd](https://packages.gentoo.org/useflags/signalfd) | Use the signalfd function in MPD's event loop | 
| [snapcast](https://packages.gentoo.org/useflags/snapcast) | Snapcast audio plugin | 
| [sndfile](https://packages.gentoo.org/useflags/sndfile) | Add support for libsndfile | 
| [sndio](https://packages.gentoo.org/useflags/sndio) | Enable support for the media-sound/sndio backend | 
| [sqlite](https://packages.gentoo.org/useflags/sqlite) | Add support for sqlite - embedded sql database | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable support for systemd socket activation | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [tremor](https://packages.gentoo.org/useflags/tremor) | Enable support for media-libs/tremor, a fixed-point version of the Ogg Vorbis decoder | 
| [twolame](https://packages.gentoo.org/useflags/twolame) | Enable MP2 encoding via media-sound/twolame | 
| [upnp](https://packages.gentoo.org/useflags/upnp) | Enable UPnP port mapping support | 
| [vorbis](https://packages.gentoo.org/useflags/vorbis) | Add support for the OggVorbis audio codec | 
| [wav](https://packages.gentoo.org/useflags/wav) | Support WAV encoding | 
| [wavpack](https://packages.gentoo.org/useflags/wavpack) | Add support for wavpack audio compression tools | 
| [webdav](https://packages.gentoo.org/useflags/webdav) | Enable using music from a WebDAV share | 
| [wildmidi](https://packages.gentoo.org/useflags/wildmidi) | Enable MIDI support via media-sound/wildmidi | 
| [zeroconf](https://packages.gentoo.org/useflags/zeroconf) | Support for DNS Service Discovery (DNS-SD) | 
| [zip](https://packages.gentoo.org/useflags/zip) | Enable support for ZIP archives | 
| [zlib](https://packages.gentoo.org/useflags/zlib) | Enable database compression with virtual/zlib | 


### Emerge

`root #``emerge --ask media-sound/mpd`

## Configuration

After installation, MPD should work "out of the box", via Gentoo's default configuration.

A list of supported features/plugins can be obtained by running:

`user $``mpd --version`
MPD can be configured system-wide or per-user, depending on the intended usage.


### System-wide configuration


#### Basic configuration

An example of a simple configuration:

**`/etc/mpd.conf`**

With this configuration, MPD should be able to run as a system daemon under the `mpd` user, which is the default setting.


### Per-user configuration


#### Basic configuration

Copy /etc/mpd.conf to ${XDG\_CONFIG\_HOME}/mpd/mpd.conf, then modify the latter as appropriate.

Sample configuration using PulseAudio output:

**`${XDG_CONFIG_HOME}/mpd/mpd.conf`**

Now it should be possible to start MPD simply by running:

`user $``mpd`

#### PipeWire (optional)

If MPD was built with the [pipewire](https://packages.gentoo.org/useflags/pipewire) [USE flag), add a dedicated](https://wiki.gentoo.org/wiki/USE_flag) `audio_output` section to ${XDG\_CONFIG\_HOME}/mpd/mpd.conf:

**`/etc/mpd.conf`**


### General configuration


#### PulseAudio (optional)

If MPD was built with the [pulseaudio](https://packages.gentoo.org/useflags/pulseaudio) [USE flag, a dedicated](https://wiki.gentoo.org/wiki/USE_flag) `audio_output` section can be added to the configuration file:


#### HTTP streaming server (optional)

If MPD was built with the [httpd](https://packages.gentoo.org/useflags/httpd) [USE flag, a dedicated](https://wiki.gentoo.org/wiki/USE_flag) `audio_output` section can be added to the configuration file:

Binding to `0.0.0.0` enables streaming on all discovered local IP interfaces; binding to a specific IP address such as `192.168.1.2` will enable streaming via only that IP.


#### Bluetooth headset (optional)

To set up a Bluetooth headset, first follow the [Bluetooth headset](https://wiki.gentoo.org/wiki/Bluetooth_headset) page.


##### Pure ALSA setup

For Bluetooth support on a pure [ALSA](https://wiki.gentoo.org/wiki/ALSA) setup, add an ALSA configuration file (e.g. /etc/asound.conf or ${HOME}/.asoundrc for a system-wide or per-user configuration, respectively) to allow MPD to control the volume of the headset, changing the values of the `interface` and `device` settings as appropriate:

To be able to switch between the Bluetooth headset and the default sound card, add the following to the `audio_output` section in the MPD configuration file:

If trying to play AAC or other files results in an error such as:

`user $``mpd --no-daemon --stderr`
...
alsa\_output: Failed to open "My ALSA Device" \[alsa\]: Error opening ALSA device "bluealsa" (snd\_pcm\_hw\_params): Invalid argument
...

then add the following line to the relevant section of the relevant MPD configuration file:


#### FIFO (optional)

Use the following to set up a [fifo(7)](https://man.archlinux.org/man/fifo.7.en) [for](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [ncmpcpp](https://wiki.gentoo.org/wiki/Ncmpcpp)'s visualiser plugin, modifying the values of the `name` and `path` variables as appropriate:


### Services


#### System-wide service


##### OpenRC

To start MPD:

`root #``rc-service mpd start`
To start MPD at boot, add it the default runlevel:

`root #``rc-update add mpd default`

##### systemd

To start MPD immediately:

`root #``systemctl start mpd`
To start MPD at boot:

`root #``systemctl enable mpd`

#### Per-user service


##### systemd

It is possible to leverage systemd [user services](https://wiki.gentoo.org/wiki/Systemd#User_services) to control MPD per-user.

To activate MPD, start the systemd socket:

`user $``systemctl --user enable --now mpd.socket`

## Clients


### Packaged for Gentoo

| Name | Package | Toolkit | Description | 
|---|---|---|---|
| [ampc](https://github.com/chep/ampc) | [app-emacs/ampc::gnu-elpa](https://repos.gentoo.org/#gnu-elpa) | Other | Asynchronous Music Player Controller \[for [Emacs](https://wiki.gentoo.org/wiki/Emacs)\] | 
| [Ario](https://sourceforge.net/projects/ario-player/) | [media-sound/ario](https://packages.gentoo.org/packages/media-sound/ario) | [GTK](https://wiki.gentoo.org/wiki/GTK) | GTK client for MPD inspired by Rhythmbox but much lighter and faster | 
| [Cantata](https://github.com/nullobsi/cantata) | [media-sound/cantata](https://packages.gentoo.org/packages/media-sound/cantata) | [Qt](https://wiki.gentoo.org/wiki/Qt) | Featureful and configurable Qt client for the music player daemon (MPD) | 
| [Gimmix](https://launchpad.net/gimmix) | [media-sound/gimmix](https://packages.gentoo.org/packages/media-sound/gimmix) | [GTK](https://wiki.gentoo.org/wiki/GTK) | Graphical music player daemon (MPD) client using GTK+2 | 
| [Glurp](https://sourceforge.net/projects/glurp/) | [media-sound/glurp](https://packages.gentoo.org/packages/media-sound/glurp) | [GTK](https://wiki.gentoo.org/wiki/GTK) | GTK2 based graphical client for the Music Player Daemon | 
| [MPC](https://www.musicpd.org/) | [media-sound/mpc](https://packages.gentoo.org/packages/media-sound/mpc) | CLI/TUI | Commandline client for Music Player Daemon (media-sound/mpd) | 
| [ncmpc](https://www.musicpd.org/clients/ncmpc/) | [media-sound/ncmpc](https://packages.gentoo.org/packages/media-sound/ncmpc) | CLI/TUI | Ncurses client for the Music Player Daemon (MPD)\] | 
| [ncmpcpp](https://wiki.gentoo.org/wiki/Ncmpcpp) | [media-sound/ncmpcpp](https://packages.gentoo.org/packages/media-sound/ncmpcpp) | CLI/TUI | An ncurses mpd client, ncmpc clone with some new features, written in C++\] | 
| PMS | [media-sound/pms](https://packages.gentoo.org/packages/media-sound/pms) | CLI/TUI | Practical Music Search: open source ncurses client for mpd, written in C++ | 
| [Quimup](https://quimup.sourceforge.io) | [media-sound/quimup](https://packages.gentoo.org/packages/media-sound/quimup) | [Qt](https://wiki.gentoo.org/wiki/Qt) | Qt client for the music player daemon (MPD) | 
| [Rusty Music Player Client](https://rmpc.mierak.dev/) | [media-sound/rmpc::guru](https://github.com/gentoo-mirror/guru/tree/master/media-sound/rmpc) | CLI/TUI | Beautiful and configurable TUI client for MPD | 
| [Sonata](https://github.com/multani/sonata) | [media-sound/sonata](https://packages.gentoo.org/packages/media-sound/sonata) | [GTK](https://wiki.gentoo.org/wiki/GTK) | Elegant GTK+ music client for the Music Player Daemon (MPD) | 
| [vimpc](https://github.com/boysetsfrog/vimpc) | [media-sound/vimpc](https://packages.gentoo.org/packages/media-sound/vimpc) | CLI/TUI | ncurses based mpd client with vi-like key bindings | 
| [wmmp](https://github.com/yogsothoth/wmmp) | [x11-plugins/wmmp](https://packages.gentoo.org/packages/x11-plugins/wmmp) | Other | Window Maker dock app client for mpd (Music Player Daemon) | 
| [xfmpc](https://gitlab.xfce.org/apps/xfmpc/) | [media-sound/xfmpc](https://packages.gentoo.org/packages/media-sound/xfmpc) | [GTK](https://wiki.gentoo.org/wiki/GTK) | Music Player Daemon (MPD) client for the [Xfce](https://wiki.gentoo.org/wiki/Xfce) desktop environment | 
| [ympd](https://ympd.org/) | [media-sound/ympd::bgo-overlay](https://repos.gentoo.org/#bgo-overlay) | Web | Standalone MPD Web GUI written in C, utilizing Websockets and Bootstrap/JS | 


### Not yet packaged for Gentoo

| Name | Toolkit | Description | 
|---|---|---|
| [clerk](https://github.com/carnager/clerk) | CLI/TUI | mpd client, based on rofi/fzf | 
| [vimus](https://hackage.haskell.org/package/vimus) | CLI/TUI | MPD client with vim-like key bindings | 

The MPD site also provides a [sorted list of MPD clients](https://www.musicpd.org/clients/).


### Scrobblers

Scrobblers are clients that submit to [Audioscrobbler](https://www.audioscrobbler.net/) — compatible services (eg. [libre.fm](https://libre.fm) or [last.fm](https://last.fm))


## Troubleshooting

For general troubleshooting refer to the [MPD user manual troubleshooting section](https://mpd.readthedocs.io/en/stable/user.html#troubleshooting)


### No Audio from PulseAudio

Running MPD as a system service under the `mpd` user causes permissions issues. Change the user in mpd.conf to the appropriate user:

**`/etc/mpd.conf`**


## See also

- [Snapcast](https://wiki.gentoo.org/wiki/Snapcast) — a multiroom client-server audio player, where all clients are time synchronized with the server to play perfectly synced audio. Can be configured as a 'frontend' to MPD.
- [Upmpdcli](https://wiki.gentoo.org/wiki/Upmpdcli) — a free and open source UPnP media renderer front-end for Music Player Daemon (MPD).
