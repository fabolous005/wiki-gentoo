<!-- source: https://wiki.gentoo.org/wiki/Rygel | group: Gentoo Wiki (Main) | wiki-title: Rygel -->
---
title: Rygel
url: https://wiki.gentoo.org/wiki/Rygel
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-10-16"
fingerprint: "95f1cf0a05e6d9dd"
license: CC BY-SA 4.0
---

# Rygel

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Rygel** is a home media solution (UPnP AV MediaServer).

## Setup using an init script

### Setup Rygel upnp/dlna media server

For this howto we can not (yet) use the tracker and dbus interface. The reason is that we start rygel as a daemon with an init script. This is means we do not have a dbus user session and thus can not use tracker and have to rely on the media export plugin's ability to scan the directories.

This howto is split over the following sections:

- USE flags and merging rygel
- create user and group
- copy/create the files
- The init script + config file
- Modify the config file

### Installation

#### USE flags


| [+introspection](https://packages.gentoo.org/useflags/+introspection) | Add support for GObject based introspection | 
| [+sqlite](https://packages.gentoo.org/useflags/+sqlite) | Add support for sqlite - embedded sql database | 
| [gtk](https://packages.gentoo.org/useflags/gtk) | Add support for x11-libs/gtk+ (The GIMP Toolkit) | 
| [gtk-doc](https://packages.gentoo.org/useflags/gtk-doc) | Build and install gtk-doc based developer documentation for dev-util/devhelp, IDE and offline use | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [tracker](https://packages.gentoo.org/useflags/tracker) | Install dependencies for the tracker plugin | 
| [transcode](https://packages.gentoo.org/useflags/transcode) | Install dependencies for transcoding support | 

#### Emerge

Install Rygel:

`root #``emerge --ask net-misc/rygel`
### create user and group

`root #``useradd --system -c "Rygel media server" --user-group --create-home --base-dir /var/lib --shell /sbin/nologin rygel`
### copy files

Copy the 2 files to the respective folders and you should be able to start the server.

**`/etc/init.d/rygel`**

```
#!/sbin/openrc-run
# Copyright 1999-2012 Gentoo Foundation
# Distributed under the terms of the GNU General Public License v2
# $Header: $
depend() {
		need net
}
start() {
		checkpath -f -o ${RYGEL_USER} -m 644 ${RYGEL_LOGFILE}
		ebegin "Starting Rygel"
		start-stop-daemon --start  --background --quiet \
				--make-pidfile \
				--pidfile ${RYGEL_PIDFILE} \
				--user ${RYGEL_USER} \
				--group ${RYGEL_GROUP} \
				--stdout ${RYGEL_LOGFILE} \
				--stderr ${RYGEL_LOGFILE} \
				--exec /usr/bin/rygel -- --config ${RYGEL_CONFIG} \
				${RYGEL_OPTIONS}
		eend $?
}
stop() {
		ebegin "Stopping Rygel"
		start-stop-daemon --stop --retry 10 --quiet --pidfile ${RYGEL_PIDFILE}
		eend $?
}
```
**`/etc/conf.d/rygel`**

```
# /etc/conf.d/rygel: config file for /etc/init.d/rygel
#Rygel pid file
RYGEL_PIDFILE="/var/run/rygel.pid"
#Log file
RYGEL_LOGFILE="/var/log/rygel.log"
#Run as user
RYGEL_USER="rygel"
#Rygel group
RYGEL_GROUP="rygel"
#Path to config file
RYGEL_CONFIG="/etc/rygel.conf"
#Rygel options
#RYGEL_OPTIONS="--disable-transcoding"
```
### Modify config file

If started with the init script you will want to modify the uri option. Add the directories you like it to search for media.

**`/etc/rygel.conf`**

```
[MediaExport]
enabled=true
title=@REALNAME@'s media
# List of URIs to export. Following variables are automatically substituted by
# the appropriate XDG standard media folders by Rygel for you.
#
#       * @MUSIC@: The standard music folder (typically ${HOME}/Music).
#       * @VIDEOS@: The standard videos folder (typically ${HOME}/Videos).
#       * @PICTURES@: The standard pictures folder (typically ${HOME}/Pictures).
#
#uris=@MUSIC@;@VIDEOS@;@PICTURES@
uris=@MUSIC@;@VIDEOS@;@PICTURES@;/path/to/music;/path/to/video
extract-metadata=true
monitor-changes=true
```
## Setup using shell-session

shell-session is a script created by eva and is available from [github](https://github.com/EvaSDK/shell-session/blob/master/shell-session). It is supposed to create a dbus session for rygel under a regular user and from /home/\<user> and then start /etc/xdg/autostart (which has en entry for rygel). I tried this and it seems to not work very well for me. At this point I can't really recommend this but if someone has more success please update this wiki page.
