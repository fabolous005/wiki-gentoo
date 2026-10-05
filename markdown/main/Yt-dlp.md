<!-- source: https://wiki.gentoo.org/wiki/Yt-dlp | group: Gentoo Wiki (Main) | wiki-title: Yt-dlp -->
---
title: yt-dlp
url: https://wiki.gentoo.org/wiki/Yt-dlp
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-10-03"
fingerprint: "165144598cbdf8f9"
license: CC BY-SA 4.0
---

# yt-dlp

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


yt-dlp is a youtube-dl fork based on the now inactive youtube-dlc. The main focus of this project is adding new features and patches while also keeping up to date with the original project.

## Installation

### USE flags


| [+deno](https://packages.gentoo.org/useflags/+deno) | Pull in dev-lang/deno-bin by default needed for proper YouTube support (if this USE is masked on your profile, refer to yt-dlp's documentation for lesser supported alternatives which are not supported out-of-the-box due to security concerns) | 
| [man](https://packages.gentoo.org/useflags/man) | Build and install man pages | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

`root #``emerge --ask net-misc/yt-dlp`
## Usage

### Downloading a video

The easiest way to download a video from YouTube with yt-dlp is by passing it as the only argument:

### Output format

To output the video in a different format, use the `-f,--format` flags:

### Extracting audio

To download just the audio of a video, use `-x,--extract-audio`:

Or, if a format other than `.opus` is required, use `--audio-format`

In order for the encoding to mp3 to work, the package media-video/ffmpeg must be installed with the `lame` USE flag (which is no longer the default). A check with ffmpeg -version should show `--enable-libmp3lame`.

### Adding metadata

To add chapters and infojson to a downloaded video, use `--embed-metadata`:

### Subtitles

To list available subtiles for a video, use `--list-subs`:

If a video provides subtitles then the ones provided by the channel will be listed below the auto-generated ones by YouTube.

Downloading a video with subtitles can then be done using `--write-subs/--write-auto-subs` if the manual or auto-generated subtitles wish to be used, respectively.

## Configuration

yt-dlp's system-wide configuration files are located at: /etc/yt-dlp.conf, /etc/yt-dlp/config, /etc/yt-dlp/config.txt and the recommended path for user configuration is ${XDG\_CONFIG\_HOME}/yt-dlp/config

### Default arguments

Adding default arguments to yt-dlp can be done by using the configuration file, for example, in ${XDG\_CONFIG\_HOME}/yt-dlp/config

**`${XDG_CONFIG_HOME}/yt-dlp/config`**

### Encoding

The default encoding yt-dlp uses for configuration files is UTC BOM, if that is not present then the system locale.

To use a encoding format other than these, add the comment `# coding: ENCODING` to the top of the configuration file.
