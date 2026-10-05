<!-- source: https://wiki.gentoo.org/wiki/BitTorrent | group: Gentoo Wiki (Main) | wiki-title: BitTorrent -->
---
title: BitTorrent
url: https://wiki.gentoo.org/wiki/BitTorrent
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-03-09"
fingerprint: "1eefee7abbaf8f21"
license: CC BY-SA 4.0
---

# BitTorrent

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**BitTorrent** is a decentralized file sharing protocol.

A torrent is a group of peer-to-peer (P2P) clients participating in a coordinated sharing of one or more files. The structure of this torrent group as well as the metadata involved with the torrent is defined by the tracker file.

Clients that want to participate in the torrent connect to the tracker to find out from who to download files. Recent versions of this method also support *peer exchange* so that clients can communicate to other clients who participates without the need for registering this in the tracker.

## Clients

### Command-line clients

| Name | Package | Description | 
|---|---|---|
| [RTorrent](https://wiki.gentoo.org/wiki/RTorrent) | [net-p2p/rtorrent](https://packages.gentoo.org/packages/net-p2p/rtorrent) | Text-based ncurses BitTorrent client written in C++. | 
| ELinks | [www-client/elinks](https://packages.gentoo.org/packages/www-client/elinks) | Built with the [bittorrent](https://packages.gentoo.org/useflags/bittorrent) [USE flag](https://wiki.gentoo.org/wiki/USE_flag), ELinks has a [BitTorrent client add-on](http://elinks.or.cz/documentation/html/manual.html-chunked/ch13.html). | 

### Graphical clients

| Name | Package | Description | 
|---|---|---|
| KTorrent | [net-p2p/ktorrent](https://packages.gentoo.org/packages/net-p2p/ktorrent) | [KDE](https://wiki.gentoo.org/wiki/KDE) based BitTorrent client. | 
| [QBittorrent](https://wiki.gentoo.org/wiki/QBittorrent) | [net-p2p/qbittorrent](https://packages.gentoo.org/packages/net-p2p/qbittorrent) | [Qt](https://wiki.gentoo.org/wiki/Qt) based BitTorrent client. | 

### Command-line and graphical clients

| Name | Package | Description | 
|---|---|---|
| [Deluge](https://wiki.gentoo.org/wiki/Deluge) | [net-p2p/deluge](https://packages.gentoo.org/packages/net-p2p/deluge) | Client/server based BitTorrent capable application with both CLI and GUI (GTK 3). | 
| Transmission | [net-p2p/transmission](https://packages.gentoo.org/packages/net-p2p/transmission) | Supports both CLI and GUI (GTK 3 and Qt 5). | 
| BiglyBT | [net-p2p/biglybt](https://packages.gentoo.org/packages/net-p2p/biglybt) | Feature-filled Bittorrent client based on the Azureus open source project | 

## External resources

- [Torrent file](https://en.wikipedia.org/wiki/Torrent_file) (Wikipedia)
