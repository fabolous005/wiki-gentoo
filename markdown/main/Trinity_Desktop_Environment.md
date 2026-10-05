<!-- source: https://wiki.gentoo.org/wiki/Trinity_Desktop_Environment | group: Gentoo Wiki (Main) | wiki-title: Trinity Desktop Environment -->
---
title: Trinity Desktop Environment
url: https://wiki.gentoo.org/wiki/Trinity_Desktop_Environment
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-21"
fingerprint: b0a7467a79a7f156
license: CC BY-SA 4.0
---

# Trinity Desktop Environment

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The **Trinity Desktop Environment** (**TDE**) is a free and open-source desktop environment forked from [KDE](https://wiki.gentoo.org/wiki/KDE) 3.5. Due to the fact it is forked from an old version of KDE (which is itself a resource-heavy desktop environment) it is considered light-medium weight by today's standards.



## Installation

### Installing TDE

#### Profiles

It is highly recommended to run a desktop profile when running a [Desktop Environment](https://wiki.gentoo.org/wiki/Desktop_environment), using **amd64** as an example this can be done by running:

`root #``eselect profile set default/linux/amd64/23.0/desktop``root #``emerge -vauDU @world`
#### Enabling the Overlay

TDE is packaged in the trinity-official overlay. To enable this overlay:

To add this overlay manually:

**`/etc/portage/repos.conf/tde.conf`**

Sync the overlay with emaint:

`root #``emaint sync -r trinity-official`
The packages on the overlay are keyworded under the unstable \~amd64 keyword, so users on a stable system should unmask the packages on the repository.

**`/etc/portage/package.accept_keywords/trinity-official`**

#### Emerge

To install the Trinity Desktop Environment meta package run the following command:

`root #``emerge --ask trinity-base/tdebase-meta`
### Display Manager

#### TDM

Trinity Desktop Environment has a matching display manager called TDM.

#### Emerge

To install the Trinity Display Manager package run the following command:

`root #``emerge --ask trinity-base/tdm`
Change your Display manager to "tdm." An example for **display-manager-init** users:

**`/etc/conf.d/display-manager`**

#### Boot Service

To start TDM on boot, add display-manager to the default runlevel:

`root #``rc-update add display-manager default`
To start TDM now:

`root #``rc-service display-manager start`
#### systemd

To start TDM on boot, run the following:

`root #``systemctl enable tdm.service`
And start the service:

`root #``systemctl start tdm.service`
#### LightDM

Users not wishing to use TDM may instead install LightDM.

To install LightDM, run the following command:

`root #``emerge --ask x11-misc/lightdm`
Enable the LightDM service.

`root #``systemctl enable lightdm`
To start LightDM now:

`root #``systemctl start lightdm`
### Pipewire

Trinity Desktop Environment does not pull in a sound server, so we will have to enable one ourselves.

#### elogind

First, ensure elogind is enabled:

`root #``rc-update add elogind boot`
#### Emerge

Enable the pulseaudio and pipewire USE flags globally:

**`/etc/portage/make.conf`**

Then, emerge PipeWire:

`root #``emerge --ask media-video/pipewire`
#### gentoo-pipewire-launcher

Add gentoo-pipewire-launcher to your xprofile:

**`~/.xprofile`**

#### pavucontrol

Trinity's default sound server application is unable to use pipewire, so you will want something like pavucontrol:

`root #``emerge --ask media-sound/pavucontrol`
Reboot, and you should have sound.
