<!-- source: https://wiki.gentoo.org/wiki/DDE | group: Gentoo Wiki (Main) | wiki-title: DDE -->
---
title: DDE
url: https://wiki.gentoo.org/wiki/DDE
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-21"
fingerprint: f211705b39a4a9fe
license: CC BY-SA 4.0
---

# DDE

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



[checking over the content](https://wiki.gentoo.org/index.php?title=DDE&action=edit)(

[how to get started](https://wiki.gentoo.org/wiki/Gentoo_Wiki:Contributor%27s_guide)).

**Deepin Desktop Environment** is an elegant, easy to use and reliable domestic desktop environment released by Deepin Technology Co., Ltd. It is primarily written in Golang and Qt5. It was originally written for Deepin Linux, and is ported to Gentoo.

## Prerequisites

### Add deepin overlay

DDE currently resides in the [deepin](https://github.com/zhtengw/deepin-overlay) ebuild repository. There are three primary methods for installing repositories in Gentoo:

#### Option 1: Install by eselect repository

First emerge [app-eselect/eselect-repository](https://packages.gentoo.org/packages/app-eselect/eselect-repository) and [dev-vcs/git](https://packages.gentoo.org/packages/dev-vcs/git):

`root #``emerge --ask --noreplace app-eselect/eselect-repository dev-vcs/git`
Then add the overlay and sync:

`root #````
eselect repository add deepin git https://github.com/zhtengw/deepin-overlay.git
```
`root #````
emerge --sync deepin
```
#### Option 2: Manually add repos.conf

Create a file in the [/etc/portage/repos.conf/](https://wiki.gentoo.org/wiki//etc/portage/repos.conf) directory (create the repos.conf directory first if it does not exist) called deepin.conf. Fill the file's contents with the following code:

**`/etc/portage/repos.conf/deepin.conf`**

```
[deepin]
location = /usr/local/overlay/deepin
sync-type = git
sync-uri = https://github.com/zhtengw/deepin-overlay.git
auto-sync = yes
```
### KEYWORDS

For now, only the **\~amd64** and **\~x86** keywords are supported. So enabling testing global keyword will make it 'quick and easy' for installing DDE.

**`/etc/portage/make.conf`**

```
ACCEPT_KEYWORDS="~amd64"
```
### Profile and USE flags

#### OpenRC

Using the basic desktop profile:

`root #``eselect profile set default/linux/amd64/23.0/desktop`
Add `elogind` USE flag in [/etc/portage/make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf), it's also recommended to disable support for other session trackers to avoid conflicts:

**`/etc/portage/make.conf`**

```
USE="elogind -systemd"
```
#### systemd

Using the systemd profile:

`root #``eselect profile set default/linux/amd64/23.0/systemd`
Ensure that `X` USE flag is included in the system's global [USE flags](https://wiki.gentoo.org/wiki/USE_flag).

### Updating system

After setting, ensure everything is up-to-date and remerge @world to make the changes take effect:

`root #````
emerge --sync
```
`root #````
emerge --deep --with-bdeps=y --changed-use --update --ask --verbose @world
```
## Emerging Deepin Desktop Environment

Set proper USE flags for dde-base/dde-meta, as describe below:

| USE flag | Default | Description | 
|---|---|---|
| `elogind` |  | Use [sys-auth/elogind](https://packages.gentoo.org/packages/sys-auth/elogind) to make DDE runs under [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) | 
| `extra` |  | Install extra applicaions developed by Deepin | 
| `grub` |  | Install Deepin themes for [sys-boot/grub](https://packages.gentoo.org/packages/sys-boot/grub) | 
| `kwin` |  | Use the WM based on kwin | 
| `manual` |  | Install dde-extra/deepin-manual User Manual | 
| `multimedia` |  | Install Deepin multimedia suite | 
| `mutter` |  | Use the WM based on mutter | 
| `plymouth` |  | Install Deepin themes for [sys-boot/plymouth](https://packages.gentoo.org/packages/sys-boot/plymouth) | 
| `policykit` |  | Enable PolicyKit authentication support | 
| `screensaver` |  | Install Deepin Screensaver module | 
| `systemd` |  | Run with [Systemd](https://wiki.gentoo.org/wiki/Systemd) | 
| `terminal` |  | Install dde-extra/deepin-terminal Terminal Emulator | 
| `turbo` |  | Enable DTK Apps Turbo by dde-extra/deepin-turbo | 

For example, to add deepin multimedia suit to the default set of installed applications, do:

`root #````
mkdir -pv /etc/portage/package.use
```
`root #````
echo "dde-base/dde-meta multimedia" >> /etc/portage/package.use/deepin
```
Then emerge DDE:

`root #``emerge --ask --verbose --keep-going dde-base/dde-meta`
## Configuring and running DDE

Assuming that you have setup [X11](https://wiki.gentoo.org/wiki/X11) properly, now going to configure DDE.

### Configuring lightdm greeter

There are two greeter we can choose -- lightdm-gtk-greeter and lightdm-deepin-greeter. But lightdm-deepin-greeter cannot run without any normal user, so we use lightdm-gtk-greeter by default.

**`/etc/lightdm/lightdm.conf`**

```
[Seat:*]
greeter-session=lightdm-gtk-greeter
```
#### Optional: adding a normal user

If there isn't any normal user able to login in to your system, lightdm-deepin-greeter won’t work. Then adding one and setting password. For instance, to create a user called aten who is member of the wheel, users, and audio groups:

`root #````
useradd -m -G users,wheel,audio -s /bin/bash aten
```
`root #````
passwd aten
```
Password: (Enter the password for aten)
Re-enter password: (Re-enter the password to verify)

### Setting default services and running DDE

#### OpenRC

##### With display-manager

The configuration file should be modified to use LightDM:

**`/etc/conf.d/display-manager`**

**Set LightDM as the display manager**

```
CHECKVT=7
DISPLAYMANAGER="lightdm"
```
Set dbus, display-manager, [NetworkManager](https://wiki.gentoo.org/wiki/NetworkManager), and [elogind](https://wiki.gentoo.org/wiki/Elogind) to come up on boot, and disable [dhcpcd](https://wiki.gentoo.org/wiki/Dhcpcd) if enabled.

`root #````
rc-update add dbus default 
```
`root #````
rc-update add display-manager default 
```
`root #````
rc-update add NetworkManager default 
```
`root #````
rc-update del dhcpcd default 
```
`root #````
rc-update add elogind boot
```
Start DDE:

`root #````
rc-service dhcpcd stop
```
`root #````
rc-service NetworkManager start
```
`root #````
rc-service elogind start
```
`root #``rc-service display-manager start`
##### With the deprecated xdm init script

Change the `DISPLAYMANGER` value in the xdm configuration file to use [lightdm](https://wiki.gentoo.org/wiki/Lightdm).

**`/etc/conf.d/xdm`**

```
DISPLAYMANAGER="lightdm"
```
Set dbus, xdm, [NetworkManager](https://wiki.gentoo.org/wiki/NetworkManager), and [elogind](https://wiki.gentoo.org/wiki/Elogind) to come up on boot, and disable [dhcpcd](https://wiki.gentoo.org/wiki/Dhcpcd) if enabled.

`root #````
rc-update add dbus default 
```
`root #````
rc-update add xdm default 
```
`root #````
rc-update add NetworkManager default 
```
`root #````
rc-update del dhcpcd default 
```
`root #````
rc-update add elogind boot
```
Start DDE:

`root #````
rc-service dhcpcd stop
```
`root #````
rc-service NetworkManager start
```
`root #````
rc-service elogind start
```
`root #``rc-service xdm start`
#### systemd

Enable [NetworkManager](https://wiki.gentoo.org/wiki/NetworkManager) and [lightdm](https://wiki.gentoo.org/wiki/Lightdm) to be started at boot time:

`root #````
systemctl enable NetworkManager
```
`root #````
systemctl enable lightdm
```
Start and login:

`root #````
systemctl start NetworkManager
```
`root #````
systemctl start lightdm
```
## Removal

To remove DDE, begin by deselecting it:

`root #``emerge --deselect dde-base/dde-meta`
Then, clean the package and its dependencies:

`root #``emerge --ask --depclean`
Finally, remove all of the project overlays. For example, if using eselect repository:

`root #``eselect repository remove deepin`
## Troubleshooting

If you discover any issues, or if you want to contribute, just create a [new issue](https://github.com/zhtengw/deepin-overlay/issues/new) on the [deepin ebuild repository](https://github.com/zhtengw/deepin-overlay) Github project or contact the [maintainer](mailto:atenzd@gmail.com).
