<!-- source: https://wiki.gentoo.org/wiki/Gemini | group: Gentoo Wiki (Main) | wiki-title: Gemini -->
---
title: Gemini
url: https://wiki.gentoo.org/wiki/Gemini
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-01-23"
fingerprint: b9c7a1ce883e9a8d
license: CC BY-SA 4.0
---

# Gemini

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Gemini** is a lightweight application-layer Internet protocol for accessing remote documents. You may think of Gemini as stripped-down HTTP or souped-up [Gopher](https://wiki.gentoo.org/index.php?title=Gopher&action=edit&redlink=1). Quoting [the Gemini FAQ](https://portal.mozz.us/gemini/geminiprotocol.net/docs/faq.gmi):

Both the protocol and the format are deliberately limited in capabilities and scope, and the protocol is technically conservative, being built on mature, standardised, familiar, "off-the-shelf" technologies like URIs, MIME media types and TLS. Simplicity and finite scope are very intentional design decisions motivated by placing a high priority on user autonomy, user privacy, ease of implementation in diverse computing environments, and defensive non-extensibility.

...

Gemini is not intended to replace either Gopher or the web, but to co-exist peacefully alongside them as one more option which people can freely choose to use if it suits them.


Geminispace is accessed via the non-standard gemini:// [URL](https://en.wikipedia.org/wiki/URL)/[URI](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier) scheme. Most pages are written with a special [Markdown](https://wiki.gentoo.org/index.php?title=Markdown&action=edit&redlink=1)-like markup called Gemtext, and served with the unofficial MIME type text/gemini.

## Available software

Some of the software listed in this section is provided via the [GURU](https://wiki.gentoo.org/wiki/GURU) repository:

`root #````
eselect repository enable guru
```
`root #````
emaint sync -r guru
```
### Browsers

- [app-emacs/elpher](https://packages.gentoo.org/packages/app-emacs/elpher) - Practical and friendly Gopher and Gemini client for [GNU Emacs](https://wiki.gentoo.org/wiki/GNU_Emacs)
- [net-client/amfora::guru](https://gpo.zugaina.org/Overlays/guru/net-client/amfora) - A fancy terminal browser for the Gemini protocol
- [net-client/castor::guru](https://gpo.zugaina.org/Overlays/guru/net-client/castor) - A graphical browser for plain-text protocols
- [net-client/kristall::guru](https://gpo.zugaina.org/Overlays/guru/net-client/kristall) - Visual cross-platform gemini browser
- [net-client/lagrange::guru](https://gpo.zugaina.org/Overlays/guru/net-client/lagrange) - Desktop GUI client for browsing Geminispace
- [net-client/offpunk::guru](https://gpo.zugaina.org/Overlays/guru/net-client/offpunk) - Offline-First Gemini/Web/Gopher/RSS reader and browser
- [net-client/starfish::guru](https://gpo.zugaina.org/Overlays/guru/net-client/starfish) - A Gemini browser for elementary OS
- [net-client/telescope::guru](https://gpo.zugaina.org/Overlays/guru/net-client/telescope) - w3m-like browser for Gemini

If [eix](https://wiki.gentoo.org/wiki/Eix) has been installed and configured to search remote repositories, available Gemini browsers can be listed by running:

`user $``eix -RC net-client`
The [Amfora](https://github.com/makew0rld/amfora), [AV-98](https://github.com/atbradley/AV-98) and [Bombadillo](https://bombadillo.colorfield.space/) terminal-based browsers can be tried out by accessing the Gemini kiosk via **ssh**:

`user $``ssh kiosk@gemini.circumlunar.space`
#### [KDE](https://wiki.gentoo.org/wiki/KDE) support

Konqueror can become a Gemini browser, and all applications that utilize KIO and KParts frameworks can support Gemini as well.

Enable the GURU repository, as described above, and install the relevant plugins:

`root #``emerge --ask kde-misc/geminipart kde-misc/kio-gemini`
### Servers

- [net-misc/gemserv::guru](https://gpo.zugaina.org/Overlays/guru/net-misc/gemserv) (Rust)
- [net-misc/gmid::guru](https://gpo.zugaina.org/Overlays/guru/net-misc/gmid) (C)

### Utilities

- [app-vim/gemini-vim::guru](https://gpo.zugaina.org/Overlays/guru/app-vim/gemini-vim) - [Vim](https://wiki.gentoo.org/wiki/Vim) syntax highlighting for Gemtext.
- [app-text/lowdown](https://packages.gentoo.org/packages/app-text/lowdown) - Translate Markdown to Gemtext.
- [net-news/comitium::guru](https://gpo.zugaina.org/Overlays/guru/net-news/comitium) - Feed aggregator for Gemini supporting many formats and protocols.
- [net-misc/gemget::guru](https://gpo.zugaina.org/Overlays/guru/net-misc/gemget) - Command line downloader for the Gemini protocol.
- [md2gemini](https://pypi.org/project/md2gemini/) - Markdown-to-Gemtext converter written in Python.
- [mdbook-gemini](https://github.com/void-linux/void-docs/blob/master/res/mdbook-gemini) - Simple Perl script to convert mdBook to Gemtext using **md2gemini**.

## Some starting points in Geminispace

### Search engines

### Aggregators

### Directories

## See also

- [Usenet](https://wiki.gentoo.org/wiki/Usenet) — a federated and decentralized worldwide Internet forum and the world's oldest digital social network

## External resources

- [awesome-gemini](https://git.sr.ht/~kr1sp1n/awesome-gemini) - A collection of awesome things regarding the Gemini protocol ecosystem.
- [Gemini Server of the Day](gemini://sotd.sysrq.in/)
