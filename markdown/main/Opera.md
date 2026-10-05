<!-- source: https://wiki.gentoo.org/wiki/Opera | group: Gentoo Wiki (Main) | wiki-title: Opera -->
---
title: Opera
url: https://wiki.gentoo.org/wiki/Opera
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-05"
fingerprint: "4440b7528f0b316f"
license: CC BY-SA 4.0
---

# Opera

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Opera** is a multi-platform web browser.

## Installation

### USE flags


| [+ffmpeg-chromium](https://packages.gentoo.org/useflags/+ffmpeg-chromium) | Use Chromium FFmpeg fork (media-video/ffmpeg-chromium) rather than mainline FFmpeg (media-video/ffmpeg) | 
| [+proprietary-codecs](https://packages.gentoo.org/useflags/+proprietary-codecs) | Enable codecs for patent-encumbered audio and video formats. | 
| [+suid](https://packages.gentoo.org/useflags/+suid) | Enable setuid root program(s) | 
| [qt6](https://packages.gentoo.org/useflags/qt6) | Add support for the Qt 6 application and UI framework | 

### Accept License

In order to install Opera, the user needs to accept the 'OPERA-2018' license agreement. A copy of the license can be found at '/var/db/repos/gentoo/licenses/OPERA-2018'. Read with:

`user $``less /var/db/repos/gentoo/licenses/OPERA-2018`
And to agree:

`root #``echo "www-client/opera OPERA-2018" >> /etc/portage/package.license`
### Emerge

Opera is distributed as only pre-built binaries. To install:

`root #``emerge --ask www-client/opera`
### Using Opera as a media player

You can play media files in opera as a basic media player:

`user $``opera ~/Videos/example.mp4`
## See also

- [Vivaldi](https://wiki.gentoo.org/wiki/Vivaldi) — a browser for our friends.
- [Chrome](https://wiki.gentoo.org/wiki/Chrome) — Google's proprietary (closed source) web browser.
- [Chromium](https://wiki.gentoo.org/wiki/Chromium) — the open source browser that [Google Chrome](https://wiki.gentoo.org/wiki/Google_Chrome) and many other browsers are based on.
- [Firefox](https://wiki.gentoo.org/wiki/Firefox) — [open source](https://en.wikipedia.org/wiki/Open_source), [multiplatform](https://en.wikipedia.org/wiki/Cross-platform_software), [web browser](https://wiki.gentoo.org/wiki/Recommended_applications#Web_browsers) developed by [Mozilla](https://en.wikipedia.org/wiki/Mozilla).
- [Recommended applications](https://wiki.gentoo.org/wiki/Recommended_applications) — applications recommended for use in a graphical environment ([X11](https://wiki.gentoo.org/wiki/Xorg), [Wayland](https://wiki.gentoo.org/wiki/Wayland)) - [web browser section](https://wiki.gentoo.org/wiki/Recommended_applications#Web_browsers).
