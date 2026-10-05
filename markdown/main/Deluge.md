<!-- source: https://wiki.gentoo.org/wiki/Deluge | group: Gentoo Wiki (Main) | wiki-title: Deluge -->
---
title: Deluge
url: https://wiki.gentoo.org/wiki/Deluge
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-01-27"
fingerprint: "3c8af95e790279b6"
license: CC BY-SA 4.0
---

# Deluge

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Deluge** is an open-source, cross platform [BitTorrent](https://wiki.gentoo.org/wiki/BitTorrent) client. It features a GTK-based GUI, a command line interface, a web interface, and also supports remote clients. Deluge is written in Python 3.

## Installation

### USE flags


| [appindicator](https://packages.gentoo.org/useflags/appindicator) | Build in support for notifications using the libindicate or libappindicator plugin | 
| [console](https://packages.gentoo.org/useflags/console) | Enable default console UI | 
| [gui](https://packages.gentoo.org/useflags/gui) | Enable support for a graphical user interface | 
| [libnotify](https://packages.gentoo.org/useflags/libnotify) | Enable desktop notification support | 
| [sound](https://packages.gentoo.org/useflags/sound) | Enable sound support | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [webinterface](https://packages.gentoo.org/useflags/webinterface) | Install dependencies needed for the web interface | 

### Emerge

`root #``emerge --ask net-p2p/deluge`
## Configuration

### Headless server

1. Start the deluged daemon `root #``/etc/init.d/deluged start`
  - To start the daemon on system launch, issue: `root #``rc-update add deluged default`
2. Make sure that Deluge is built with the `console` USE flag.
3. Enable remote connections
  - Switch to the deluge user `root #``su --shell /bin/bash deluge`
  - Open the deluge cli `deluge $``deluge-console`
  - Change the configuration value `>>>``config -s allow_remote True`
  - Leave the deluge cli `>>>``exit`
4. Set the server username and password `deluge $``echo "Larry:GentooLinuxrocks" >> ~/.config/deluge/auth`
5. Restart deluged `root #``/etc/init.d/deluged restart`

### Deluge web UI

1. Make sure that Deluge is built with the `webinterface` USE flag.
2. Configure deluge-web FILE**`/etc/conf.d/deluge-web`****Setting up the UI** \# /etc/conf.d/deluge-web # Change this to the user:group that should run deluge. DELUGE\_WEB\_USER="deluge:deluge" DELUGE\_WEB\_HOME="/var/lib/deluge" DELUGE\_WEB\_OPTS="-p 8112 --interface "\<ipv6 address or ipv4 address to bind>" -L=debug -c /var/lib/deluge -l /var/lib/deluge/deluge-web.log" 
  - Starting the daemon on system launch can be done with `root #``rc-update add deluge-web default`

#### Adding SSL support to UI

1. Make sure the certificate and key files are PEM encoded, and copy certificate.crt.pem and certificate.key.pem to the deluge home directory. Usually this is the /var/lib/deluge directory by default.
2. Edit the web.conf file. FILE**`/var/lib/deluge/web.conf`****Adding SSL support** { "file": 2, "format": 1 }{ "base": "/", "cert": "ssl/seedbox.cert", "pkey": "ssl/seedbox.key",

### Remote GTK client

1. Make sure that Deluge is built with the `gui` USE flag.
2. Launch deluge-gtk, go to *Edit -> Preferences -> Interface* and disable classic mode, then restart deluge-gtk.
3. Add the headless server to the connection manager, filling in the username, password, IP address and port. Then click connect.
4. Optionally, add the server as a default to hide the Connection Manager prompt.

## External resources

- [https://dev.deluge-torrent.org/wiki/Plugins](https://dev.deluge-torrent.org/wiki/Plugins) - A list of useful plugins, both included and third-party.
