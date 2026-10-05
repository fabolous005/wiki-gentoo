<!-- source: https://wiki.gentoo.org/wiki/IRC | group: Gentoo Wiki (Main) | wiki-title: IRC -->
---
title: IRC
url: https://wiki.gentoo.org/wiki/IRC
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-19"
fingerprint: "609bff184aa92b9c"
license: CC BY-SA 4.0
---

# IRC

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Internet Relay Chat** (**IRC**) is a stable, mature, text-based chat (instant messaging) system. IRC is one of the primary avenues of communication for those involved at all levels in the [Gentoo project](https://wiki.gentoo.org/wiki/Project:Gentoo).

## IRC and Gentoo

Since chat messages are sent and received almost instantly, IRC allows the Gentoo project to have a very reactive collaboration processes.

There are Gentoo IRC channels for user support. For any issue, a preexisting solution should be sought using a search engine, the [forums](https://forums.gentoo.org/), [mailing lists](https://www.gentoo.org/get-involved/mailing-lists/all-lists.html), and here on the wiki, before asking on a support channel.

Most Gentoo subprojects have a specific channel on [Libera Chat](https://libera.chat/) for inter-project communication. Project-related channels are used for: tracking changes to source code, requesting and discussing (new) features, addressing various issues, real-time team meetings, internal support, and communication between Gentoo developers and the community.  The wiki itself has several [wiki IRC channels](https://wiki.gentoo.org/wiki/Project:Wiki/Wiki_IRC_channels).

There is a bot in many channels called [Willikins](https://wiki.gentoo.org/wiki/Willikins) that has some handy functionality.

## Available software

The following table contains some of the IRC software available in Gentoo. Discover which client works best by emerging each one or by visiting each homepage:

| Name | Package | Homepage | Description | 
|---|---|---|---|
| catgirl | [net-irc/catgirl::guru](https://gpo.zugaina.org/Overlays/guru/net-irc/catgirl) | [https://git.causal.agency/catgirl/about/](https://git.causal.agency/catgirl/about/) | A TLS-only terminal IRC client | 
| [HexChat](https://wiki.gentoo.org/wiki/HexChat) | [net-irc/hexchat](https://packages.gentoo.org/packages/net-irc/hexchat) | [https://hexchat.github.io/](https://hexchat.github.io/) | Graphical IRC client based on XChat. | 
| [Irssi](https://wiki.gentoo.org/wiki/Irssi) | [net-irc/irssi](https://packages.gentoo.org/packages/net-irc/irssi) | [https://irssi.org/](https://irssi.org/) | A modular text UI IRC client with IPv6 support. | 
| ircii | [net-irc/ircii](https://packages.gentoo.org/packages/net-irc/ircii) | [http://eterna.com.au/ircii/](http://eterna.com.au/ircii/) | An IRC and ICB client that runs under most UNIX platforms. | 
| [Konversation](https://wiki.gentoo.org/wiki/Konversation) | [net-irc/konversation](https://packages.gentoo.org/packages/net-irc/konversation) | [https://konversation.kde.org/](https://konversation.kde.org/) | User friendly IRC Client based on KDE Frameworks. | 
| kvirc | [net-irc/kvirc](https://packages.gentoo.org/packages/net-irc/kvirc) | [http://www.kvirc.net/](http://www.kvirc.net/) | A portable IRC client that uses the Qt GUI toolkit. | 
| [Pidgin](https://wiki.gentoo.org/wiki/Pidgin) | [net-im/pidgin](https://packages.gentoo.org/packages/net-im/pidgin) | [https://pidgin.im/](https://pidgin.im/) | GTK Instant Messenger client. | 
| Polari | [net-irc/polari](https://packages.gentoo.org/packages/net-irc/polari) | [https://wiki.gnome.org/Apps/Polari](https://wiki.gnome.org/Apps/Polari) | An IRC client for GNOME | 
| [Quassel](https://wiki.gentoo.org/wiki/Quassel) | [net-irc/quassel](https://packages.gentoo.org/packages/net-irc/quassel) | [https://quassel-irc.org/](https://quassel-irc.org/) | Qt5 IRC client supporting a remote daemon for 24/7 connectivity | 
| Srain | [net-irc/srain::guru](https://gpo.zugaina.org/Overlays/guru/net-irc/srain) | [https://srain.silverrainz.me/](https://srain.silverrainz.me/) | Modern, beautiful IRC client written in GTK+ 3 | 
| [WeeChat](https://wiki.gentoo.org/wiki/WeeChat) | [net-irc/weechat](https://packages.gentoo.org/packages/net-irc/weechat) | [https://weechat.org/](https://weechat.org/) | Portable and multi-interface (text, web, and GUI) IRC client. | 

## Usage

Learning can be more challenging when starting using a text-mode (CLI) client like [Irssi](https://wiki.gentoo.org/wiki/Irssi) or [WeeChat](https://wiki.gentoo.org/wiki/WeeChat), the [IRC guide](https://wiki.gentoo.org/wiki/IRC/Guide) can be particularly helpful with this.

### Spam protection

In order to prevent **private message spam** on IRC from unregistered users, one can set **user modes** `+g` or `+R`.

Larry wants to ignore private messages from users who are not identified, but wants to get an information that someone wanted to send a message to Larry. Larry can decide to receive messages with the /accept command:

`/mode larry +g`
Larry wants to ignore private messages from users who are not identified totally:

`/mode larry +R`
## External resources

- [https://gentoo.org/get-involved/irc-channels/](https://gentoo.org/get-involved/irc-channels/)
- [https://www.irchelp.org/](https://www.irchelp.org/) - A site dedicated to helping users understand IRC.
- [https://libera.chat/](https://libera.chat/) - A next-generation IRC network for free and open source software projects and similarly-spirited collaborative endeavors.
- [https://libera.chat/guides/usermodes](https://libera.chat/guides/usermodes) - Libera Chat article on user modes\*
- [https://www.oftc.net/](https://www.oftc.net/) - A stable and effective IRC network that provides collaboration services to members of the free software community in any part of the world.
