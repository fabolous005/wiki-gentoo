<!-- source: https://wiki.gentoo.org/wiki/Luanti | group: Gentoo Wiki (Main) | wiki-title: Luanti -->
---
title: Luanti
url: https://wiki.gentoo.org/wiki/Luanti
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-01-28"
fingerprint: de43ad5ac58619c6
license: CC BY-SA 4.0
---

# Luanti

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Luanti** (formerly **Minetest**, see [bug #943292](https://bugs.gentoo.org/show_bug.cgi?id=943292)) is a voxel game engine. Luanti should not be confused with [Minetest Game](https://content.luanti.org/packages/Minetest/minetest_game/), which uses this engine.

## Installation

### USE flags


| [+client](https://packages.gentoo.org/useflags/+client) | Build Minetest client | 
| [+curl](https://packages.gentoo.org/useflags/+curl) | Add support for client-side URL transfer library | 
| [+server](https://packages.gentoo.org/useflags/+server) | Build Minetest server | 
| [+sound](https://packages.gentoo.org/useflags/+sound) | Enable sound support | 
| [+test](https://packages.gentoo.org/useflags/+test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [leveldb](https://packages.gentoo.org/useflags/leveldb) | Enable LevelDB backend | 
| [ncurses](https://packages.gentoo.org/useflags/ncurses) | Add ncurses support (console display library) | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [postgres](https://packages.gentoo.org/useflags/postgres) | Add support for the postgresql database | 
| [prometheus](https://packages.gentoo.org/useflags/prometheus) | Enable prometheus client support | 
| [redis](https://packages.gentoo.org/useflags/redis) | Enable redis backend via dev-libs/hiredis | 
| [spatial](https://packages.gentoo.org/useflags/spatial) | Enable SpatialIndex AreaStore backend | 

### Emerge

`root #``emerge --ask games-engines/minetest`
### Flatpak

Luanti can also be installed via [Flatpak](https://wiki.gentoo.org/wiki/Flatpak). To install the [official package](https://flathub.org/apps/net.minetest.Minetest), run:

`user $``flatpak install --user flathub net.minetest.Minetest`
To run Luanti use the following command:

`user $``flatpak run --user net.minetest.Minetest`
## Configuration

### Server

#### Server-side game installation

All games should be manually downloaded and installed in the /var/lib/minetest/.minetest/games/ directory. Games can be downloaded from the [official website](https://content.luanti.org/packages/?type=game). The games are distributed as zip archives that need to be extracted. As an example, to install the [Minetest Game](https://content.luanti.org/packages/Minetest/minetest_game/), the following steps should be performed:

`root #````
cd /var/lib/minetest/.minetest/games
```
`root #``wget` [https://content.luanti.org/packages/Minetest/minetest_game/releases/29428/download/](https://content.luanti.org/packages/Minetest/minetest_game/releases/29428/download/) -O minetest_game.zip
`root #``unzip minetest_game.zip`
In order to launch the game, provide its name to the [minetestserver(6)](https://manpages.org/minetestserver/6) [using the](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) `--gameid` command line argument (`--gameid minetest_game` for the above example).

The worlds will be located in the /var/lib/minetest/.minetest/worlds/ directory.

#### Server-side mod installation

As games, mods need to be downloaded manually and installed in the /var/lib/minetest/.minetest/mods/ directory. Mods can be downloaded from the [official website](https://content.luanti.org/packages/?type=mod). However, it is important to check the compatibility of the mod with the installed game, as well as to install all its dependencies. As an example, to install the [Mobs Monster](https://content.luanti.org/packages/TenPlus1/mobs_monster/) mod, the [Mobs Redo API](https://content.luanti.org/modnames/mobs/) mod must also be installed, otherwise Luanti will crash with no error messages. After the mods were installed, the world.mt file needs to be modified to load the mods:

**`/var/lib/minetest/.minetest/worlds/world/world.mt`**

#### Server configuration

The configuration is done in the /etc/minetest/minetest.conf file, which must be created manually. An example script can be found [here](https://github.com/luanti-org/luanti/blob/master/minetest.conf.example) (check the `## Server` section).

The minimal configuration file requires only one field to be specified (replace `7777:777:7777:7777::1` with the IP address):

**`/etc/minetest/minetest.conf`**

Luanti uses port 30000 by default, which is recommended <sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.

Luanti only uses [UDP protocol](https://en.wikipedia.org/wiki/User_Datagram_Protocol), all other traffic can be safely dropped by a firewall.

#### OpenRC

The Luanti package comes with a [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) service script, which is designed to simplify server startup.

- /etc/init.d/minetest-server - Run script for OpenRC.
- /etc/conf.d/minetest-server - Configuration run script for OpenRC.

To start the server, run the command:

`root #``rc-service minetest-server start`
To start the server at system boot, run:

`root #``rc-update add minetest-server default`
#### systemd

To start the server, issue the following:

`root #``systemctl start minetest-server`
If the server should automatically start when the system reboots, run:

`root #``systemctl enable minetest-server`
## SELinux

As of 2025-01-24, there are no official [SELinux](https://wiki.gentoo.org/wiki/SELinux) policies for Luanti.

### OpenRC service policy

The following policy covers only the server side of Luanti and assumes that the server will run via the OpenRC service.

**`luanti-server.te`**

**`luanti-server.fc`**

#### Installation of OpenRC service policy

.te and .fc files defined above should be in the same directory.

`root #``make -f /usr/share/selinux/strict/include/Makefile``root #``semodule --install luanti-server``root #````
restorecon /usr/bin/minetestserver
```
`root #````
restorecon -R /var/lib/minetest
```
`root #````
restorecon -R /var/log/minetest
```
#### Removal of OpenRC service policy

`root #``semodule --remove luanti-server``root #````
restorecon /usr/bin/minetestserver
```
`root #````
restorecon -R /var/lib/minetest
```
`root #````
restorecon -R /var/log/minetest
```
## Troubleshooting

### The server is not running

If the server is not running, the server status should be checked.

OpenRC:

`root #``rc-service minetest-server status`
systemd:

`root #``systemctl status minetest-server`
### No sound

If there is no sound in the client, set the necessary (depending on the system configuration) USE flags of the [media-libs/openal](https://packages.gentoo.org/packages/media-libs/openal) package and recompile it.


| [alsa](https://packages.gentoo.org/useflags/alsa) | Add support for media-libs/alsa-lib (Advanced Linux Sound Architecture) | 
| [coreaudio](https://packages.gentoo.org/useflags/coreaudio) | Build the CoreAudio driver on Mac OS X systems | 
| [dbus](https://packages.gentoo.org/useflags/dbus) | Request real-time priority via sys-auth/rtkit | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [gui](https://packages.gentoo.org/useflags/gui) | Enable support for a graphical user interface | 
| [jack](https://packages.gentoo.org/useflags/jack) | Add support for the JACK Audio Connection Kit | 
| [oss](https://packages.gentoo.org/useflags/oss) | Add support for OSS (Open Sound System) | 
| [pipewire](https://packages.gentoo.org/useflags/pipewire) | Enable support for the media-video/pipewire audio backend | 
| [portaudio](https://packages.gentoo.org/useflags/portaudio) | Add support for the crossplatform portaudio audio API | 
| [pulseaudio](https://packages.gentoo.org/useflags/pulseaudio) | Add sound server support via media-libs/libpulse (may be PulseAudio or PipeWire) | 
| [sdl](https://packages.gentoo.org/useflags/sdl) | Add support for Simple Direct Layer (media library) | 
| [sndio](https://packages.gentoo.org/useflags/sndio) | Enable support for the media-sound/sndio backend | 

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose games-engines/minetest`
## See also

- [Games](https://wiki.gentoo.org/wiki/Games) — a landing page for many of the games (especially open source variants) available in Gentoo's main ebuild repository.
