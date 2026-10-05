<!-- source: https://wiki.gentoo.org/wiki/Telegram | group: Gentoo Wiki (Main) | wiki-title: Telegram -->
---
title: Telegram
url: https://wiki.gentoo.org/wiki/Telegram
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-07"
fingerprint: c295545cd88f19c7
license: CC BY-SA 4.0
---

# Telegram

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Telegram** is a freeware, cross-platform, cloud-based instant messaging (IM) system. It is written in C++.

## Installation

### USE flags


| [+X](https://packages.gentoo.org/useflags/+X) | Add support for X11 | 
| [+fonts](https://packages.gentoo.org/useflags/+fonts) | Use builtin patched copy of open-sans fonts (overrides fontconfig) | 
| [dbus](https://packages.gentoo.org/useflags/dbus) | Enable dbus support for anything that needs it (gpsd, gnomemeeting, etc) | 
| [enchant](https://packages.gentoo.org/useflags/enchant) | Use the app-text/enchant spell-checking backend instead of app-text/hunspell | 
| [screencast](https://packages.gentoo.org/useflags/screencast) | Enable support for remote desktop and screen cast using PipeWire | 
| [wayland](https://packages.gentoo.org/useflags/wayland) | Enable dev-libs/wayland backend | 
| [webkit](https://packages.gentoo.org/useflags/webkit) | Add support for the WebKit HTML rendering/layout engine | 

### Emerge

Emerge Telegram:

`root #``emerge --ask net-im/telegram-desktop`
Alternatively, install the pre-built binary version:

`root #``emerge --ask net-im/telegram-desktop-bin`
## Removal

### Unmerge

Delete telegram-desktop with emerge:

`root #``emerge --ask --depclean --verbose net-im/telegram-desktop`
Delete telegram-desktop-bin with emerge:

`root #``emerge --ask --depclean --verbose net-im/telegram-desktop-bin`
## See Also

- [Discord](https://wiki.gentoo.org/wiki/Discord) — a proprietary VoIP instant messaging and digital distribution platform for voice, video, and text communication.
- [Recommended applications](https://wiki.gentoo.org/wiki/Recommended_applications) — applications recommended for use in a graphical environment ([X11](https://wiki.gentoo.org/wiki/Xorg), [Wayland](https://wiki.gentoo.org/wiki/Wayland))
