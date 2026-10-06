<!-- source: https://wiki.gentoo.org/wiki/Gerbera | group: Gentoo Wiki (Main) | wiki-title: Gerbera -->
---
title: Gerbera
url: https://wiki.gentoo.org/wiki/Gerbera
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-10-16"
fingerprint: "6c03e3482db339ec"
license: CC BY-SA 4.0
---

# Gerbera

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Gerbera** is an open source [UPnP](https://en.wikipedia.org/wiki/Universal_Plug_and_Play) media server that streams digital media through a home network to UPnP compatible devices. Gerbera is based on [MediaTomb](https://sourceforge.net/projects/mediatomb/) 0.12.1, which is no longer maintained.

## Installation

### Kernel

Gerbera requires IP multicast support for automatic discovery by UPnP devices.

**Enabling IP multicast support**

```
[*] Networking support --->
            Networking options --->
              [*] TCP/IP networking
              [*]   IP: multicasting
```
Gerbera supports [inotify](https://en.wikipedia.org/wiki/Inotify), which is a file-monitoring mechanism that allows Gerbera to be notified about changes to files immediately. For more information, please consult the Gerbera [trail operations](http://docs.gerbera.io/en/latest/ui.html#trail-operations) documentation.

**Enabling inotify support**

```
    File systems --->
      [*] Inotify support for userspace
```
### USE flags


### USE flags for
            [net-misc/gerbera](https://packages.gentoo.org/packages/net-misc/gerbera)
            
            UPnP Media Server

| [+exif](https://packages.gentoo.org/useflags/+exif) | Add support for reading EXIF headers from JPEG and TIFF images | 
| [+ffmpeg](https://packages.gentoo.org/useflags/+ffmpeg) | Enable ffmpeg/libav-based audio/video codec support | 
| [+javascript](https://packages.gentoo.org/useflags/+javascript) | Enable javascript support | 
| [+magic](https://packages.gentoo.org/useflags/+magic) | Add support for file type detection via magic bytes (usually via libmagic from sys-apps/file) | 
| [+matroska](https://packages.gentoo.org/useflags/+matroska) | Add support for the matroska container format (extensions .mkv, .mka and .mks) | 
| [+taglib](https://packages.gentoo.org/useflags/+taglib) | Use media-libs/taglib for reading files' metadata | 
| [curl](https://packages.gentoo.org/useflags/curl) | Support HTTP media sources (e.g. internet radio) | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [exiv2](https://packages.gentoo.org/useflags/exiv2) | Use media-gfx/exiv2 to extract EXIF information | 
| [ffmpegthumbnailer](https://packages.gentoo.org/useflags/ffmpegthumbnailer) | Enable video thumbnail support with media-video/ffmpegthumbnailer | 
| [icu](https://packages.gentoo.org/useflags/icu) | Enable ICU (Internationalization Components for Unicode) support, using dev-libs/icu | 
| [mysql](https://packages.gentoo.org/useflags/mysql) | Use dev-db/mysql as backend rather than SQLite3 | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

After setting any USE configuration, proceed to install Gerbera:

`root #``emerge --ask net-misc/gerbera`
## Configuration

### Network

- To set the network interface that Gerbera binds to, add the following to the `GERBERA_OPTIONS` variable, and substitute `network_interface` with the appropriate value e.g. `GERBERA_OPTIONS="-e eth0"`

**`/etc/conf.d/gerbera`**

```
GERBERA_OPTIONS="-e network_interface"
```
- To set the IP address that Gerbera binds to, add the following to the `GERBERA_OPTIONS` variable, and substitute `ip_address` with the appropriate value e.g. `GERBERA_OPTIONS="-i 192.168.0.1"`

**`/etc/conf.d/gerbera`**

```
GERBERA_OPTIONS="-i ip_address"
```
### MySQL

- To enable MySQL support, set the `enabled` attribute to `yes` for the `<mysql>` element:

**`/etc/gerbera/config.xml`**

```
      <mysql enabled="yes">
```
- Set the `enabled` attribute to `no` for the `<sqlite3>` element:

**`/etc/gerbera/config.xml`**

```
      <sqlite3 enabled="no">
```
- Set the `<host>`, `<database>`, `<username>` and `<password>` elements:

**`/etc/gerbera/config.xml`**

```
        <host>host</host>
        <database>database</database>
        <username>username</username>
        <password>password</password>
```
- Login to MySQL and create the Gerbera database and user, using the `host`, `database`, `username` and `password` values set in the Gerbera configuration file:

`mysql>````
CREATE DATABASE database;
```
`mysql>````
GRANT ALL ON database.* TO 'username'@'host' IDENTIFIED BY 'password';
```
### Transcoding

Gerbera supports transcoding media files to formats that are supported by the UPnP device being used. The default Gentoo configuration file uses FFmpeg to transcode FLAC, Flash, Theora and Vorbis files. For more information, please consult the Gerbera [transcoding configuration](http://docs.gerbera.io/en/latest/config-transcode.html) documentation.

- To enable transcoding support, set the `enabled` attribute to `yes` for the `<transcoding>` element:

**`/etc/gerbera/config.xml`**

```
  <transcoding enabled="yes">
```
- For every additional mimetype that requires transcoding, add the following section in between the `<mimetype-profile-mappings>` and `</mimetype-profile-mappings>` elements and substitute `mimetype` and `profile` with the appropriate values e.g. `<transcode mimetype="video/quicktime" using="video2mpeg"/>`

**`/etc/gerbera/config.xml`**

```
      <transcode mimetype="mimetype" using="profile"/>
```
- Install FFmpeg:

`root #``emerge --ask --noreplace media-video/ffmpeg`
#### Alternative transcoders

Gerbera's transcoding support is very flexible and any application capable of transcoding can be used. For more information, please consult the Gerbera [transcoding](http://docs.gerbera.io/en/latest/transcoding.html) documentation.

##### VLC

**Audio**

- To enable VLC audio transcoding, replace the `<agent>` element for the `audio2pcm` profile with the following:

**`/etc/gerbera/config.xml`**

```
        <agent command="vlc" arguments="%in -I dummy --sout=#transcode{acodec=s16b,ab=192,samplerate=44100,channels=2}:standard{access=file,mux=raw,dst=%out} vlc://quit"/>
```
- Set the `<accept-url>` element to `yes` for the `audio2pcm` profile:

**`/etc/gerbera/config.xml`**

```
        <accept-url>yes</accept-url>
```
**Video**

- To enable VLC video transcoding, replace the `<agent>` element for the `video2mpeg` profile with the following:

**`/etc/gerbera/config.xml`**

```
        <agent command="vlc" arguments="%in -I dummy --sout=#transcode{vcodec=mp2v,vb=4096,fps=25,acodec=mpga,ab=192,samplerate=48000,channels=2,audio-sync}:standard{access=file,mux=ps,dst=%out} vlc://quit"/>
```
- Set the `<accept-url>` element to `yes` for the `video2mpeg` profile:

**`/etc/gerbera/config.xml`**

```
        <accept-url>yes</accept-url>
```
- Install VLC:

`root #``emerge --ask --noreplace media-video/vlc`
##### MPlayer

**Video**

- To enable MPlayer video transcoding, replace the `<agent>` element for the `video2mpeg` profile with the following:

**`/etc/gerbera/config.xml`**

```
        <agent command="mencoder" arguments="%in -o %out -ovc lavc -oac lavc -lavcopts vcodec=mpeg2video:vbitrate=4096:vrc_minrate=0:vrc_maxrate=9800:vrc_buf_size=1835:keyint=15:vstrict=0:acodec=mp2:abitrate=192 -vf harddup -af lavcresample=48000:channels=2 -srate 48000 -ofps 25 -of mpeg -mpegopts format=mpeg2:tsaf"/>
```
- Set the `<accept-url>` element to `yes` for the `video2mpeg` profile:

**`/etc/gerbera/config.xml`**

```
        <accept-url>yes</accept-url>
```
- Install MPlayer:

`root #``emerge --ask --noreplace media-video/mplayer`
### DLNA

- To enable DLNA support, set the `extend` attribute to `yes` for the `<protocolInfo>` element:

**`/etc/gerbera/config.xml`**

```
    <protocolInfo extend="yes">
```
- For DLNA enabled TVs (e.g. Samsung), add the following section in between the `<server>` and `</server>` elements:

**`/etc/gerbera/config.xml`**

```
    <custom-http-headers>
      <add header="transferMode.dlna.org: Streaming"/>
      <add header="contentFeatures.dlna.org: DLNA.ORG_OP=01;DLNA.ORG_CI=0;DLNA.ORG_FLAGS=01500000000000000000000000000000"/>
    </custom-http-headers>
```
### Video thumbnails

- To enable video thumbnail support, set the `enabled` attribute to `yes` for the `<ffmpegthumbnailer>` element:

**`/etc/gerbera/config.xml`**

```
      <ffmpegthumbnailer enabled="yes">
```
- To overlay a filmstrip border on the generated thumbnail, set the `<filmstrip-overlay>` element to `yes`:

**`/etc/gerbera/config.xml`**

```
        <filmstrip-overlay>yes</filmstrip-overlay>
```
- For [DLNA](https://wiki.gentoo.org#DLNA) enabled devices that support video thumbnails (e.g. PlayStation 3), set the `extend` attribute to `yes` for the `<protocolInfo>` element:

**`/etc/gerbera/config.xml`**

```
    <protocolInfo extend="yes">
```
### Raw images

- To enable (Canon CR2 and Nikon NEF) [raw image](https://en.wikipedia.org/wiki/Raw_image_format) support, enable [transcoding](https://wiki.gentoo.org#Transcoding) and add the following section in between the `<profile>` and `</profile>` elements:

**`/etc/gerbera/config.xml`**

```
      <profile name="raw2jpeg" enabled="yes" type="external">
        <mimetype>image/jpeg</mimetype>
        <accept-url>no</accept-url>
        <first-resource>yes</first-resource>
        <hide-original-resource>yes</hide-original-resource>
        <use-chunked-encoding>no</use-chunked-encoding>
        <agent command="/usr/local/bin/gerbera-raw2jpeg" arguments="%in %out"/>
        <buffer size="524288" chunk-size="512" fill-size="1024"/>
      </profile>
```
- Add the following section in between the `<mimetype-profile-mappings>` and `</mimetype-profile-mappings>` elements:

**`/etc/gerbera/config.xml`**

```
      <transcode mimetype="image/raw" using="raw2jpeg"/>
```
- For every additional raw image format (supported by [dcraw](http://www.cybercom.net/~dcoffin/dcraw/)), add the following section in between the `<extension-mimetype ignore-unknown="no">` and `</extension-mimetype>` elements, and substitute `extension` with the appropriate value e.g. `<map from="kdc" to="image/raw"/>`

**`/etc/gerbera/config.xml`**

```
        <map from="extension" to="image/raw"/>
```
- Since dcraw only outputs to stdout, the output will need to be redirected with the following script:

**`/usr/local/bin/gerbera-raw2jpeg`**

```
#!/bin/sh
DCRAW_PATH="/usr/bin/dcraw"
INPUT="$1"
OUTPUT="$2"
exec "${DCRAW_PATH}" -e -c "${INPUT}" > "${OUTPUT}"
```
- Install dcraw:

`root #``emerge --ask --noreplace media-gfx/dcraw`
### Service

#### OpenRC

- To start Gerbera:

`root #``rc-service gerbera start`
- To start Gerbera at boot:

`root #``rc-update add gerbera default`
#### systemd

Start Gerbera:

`root #``systemctl start gerbera`
Start Gerbera at boot:

`root #``systemctl enable gerbera`
## Troubleshooting

### Mimetype mapping

Gerbera (via libmagic) may identify the mimetype of some files incorrectly. A common case is where *videos* with the `mp4` extension are identified as the mimetype `audio/mp4`. To override the mimetype returned by libmagic, add the following section in between the `<extension-mimetype ignore-unknown="no">` and `</extension-mimetype>` elements, and substitute `extension` and `mimetype` with the appropriate values e.g. `<map from="mp4" to="video/mp4"/>`

**`/etc/gerbera/config.xml`**

```
        <map from="extension" to="mimetype"/>
```
## See also

- [MiniDLNA](https://wiki.gentoo.org/wiki/MiniDLNA) — a media server aiming to be DLNA/UPnP-AV compliant.

## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) seppbiersack. [Samsung TV Support](https://github.com/gerbera/gerbera/issues/352), [Gerbera GitHub](https://github.com/gerbera/gerbera), September 26th, 2018. Retrieved on March 7th, 2019.
