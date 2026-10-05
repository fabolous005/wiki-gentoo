<!-- source: https://wiki.gentoo.org/wiki/TeamViewer | group: Gentoo Wiki (Main) | wiki-title: TeamViewer -->
---
title: TeamViewer
url: https://wiki.gentoo.org/wiki/TeamViewer
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-24"
fingerprint: "283bbf4f1d3b0987"
license: CC BY-SA 4.0
---

# TeamViewer

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**TeamViewer** is a proprietary (closed source), all-in-one solution for remote access and support over the Internet. It has both a client and a server mode and requires either a built-in or system [Wine](https://wiki.gentoo.org/wiki/Wine).

## Installation

Be aware there are a few versions of TeamViewer available in the main Gentoo repository.

### USE flags


### Emerge

Be aware it is necessary to accept TeamViewer's End User License Agreement (EULA) before the software can be installed.

`root #``emerge --ask net-misc/teamviewer`
It is recommended that OpenRC users using the desktop or plasma profiles enable `elogind` in make.conf:

**`/etc/portage/make.conf`**

```
USE="${USE} elogind"
```
Then rebuild the system for the USE change:

`root #``emerge -avDU @world`
## Configuration

### Files

Supposing TeamViewer 15 has been installed, the configuration files would be found in the following locations:

- /etc/teamviewer15/global.conf - Global (system wide) configuration file.
- \~/.config/teamviewer/ - Local (per user) configuration file.

### Service

The TeamViewer daemon must be started before using the TeamViewer front-end. Those who want TeamViewer to be running every time they boot their system will need to add it the list of services that run during system start.

#### OpenRC

Start TeamViewer during system start:

`root #``rc-update add teamviewerd default`
Start the TeamViewer daemon now:

`root #``rc-service teamviewerd start`
#### systemd

Start TeamViewer during system start:

`root #``systemctl enable teamviewerd`
Start the TeamViewer daemon now:

`root #``systemctl start teamviewerd`
## Usage

### Invocation

After the service has been started, simply start TeamViewer from the Applications menu in a desktop environment. Those using a command line can start it via:

`user $``teamviewer`
## Removal

### Files

In order to scrub all traces of TeamViewer from the system each user's \~/.config/teamviewer folder should be removed:

`user $``rm -rf ~/.config/teamviewer*`
### Unmerge

`root #``emerge --ask --depclean --verbose net-misc/teamviewer`
### Crash with Wayland

If the client crashes on Wayland. Try

`user $``QT_QPA_PLATFORM="xcb" teamviewer`
## See also

- [TigerVNC](https://wiki.gentoo.org/wiki/TigerVNC) — a client/server software package allowing remote network access to graphical desktops.
- [AnyDesk](https://wiki.gentoo.org/wiki/AnyDesk) — a proprietary (closed source), software for remote access and support over the Internet similar to [TeamViewer]
