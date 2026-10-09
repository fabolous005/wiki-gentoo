<!-- source: https://wiki.gentoo.org/wiki/PulseAudio | group: Gentoo Wiki (Main) | wiki-title: PulseAudio -->
---
title: PulseAudio
url: https://wiki.gentoo.org/wiki/PulseAudio
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-26"
fingerprint: fc4e0c5c49273986
license: CC BY-SA 4.0
---

# PulseAudio

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

**PulseAudio** (or **PA** for short) is a multi-platform, open source, *sound server* that provides a number of features on top of the low-level audio interface [ALSA](https://wiki.gentoo.org/wiki/ALSA), such as:

- Networking support (P2P and server mode).
- Per-application volume controls.
- Better cross-platform support.
- Dynamic latency adjustment, which can be used to save power.
- [Plugin modules](https://www.freedesktop.org/wiki/Software/PulseAudio/Documentation/User/Modules/).



## Installation

### Prerequisites

PulseAudio can use, but does not need, either [systemd](https://wiki.gentoo.org/wiki/Systemd) or [sys-auth/elogind](https://wiki.gentoo.org/wiki/Elogind). If using the latter, remember to add [elogind](https://packages.gentoo.org/useflags/elogind) [and](https://wiki.gentoo.org/wiki/USE_flag) [-systemd](https://packages.gentoo.org/useflags/systemd) [as global USE flags.](https://wiki.gentoo.org/wiki/USE_flag)

### Kernel

For motherboards containing HDA sound cards, use the following kernel option for improved power-saving only on machines that are **not amd64**- or **x86**-based.

Device Drivers  --->
  \<\*> Sound card support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SOUND\</code> to find this item.  --->
    \<\*> Advanced Linux Sound Architecture [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\</code> to find this item.

Refer to [the "Kernel" section of the "ALSA" page](https://wiki.gentoo.org/wiki/ALSA#Kernel) for setting the right kernel options for sound card detection.

PulseAudio uses [udev](https://wiki.gentoo.org/wiki/Udev) to dynamically give the currently 'active' user access to the soundcard(s). To make this possible, [ACLs](https://wiki.gentoo.org/wiki/Filesystem/Access_Control_List_Guide) (Access Control Lists) are required.

```
File systems  --->
  Pseudo filesystems  --->
    [*] Tmpfs virtual memory file system support (former shm fs) 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_TMPFS</code> to find this item.
    [*]   Tmpfs POSIX Access Control Lists [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_TMPFS_POSIX_ACL</code> to find this item.
`CONFIG_HIGH_RES_TIMERS` is needed to avoid `(snd_pcm_recover) underrun` errors and degraded audio when some applications are using PulseAudio. Not all applications require `CONFIG_HIGH_RES_TIMERS` to operate properly; however, it is recommended for applications such as [Audacity](https://wiki.gentoo.org/wiki/Audacity), and good practice to enable it to ensure compatibility with other audio applications.

```
General setup  --->
  Timers subsystem  --->
    [*] High Resolution Timer Support 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_HIGH_RES_TIMERS</code> to find this item.
### USE flags

The global [pulseaudio](https://packages.gentoo.org/useflags/pulseaudio) [USE flag](https://wiki.gentoo.org/wiki/USE_flag) enables support for PulseAudio in packages. Enabling it in [make.conf](https://wiki.gentoo.org/wiki/Make.conf) will cause [media-libs/libpulse](https://packages.gentoo.org/packages/media-libs/libpulse) to be installed when emerging such packages.

The USE flags for libpulse itself are:


### USE flags for
            [media-libs/libpulse](https://packages.gentoo.org/packages/media-libs/libpulse)
            
            Libraries for PulseAudio clients

| [+asyncns](https://packages.gentoo.org/useflags/+asyncns) | Use libasyncns for asynchronous name resolution. | 
| [+glib](https://packages.gentoo.org/useflags/+glib) | Add support to dev-libs/glib-based mainloop for the libpulse client library, to allow using libpulse on glib-based programs. | 
| [X](https://packages.gentoo.org/useflags/X) | Add support for X11 | 
| [dbus](https://packages.gentoo.org/useflags/dbus) | Enable dbus support for anything that needs it (gpsd, gnomemeeting, etc) | 
| [doc](https://packages.gentoo.org/useflags/doc) | Build the doxygen-described API documentation. | 
| [gtk](https://packages.gentoo.org/useflags/gtk) | Add support for x11-libs/gtk+ (The GIMP Toolkit) | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [valgrind](https://packages.gentoo.org/useflags/valgrind) | Enable annotations for accuracy. May slow down runtime slightly. Safe to use even if not currently using dev-debug/valgrind | 

The USE flags for the [media-sound/pulseaudio-daemon](https://packages.gentoo.org/packages/media-sound/pulseaudio-daemon) package are:


### USE flags for
            [media-sound/pulseaudio-daemon](https://packages.gentoo.org/packages/media-sound/pulseaudio-daemon)
            
            Daemon component of PulseAudio (networked sound server)

| [+X](https://packages.gentoo.org/useflags/+X) | Build the X11 publish module to export PulseAudio information through X11 protocol for clients to make use. Don't enable this flag if you want to use a system wide instance. If unsure, enable this flag. | 
| [+alsa](https://packages.gentoo.org/useflags/+alsa) | Add support for media-libs/alsa-lib (Advanced Linux Sound Architecture) | 
| [+alsa-plugin](https://packages.gentoo.org/useflags/+alsa-plugin) | Request installing media-plugins/alsa-plugins with PulseAudio plugin enabled. This ensures that clients supporting ALSA only will use PulseAudio. | 
| [+asyncns](https://packages.gentoo.org/useflags/+asyncns) | Use libasyncns for asynchronous name resolution. | 
| [+gdbm](https://packages.gentoo.org/useflags/+gdbm) | Use sys-libs/gdbm to store PulseAudio databases. Recommended for desktop usage. This flag causes the whole package to be licensed under GPL-2 or later. | 
| [+glib](https://packages.gentoo.org/useflags/+glib) | Build the GSettings PA module. | 
| [+orc](https://packages.gentoo.org/useflags/+orc) | Use dev-lang/orc for just-in-time optimization of array operations | 
| [+udev](https://packages.gentoo.org/useflags/+udev) | Enable virtual/udev integration (device discovery, power and storage device support, etc) | 
| [+webrtc-aec](https://packages.gentoo.org/useflags/+webrtc-aec) | Uses the webrtc.org AudioProcessing library for enhancing VoIP calls greatly in applications that support it by performing acoustic echo cancellation, analog gain control, noise suppression and other processing. | 
| [aptx](https://packages.gentoo.org/useflags/aptx) | aptX (HD) over Bluetooth (many Android compatible headphones), requires media-plugins/gst-plugins-openaptx. | 
| [bluetooth](https://packages.gentoo.org/useflags/bluetooth) | Enable Bluetooth Support | 
| [dbus](https://packages.gentoo.org/useflags/dbus) | Enable dbus support for anything that needs it (gpsd, gnomemeeting, etc) | 
| [elogind](https://packages.gentoo.org/useflags/elogind) | Use sys-auth/elogind for giving each session a PA client | 
| [equalizer](https://packages.gentoo.org/useflags/equalizer) | Enable the equalizer module (requires sci-libs/fftw and sys-apps/dbus). | 
| [fftw](https://packages.gentoo.org/useflags/fftw) | Enable the virtual surround sink module (requires sci-libs/fftw). | 
| [gstreamer](https://packages.gentoo.org/useflags/gstreamer) | Build GStreamer-based RTP protocol module which supports more advanced RTP features like OPUS payload encoding. | 
| [jack](https://packages.gentoo.org/useflags/jack) | Add support for the JACK Audio Connection Kit | 
| [ldac](https://packages.gentoo.org/useflags/ldac) | LDAC over Bluetooth (primarily Sony headphones), requires media-plugins/gst-plugins-ldac. | 
| [lirc](https://packages.gentoo.org/useflags/lirc) | Add support for lirc (Linux's Infra-Red Remote Control) | 
| [ofono-headset](https://packages.gentoo.org/useflags/ofono-headset) | Build with optional oFono HFP backend for bluez 5, requires net-misc/ofono. | 
| [oss](https://packages.gentoo.org/useflags/oss) | Enable OSS sink/source (output/input). Deprecated, upstream does not support this on systems where other sink/source systems are available (i.e.: Linux). The padsp wrapper is now always build if the system supports OSS at all. | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [sox](https://packages.gentoo.org/useflags/sox) | Add support for Sound eXchange (SoX) | 
| [ssl](https://packages.gentoo.org/useflags/ssl) | Use dev-libs/openssl to provide support for RAOP (AirPort) streaming. | 
| [system-wide](https://packages.gentoo.org/useflags/system-wide) | Allow preparation and installation of the system-wide init script for PulseAudio. Since this support is only supported for embedded situations, do not enable without reading the upstream instructions at https://www.freedesktop.org/wiki/Software/PulseAudio/Documentation/User/WhatIsWrongWithSystemWide/ . | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Build with sys-apps/systemd support to replace standalone ConsoleKit. | 
| [tcpd](https://packages.gentoo.org/useflags/tcpd) | Add support for TCP wrappers | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [valgrind](https://packages.gentoo.org/useflags/valgrind) | Enable annotations for accuracy. May slow down runtime slightly. Safe to use even if not currently using dev-debug/valgrind | 
| [zeroconf](https://packages.gentoo.org/useflags/zeroconf) | Support for DNS Service Discovery (DNS-SD) | 

### Emerge

After setting USE flags, be sure to update the system so the changes take effect:

`root #``emerge --ask --changed-use --deep @world`
Once the system has been updated, the libpulse and pulseaudio-daemon packages can be installed:

`root #``emerge --ask media-libs/libpulse media-sound/pulseaudio-daemon`
### Additional software

- [media-sound/pavucontrol](https://packages.gentoo.org/packages/media-sound/pavucontrol) - Pulseaudio Volume Control, a [GTK](https://wiki.gentoo.org/wiki/GTK) mixer. [media-sound/pavucontrol-qt](https://packages.gentoo.org/packages/media-sound/pavucontrol-qt) is the [Qt](https://wiki.gentoo.org/wiki/Qt) version.
- [media-sound/pulsemixer](https://packages.gentoo.org/packages/media-sound/pulsemixer) - TUI mixer.
- [media-sound/paprefs](https://packages.gentoo.org/packages/media-sound/paprefs) - PulseAudio Preferences, a GTK-based configuration UI.
- [kde-plasma/plasma-pa](https://packages.gentoo.org/packages/kde-plasma/plasma-pa) - [KDE Plasma](https://wiki.gentoo.org/wiki/KDE) applet integrating configuration and mixing, but not as powerful as pavucontrol or paprefs.

## Configuration

### Permissions

If a `desktop` [profile](https://wiki.gentoo.org/wiki/Portage/Profiles) is not being used:

- for [systemd](https://wiki.gentoo.org/wiki/Systemd), check that [sys-apps/systemd](https://packages.gentoo.org/packages/sys-apps/systemd) is installed with the [acl](https://packages.gentoo.org/useflags/acl)- for [OpenRC](https://wiki.gentoo.org/wiki/OpenRC), check that [sys-auth/pambase](https://packages.gentoo.org/packages/sys-auth/pambase) is installed.

Then, verify the permissions are working correctly:

`user $````
getfacl /dev/snd/controlC0 | grep -Eo "user:.+:"  | cut -d: -f2
```
This should output the name of the user using PulseAudio.

### Files

- /etc/pulse/default.pa - daemon startup file. Refer to [default.pa(5)](https://man.archlinux.org/man/default.pa.5.en)- $HOME/.config/pulse/default.pa - user-specific version of /etc/pulse/default.pa.
- /etc/pulse/system.pa - daemon startup file for system-wide setups.
- /etc/pulse/client.conf - client configuration file. Refer to [pulse-client.conf(5)](https://man.archlinux.org/man/pulse-client.conf.5.en)- $HOME/.config/pulse/client.conf - user-specific version of /etc/pulse/client.conf.

### Configuring other applications

Some applications need to be configured to output to PulseAudio by default. A detailed list of these can be found on the PulseAudio wiki's [PerfectSetup page](http://www.freedesktop.org/wiki/Software/PulseAudio/Documentation/User/PerfectSetup).

The [media-plugins/alsa-plugins](https://packages.gentoo.org/packages/media-plugins/alsa-plugins) must be installed with the [pulseaudio](https://packages.gentoo.org/useflags/pulseaudio) [USE flag enabled:](https://wiki.gentoo.org/wiki/USE_flag)

`root #``emerge --ask media-plugins/alsa-plugins`
Enable the following module in /etc/pulse/default.pa:

**`/etc/pulse/default.pa`**

```
load-module module-oss
```
- GStreamer

Several GConf keys must be set:

- Manual with gconftool:

`user $````
gconftool-2 -t string --set /system/gstreamer/0.10/default/audiosink pulsesink
```
`user $````
gconftool-2 -t string --set /system/gstreamer/0.10/default/audiosrc pulsesrc
```
- libao

Set the following in /etc/libao.conf:

**`/etc/libao.conf`**

```
default_driver=pulse
```
- OpenAL

Set the following in /etc/openal/alsoft.conf:

**`/etc/openal/alsoft.conf`**

```
drivers = pulse
```
Set the following in /etc/mplayer/mplayer.conf:

**`/etc/mplayer/mplayer.conf`**

```
ao=pulse
```
### Without udev or systemd

**outdated**. You can help the Gentoo community by verifying and

[updating this section](https://wiki.gentoo.org/index.php?title=PulseAudio&action=edit).

If using [ALSA](https://wiki.gentoo.org/wiki/ALSA) as a PulseAudio sink (output) and routing ALSA apps to PulseAudio but not using udev, the device to use must be specified. Otherwise, PulseAudio will use the ALSA device `default` as the sink, which may be routed back to PulseAudio, forming a loop.

To avoid this, add the parameter `device=hw:0,0` to /etc/pulse/default.pa, where `0,0` should be replaced by the appropriate values as found in the output of aplay -l. Without this, problems may only show up on restating PulseAudio (e.g. by logging out and back in or rebooting): there will be with no audio, a slow desktop environment and hanging applications until the loop is resolved. Once the configuration is updated, restart `alsasound` and kill all running PulseAudio processes.

In the following example, there are two soundcards specified: card 0, device 0 is used as a sink (audio output, e.g. speakers), and card 1, device 0 is used as a source (audio input, e.g. microphone). PulseAudio will still be able to access other cards.

**`/etc/pulse/default.pa`**

**Using a specific ALSA device as PulseAudio sink/source**

```
load-module module-alsa-sink device=hw:0,0
load-module module-alsa-source device=hw:1,0
```
### Headless server

**outdated**. You can help the Gentoo community by verifying and

[updating this section](https://wiki.gentoo.org/index.php?title=PulseAudio&action=edit).

A *headless* PulseAudio server is a server which has no display attached to it but does have speakers. This provides the ability to use the remote server's speakers for audio output.

#### Server

`root #````
mkdir -p /etc/portage/profile
```
`root #````
echo "-system-wide" >> /etc/portage/profile/use.mask
```
`root #``echo "media-sound/pulseaudio-daemon system-wide" >> /etc/portage/package.use`
Then re-emerge pulseaudio-daemon:

`root #``emerge --ask --oneshot pulseaudio-daemon`
Next, add the following two lines somewhere in the /etc/pulse/system.pa file:

**`/etc/pulse/system.pa`**

```
load-module module-native-protocol-tcp auth-ip-acl=x.x.x.x/24
load-module module-alsa-sink
```
where `x.x.x.x/24` should be replaced by the network mask for accessing the server.

Finally, on OpenRC systems, configure the `pulseaudio` service to confirm the required setup, and start the service:

`root #````
echo "PULSEAUDIO_SHOULD_NOT_GO_SYSTEMWIDE=1" >> /etc/conf.d/pulseaudio
```
`root #``rc-update add pulseaudio default``root #````
rc-service pulseaudio start
```
#### Client

`user $``pacmd load-module module-tunnel-sink server=x.x.x.x`
server (x.x.x.x) is the IP of the server.

where `x.x.x.x` should be replaced by the IP address of the server.

For a more permanent solution, add the following to the /etc/pulse/default.pa file:

**`/etc/pulse/default.pa`**

```
load-module module-tunnel-sink server=x.x.x.x
```
where `x.x.x.x` should again be replaced by the IP address of the server.

When setup is complete, pavucontrol should list the remote server listed under "Output Devices", and under "Playback" there should be a button next to the "Mute audio" button which, when selected, will switch that audio stream to whichever output is required.

### Multiple concurrent users

By default, the PulseAudio daemon doesn't accept connections from secondary users. However, in some situations, like software isolation, it may be desirable for a user to run programs as some other user and still have access to the PulseAudio daemon.

In this situation, configure PulseAudio to use a UNIX domain socket that accepts connections from secondary users:

**`/etc/pulse/default.pa.d/multi-user.pa`**

**Configure PulseAudio to use a UNIX domain socket**

```
load-module module-native-protocol-unix auth-anonymous=1 socket=/tmp/pulse-socket
```
Then configure the PulseAudio client library to use this socket:

**`/etc/pulse/client.conf`**

**Make the clients use the UNIX socket**

```
default-server = unix:/tmp/pulse-socket
```
This configuration does not need users to be in the `audio`, `pulse-access`, or `pulse` groups.

### Equalizer

To enable the equalizer, build [media-sound/pulseaudio-daemon](https://packages.gentoo.org/packages/media-sound/pulseaudio-daemon) with the [equalizer](https://packages.gentoo.org/useflags/equalizer) [USE flag enabled. This also requires that the](https://wiki.gentoo.org/wiki/USE_flag) [dbus](https://packages.gentoo.org/useflags/dbus) [USE flag be enabled.](https://wiki.gentoo.org/wiki/USE_flag)

Add the following two lines somewhere in the default.pa file :

**`/etc/pulse/default.pa`**

```
load-module module-dbus-protocol
load-module module-equalizer-sink
```
## Usage

### Starting the daemon

#### systemd

To enable PulseAudio for the current user run:

`user $``systemctl --user enable --now pulseaudio.service pulseaudio.socket`
Root can enable PulseAudio for all users:

`root #``systemctl --global enable pulseaudio.service pulseaudio.socket`
#### OpenRC

Running PulseAudio "system-wide" via the `pulseaudio` OpenRC system service is strongly discouraged unless there is good reason to do so.

The standard way of starting the PulseAudio daemon is autospawning<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>, in which D-Bus is used to request that the daemon be started. On non-systemd systems, the [media-sound/pulseaudio-daemon](https://packages.gentoo.org/packages/media-sound/pulseaudio-daemon) package will install the file enable-autospawn.conf to enable autospawning.

The pulseaudio.desktop file calls the start-pulseaudio-x11 script, which configures PulseAudio for running under [X](https://wiki.gentoo.org/wiki/Xorg).

#### Manually

To start a PulseAudio daemon:

`user $``pulseaudio -D`
### Equalizer

To list the index and name of the equalizer sink:

`user $``pacmd list-sinks | grep -B1 -e "name:.*equalizer"`
Use pavucontrol or [a similar program](https://wiki.gentoo.org/wiki/List_of_audio_software#Mixing.2Fvolume) to select the equalizer sink for sound output. It may be listed as a device starting with "FFT based equalizer".

Equalizer levels can be controlled with [media-sound/qpaeq](https://packages.gentoo.org/packages/media-sound/qpaeq), a [Qt](https://wiki.gentoo.org/wiki/Qt) GUI.

## Troubleshooting

Refer to [PulseAudio/troubleshooting](https://wiki.gentoo.org/wiki/PulseAudio/troubleshooting).

## See also

- [ALSA](https://wiki.gentoo.org/wiki/ALSA) — the Linux kernel's API for sound cards, together with an associated software framework
- [PipeWire](https://wiki.gentoo.org/wiki/PipeWire) — low-latency, graph-based, processing engine and server, for interfacing with audio and video devices.

## External resources

### Official documentation

### Other information

- [Getting DTS 5.1+ sound via S/PDIF or HDMI using PulseAudio](https://blogs.gentoo.org/mgorny/2021/07/25/getting-dts-5-1-sound-via-s-pdif-or-hdmi-using-pulseaudio/)
- [More general troubleshooting tips](https://wiki.archlinux.org/index.php/PulseAudio/Troubleshooting)
- [Why you should care about PulseAudio (and how to start doing it)](https://www.linux.com/news/why-you-should-care-about-pulseaudio-and-how-start-doing-it/)



## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) ["Running PulseAudio"](https://www.freedesktop.org/wiki/Software/PulseAudio/Documentation/User/Running/). Retrieved on 2026-08-18.
