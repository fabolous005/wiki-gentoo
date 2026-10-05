<!-- source: https://wiki.gentoo.org/wiki/Gentoo-zh | group: Gentoo Wiki (Main) | wiki-title: Gentoo-zh -->
---
title: Gentoo-zh
url: https://wiki.gentoo.org/wiki/Gentoo-zh
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-24"
fingerprint: "7f3d669bdb1666ae"
license: CC BY-SA 4.0
---

# Gentoo-zh

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Gentoo-zh Community** (**gentoo-zh**) started in 2003 among Chinese-speaking Gentoo Linux users and now serves all Gentoo users. It maintains the **gentoo-zh** [overlay](https://wiki.gentoo.org/wiki/Overlay), an ebuild repository for packages the main tree does not carry.

## History

The project dates back to 2003. Gentoo Linux Taiwan (GOT, then at gentoo.org.tw) was founded in April 2003 to promote Gentoo among Traditional Chinese users, alongside a parallel gentoo-china community serving Simplified Chinese users. Their two overlays, gentoo-taiwan-overlay and gentoo-china-overlay, were later merged into a single **gentoo-zh** overlay. The [gentoo-taiwan project archive](https://code.google.com/archive/p/gentoo-taiwan/issues/2) on Google Code still carries issues from that period.

## Overlay

gentoo-zh is a `masters = gentoo` overlay carrying 490+ packages. All packages use testing keywords only (`~amd64`, `~arm64`, etc.); no stable keywords are provided. Not all of it is Chinese-specific: the kernels, networking tools and version bumps apply to any Gentoo user. It roughly covers:

- **Input methods and fonts** — Fcitx engines for Chinese, Japanese, Korean, Thai, Vietnamese, Bengali and Sinhala, Rime and pinyin dictionaries, and CJK fonts.
- **Applications widely used in China** — WeChat, QQ, DingTalk, WPS Office, Feishu, NetEase Cloud Music.
- **Networking and proxy tools.**
- **Patched desktop and performance kernels** — `cachyos-sources`, `xanmod`, `liquorix`.
- **Development and everyday tools** not yet in the main tree, plus version bumps for packages temporarily unmaintained there.

### Adding the overlay

`root #``eselect repository enable gentoo-zh``root #``emaint sync`
Since October 2025, Gentoo no longer caches third-party repositories, so gentoo-zh syncs directly from GitHub. GitHub is slow from mainland China, so mirrors of the sync URI are listed at [gentoozh.org/overlay](https://gentoozh.org/overlay/). The repository is also mirrored to [Codeberg](https://codeberg.org/gentoo-zh/overlay) on every push.

### Distfiles

The community runs a distfiles mirror that caches everything the overlay fetches, so upstream tarballs stay reachable even when the original host disappears.

**`/etc/portage/make.conf`**

```
GENTOO_MIRRORS="${GENTOO_MIRRORS} https://distfiles.gentoozh.org"
```
University mirrors in mainland China also carry the distfiles; the current list, and the setup for both distfiles and binary packages, are at [distfiles.gentoozh.org](https://distfiles.gentoozh.org).

### Binary packages

Part of the overlay is rebuilt nightly and published as prebuilt binary packages, signed with the community key. This mainly helps with packages that take a long time to compile. Setup instructions and the key are at [distfiles.gentoozh.org](https://distfiles.gentoozh.org).

## Community Group / Channels

- Discussion Group / Channel

- Telegram: [@gentoo\_zh](https://telegram.me/gentoo_zh)
- Matrix: `#gentoo-zh:matrix.gentoozh.org`
- IRC ([Libera.Chat](https://libera.chat)): `#gentoo-zh`

- Simplified Chinese Info Channel

- Telegram: [@gentoocn](https://telegram.me/gentoocn)
- Matrix: `#gentoocn:matrix.gentoozh.org`

- Traditional Chinese Info Channel

- Telegram: [@gentootw](https://telegram.me/gentootw)
- Matrix: `#gentootw:matrix.gentoozh.org`

## Services

- [Website](https://gentoozh.org) — news, documentation and the mirror list.
- [Forum](https://forum.gentoozh.org) — Discourse, for questions and longer discussions.
- [distfiles.gentoozh.org](https://distfiles.gentoozh.org) — distfiles mirror and binary package host.
- [paste.gentoozh.org](https://paste.gentoozh.org) — pastebin for build logs, with the [gzpaste](https://github.com/gentoo-zh/gzpaste) command line client.
- `matrix.gentoozh.org` — Matrix homeserver for the channels listed above.
- Bug and security feeds — Gentoo bug and security news, pushed automatically. Chinese: [@GentoozhBug](https://telegram.me/GentoozhBug), `#gentoo-zh-bug:matrix.gentoozh.org`. English: [@GentooBug](https://telegram.me/GentooBug), `#gentoo-bug:matrix.gentoozh.org`.

## External links

- [Gentoo-zh on GitHub](https://github.com/gentoo-zh) — the overlay and the tooling around it.
- [Overlay mirror on Codeberg](https://codeberg.org/gentoo-zh/overlay)
- [Overlay bug tracker](https://github.com/gentoo-zh/overlay/issues) — for packages in the gentoo-zh overlay. Bugs in the main tree go to [Gentoo Bugzilla](https://bugs.gentoo.org).
- [overlay@gentoozh.org](mailto:overlay@gentoozh.org) — contact for the overlay maintainers.
