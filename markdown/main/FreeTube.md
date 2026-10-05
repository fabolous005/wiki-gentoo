<!-- source: https://wiki.gentoo.org/wiki/FreeTube | group: Gentoo Wiki (Main) | wiki-title: FreeTube -->
---
title: FreeTube
url: https://wiki.gentoo.org/wiki/FreeTube
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-01-14"
fingerprint: b380138f98a19820
license: CC BY-SA 4.0
---

# FreeTube

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

![2022-02-25-120300 1917x1079 scrot.png](https://wiki.gentoo.org/images/thumb/8/86/2022-02-25-120300_1917x1079_scrot.png/300px-2022-02-25-120300_1917x1079_scrot.png)

FreeTube is a Free and Open Source client for [YouTube](https://en.wikipedia.org/wiki/YouTube) built with privacy in mind. Its goal is to make a front end that would interface with YouTube without its users needing to run proprietary JavaScript code. It can connect both directly to YouTube's servers or route the connection through [invidious](https://github.com/iv-org/invidious). FreeTube supports both x86\_64 and arm64, and it is currently still in beta.

## Installation

### Installing with eselect-repository

FreeTube is not available in the official gentoo repository, but it is available in the GURU repository and other repositories as well.

There are both source and binary versions of FreeTube available. This guide will give instructions for both.

#### Installing the binary version

To install from eselect repository [app-eselect/eselect-repository](https://packages.gentoo.org/packages/app-eselect/eselect-repository) and [dev-vcs/git](https://packages.gentoo.org/packages/dev-vcs/git)

`root #``emerge --ask dev-vcs/git app-eselect/eselect-repository`
Add the GURU repository:

`root #``eselect repository enable guru`
Then sync the repositories:

`root #``emaint sync -r guru`
freetube-bin is likely to be masked with the \~amd64 keyword. To unmask it:

**`/etc/portage/package.keywords/package.keywords`**

```
 ~amd64
```
And Finally, install `freetube-bin`:

`root #``emerge --ask net-misc/freetube-bin`
#### Building FreeTube from source code

FreeTube was moved from media-sound to media-video category

Source version of FreeTube can be installed from the zGentoo repository.

To install from eselect repository [app-eselect/eselect-repository](https://packages.gentoo.org/packages/app-eselect/eselect-repository) and [dev-vcs/git](https://packages.gentoo.org/packages/dev-vcs/git)

`root #``emerge --ask dev-vcs/git app-eselect/eselect-repository`
Add the zGentoo repository:

`root #``eselect repository enable zGentoo`
Then sync the repositories:

`root #``emaint sync -r zGentoo`
FreeTube is likely to be masked with the \~amd64 keyword. To unmask it:

**`/etc/portage/package.keywords/package.keywords`**

```
 ~amd64
```
And finally, compile and install FreeTube:

`root #``emerge --ask media-video/freetube`
### Installing with a custom ebuild repository

Besides installing FreeTube through overlays, there is also an option to install it with a repository created on your system locally. This guide will assume that you have your custom repository already set up and that it is located in `/usr/local/portage`. See  [creating an ebuild repository](https://wiki.gentoo.org/wiki/Creating_an_ebuild_repository) for more info.

#### Installing the binary version

Download the ebuild (Example source: \[[gpo.zugaina.org](https://gpo.zugaina.org/net-misc/freetube-bin)\])

`root #``cd /usr/local/portage/``root #``mkdir -p net-misc/freetube-bin && cd net-misc/freetube-bin``root #``cp $HOME/Downloads/freetube-bin-0.16.0.ebuild .``root #``pkgdev manifest`
freetube-bin is likely to be masked with the \~amd64 keyword. To unmask it:

**`/etc/portage/package.keywords/package.keywords`**

```
 ~amd64
```
emerge:

`root #``emerge --ask net-misc/freetube-bin`
#### Installing the source version

Download the ebuild (Example source: [gpo.zugaina.org](https://gpo.zugaina.org/media-sound/freetube))

`root #``cd /usr/local/portage/``root #``mkdir -p media-sound/freetube && cd media-sound/freetube``root #``cp $HOME/Downloads/freetube-0.16.0.ebuild .``root #``pkgdev manifest`
FreeTube is likely to be masked with the \~amd64 keyword. To unmask it:

**`/etc/portage/package.keywords/package.keywords`**

```
 ~amd64
```
emerge:

`root #``emerge --ask media-sound/freetube`
### Flatpak Installation

FreeTube also has a Flatpak version.

Install Flatpak:

`root #``emerge --ask sys-apps/flatpak`
Download the official FreeTube flatpakref:

Install:

`root #``flatpak install flathub io.freetubeapp.FreeTube`
Run through the command line:

`user $``flatpak run io.freetubeapp.FreeTube`
See [Flatpak](https://wiki.gentoo.org/wiki/Flatpak) for more information.

### AppImage Installation

Most appimages, including FreeTube, requires [sys-fs/fuse](https://packages.gentoo.org/packages/sys-fs/fuse) for them to work.

`root #``emerge --ask sys-fs/fuse`
Download the `.appimage` file:

Make the `.appimage` file executable:

`user $``chmod +x FreeTube_0.16.0_amd64.AppImage`
Run FreeTube:

`user $``./FreeTube_0.16.0_amd64.AppImage`
See [Appimage](https://wiki.gentoo.org/wiki/Appimage) for more information.

## Removal

#### Portage Version

To remove the binary package:

`root #``emerge --unmerge net-misc/freetube-bin`
To remove the source package:

`root #``emerge --unmerge media-sound/freetube`
#### Flatpak Version

`root #``flatpak remove io.freetubeapp.FreeTube`
## Data Location

### User Directories

User directories for FreeTube. For more information on customizing check out the **External resources**.

#### Linux Filesystem

- `$HOME/.config/FreeTube`

#### Flatpak Version

- `$HOME/.var/app/io.freetubeapp.FreeTube/config/FreeTube/`

### Notable Files

#### Linux Filesystem

- $HOME/.config/FreeTube/profiles.db

- $HOME/.config/FreeTube/settings.db

- $HOME/.config/FreeTube/playlists.db

- $HOME/.config/FreeTube/history.db

#### Flatpak Version

- $HOME/.var/app/io.freetubeapp.FreeTube/config/FreeTube/profiles.db

- $HOME/.var/app/io.freetubeapp.FreeTube/config/FreeTube/settings.db

- $HOME/.var/app/io.freetubeapp.FreeTube/config/FreeTube/playlists.db

- $HOME/.var/app/io.freetubeapp.FreeTube/config/FreeTube/history.db

## External resources

- [https://docs.freetubeapp.io/](https://docs.freetubeapp.io/) - Official FreeTube docummentation.

- [https://docs.appimage.org/](https://docs.appimage.org/) - AppImage docummentation
