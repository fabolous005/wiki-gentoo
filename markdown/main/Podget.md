<!-- source: https://wiki.gentoo.org/wiki/Podget | group: Gentoo Wiki (Main) | wiki-title: Podget -->
---
title: podget
url: https://wiki.gentoo.org/wiki/Podget
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-05-22"
fingerprint: f3787bfc778773d1
license: CC BY-SA 4.0
---

# podget

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


podget is a simple [podcast](https://en.wikipedia.org/wiki/podcast) aggregator optimized for running as a scheduled job written in [Bash](https://wiki.gentoo.org/wiki/Bash). Despite its size and complexity podget only has two significant dependencies: [wget](https://wiki.gentoo.org/wiki/Wget) and [iconv](https://wiki.gentoo.org/index.php?title=Iconv&action=edit&redlink=1). It supports downloading media from the following feed types:

- RSS.
- ATOM.
- iTunes PCAST.

## Installation

### Emerge

Install [media-sound/podget](https://packages.gentoo.org/packages/media-sound/podget):

`root #``emerge --ask media-sound/podget`
## Configuration

Configuration is mostly handled through the podgetrc and serverlist files located in the user's home directory. It's also possible to override the defaults set in these files via command line switches at runtime.

### Files

- \~/.podget/podgetrc — the configuration file location for podget.
- \~/.podget/serverlist — the list of podcast feeds by URL, category, and podcast name.
- \~/POD/ — the default location where podget drops fetched podcast media.

Unfortunately, podget does not obey [XDG](https://wiki.gentoo.org/wiki/XDG) paths by default. For the configuration file, it's possible to fake support for XDG paths by passing podget --config ${XDG\_CONFIG\_HOME}/podget/podgetrc; assuming the file already exists. In order to redirect podget to do the same with the serverlist file, the option `config_serverlist`=`${XDG_CONFIG_HOME}/podget/serverlist` must be set the podgetrc file.

## Usage

Typically podget is run in the background via task scheduler such as a [cron](https://wiki.gentoo.org/wiki/Cron) task or [systemd](https://wiki.gentoo.org/wiki/Systemd) timer.

### Scheduled Task

#### cron

A typical crontab file to periodically run podget might include something like this:

**`/etc/crontab`**

**Example crontab entry**

The above fetches podcast media files at 02:15. The `--silent` option is required to suppress output.

#### systemd

The first step is to create a unit file to tell systemd what to run:

**`podget.service`**

```
[Unit]
Description=A service for fetching podcast media.
 
[Service]
Type=simple
ExecStart=/usr/bin/podget --silent
 
[Install]
WantedBy=default.target
```
With the unit file created it's time to create the timer file to tell systemd when to run the podget service.

**`podget.timer`**

```
[Unit]
Description=Fetch podcast media once per day.
[Timer]
Persistent=true
OnCalendar=*-*-* 02:15:00
Unit=podget.service
 
[Install]
WantedBy=timers.target
```
Similar to the cron example the above systemd example fetches podcast media at 02:15. If the server is not online at 02:15 then the media is fetched the next time the server comes up.

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose media-sound/podget`
