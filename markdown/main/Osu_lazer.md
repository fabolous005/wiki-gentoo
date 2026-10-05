<!-- source: https://wiki.gentoo.org/wiki/Osu!lazer | group: Gentoo Wiki (Main) | wiki-title: Osu!lazer -->
---
title: osu!lazer
url: https://wiki.gentoo.org/wiki/Osu!lazer
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-21"
fingerprint: a69996188b097117
license: CC BY-SA 4.0
---

# osu!lazer

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**osu!** is a free-to-win, cross-platform rhythm game. **osu!lazer** is the open-source client intended to eventually replace the legacy osu! client, which is only available for Windows and macOS.

## Installation

### License

Installing [games-arcade/osu-lazer](https://packages.gentoo.org/packages/games-arcade/osu-lazer) requires accepting the [all-rights-reserved](https://gitweb.gentoo.org/repo/gentoo.git/plain/licenses/all-rights-reserved) license.

### USE flags


### Emerge

`root #``emerge --ask games-arcade/osu-lazer`
### Alternative: AppImage release

lazer is also provided as a pre-built [AppImage](https://wiki.gentoo.org/wiki/AppImage).

[FUSE](https://wiki.gentoo.org/wiki/FUSE) is required to run the AppImage. See the instructions at [FUSE](https://wiki.gentoo.org/wiki/FUSE) to set up AppImage support if not already present.

#### Required old-format AppImage package

In order to be able to run the osu!lazer AppImage, the below emerge will be required:

`root #``emerge --ask sys-fs/fuse:0`
If a distrubution kernel is being used, then this step should be sufficient by itself for being able to run osu!. (Otherwise, FUSE support will simply also need to be enabled within the kernel.)

#### AppImage download

`user $``chmod +x osu.AppImage``user $``./osu.AppImage`
## Legacy client

The old osu! client (i.e., not lazer) is only natively supported on Windows and macOS. Users have reported success running it with [Wine](https://wiki.gentoo.org/wiki/Wine), however, so users who would prefer to stick to the old, and still more popular, client may attempt to run the Windows build available from the [osu! website](https://osu.ppy.sh).
