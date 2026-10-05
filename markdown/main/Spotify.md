<!-- source: https://wiki.gentoo.org/wiki/Spotify | group: Gentoo Wiki (Main) | wiki-title: Spotify -->
---
title: Spotify
url: https://wiki.gentoo.org/wiki/Spotify
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-08-02"
fingerprint: a9747559c994cde5
license: CC BY-SA 4.0
---

# Spotify

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Spotify is a digital music streaming service that provides a proprietary Linux client.

## Installation

### USE flags


| [libnotify](https://packages.gentoo.org/useflags/libnotify) | Enable desktop notification support | 
| [local-playback](https://packages.gentoo.org/useflags/local-playback) | Allows playing local files with the Spotify client | 
| [pax-kernel](https://packages.gentoo.org/useflags/pax-kernel) | Triggers a paxmarking of the main Spotify binary | 
| [pulseaudio](https://packages.gentoo.org/useflags/pulseaudio) | Controls the dependency on pulseaudio or apulse | 

### Emerge

Install Spotify:

`root #``emerge --ask media-sound/spotify`
## Control via MPRIS

[MPRIS](https://specifications.freedesktop.org/mpris-spec/latest/)is a [D-Bus](https://wiki.gentoo.org/wiki/D-Bus) interface which provides a common API to control media players.

Spotify supports MPRIS.

### Prequisites if using a window manager

In $HOME/.xinitrc, make the window manager start with dbus-run-session.

Otherwise, Spotify won't provide an MPRIS interface.

**`$HOME/.xinitrc`**

**Starting bspwm with D-Bus**

```
exec dbus-run-session bspwm
```
### With dbus-send

The most simple approach to controlling Spotify is using dbus-send.

**Play/pause**:

`user $``dbus-send --print-reply --dest=org.mpris.MediaPlayer2.spotify /org/mpris/MediaPlayer2 org.mpris.MediaPlayer2.Player.PlayPause`
**Next**:

`user $``dbus-send --print-reply --dest=org.mpris.MediaPlayer2.spotify /org/mpris/MediaPlayer2 org.mpris.MediaPlayer2.Player.Next`
**Previous**:

`user $``dbus-send --print-reply --dest=org.mpris.MediaPlayer2.spotify /org/mpris/MediaPlayer2 org.mpris.MediaPlayer2.Player.Previous`
### With playerctl

Install [media-sound/playerctl](https://packages.gentoo.org/packages/media-sound/playerctl).

**Play/pause**:

`user $``playerctl play-pause`
**Next**:

`user $``playerctl next`
**Previous**:

`user $``playerctl previous`
More info about playerctl can be found on its [GitHub page](https://github.com/altdesktop/playerctl).

## Starting with Wayland

Spotify uses Xorg by default. In order to make Spotify start with Wayland, you can add

--enable-features=UseOzonePlatform --ozone-platform=wayland

to Spotify's .desktop file in /usr/share/applications/spotify.desktop on the

Exec=spotify

line. It should look like this:

**`/usr/share/applications/spotify.desktop`**

**Starting Spotify with Wayland**

```
Exec=spotify --enable-features=UseOzonePlatform --ozone-platform=wayland %U
```
You can also add these options to anything that launches Spotify through the command line.
