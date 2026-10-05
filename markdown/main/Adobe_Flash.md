<!-- source: https://wiki.gentoo.org/wiki/Adobe_Flash | group: Gentoo Wiki (Main) | wiki-title: Adobe Flash -->
---
title: Adobe Flash
url: https://wiki.gentoo.org/wiki/Adobe_Flash
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-05-23"
fingerprint: b257f4e469e9649b
license: CC BY-SA 4.0
---

# Adobe Flash

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **archived (obsolete)**. Contents are surely incorrect for current usage, and are intended for historical reference only.

TLDR:

**Do not use this article!**



## Alternatives

Alternatives to Adobe Flash may be available.

An alternative to using Adobe Flash Player when only playing video streams from YouTube, Vimeo, Twitch, Internet TV stream, etc., is to install [media-video/mpv](https://packages.gentoo.org/packages/media-video/mpv) with the `lua` USE flag, and [media-video/ffmpeg](https://packages.gentoo.org/packages/media-video/ffmpeg) or [media-video/libav](https://packages.gentoo.org/packages/media-video/libav) with the `openssl` USE flag. The `lua` USE flag allows [net-misc/youtube-dl](https://packages.gentoo.org/packages/net-misc/youtube-dl) to work in mpv, and the `openssl` USE flag allows FFmpeg/Libav to open https:// streams. Next, install the [Open-with Firefox add-on](https://addons.mozilla.org/en-US/firefox/addon/open-with/) and configure it in the following way:

- Open about:openwith, select Add...
- In the dialog select a video streaming capable player (e.g. /usr/bin/mpv).
- (Optional step) Choose how to display the dialogs using the left panel of Open-with add-on.
- Right click on links or visit pages containing videos. If the site is supported, the player will be open as expected.

The same procedure can be used to associate other video downloaders such as [net-misc/youtube-dl](https://packages.gentoo.org/packages/net-misc/youtube-dl).
