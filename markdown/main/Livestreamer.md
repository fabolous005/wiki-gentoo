<!-- source: https://wiki.gentoo.org/wiki/Livestreamer | group: Gentoo Wiki (Main) | wiki-title: Livestreamer -->
---
title: Livestreamer
url: https://wiki.gentoo.org/wiki/Livestreamer
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-10-16"
fingerprint: f346f1df9161bf9e
license: CC BY-SA 4.0
---

# Livestreamer

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Livestreamer** is a CLI tool that pipes video streams from services like twitch.tv into a video player. A full list of the video players supported can be found [here](http://docs.livestreamer.io/players.html#player-compatibility). Dailymotion, Livestream, Twitch, Ustream, and YouTube Live are all supported without add-ons.

## Installation

## Overlay

For now. Livestreamer is accessible in the Pamler overlay:

`root #``eselect repository enable palmer``root #``emerge --sync palmer`
### USE flags

USE flags will be present once the package is available in the main ebuild repository.

### Emerge

Once the overlay is installed:

`root #``emerge --ask net-misc/livestreamer`
## Usage

Using livestreamer to watch a favorite stream is fairly simple. Use the following command and adjust the options as needed:

`user $``livestreamer -p <player> <URL> <stream-quality>`
Where `<player>` is the selected player. The default player is vlc, but other players such as mplayer or mpv can be used as well. Livestreamer should be able to locate the player by the name, but the full path to the player can be used as well.

`<URL>` is the location of the stream to watch. In all cases, `http://` can be omitted, and in some cases `www` can be omitted as well.

`<stream-quality>` is a text value that defines the stream's quality. For some streams it can be from `best` to `worst` and for others it is a different scale. For Twitch, values include: `audio`, `mobile`, `low`, `medium`, `high`, and `source`.
