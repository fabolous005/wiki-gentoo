<!-- source: https://wiki.gentoo.org/wiki/PipeWire | group: Gentoo Wiki (Main) | wiki-title: PipeWire -->
---
title: PipeWire
url: https://wiki.gentoo.org/wiki/PipeWire
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-04"
fingerprint: "9f238b5a896638c0"
license: CC BY-SA 4.0
---

# PipeWire

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

**PipeWire** is a low-latency, graph-based, processing engine and server, for interfacing with audio and video devices. It can be used to support use-cases currently handled by [ALSA](https://wiki.gentoo.org/wiki/ALSA), [PulseAudio](https://wiki.gentoo.org/wiki/PulseAudio), and/or [JACK](https://wiki.gentoo.org/wiki/JACK), and aims to improve handling of audio and video under Linux.

Some key features of PipeWire include:

- Minimal latency capture/playback of audio *and* video.
- Real-time multimedia processing.
- Multi-process architecture allowing multimedia content sharing between applications.
- Seamless support for PulseAudio, JACK, ALSA, and [GStreamer](https://wiki.gentoo.org/wiki/GStreamer).
- Applications sandboxing support with [Flatpak](https://wiki.gentoo.org/wiki/Flatpak), with a security model that facilitates interacting containerized applications.

Most applications - including e.g. [Firefox](https://wiki.gentoo.org/wiki/Firefox) - don't yet support PipeWire's native API. However, PipeWire can emulate PulseAudio, thus removing the need for a separate PulseAudio sound server. Refer to [the "USE flags" section](https://wiki.gentoo.org/wiki/PipeWire#USE_flags) for details.

PipeWire currently ships a PipeWire daemon, an example session manager, tools to introspect and use the PipeWire daemon, a library to develop PipeWire applications and plugins, and the [SPA (Simple Plugin API)](https://docs.pipewire.org/page_spa.html) used by both the PipeWire daemon and the PipeWire library.

PipeWire users will typically need to install and use [WirePlumber](https://wiki.gentoo.org/wiki/WirePlumber) for session/policy management functionality, such as volume management; refer to that page for details.

Device Drivers  --->
  \<\*> Sound card support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SOUND\</code> to find this item.  --->
    \<\*> Advanced Linux Sound Architecture [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\</code> to find this item.  --->
      -\*-  Sound Proc FS Support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_PROC\_FS\</code> to find this item.
      \[\*\]    Verbose procfs contents [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_VERBOSE\_PROCFS\</code> to find this item.

All desktop profiles now enable PipeWire by default, so no installation should be required.

systemd users need to enable the `wireplumber` service by following [this section](https://wiki.gentoo.org/wiki/PipeWire#systemd).

To use PipeWire as a sound server, specify the [sound-server](https://packages.gentoo.org/useflags/sound-server) [USE flag](https://wiki.gentoo.org/wiki/USE_flag) on [media-video/pipewire](https://packages.gentoo.org/packages/media-video/pipewire)<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.

Then, if using PipeWire as a replacement for the PulseAudio sound server:

1. Ensure [media-sound/pulseaudio-daemon](https://packages.gentoo.org/packages/media-sound/pulseaudio-daemon) is not installed. This is necessary to avoid issues resulting from running more than one sound server.
2. Ensure [media-libs/libpulse](https://packages.gentoo.org/packages/media-libs/libpulse) is installed, which will allow PipeWire to emulate a PulseAudio sound server. Not many applications currently support PipeWire's native API.
3. Ensure the [pulseaudio](https://packages.gentoo.org/useflags/pulseaudio)

If compiled with the [dbus](https://packages.gentoo.org/useflags/dbus) [USE flag enabled, PipeWire requires the presence of a](https://wiki.gentoo.org/wiki/USE_flag) [D-Bus](https://wiki.gentoo.org/wiki/D-Bus) session bus and an XDG-compliant environment. Both requirements should be met by a desktop [profile](https://wiki.gentoo.org/wiki/Portage/Profiles); on such systems, starting PipeWire is as simple as running the pipewire binary. On other profiles, if using OpenRC, permissions requirements might also need the [elogind](https://packages.gentoo.org/useflags/elogind) [USE flag on the](https://wiki.gentoo.org/wiki/USE_flag) [media-video/wireplumber](https://packages.gentoo.org/packages/media-video/wireplumber) package, together with [elogind](https://wiki.gentoo.org/wiki/Elogind) itself.

D-Bus is required for [Bluetooth](https://wiki.gentoo.org/wiki/Bluetooth) support, interacting with RTKit to acquire real-time priorities, and interacting with other multimedia clients.<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup> D-Bus is also used by KDE Plasma to notify PipeWire of volume changes.

To enable direct screencasting support on applications offering it, specify the [screencast](https://packages.gentoo.org/useflags/screencast) [USE flag on the relevant packages. Otherwise, screencasting support may also be provided through the PulseAudio or JACK compatibility layers.](https://wiki.gentoo.org/wiki/USE_flag)


| [+man](https://packages.gentoo.org/useflags/+man) | Build and install man pages | 
| [X](https://packages.gentoo.org/useflags/X) | Enable audible bell for X11 | 
| [bluetooth](https://packages.gentoo.org/useflags/bluetooth) | Enable Bluetooth Support | 
| [dbus](https://packages.gentoo.org/useflags/dbus) | Enable dbus support for anything that needs it (gpsd, gnomemeeting, etc) | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [echo-cancel](https://packages.gentoo.org/useflags/echo-cancel) | Enable WebRTC-based echo canceller via media-libs/webrtc-audio-processing | 
| [elogind](https://packages.gentoo.org/useflags/elogind) | Enable session tracking via sys-auth/elogind | 
| [extra](https://packages.gentoo.org/useflags/extra) | Build pw-cat/pw-play/pw-record | 
| [ffmpeg](https://packages.gentoo.org/useflags/ffmpeg) | Enable ffmpeg/libav-based audio/video codec support | 
| [fftw](https://packages.gentoo.org/useflags/fftw) | Use FFTW library for computing Fourier transforms | 
| [flatpak](https://packages.gentoo.org/useflags/flatpak) | Enable Flatpak support | 
| [gsettings](https://packages.gentoo.org/useflags/gsettings) | Use gsettings (dev-libs/glib) to read/save used modules (useful for e.g. media-sound/paprefs | 
| [gstreamer](https://packages.gentoo.org/useflags/gstreamer) | Add support for media-libs/gstreamer (Streaming media) | 
| [ieee1394](https://packages.gentoo.org/useflags/ieee1394) | Enable FireWire/iLink IEEE1394 support (dv, camera, ...) | 
| [jack-client](https://packages.gentoo.org/useflags/jack-client) | Install a plugin for running PipeWire as a JACK client | 
| [jack-sdk](https://packages.gentoo.org/useflags/jack-sdk) | Use PipeWire as JACK replacement | 
| [libcamera](https://packages.gentoo.org/useflags/libcamera) | Enable libcamera plugin via media-libs/libcamera | 
| [loudness](https://packages.gentoo.org/useflags/loudness) | Enable loudness normalisation according to the EBU R128 standard using media-libs/libebur128 | 
| [lv2](https://packages.gentoo.org/useflags/lv2) | Allow loading LV2 plugins via media-libs/lv2 | 
| [modemmanager](https://packages.gentoo.org/useflags/modemmanager) | Combined with USE=bluetooth, allows PipeWire to perform telephony on mobile devices. | 
| [pipewire-alsa](https://packages.gentoo.org/useflags/pipewire-alsa) | Install ALSA plugin, similar to media-plugins/alsa-plugins's USE=pulseaudio. | 
| [pulseaudio](https://packages.gentoo.org/useflags/pulseaudio) | Add sound server support via media-libs/libpulse (may be PulseAudio or PipeWire) | 
| [readline](https://packages.gentoo.org/useflags/readline) | Enable support for libreadline, a GNU line-editing library that almost everyone wants | 
| [roc](https://packages.gentoo.org/useflags/roc) | Enable roc support for real-time audio streaming over the network, using media-libs/roc-toolkit. See https://gitlab.freedesktop.org/pipewire/pipewire/-/wikis/Network#roc | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [sofa](https://packages.gentoo.org/useflags/sofa) | Spatially Oriented Format for Acoustics (SOFA) support via media-libs/libmysofa | 
| [sound-server](https://packages.gentoo.org/useflags/sound-server) | Provide sound server using ALSA and bluetooth devices | 
| [ssl](https://packages.gentoo.org/useflags/ssl) | Enable raop-sink support (needs dev-libs/openssl) | 
| [system-service](https://packages.gentoo.org/useflags/system-service) | Install systemd unit files for running as a system service. Not recommended. | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [v4l](https://packages.gentoo.org/useflags/v4l) | Enable support for video4linux (using linux-headers or userspace libv4l libraries) | 
| [zeroconf](https://packages.gentoo.org/useflags/zeroconf) | Support for DNS Service Discovery (DNS-SD) | 

Once the USE flags have been specified, rebuild the affected packages:

`root #``emerge --ask --verbose --changed-use --update --deep @world`
Alternatively, PipeWire may be emerged independently, though the previous method is usually what is required:

`root #``emerge --ask media-video/pipewire`
PipeWire recognizes multiple environment variables that allow settings to be changed, either per-user, or for individual commands: for example, `PIPEWIRE_RUNTIME_DIR`, `PIPEWIRE_MODULE_DIR`, and `DISABLE_RTKIT`. Refer to the [pipewire(1)](https://man.archlinux.org/man/pipewire.1.en) [man page for a complete list.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

It's recommended that users are in the `pipewire` group.

`root #``usermod -aG pipewire larry`
PipeWire's default configuration tries to use *realtime* scheduling to increase audio thread priorities. If the user doesn't have the necessary permissions for this, the configuration will try to use [RealtimeKit](https://gitlab.freedesktop.org/pipewire/rtkit/) (RTKit) instead, such that [sys-auth/rtkit](https://packages.gentoo.org/packages/sys-auth/rtkit) will need to be installed. This behavior is defined under the `context.modules` portion of PipeWire's configuration.

`root #``emerge --ask sys-auth/rtkit`
In general, for the best experience with fast user switching, users should not be in the `audio` group, in order to avoid a user application being able to take exclusive control of the audio device. Exceptions include systems that use [seatd](https://wiki.gentoo.org/wiki/Seatd) or that rely on the `audio` group for device access control / ACLs.

To remove a user from the `audio` group:

`root #``usermod -rG audio larry`
Use pw-config to output the current configuration paths, and use pw-config list to list the current configuration.

The default configuration should be fine for most users. This configuration is described in /usr/share/pipewire/pipewire.conf.

If customization is required, do *not* modify that file. Instead, copy it to either or both of:

- /etc/pipewire/, for system-wide configuration; or
- ${XDG\_CONFIG\_HOME}/pipewire/, for per-user configuration,

and modify either or both of those files as appropriate.

By default, `XDG_CONFIG_HOME` is \~/.config/. Refer to the [XDG/Base\_Directories](https://wiki.gentoo.org/wiki/XDG/Base_Directories) page for further information.

Configuration fragments can be specified via a file with a .conf extension (e.g. 90-local.conf) in the following directories<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup>:

1. /usr/share/pipewire/pipewire.conf.d/
2. /etc/pipewire/pipewire.conf.d/
3. ${XDG\_CONFIG\_HOME}/pipewire/pipewire.conf.d/

User services are available for both systemd and OpenRC. Those not using systemd or OpenRC can instead use [PipeWire/gentoo-pipewire-launcher](https://wiki.gentoo.org/wiki/PipeWire/gentoo-pipewire-launcher).

PipeWire provides socket and service files when built with the [systemd](https://packages.gentoo.org/useflags/systemd) [USE flag.](https://wiki.gentoo.org/wiki/USE_flag)

If the PulseAudio user service is enabled, disable it; this is safe to do even if the user service was not in use.

`user $``systemctl --user disable --now pulseaudio.socket pulseaudio.service`
While PipeWire does not appear to utilize the \~/.config/pulse/ directory beyond the cookie file, it may be a good idea to rename or delete it.

Enable the pipewire-pulse socket; enabling the `pipewire-pulse` socket will cause the `pipewire-pulse` service to be started if required. That service will in turn start the `pipewire` service.

`user $``systemctl --user enable --now pipewire-pulse.socket`
Socket activation means the `pipewire` service will only be started when required, which is usually sufficient. However, the `pipewire` service can be always started when the user logs in by enabling pipewire.service:

`user $``systemctl --user enable --now pipewire.service`
Enable the `wireplumber` service:

`user $``systemctl --user enable --now wireplumber.service`
In these cases, the `--now` flag is optional, but probably safe to use, as starting PipeWire with default configuration merely allows using new interfaces and doesn't change the existing ones, i.e. non-PipeWire clients continue using the same libraries and services they were using previously.

OpenRC has built-in and enabled by default support for [user services](https://wiki.gentoo.org/wiki/OpenRC#User_services) since version 0.60. As with systemd, they can be used to start and stop PipeWire and [WirePlumber](https://wiki.gentoo.org/wiki/WirePlumber) on login and logout.

To enable these services:

`user $````
rc-update add -U pipewire default
```
`user $````
rc-update add -U pipewire-pulse default
```
`user $````
rc-update add -U wireplumber default
```
To start the services without enabling them:

`user $````
rc-service --user pipewire start
```
`user $````
rc-service --user pipewire-pulse start
```
`user $````
rc-service --user wireplumber start
```
To confirm PulseAudio server emulation:

`user $``LANG=C pactl info | grep "Server Name"`
Server Name: PulseAudio (on PipeWire 0.3.39)

Multi-user support requires the UNIX socket interface.

If there is not yet a pipewire-pulse.conf file in /etc/pipewire/, add it (creating the /etc/pipewire/ directory if necessary):

`root #``cp /usr/share/pipewire/pipewire-pulse.conf /etc/pipewire/`
Then edit /etc/pipewire/pipewire-pulse.conf to specify the UNIX socket location, which must match the [PulseAudio client configuration](https://wiki.gentoo.org/wiki/PulseAudio#Allow_multiple_users_to_use_PulseAudio_concurrently):

**`/etc/pipewire/pipewire-pulse.conf`**

**PulseAudio UNIX socket**

For information about more advanced PipeWire configuration, refer to the [PipeWire/extra](https://wiki.gentoo.org/wiki/PipeWire/extra) page.

A command-line interface to PipeWire is provided by [pw-cli(1)](https://man.archlinux.org/man/pw-cli.1.en)[. This tool can be used to e.g. list the IDs of PipeWire nodes:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`user $``pw-cli ls Node`
and to check the current properties of a given node:

`user $``pw-cli e <node-id> Props`
Ways to control the volume - which on typical setups will be managed by [WirePlumber](https://wiki.gentoo.org/wiki/WirePlumber) - include:

- [media-sound/pwvucontrol](https://packages.gentoo.org/packages/media-sound/pwvucontrol), a pavucontrol-like GUI.

- [wiremix](https://wiki.gentoo.org/wiki/Wiremix), a TUI.

- [media-sound/pipemixer::guru](https://github.com/gentoo-mirror/guru/tree/master/media-sound/pipemixer), a TUI.

- PulseAudio tools such as [media-sound/pavucontrol](https://packages.gentoo.org/packages/media-sound/pavucontrol) and [pactl(1)](https://man.archlinux.org/man/pactl.1.en)[media-libs/libpulse](https://packages.gentoo.org/packages/media-libs/libpulse)).

- `user $``pw-cli s <node-id> Props '{ mute: false, channelVolumes: [ 0.3, 0.3 ] }'`

pw-metadata can be used to check the current sample rate and other settings:

`user $``pw-metadata -n settings`
Found "settings" metadata 31
update: id:0 key:'log.level' value:'2' type:''
update: id:0 key:'clock.rate' value:'192000' type:''
update: id:0 key:'clock.allowed-rates' value:'\[ 192000, 48000, 44100 \]' type:''
update: id:0 key:'clock.quantum' value:'1024' type:''
update: id:0 key:'clock.min-quantum' value:'32' type:''
update: id:0 key:'clock.max-quantum' value:'2048' type:''
update: id:0 key:'clock.force-quantum' value:'0' type:''
update: id:0 key:'clock.force-rate' value:'0' type:''

If PipeWire is being used as a PulseAudio backend, the sample rate and bit depth, or Default Sample Specification, can be checked with:

`user $``pactl info`
Server String: /run/user/1000/pulse/native
Library Protocol Version: 35
Server Protocol Version: 35
Is Local: yes
Client Index: 213
Tile Size: 65472
User Name: larry
Host Name: gentoo
Server Name: PulseAudio (on PipeWire 0.3.71)
Server Version: 15.0.0
Default Sample Specification: float32le 2ch 192000Hz
Default Channel Map: front-left,front-right
Default Sink: alsa\_output.usb-Generic\_USB\_Audio-00.pro-output-2
Default Source: alsa\_input.usb-Focusrite\_Scarlett\_Solo\_USB-00.pro-input-0

GUI patchbays available via the [gentoo](https://repos.gentoo.org/#gentoo) repository include:

- [media-sound/helvum](https://packages.gentoo.org/packages/media-sound/helvum), a [GTK](https://wiki.gentoo.org/wiki/GTK)-based patchbay.

- [media-sound/qpwgraph](https://packages.gentoo.org/packages/media-sound/qpwgraph), a [Qt](https://wiki.gentoo.org/wiki/Qt)-based patchbay that can save layout.

Additionally, [coppwr](https://github.com/dimtpap/coppwr) is a low-level patchbay available via [Flatpak](https://wiki.gentoo.org/wiki/Flatpak), [io.github.dimtpap.coppwr](https://flathub.org/en/apps/io.github.dimtpap.coppwr).

For information about more advanced PipeWire usage, refer to the [PipeWire/extra](https://wiki.gentoo.org/wiki/PipeWire/extra) page.

If the [jack-sdk](https://packages.gentoo.org/useflags/jack-sdk) [USE flag is enabled, PipeWire can be used as the server for](https://wiki.gentoo.org/wiki/USE_flag) [JACK](https://wiki.gentoo.org/wiki/JACK) clients; calls to the JACK API will be translated into calls to PipeWire's native API. Clients can be connected via a patchbay interface such as [qjackctl(1)](https://man.archlinux.org/man/qjackctl.1.en)[. Refer to](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [pipewire-jack.conf(5)](https://man.archlinux.org/man/pipewire-jack.conf.5.en) [for information about configuring PipeWire for JACK clients.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

When PipeWire is configured this way, the [media-sound/jack2](https://packages.gentoo.org/packages/media-sound/jack2) package must be uninstalled; /usr/lib/libjack.so will be owned by the [media-video/pipewire](https://packages.gentoo.org/packages/media-video/pipewire) package. This can be checked via e.g. qfile (provided by [app-portage/portage-utils](https://packages.gentoo.org/packages/app-portage/portage-utils)) or equery (provided by [app-portage/gentoolkit](https://packages.gentoo.org/packages/app-portage/gentoolkit)).

If the [jack-sdk](https://packages.gentoo.org/useflags/jack-sdk) [USE flag is not enabled,](https://wiki.gentoo.org/wiki/USE_flag) [media-sound/jack2](https://packages.gentoo.org/packages/media-sound/jack2) will be installed (and its [dbus](https://packages.gentoo.org/useflags/dbus) [USE flag must be enabled). In this case, individual JACK clients can be run via](https://wiki.gentoo.org/wiki/USE_flag) [pw-jack(1)](https://man.archlinux.org/man/pw-jack.1.en)[, e.g. pw-jack qjackctl. When using pw-jack, do](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) **not** use either [jackd(1)](https://man.archlinux.org/man/jackd.1.en) [nor jackdbus.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

Not every client will necessarily work; some may even ungracefully exit due to missing symbols. Re-configuration of JACK clients might be required.

Refer to [PipeWire/troubleshooting](https://wiki.gentoo.org/wiki/PipeWire/troubleshooting).

- [WirePlumber](https://wiki.gentoo.org/wiki/WirePlumber) — a modular session / policy manager for [PipeWire]
- [PulseAudio](https://wiki.gentoo.org/wiki/PulseAudio) — a multi-platform, open source, *sound server* that provides a number of features on top of the low-level audio interface [ALSA](https://wiki.gentoo.org/wiki/ALSA)
- [ALSA](https://wiki.gentoo.org/wiki/ALSA) — the Linux kernel's API for sound cards, together with an associated software framework
- [Technical notes on the packaging of PipeWire](https://wiki.gentoo.org/wiki/User:Sam/PipeWire_changes)

- [Pipewire Guide](https://github.com/mikeroyal/PipeWire-Guide/blob/main/README.md)
- [PipeWire FAQ](https://gitlab.freedesktop.org/pipewire/pipewire/-/wikis/FAQ)
- [Introduction to Pipewire](https://fedoramagazine.org/introduction-to-pipewire/) (2025-02-07)
- [An introduction to PipeWire](https://bootlin.com/blog/an-introduction-to-pipewire/) (2022-06-20)
- [PipeWire under the hood](https://venam.nixers.net/blog/unix/2021/06/23/pipewire-under-the-hood.html) (2021-06-23) - blog post explaining PipeWire from a unique perspective
- [PipeWire: The Linux audio/video bus](https://lwn.net/Articles/847412/), on Linux Weekly News (2021-03-02)
- [Desktop Profile to enable PipeWire support](https://www.gentoo.org/support/news-items/2026-01-15-desktop-profile-pipewire.html) (2026-01-15) -  official Gentoo news about pipewire support
- [libpipewire-modules(7)](https://man.archlinux.org/man/libpipewire-modules.7.en)
