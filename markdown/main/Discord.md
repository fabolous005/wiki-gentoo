<!-- source: https://wiki.gentoo.org/wiki/Discord | group: Gentoo Wiki (Main) | wiki-title: Discord -->
---
title: Discord
url: https://wiki.gentoo.org/wiki/Discord
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-01"
fingerprint: d6371c090e23b9d4
license: CC BY-SA 4.0
---

# Discord

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Discord** is a proprietary VoIP instant messaging and digital distribution platform for voice, video, and text communication.

Discord is written in JavaScript (with React), [Elixir](https://wiki.gentoo.org/wiki/Elixir), [Python](https://wiki.gentoo.org/wiki/Python), [Rust](https://wiki.gentoo.org/wiki/Rust) and [C++](https://wiki.gentoo.org/wiki/C%2B%2B).

## Installation

### USE flags


### USE flags for
            [net-im/discord](https://packages.gentoo.org/packages/net-im/discord)
            
            All-in-one voice and text chat for gamers

| [+seccomp](https://packages.gentoo.org/useflags/+seccomp) | Enable seccomp (secure computing mode) to perform system call filtering at runtime to increase security of programs | 
| [appindicator](https://packages.gentoo.org/useflags/appindicator) | Build in support for notifications using the libindicate or libappindicator plugin | 
| [wayland](https://packages.gentoo.org/useflags/wayland) | Enable dev-libs/wayland backend | 

### Emerge

Discord has a package in the official Gentoo repository - this is the recommended way to install Discord.

Emerge Discord:

`root #``emerge --ask net-im/discord`
### Alternative installation possibilities

For users that may have reason to prefer other methods of installing Discord on Gentoo, these alternative options are available.

#### Flatpak

Discord is available as a [Flatpak](https://wiki.gentoo.org/wiki/Flatpak) application that can be automatically downloaded and installed from Flathub.

Once Flatpak is available, install Discord from Flathub:

`user $``flatpak install flathub com.discordapp.Discord`
After successful installation, Discord may be launched from the command line:

`user $``flatpak run com.discordapp.Discord`
#### Snap

First, install [Snap](https://wiki.gentoo.org/wiki/Snap), paying attention to the recommendations from that article.

Once Snap is available, install Discord:

`root #``snap connect discord:system-observe`
## Troubleshooting

### Discord shows the GTK file picker while using KDE or other QT environment

In order to display the correct file picker, Discord reads from the `GTK_USE_PORTAL` environment variable. To use the right file picker from KDE/QT, launch Discord with the following command or edit the shortcut:

### Discord doesn't start upon a launcher update

![Screenshot from 2022-03-01 13-08-39.png](https://wiki.gentoo.org/images/4/49/Screenshot_from_2022-03-01_13-08-39.png)

On GNU/Linux systems, Discord expects the launcher to be always up to date. When a launcher update is available, Discord prompts the user to download the latest `.deb` package from the official website, this of course works only on Debian-based distributions.

#### Updating the package via portage

In Gentoo, solve this by syncing the repositories and updating the [net-im/discord](https://packages.gentoo.org/packages/net-im/discord) package.

`root #``emerge --sync``root #``emerge --ask net-im/discord`
There are reasons why the user might not want to use this method. The most common reason is that the package is not yet updated in the Gentoo repository. In that case using [Flatpak](https://wiki.gentoo.org/wiki/Discord#Flatpak) or [Snap](https://wiki.gentoo.org/wiki/Discord#Snap) version is advised.

Should the Snap or Flatpak version not be desired by the user, a manual update can be performed. This can be done, by downloading the tar.gz file that Discord offers and moving it to `/opt/discord` manually. It should be noted, that this operation may not always work, since it does not update system level dependencies.

`root #``tar -xf discord-*.tar.gz -C /opt/discord --strip-components=1`
#### Alternative: disabling the update check

To disable the update check during startup put `"SKIP_HOST_UPDATE": true` into \~/.config/discord/settings.json.

### Enabling Discord Rich Presence on Flatpak

When using the [Flatpak](https://wiki.gentoo.org/wiki/Discord#Flatpak) version of Discord, Rich Presence will not work out of the box. To make it work, a symlink must be created for the current user session. Run:

`user $``ln -sf $XDG_RUNTIME_DIR/{app/com.discordapp.Discord,}/discord-ipc-0`
### Discord doesn't show emojis or other glyphs correctly

In order to display some characters correctly, [media-fonts/noto-emoji](https://packages.gentoo.org/packages/media-fonts/noto-emoji) and [media-fonts/noto-cjk](https://packages.gentoo.org/packages/media-fonts/noto-cjk) can be merged like this:

`root #``emerge --ask media-fonts/noto-emoji media-fonts/noto-cjk`
Should the fonts still fail to display, the user may reload the font information cache by running:

`user $``fc-cache -fv`
### Discord icon in Plasma systray is blurry

If using [Plasma](https://wiki.gentoo.org/wiki/Plasma), [dev-libs/libappindicator](https://packages.gentoo.org/packages/dev-libs/libappindicator) may be merged, to have a nice icon in the systray instead of a blurry one:

`root #``emerge --ask dev-libs/libappindicator`
### Discord screensharing issue with Wayland: zkde\_screencast\_unstable\_v1 does not seem to be available

If using [Wayland](https://wiki.gentoo.org/wiki/Wayland), in order to be able to screenshare, enable [screencast](https://packages.gentoo.org/useflags/screencast) [USE flag for](https://wiki.gentoo.org/wiki/USE_flag) [kde-plasma/kwin](https://packages.gentoo.org/packages/kde-plasma/kwin).

### Discord web on Firefox: unable to connect to voice chat

Discord uses DAVE protocol (Discord Audio & Video End-to-End Encryption) for voice and video chats. This protocol requires Firefox version 142.0 or higher<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.

## See Also

- [Telegram](https://wiki.gentoo.org/wiki/Telegram) — a freeware, cross-platform, cloud-based instant messaging (IM) system.
- [Recommended applications](https://wiki.gentoo.org/wiki/Recommended_applications) — applications recommended for use in a graphical environment ([X11](https://wiki.gentoo.org/wiki/Xorg), [Wayland](https://wiki.gentoo.org/wiki/Wayland))
