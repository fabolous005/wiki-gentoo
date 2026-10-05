<!-- source: https://wiki.gentoo.org/wiki/MythTV | group: Gentoo Wiki (Main) | wiki-title: MythTV -->
---
title: MythTV
url: https://wiki.gentoo.org/wiki/MythTV
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-10-16"
fingerprint: ee03d14e9d2f398c
license: CC BY-SA 4.0
---

# MythTV

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**MythTV** is a powerful media center and video recording software system. The distributed architecture allows analog and digital media to be captured, organized, and streamed over the network to other MythTV instances or network attached devices.

## Installation

MythTV is a complex and capable system and during the first install can be a little daunting to configure if there is no familiarity with the software. Once the backend has been set up properly and configured with recording devices/sources (DVB or analog capture etc.), there is usually little maintenance in the future.

### Kernel

Depending on how MythTV is going to be configured, there may be options in the kernel that need to be enabled. If support is required for recording media from DVB devices or controlling with a remote control, the relevant devices need to be enabled in the kernel before MythTV or any other applications will be able to access them.

**Example enabling support for DVB devices**

### USE flags

MythTV has quite a few USE flags to customize configuration according to hardware specification and software requirements.


| [+X](https://packages.gentoo.org/useflags/+X) | Add support for X11 | 
| [+lame](https://packages.gentoo.org/useflags/+lame) | Add support for MP3 encoding using LAME | 
| [+opengl](https://packages.gentoo.org/useflags/+opengl) | Add support for OpenGL (3D graphics) | 
| [+wrapper](https://packages.gentoo.org/useflags/+wrapper) | Use Ubuntu mythtfrontend wrapper | 
| [+xml](https://packages.gentoo.org/useflags/+xml) | Add support for XML files | 
| [+xvid](https://packages.gentoo.org/useflags/+xvid) | Add support for xvid.org's open-source mpeg-4 codec | 
| [alsa](https://packages.gentoo.org/useflags/alsa) | Allows MythTV to directly output sound to ALSA devices | 
| [asi](https://packages.gentoo.org/useflags/asi) | Support for DVEO ASI recorder | 
| [autostart](https://packages.gentoo.org/useflags/autostart) | Use a custom autostart configuration | 
| [cdda](https://packages.gentoo.org/useflags/cdda) | Add Compact Disk Digital Audio (Standard Audio CD) support | 
| [cdr](https://packages.gentoo.org/useflags/cdr) | Add support for CD writer hardware | 
| [cec](https://packages.gentoo.org/useflags/cec) | Allows control of CEC enabled TVs via HDMI | 
| [ceton](https://packages.gentoo.org/useflags/ceton) | Ceton InfiniTV 4 a CableCARD-enabled tuner support | 
| [debug](https://packages.gentoo.org/useflags/debug) | Instructs Qt to use the 'debug' target | 
| [dvb](https://packages.gentoo.org/useflags/dvb) | Add support for DVB (Digital Video Broadcasting) | 
| [dvd](https://packages.gentoo.org/useflags/dvd) | Add support for DVDs | 
| [exif](https://packages.gentoo.org/useflags/exif) | Add support for reading EXIF headers from JPEG and TIFF images | 
| [hdhomerun](https://packages.gentoo.org/useflags/hdhomerun) | Silicondust USA Inc.network-attached tuner support | 
| [ieee1394](https://packages.gentoo.org/useflags/ieee1394) | Firewire enabled Cable boxe support | 
| [jack](https://packages.gentoo.org/useflags/jack) | Allows MythTV to use JACK as your sound output device | 
| [java](https://packages.gentoo.org/useflags/java) | BD-J support for Blu-ray discs | 
| [lcd](https://packages.gentoo.org/useflags/lcd) | Enable use of app-misc/lcdproc data display | 
| [libass](https://packages.gentoo.org/useflags/libass) | SRT/SSA/ASS (SubRip / SubStation Alpha) subtitle support | 
| [lirc](https://packages.gentoo.org/useflags/lirc) | LIRC remote control device support | 
| [nvdec](https://packages.gentoo.org/useflags/nvdec) | Enable NVDEC (NVCUVID) hardware accelerated video decoding | 
| [oss](https://packages.gentoo.org/useflags/oss) | Add support for OSS (Open Sound System) | 
| [perl](https://packages.gentoo.org/useflags/perl) | Build the perl bindings for MythTV | 
| [pulseaudio](https://packages.gentoo.org/useflags/pulseaudio) | Add sound server support via media-libs/libpulse (may be PulseAudio or PipeWire) | 
| [python](https://packages.gentoo.org/useflags/python) | Add optional support/bindings for the Python language | 
| [raw](https://packages.gentoo.org/useflags/raw) | Add support for raw image formats | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [v4l](https://packages.gentoo.org/useflags/v4l) | Enable support for video4linux (using linux-headers or userspace libv4l libraries) | 
| [vaapi](https://packages.gentoo.org/useflags/vaapi) | Enable Video Acceleration API for hardware decoding | 
| [vbox](https://packages.gentoo.org/useflags/vbox) | V@Box Communications network-attached tuner devices support | 
| [vdpau](https://packages.gentoo.org/useflags/vdpau) | Enable the Video Decode and Presentation API for Unix acceleration interface | 
| [vpx](https://packages.gentoo.org/useflags/vpx) | Add support for VP8/VP9 codecs (usually via media-libs/libvpx) | 
| [x264](https://packages.gentoo.org/useflags/x264) | Enable H.264 encoding using x264 | 
| [x265](https://packages.gentoo.org/useflags/x265) | Enable h265 encoding using x265 | 
| [xmltv](https://packages.gentoo.org/useflags/xmltv) | Support media-tv/xmltv TV listing - not used by Schedules Direct\] | 
| [zeroconf](https://packages.gentoo.org/useflags/zeroconf) | Support for DNS Service Discovery (DNS-SD) | 

### Emerge

`root #``emerge --ask media-tv/mythtv`
## Configuration

MythTV includes a mythbackend server which handles recordings and manages databases of media, this has to be configured and running before the a client, mythfrontend, or other media player can interact with the MythTV system. MythTV also requires a running MySQL compatible database server such as [MySQL](https://wiki.gentoo.org/wiki/MySQL) or [MariaDB](https://wiki.gentoo.org/wiki/MariaDB) to function, this is handled through the database modules of Qt.

### User mythtv

A daemon user named mythtv for `mythbackend` is created and maintained by [acct-user/mythtv](https://packages.gentoo.org/packages/acct-user/mythtv).

Some existing installations also use mythtv user with `mythfrontend` which works, but can cause problems. An actual non-root user should be used with `mythfrontend`.

Old installations that use a modified mythtv user may either need updating or a local [acct-user/mythtv](https://packages.gentoo.org/packages/acct-user/mythtv) overlay to prevent undesired changes to the mythtv user. After [acct-user/mythtv](https://packages.gentoo.org/packages/acct-user/mythtv) is successfully emerged restore the mythtv user. It will remain at its restored state until [acct-user/mythtv](https://packages.gentoo.org/packages/acct-user/mythtv) is forced/updated. Installing the MythTV package [media-tv/mythtv](https://packages.gentoo.org/packages/media-tv/mythtv) depends on [acct-user/mythtv](https://packages.gentoo.org/packages/acct-user/mythtv) and does not install it again on subsequent updates.

### Database

A database will need to be set up before the MythTV backend can be started. The emerge command should have already pulled in MySQL or MariaDB but the database server may not have been configured yet, if this is the case please refer to [MySQL](https://wiki.gentoo.org/wiki/MySQL) or [MariaDB](https://wiki.gentoo.org/wiki/MariaDB) for information. The following command will set up the MythTV database using the emerge config phase, this will require a root user and password for the database.

`root #``emerge --config media-tv/mythtv`


Starting with MythTV version 0.26, the time zone tables must be loaded in MySQL.  The [mythtv wiki](https://www.mythtv.org/wiki/MySQL_Time_Zone_Tables) has additional details.  For the time being, the following can be used to check if the timezone data is already in the running mysql instance:

```
SELECT CONVERT_TZ(NOW(), 'SYSTEM', 'Etc/UTC');
```
The result should output a datetime value, or a TIMESTAMP.

If the output, instead, shows 'NULL', then the timezone information needs to be loaded into the "mysql" database using the "mysql\_tzinfo\_to\_sql" script:

`user $``mysql_tzinfo_to_sql /usr/share/zoneinfo | mysql -u root mysql`
### MythTV backend configuration

MythTV backend is configured by a GUI that can stop `mythbackend` when configuring. Run `mythtv-setup` GUI to setup MythTV before starting `mythfrontend` on a new installation. It is not normally used in daily operation once the system is configured for your setup/hardware. This GUI can setup:

\* Connection to the database
\* Setup TV tuners and scan channels
\* Setup media storage locations
\* Setup Program Guide Provider
\* Multiple MythTV backend configuration

Execute the setup program to configure MythTV. The [documentation for MythTV](https://www.mythtv.org/wiki/Setup_General) is a good reference at this point.

`root #``mythtv-setup`
### Mythbackend daemon

At least one backend must be running to use MythTV, this can be on the same host as the frontend or a different host on the network.

Start the MythTV backend on the chosen host(s) and add it to the default runlevel.

`root #````
/etc/init.d/mythbackend start
```
`root #````
rc-update add mythbackend default
```
## Usage

Once the database is set up and mythtv-setup has been completed, and the mythbackend started. General use is through the included mythfrontend.

### Invocation

#### MythTV frontend

The mythfrontend application is the frontend process of MythTV with a graphical user interface for the MythTV system. It can select TV shows to record/watch, play media, select GUI theme, show the status of MythTV, and configure playback display/audio.

`user $``mythfrontend`
## Troubleshooting

## Removal

### Unmerge

Removal can be as simple as running:

`root #``emerge --ask --depclean --verbose media-tv/mythtv`
No media, recordings, database, or configuration setup is deleted.

## See also

- [XmlTV](https://github.com/XMLTV/xmltv) - Gather television listings from various countries for MythTV database.
- [MythTV/MythPlugins](https://wiki.gentoo.org/index.php?title=MythTV/MythPlugins&action=edit&redlink=1) - Plugins for MythTV handling games, news, weather, etc.
- [MythTV/MythWeb](https://wiki.gentoo.org/index.php?title=MythTV/MythWeb&action=edit&redlink=1) - Web application for allowing control of MythTV from a web browser.
- [Kodi](https://wiki.gentoo.org/wiki/Kodi) — an open source home theater application.

## External resources

- [https://www.mythtv.org/wiki/](https://www.mythtv.org/wiki/) - Official MythTV Wiki
- [https://www.linuxtv.org/](https://www.linuxtv.org/) - Television with Linux
- [https://www.linuxtv.org/wiki/index.php/Main\_Page](https://www.linuxtv.org/wiki/index.php/Main_Page) - The Television with Linux wiki providing lots of information on configuring specific devices
