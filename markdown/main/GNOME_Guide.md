<!-- source: https://wiki.gentoo.org/wiki/GNOME/Guide | group: Gentoo Wiki (Main) | wiki-title: GNOME/Guide -->
---
title: GNOME/Guide
url: https://wiki.gentoo.org/wiki/GNOME/Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-20"
fingerprint: d07de369ad37a994
license: CC BY-SA 4.0
---

# GNOME/Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**GNOME** is a popular [desktop environment](https://wiki.gentoo.org/wiki/Desktop_environment) capable launching [Xorg](https://wiki.gentoo.org/wiki/Xorg) and [Wayland](https://wiki.gentoo.org/wiki/Wayland) sessions. This guide attempts to describe all aspects of GNOME, including installation, configuration, and usage.

## What is GNOME?

### The project

The [GNOME project](https://www.gnome.org/) is a free software organization dedicated to the development of GNOME, a Unix/Linux desktop suite and development platform. The [GNOME Foundation](https://www.gnome.org/foundation/) coordinates the development and other aspects of the GNOME Project.

### The software

GNOME is a desktop environment and a development platform. This piece of free software is the desktop of choice for several industry leaders including Canonical (Ubuntu) and Red Hat (Red Hat Enterprise Linux, Fedora, CentOS Stream).

### The community

Like any large free software project, GNOME has an extensive user and development base. [GNOME Planet](https://planet.gnome.org/) is a popular blog aggregator for GNOME hackers and contributors whereas [developer.gnome.org](https://developer.gnome.org/) is for the GNOME developers. [GNOME Library](https://help.gnome.org/users/) contains a huge list of GNOME resources for end users.

## Prerequisites

Historically speaking, the Xorg display server was the standard display base for all desktop environments on Linux. With GNOME 3 and beyond, a shift to the Wayland, a newer display server protocol, has begun. Systems other than NVIDIA will have no problem running GNOME sessions over Wayland.

That said, as a general fall back, it is a good idea to first read and follow the instructions in the [Xorg Guide](https://wiki.gentoo.org/wiki/Xorg/Guide) to setup a X environment.

According to GNOME upstream, GNOME 40 is written with the systemd init system in mind. Because of this, it is a good idea for systemd users to read and comply with all necessary kernel settings from the [systemd](https://wiki.gentoo.org/wiki/Systemd) article.

## Installation

### USE Flags


| [+bluetooth](https://packages.gentoo.org/useflags/+bluetooth) | Enable Bluetooth Support | 
| [+classic](https://packages.gentoo.org/useflags/+classic) | Install gnome-extra/gnome-shell-extensions for the Gnome Shell Classic mode | 
| [+extras](https://packages.gentoo.org/useflags/+extras) | Install additional GNOME applications | 
| [accessibility](https://packages.gentoo.org/useflags/accessibility) | Add support for accessibility (eg 'at-spi' library) | 
| [cups](https://packages.gentoo.org/useflags/cups) | Add support for CUPS (Common Unix Printing System) | 

### Profile

Before installing the GNOME suite, editing the system's USE variables is a good idea. The [Gentoo GNOME project developers](https://wiki.gentoo.org/wiki/Project:GNOME) provide GNOME profiles in order to aid system-wide tuning for the GNOME software stack. Select the latest stable GNOME profile before emerging GNOME.

#### OpenRC

OpenRC users using logind can select the GNOME OpenRC profile:

`root #``eselect profile set default/linux/amd64/23.0/desktop/gnome`
#### systemd

systemd users will want to select the following profile:

`root #``eselect profile set default/linux/amd64/23.0/desktop/gnome/systemd`
Make sure that `X`, `gtk`, and `gnome` are in the USE variable located in /etc/portage/make.conf. It is recommended to enable support for [D-Bus](https://wiki.gentoo.org/wiki/D-Bus) system-wide. systemd includes this system message bus.

**`/etc/portage/make.conf`**

**Example global USE flags for a GNOME desktop environment**

```
USE="-kde X gtk gnome systemd"
```
#### Combined hardened profiles

Users that run hardened profiles can also combine it with all the features of the GNOME desktop profile. For steps on doing this please follow [GNOME/Guide/Hardened GNOME Profiles](https://wiki.gentoo.org/wiki/GNOME/Guide/Hardened_GNOME_Profiles).

### Emerge

Once finished, begin the GNOME installation by emerging the GNOME desktop suite:

`root #``emerge --ask gnome-base/gnome`
This will take a while. Once it’s done, update environment variables:

`root #``env-update && source /etc/profile`
Next the remaining services and user groups will be cleaned.

Verify the `plugdev` group exists. If it does, it is advisable to make each prospective GNOME user member of this group, but this step is optional (the group is not common anymore).

`root #``getent group plugdev`
plugdev:x:104:

Substitute `<username>` in the next command with each GNOME user's user name:

`root #``gpasswd -a <username> plugdev`
## First impressions

It is time to take a look at what was just built. Either configure the session manager to run GNOME when the startx command is invoked (see [using startx](https://wiki.gentoo.org/wiki/Xorg/Guide#Using_startx) in the Xorg guide for more information), or enable the [GDM service](https://wiki.gentoo.org/wiki/GNOME/GDM), for a more convenient way to start GNOME.

### Enabling GDM

#### OpenRC

For OpenRC systems, elogind is a dependency of GDM and must be started for GDM to run properly:

`root #````
rc-update add elogind boot
```
`root #````
rc-service elogind start
```
Next add display-manager-init to the default runlevel and start the service:

`root #``emerge --ask --noreplace gui-libs/display-manager-init`
In /etc/conf.d/display-manager set DISPLAYMANAGER to "gdm"

**`/etc/conf.d/display-manager`**

To start on boot, add display-manager to the default runlevel:

`root #````
rc-update add display-manager default
```
To start GDM either reboot or start it immediately:

`root #````
rc-service display-manager start
```
#### systemd

To start GDM upon boot:

`root #``systemctl enable gdm.service`
To start GDM immediately, run:

`root #``systemctl start gdm.service`
Another suggestion is to [activate Network Manager](https://wiki.gentoo.org/wiki/NetworkManager#systemd), in case no other network managing service is activated.

### Using startx

Configure the session manager to run GNOME when the the startx command is invoked (see [using startx](https://wiki.gentoo.org/wiki/Xorg/Guide#Using_startx) in the Xorg guide for more information).

`user $``echo 'XSESSION="Gnome"' > /etc/env.d/90xsession`
This will create the 90xsession file and set the default X session to Gnome. Remember to run env-update after making changes to 90xsession.

Now start the graphical environment by issuing startx as a normal user:

`user $``startx`
If all goes well GNOME should happily provide a greeting. Congratulations on setting up GNOME!

## Privacy

Some users might be concerned about the fact that there is an *online accounts* section is the GNOME control center, which enables the user to connect the system to various services like Google, Microsoft, etc. In Portage, a USE flag can be set to remove this functionality:

**`/etc/portage/make.conf`**

```
USE="... -gnome-online-accounts"
```
This will tell Portage to not install the [net-libs/gnome-online-accounts](https://packages.gentoo.org/packages/net-libs/gnome-online-accounts) package if possible.

Re-emerge world with the `--changed-use` flag and clean unused dependencies.

`root #``emerge --ask --changed-use --update --deep @world``root #``emerge --depclean`
## Configuration

### Mixed localization

It could be general advice to have `C` as the global default locale, with a different one for the desktop. This can be achieved by add settings:

**`~/.config/environment.d/01_localize.conf`**

**Override locale for user session**

```
LANG="zh_CN.utf8"
LC_MESSAGES="zh_CN.utf8"
LC_TIME="zh_CN.utf8"
```
Then choose the region for locale in gnome-setting-center, or via command:

`user $``gsettings set org.gnome.system.locale region 'zh_CN.utf8'`
Log out, make sure the old session is killed and re-login, these settings will be applied to the new session.

To override session's locale for terminal in gnome, add:

**`~/.bashrc`**

**Override locale for terminal**

```
LANG="C.utf8"
LC_MESSAGES="C.utf8" 
LC_TIME="C.utf8"
```
### Tweaking GNOME

For extra configuration options in GNOME 40 install the [gnome-extra/gnome-tweaks](https://packages.gentoo.org/packages/gnome-extra/gnome-tweaks) package. The tweak tool allows customization at a deeper level than the standard settings application.

### Advanced tweaking

Advanced tweaking for GNOME can be performed from the command line via the gsettings or dconf commands or graphically via [dconf-editor](https://wiki.gnome.org/Apps/DconfEditor). All modifiable settings are accessible using these tools. For more information, see [upstream's documentation](https://developer.gnome.org/GSettings/).

### Widgets in GNOME 40

By default on Gentoo GNOME 40 does not support widgets. For users who wish to obtain widget functionality a separate package is available:

`root #``emerge --ask gnome-extra/gnome-shell-extensions`
After the shell extensions are installed, eselect can be used to control defaults on a global level:

`root #``eselect gnome-shell-extensions list`
Available extensions (\* means enabled for all users by default):
  \[1\]   alternate-tab@gnome-shell-extensions.gcampax.github.com
  \[2\]   apps-menu@gnome-shell-extensions.gcampax.github.com
  \[3\]   auto-move-windows@gnome-shell-extensions.gcampax.github.com
  \[4\]   drive-menu@gnome-shell-extensions.gcampax.github.com
  \[5\]   launch-new-instance@gnome-shell-extensions.gcampax.github.com
  \[6\]   native-window-placement@gnome-shell-extensions.gcampax.github.com
  \[7\]   places-menu@gnome-shell-extensions.gcampax.github.com
  \[8\]   screenshot-window-sizer@gnome-shell-extensions.gcampax.github.com
  \[9\]   user-theme@gnome-shell-extensions.gcampax.github.com
  \[10\]  window-list@gnome-shell-extensions.gcampax.github.com
  \[11\]  windowsNavigator@gnome-shell-extensions.gcampax.github.com
  \[12\]  workspace-indicator@gnome-shell-extensions.gcampax.github.com

### Enable click-to-install Shell Extensions through the web browser

For web browsers such as [Google Chrome](https://wiki.gentoo.org/wiki/Google_Chrome), [Chromium](https://wiki.gentoo.org/wiki/Chromium), and [Vivaldi](https://wiki.gentoo.org/wiki/Vivaldi) be sure to get the required browser add-on through the Chrome store: [https://chrome.google.com/webstore/detail/gphhapmejobijbbhgpjhcjognlahblep](https://chrome.google.com/webstore/detail/gphhapmejobijbbhgpjhcjognlahblep)

[Firefox](https://wiki.gentoo.org/wiki/Firefox) users can get it here: [https://addons.mozilla.org/firefox/addon/gnome-shell-integration/](https://addons.mozilla.org/firefox/addon/gnome-shell-integration/)

After the add-on has been installed for the browser of choice, a backend must also be emerged:

`root #``emerge --ask gnome-extra/gnome-browser-connector`
It should now be possible to install, manage, and uninstall shell extensions at [https://extensions.gnome.org/](https://extensions.gnome.org/)

If things are not working as expected check the [upstream installation instructions](https://wiki.gnome.org/Projects/GnomeShellIntegrationForChrome/Installation) for news.

### Non-root user authentication for dialogs

Certain GNOME dialogs such as Printers, adding wireless networks, and Users require administrator authentication. This is handled through [sys-auth/polkit](https://packages.gentoo.org/packages/sys-auth/polkit) and operates independently from [app-admin/sudo](https://packages.gentoo.org/packages/app-admin/sudo). By default in Gentoo, the root account is the only administrator, and so even if a user account can run root commands through sudo, authentication in these GNOME dialogs will fail.

To make all users of the wheel group administrators, create a copy of /usr/share/polkit-1/rules.d/50-default.rules starting with a number lower than 50, and edit the line return \["unix-user:0"\] to the following:

**`/etc/polkit-1/rules.d/49-wheel.rules`**

**Administrator wheel group**

The [Polkit](https://wiki.gentoo.org/wiki/Polkit) page provides more details on rules configuration.

### GNOME hotspot

In order for gnome-hotspot to work, the wireless card must support [AP (access point) infrastructure mode](https://wireless.wiki.kernel.org/en/users/Documentation/modes#accesspoint_ap_infrastructure_mode). The following package USE flags are also needed:

**`/etc/portage/package.use`**

**Connection Sharing and Access Point Support**

```
 connection-sharing
net-wireless/wpa_supplicant ap
```
In addition, the following kernel options are necessary:

**NAT options (locations for kernel 4.14)**

## Removal

### Unmerge

A possible way to completely remove a GNOME installation is by explicitly uninstalling the [gnome-base/gnome](https://packages.gentoo.org/packages/gnome-base/gnome) package, then cleaning the dependencies of that package.

In order to do this sanely make sure the main ebuild repository has been synced:

`root #``emerge --sync`
Next, run a world update so that the system is fully up-to-date:

`root #``emerge --ask --update --newuse --deep --with-bdeps=y @world`
Unmerge the GNOME base package. Substitute the base package with [gnome-base/gnome-light](https://packages.gentoo.org/packages/gnome-base/gnome-light) if the 'light' version of the package was installed instead:

`root #``emerge --ask --depclean gnome-base/gnome`
Finally, depclean the system:

`root #``emerge --ask --depclean`
GNOME should now be removed.

## Troubleshooting

### Login failure with message "Oh no something has gone wrong"

One source of this error can be the permissions for the video device. When logging in fails and a message appears that says "Oh no, something has gone wrong", then try to become a member of the video group. Add the user to the video group with gpasswd like so:

`root #``gpasswd -a <user> video`
### GNOME on Wayland session is not launching with NVIDIA

Attempting to launch GNOME on Wayland sessions is a known issue. Unfortunately some older versions of the NVIDIA binary blob drivers are not compatible with Wayland. Systems that simply have older versions of the NVIDIA binary blob driver installed, but are not using it, can see [this workaround](https://wiki.gentoo.org/wiki/GNOME/GDM#GDM_crashes_when_attempting_to_launch_a_GNOME_Wayland_session).

For at least `gnome-base/gdm-44.1` it is required to set `NVreg_PreserveVideoMemoryAllocations=1` in `/etc/modprobe.d/nvidia.conf`, otherwise Wayland support is being disabled.

### GNOME built-in screen recorder is not working

GNOME's screen recorder uses vp8 codec which is developed by Google. The recorder needs this codec and pipewire screencast feature to record the desktop. It can be enabled it via the the `vpx` and `screencast` USE flags in either the make.conf or package.use files.

**`/etc/portage/make.conf`**

```
USE="vpx screencast"
```
### GNOME and Pinentry not working with GPG

For example when using Evolution to sign emails with PGP, the private key needs to be decrypted. If this key has a password, a Pinentry dialogue trys to open. To enable the Gtk version, these configuration files need to be edited.

**`~/.gnupg/gpg.conf`**

```
 loopback
```
**`~/.gnupg/gpg-agent.conf`**

```
 /usr/bin/pinentry-gnome3
```
### Nautilus is not showing thumbnails for .mp4 video files

The `ffmpeg` USE flag is not enabled globally in default `desktop/gnome` profile. Enable it locally for the `media-plugins/gst-plugins-meta`:

**`/etc/portage/package.use`**

```
 ffmpeg
```
Emerge @world with `--changed-use` (-U) flag:

`root #``emerge --changed-use @world`
The good idea is to delete `.cache/thumbnails` in your *$HOME* folder:

`user $``rm -r ~/.cache/thumbnails`
## External resources

- [https://github.com/dantrell/gentoo-project-gnome-without-systemd](https://github.com/dantrell/gentoo-project-gnome-without-systemd) - GNOME without systemd.
- [https://github.com/bdaase/remove-alt-tab-delay](https://github.com/bdaase/remove-alt-tab-delay) - Remove the 0.15 second Alt+Tab delay.
- [https://help.gnome.org/admin/](https://help.gnome.org/admin/) - Upstream GNOME administrator guide.
- [https://help.gnome.org/users/](https://help.gnome.org/users/) - Upstream GNOME user guide.

## References
