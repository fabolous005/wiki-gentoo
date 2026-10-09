<!-- source: https://wiki.gentoo.org/wiki/Snapcast | group: Gentoo Wiki (Main) | wiki-title: Snapcast -->
---
title: Snapcast
url: https://wiki.gentoo.org/wiki/Snapcast
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-30"
fingerprint: b602a95bf1a11c96
license: CC BY-SA 4.0
---

# Snapcast

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Snapcast** (**S**y**n**chronous **a**udio **p**layer) is a multiroom client-server audio player, where all clients are time synchronized with the server to play perfectly synced audio:

\[Snapcast is\] not a standalone player, but an extension that turns your existing audio player into a Sonos-like multiroom solution. Audio is captured by the server and routed to the connected clients. Several players can feed audio to the server in parallel and clients can be grouped to play the same audio stream.



## Installation


### USE flags


### USE flags for
            [media-sound/snapcast](https://packages.gentoo.org/packages/media-sound/snapcast)
            
            Synchronous multi-room audio player

| [+client](https://packages.gentoo.org/useflags/+client) | Build and install Snapcast client component | 
| [+expat](https://packages.gentoo.org/useflags/+expat) | Enable the use of dev-libs/expat for XML parsing | 
| [+flac](https://packages.gentoo.org/useflags/+flac) | Add support for FLAC: Free Lossless Audio Codec | 
| [+opus](https://packages.gentoo.org/useflags/+opus) | Enable Opus audio codec support | 
| [+server](https://packages.gentoo.org/useflags/+server) | Build and install Snapcast server component | 
| [+vorbis](https://packages.gentoo.org/useflags/+vorbis) | Add support for the OggVorbis audio codec | 
| [+zeroconf](https://packages.gentoo.org/useflags/+zeroconf) | Support for DNS Service Discovery (DNS-SD) | 
| [alsa](https://packages.gentoo.org/useflags/alsa) | Add support for media-libs/alsa-lib (Advanced Linux Sound Architecture) | 
| [jack](https://packages.gentoo.org/useflags/jack) | Add support for the JACK Audio Connection Kit | 
| [pipewire](https://packages.gentoo.org/useflags/pipewire) | Build with PipeWire support | 
| [python](https://packages.gentoo.org/useflags/python) | Add optional support/bindings for the Python language | 
| [sdl](https://packages.gentoo.org/useflags/sdl) | Build client with SDL2 support | 
| [soxr](https://packages.gentoo.org/useflags/soxr) | Build with audio resampler support with media-libs/soxr | 
| [ssl](https://packages.gentoo.org/useflags/ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [tremor](https://packages.gentoo.org/useflags/tremor) | Build with TREMOR version of vorbis | 


### Emerge

`root #``emerge --ask media-sound/snapcast`

## Configuration


### Server

By default, the Snapserver configuration file, /etc/snapserver.conf, specifies a `source` in the `[stream]` section:

**`/etc/snapserver.conf`**

```
...
source = pipe:///tmp/snapfifo?name=default
...
```
This creates a source named `default`: a FIFO, or named pipe, at /tmp/snapfifo. Refer to [fifo(7)](https://man.archlinux.org/man/fifo.7.en) [for a detailed introduction to FIFOs.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

Snapcast supports having multiple sources per server instance, with each source having its own FIFO. For example, a second source could be added for use by music players specifically:

**`/etc/snapserver.conf`**

```
...
source = pipe:///tmp/snapfifo?name=default
source = pipe:///tmp/snapfifo-music?name=music
...
```

#### OpenRC

As of 2026-07-14, the default configuration for OpenRC is:

**`/etc/conf.d/snapserver`**

```
# conf.d file for snapserver
SNAPSERVER_USER="snapserver:snapserver"
# For all command line options, please see `snapserver -hh`
SNAPSERVER_OPTS="--logging.sink=system --server.datadir=/var/lib/snapserver"
```
To start Snapserver:

`root #``rc-service snapserver start`
To start Snapserver at boot:

`root #``rc-update add snapserver default`
For all Snapserver command-line options, refer to the snapserver(1) man page.

#### Testing

To check that Snapserver is working, send data to the /tmp/snapfifo pipe:

`user $``cat /dev/urandom > /tmp/snapfifo`
This should result in white noise. Cancel the command with `Ctrl`+`c`.

To check that TCP-based network streaming is working, use [avahi-browse(1)](https://man.archlinux.org/man/avahi-browse.1.en)[:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`user $``avahi-browse -at`
...
+ wlp2s0 IPv4 Snapcast                                      \_snapcast-http.\_tcp  local
+ wlp2s0 IPv4 Snapcast                                      \_snapcast-ctrl.\_tcp  local
+ wlp2s0 IPv4 Snapcast                                      \_snapcast.\_tcp       local
...


#### Audio sources

Snapcast can be used with a variety of audio sources, including pipes, [ALSA](https://wiki.gentoo.org/wiki/ALSA), [PulseAudio](https://wiki.gentoo.org/wiki/PulseAudio), [PipeWire](https://wiki.gentoo.org/wiki/PipeWire), and more.

##### PipeWire

To set up audio streaming from a [desktop environment](https://wiki.gentoo.org/wiki/Desktop_environment) or [window manager](https://wiki.gentoo.org/wiki/Window_manager) using PipeWire as a source, add the following source to the server configuration file's `[stream]` section:

**`/etc/snapserver.conf`**

```
source = pipewire://?name=PipeWire
```
The stream name can be changed from `PipeWire` to whatever name is preferred.

To avoid permissions errors, the Snapcast server needs to be launched from the same desktop where PipeWire is enabled. This should be done via a user service, but if such a service is not yet available, as is the case for OpenRC ([bug #980076](https://bugs.gentoo.org/show_bug.cgi?id=980076)) the server can be started manually.

First, check the server system service is stopped and disabled. For example, under OpenRC:

`root #``rc-service snapserver stop``root #``rc-update delete snapserver default`
Then manually start Snapserver. This can be done via the GUI environment's startup files, but for testing, it can be started from a terminal:

`user $``snapserver --config /etc/snapserver.conf`
Clients should now be able to connect to Snapserver and stream the source specified in the configuration file (`GentooStream` in the example configuration above).

To stop the server, use `Ctrl` + `c`.

Once testing is completed, modify the GUI environment's relevant startup file to start Snapserver. For example:

snapserver --config /etc/snapserver.conf

Refer to the documentation for the startup file to determine the appropriate way to do so, e.g. whether the server needs to be backgrounded by adding `&` to the call:

snapserver --config /etc/snapserver.conf &


### Client

If the [client](https://packages.gentoo.org/useflags/client) [USE flag is enabled, snapclient will be built and installed.](https://wiki.gentoo.org/wiki/USE_flag)

In general, snapclient should not be run as a system service, but should instead be started directly. For example, first get a list of available sound devices by using snapclient's `-l` option:

`user $``snapclient -l`
0: null
Discard all samples (playback) or generate zero samples (capture)
1: default
Default ALSA Output (currently PipeWire Media Server)
2: pipewire
PipeWire Sound Server
...

Then specify the desired device by using the `-s` option, e.g.:

`user $``snapclient -s 1`

#### OpenRC

As of 2026-07-15, the default OpenRC configuration to start Snapclient as a daemon is:

**`/etc/conf.d/snapclient`**

```
# conf.d file for snapclient
SNAPCLIENT_USER="snapclient:audio"
# snapclient [options...] [url]
# For all command line options, please see `man snapclient.1`
# if ’url’ is not configured, snapclient defaults to ’tcp://_snapcast._tcp’
SNAPCLIENT_OPTS="--logsink=system"
```
The snapclient daemon will try to find servers on the network using [Avahi](https://wiki.gentoo.org/wiki/Avahi); it thus has a dependency on the `avahi-daemon` service.

To start the `snapclient` service:

`root #``rc-service snapclient start`
To start the `snapclient` service at boot time:

`root #``rc-update add snapclient default`

## Usage

#### MPD

To hear [MPD](https://wiki.gentoo.org/wiki/MPD) audio over Snapcast, create a new audio output in mpd.conf using the `fifo` module:

The sample rate setting in the format field is the default used by Snapcast; different sample rates can be used, but must be set in /etc/snapserver.conf first.

### Android client

An [Android](https://wiki.gentoo.org/wiki/Android) client is available, [Snapdroid](https://github.com/badaix/snapdroid). The client is no longer available on Google Play, but is still available via [F-Droid](https://f-droid.org), under the name "Snapcast".

## See also

- [MPD](https://wiki.gentoo.org/wiki/MPD) — a flexible, server-side application for playing music.
