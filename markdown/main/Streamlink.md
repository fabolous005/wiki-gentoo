<!-- source: https://wiki.gentoo.org/wiki/Streamlink | group: Gentoo Wiki (Main) | wiki-title: Streamlink -->
---
title: Streamlink
url: https://wiki.gentoo.org/wiki/Streamlink
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2020-07-03"
fingerprint: b700f2888659b99b
license: CC BY-SA 4.0
---

# Streamlink

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Streamlink** is a command-line utility that can extract video streams and pipe them into a video player. The main purpose of streamlink, according to the official documentation, is to "allow the user to avoid buggy and CPU heavy flash plugins but still be able to enjoy various streamed content." Streamlink was forked from [livestreamer](https://github.com/chrippa/livestreamer), is written in Python, and works with various media players such as mpv, mplayer, and vlc.

## Installation

`root #``emerge --ask net-misc/streamlink`
## Usage

Streamlink is a fairly simple program run from the command line with various options:

`user $``streamlink --player <video-player> <stream-url> <stream-quality>`
- `<video-player>` is the desired video player. The default player is **vlc** but **mplayer** and **mpv** are also available.

- `<stream-url>` is the location of the stream. While the full list of supported plugins is extensive, some popular supported streams are **Dailymotion**, **Livestream**, **Twitch**, **UStream**, and **YouTube**. For the full list, see the list of supported [plugins](https://streamlink.github.io/plugin_matrix.html#plugin-matrix).

- `<stream-quality>` is the available quality of the stream. The options allow either a specific quality, such as *720p*, or a more vague quality such as *high* or *low*. Further, streamlink allows *best* or *worst* as alternatives.

For example:

`user $``streamlink --player mpv twitch.tv/`*name-of-channel* best
Or, alternatively:

`user $``streamlink --player mpv twitch.tv/`*name-of-channel* 720p
## Configuration

A configuration file can be used with streamlink to avoid typing all the options for every extracted stream. Streamlink will look for the configuration file in either $XDG\_CONFIG\_HOME/streamlink/config or \~/.streamlinkrc. The file is a text file with one command-line option per line:

`option=value`

The example listed on the official documentation is:

**`~/.streamlinkrc`**

```
# Player options
player=mpv --cache=2048
player-no-close
# Authentication with Twitch
twitch-oauth-token=mytoken
```
### Authentication with Twitch

Streamlink allows the creation of an auth token that can be used to access a specific Twitch account. The following command can be run to obtain a token:

`user $``streamlink --twitch-oauth-authenticate`
The command will open a web browser pointed at Twitch. Simply login and streamlink will request access to the account and provide instructions on how to use the token. It is a good idea to save the token in the configuration file as demonstrated above. If the token is not saved in the configuration file, it will have to be manually added at the start of each twitch stream as streamlink will not save it.

### Authentication with Crunchyroll

In order to enjoy premium content on Crunchyroll, authentication with a premium account is required. Unlike Twitch, there is no way to obtain a token. Instead, logging in is done with:

`user $``streamlink --crunchyroll-username=`*user-name* --crunchyroll-password=*password* http://crunchyroll.com/a-crunchyroll-episode-link
After logging in, streamlink will save the name and password for that session. However, sessions do expire so it is a good idea to save the user name and password in the configuration file as demonstrated above.
