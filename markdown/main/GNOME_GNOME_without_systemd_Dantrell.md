<!-- source: https://wiki.gentoo.org/wiki/GNOME/GNOME_without_systemd/Dantrell | group: Gentoo Wiki (Main) | wiki-title: GNOME/GNOME without systemd/Dantrell -->
---
title: GNOME/GNOME without systemd/Dantrell
url: https://wiki.gentoo.org/wiki/GNOME/GNOME_without_systemd/Dantrell
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-03-31"
fingerprint: fa5d257f2fa7ab86
license: CC BY-SA 4.0
---

# GNOME/GNOME without systemd/Dantrell

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**outdated**. You can help the Gentoo community by verifying and

[updating this article](https://wiki.gentoo.org/index.php?title=GNOME/GNOME_without_systemd/Dantrell&action=edit).

![](https://wiki.gentoo.org/images/thumb/c/c7/GNOME_without_systemd.png/500px-GNOME_without_systemd.png)

This article provides brief instructions for using **GNOME** (3.14 onwards) under **OpenRC** (rather than systemd) in Gentoo, via Dantrell B.'s [patchset](https://github.com/dantrell/gentoo-project-gnome-without-systemd), on either the X11 or Wayland platforms.

For an alternative (OpenRC-based) approach (3.30 onwards) that does not require overlays, please see [this page](https://wiki.gentoo.org/wiki/GNOME/GNOME_without_systemd/Gentoo).

## Introduction

Dantrell B., the primary author of the Funtoo GNOME patchset, has [made his work available](https://github.com/dantrell/gentoo-project-gnome-without-systemd) for Gentoo users via a set of overlays (custom ebuild repositories).

These overlays enable the full GNOME experience (including session tracking and power management) to be enjoyed when booted under OpenRC, even on a PC running 'stable branch' (amd64) Gentoo.

This brief guide outlines the steps needed to get GNOME up and running under his patchset.

## Prerequisites

It is assumed that:

- following completion of the normal ["Installing Gentoo" process from the official Handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64#Installing_Gentoo), a stock **\~amd64**, **\~ppc**, **\~ppc64** or **\~x86** Gentoo system is running, with working internet access etc.
- at least one 'regular' (non-root) user has already been set up;
- the kernel and /etc/portage/make.conf file has been prepared for X-server installation (`VIDEO_CARDS` and `INPUT_DEVICES` variables set), as described [here](https://wiki.gentoo.org/wiki/Xorg/Guide#Installation). While the X-server itself does not *have* to actually be installed prior to emerging GNOME, this *is* recommended (since X-related problems are some of the most commonly encountered);
  - if targeting Wayland, an appropriate [Direct Rendering Manager](https://en.wikipedia.org/wiki/Direct_Rendering_Manager) ('DRM') kernel driver has been installed (see e.g. [these notes](https://wiki.gentoo.org/wiki/User:Sakaki/Sakaki%27s_EFI_Install_Guide/Setting_up_the_GNOME_3_Desktop_under_OpenRC#Enabling_the_Kernel_DRM_Driver_for_your_Graphics_Card) for further details);
- a UTF-8 locale has been selected (as described [here](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Configure_locales)).

## Installation

### Setting global USE flags

If the bindist `USE` flag is set in /etc/portage/make.conf, it is recommended to **unset** it now, to avoid issues with [dev-libs/openssl](https://packages.gentoo.org/packages/dev-libs/openssl), [net-misc/openssh](https://packages.gentoo.org/packages/net-misc/openssh) and dependencies later.

If it is desired to install GNOME on [Wayland](<https://en.wikipedia.org/wiki/Wayland_(display_server_protocol)>) (rather than the default X11), then add  `wayland egl` to the global `USE` flags, in /etc/portage/make.conf (note that it *will* still be possible to log in to an old-school GNOME-on-X11 session when needed, even when Wayland is used). It is recommended to use >= GNOME-3.24 where wayland is used (if running it with earlier versions of GNOME, the additional global `USE` flags `-ck elogind` must also be specified).

To disable GNOME's tracker software (this is optional), add `-tracker` to the global USE flags in /etc/portage/make.conf.

Then, ensure everything is up-to-date, before proceeding further:

`root #````
emerge --sync
```
`root #````
emerge --deep --with-bdeps=y --changed-use --update --ask --verbose @world
```
### Installing the overlays

There are two main ways to install Dantrell B.'s overlays:

1. [eselect repository](https://wiki.gentoo.org/wiki/Eselect/Repository) or
2. add them directly (requires version >= 2.2.16 of [sys-apps/portage](https://packages.gentoo.org/packages/sys-apps/portage)).

#### Option 1: Installation under eselect repository

First, ensure [app-eselect/eselect-repository](https://packages.gentoo.org/packages/app-eselect/eselect-repository) and [dev-vcs/git](https://packages.gentoo.org/packages/dev-vcs/git) are installed:

`root #``emerge --ask --noreplace app-eselect/eselect-repository dev-vcs/git`
Then configure and sync the repository:

`root #````
eselect repository add dantrell-gnome git https://github.com/dantrell/gentoo-overlay-dantrell-gnome.git
```
`root #``emerge --sync dantrell-gnome`
Finally, add the desired GNOME 'version' overlay(s); if using the 'stable' (amd64) branch, it is probably best to use the one which matches the current stable gnome-base/gnome in Gentoo (version 3.24, at the time of writing).

For version `nn` of GNOME 3, issue:

`root #````
eselect repository add dantrell-gnome-3-nn git https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-nn.git
```
`root #``emerge --sync dantrell-gnome-3-nn`
Substitute for `nn` in the above, with one of 14 thru 32: as a concrete example, for GNOME 3.24, issue:

`root #````
eselect repository add dantrell-gnome-3-24 git https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-24.git
```
`root #``emerge --sync dantrell-gnome-3-24`
#### Option 2: Direct installation

Under modern (>= 2.2.16) versions of portage, it isn't necessary to use [eselect-repository](https://wiki.gentoo.org/wiki/Eselect-repository) — overlays can instead be added directly.

Begin by ensuring a /etc/portage/repos.conf directory is in place:

`root #````
mkdir -pv /etc/portage/repos.conf
```
Next, create configuration files for Dantrell B.'s external ebuild repositories (aka, overlays). At the time of writing there are eleven such repos: a 'common' one, and one for each of GNOME 3.14, 3.16, ... 3.32. Create the following file for the 'common' overlay:

**`/etc/portage/repos.conf/dantrell-gnome.conf`**

**Configuration for dantrell-gnome repo**

```
[dantrell-gnome]
# Dantrell B.'s Gentoo Overlay for GNOME (generic)
# Maintainer: Dantrell B. (email: see CONTRIBUTING.md in main GitHub project)
# Homepage: https://github.com/dantrell/gentoo-project-gnome-without-systemd
location = /usr/local/portage/dantrell-gnome
sync-type = git
sync-uri = https://github.com/dantrell/gentoo-overlay-dantrell-gnome.git
priority = 150
auto-sync = yes
```
Next, create the desired GNOME 'version' overlay configuration file(s), as shown below; if using the 'stable' (amd64) branch, create *at least* the one which matches the current stable [gnome-base/gnome](https://packages.gentoo.org/packages/gnome-base/gnome) in Gentoo (version 3.24, at the time of writing).

For example, for GNOME 3.14, create the following file:

**`/etc/portage/repos.conf/dantrell-gnome-3-14.conf`**

**Configuration for dantrell-gnome-3-14 repo**

```
[dantrell-gnome-3-14]
# Dantrell B.'s Gentoo Overlay for GNOME (3.14)
# Maintainer: Dantrell B. (email: see CONTRIBUTING.md in main GitHub project)
# Homepage: https://github.com/dantrell/gentoo-project-gnome-without-systemd
location = /usr/local/portage/dantrell-gnome-3-14
sync-type = git
sync-uri = https://github.com/dantrell/gentoo-overlay-dantrell-gnome-3-14.git
priority = 100
auto-sync = yes
```
The above can actually be used as a basis to create the other (3.16 → 3.32) variants, rather than entering them manually. To do so, issue:

`(chroot) livecd / #````
for ((R=16;R<=32;R+=2)); do cp -v /etc/portage/repos.conf/dantrell-gnome-3-{14,${R}}.conf; sed -e "s/14/${R}/g" -i /etc/portage/repos.conf/dantrell-gnome-3-${R}.conf; done
```
Note that it is fine to have multiple versions installed, as the profile — which will be selected shortly — will decide which takes precedence.

Next, install [git](https://wiki.gentoo.org/wiki/Git) (if not already present):

`root #````
emerge --ask --verbose --noreplace dev-vcs/git
```
Then sync everything:

`root #````
emaint sync --auto
```
### Setting the profile, and updating

Dantrell's overlays provide a bundled set of [profiles](<https://wiki.gentoo.org/wiki/Profile_(Portage)>). By [using one of these](https://github.com/dantrell/gentoo-project-gnome-without-systemd#profile-selection), appropriate `USE` flags, masks etc. are set automatically, to ensure that installation proceeds smoothly. For any of the GNOME releases (3.14, 3.16, ... 3.32) it is possible to choose a 'standard' or 'extended' profile (and, at the time of writing, a 13.0 or 17.0 profile for each of those, giving a total of four profiles per GNOME version). The 'extended' variants have more `USE` flags enabled by default, to yield a [similar experience](https://github.com/dantrell/gentoo-project-gnome-without-systemd/blob/master/HACKING.md#profiles) to the familiar desktop/gnome profiles in Gentoo.

Select an appropriate profile now (at the time of writing, users on the "stable" (amd64) branch are probably safest sticking to one of the 3.14 - 3.24 profiles (although, for reference, 3.32 *will* build under amd64); \~amd64 users may use whichever they choose):

`root #````
eselect profile list
```
\[1\]   default/linux/amd64/13.0 (stable)
  \[2\]   default/linux/amd64/13.0/selinux (dev)
... additional output suppressed ...
  \[12\]  default/linux/amd64/17.0 (stable) \*
... additional output suppressed ...
  \[57\]  dantrell-gnome:default/linux/amd64/13.0/desktop/gnome+plasma (stable)
  \[58\]  dantrell-gnome:default/linux/amd64/17.0/desktop/gnome+plasma (stable)
  \[59\]  dantrell-gnome-3-14:default/linux/amd64/13.0/desktop/gnome/3.14 (stable)
  \[60\]  dantrell-gnome-3-14:default/linux/amd64/13.0/desktop/gnome/3.14/extended (stable)
  \[61\]  dantrell-gnome-3-14:default/linux/amd64/17.0/desktop/gnome/3.14 (stable)
  \[62\]  dantrell-gnome-3-14:default/linux/amd64/17.0/desktop/gnome/3.14/extended (stable)
  \[63\]  dantrell-gnome-3-16:default/linux/amd64/13.0/desktop/gnome/3.16 (stable)
  \[64\]  dantrell-gnome-3-16:default/linux/amd64/13.0/desktop/gnome/3.16/extended (stable)
  \[65\]  dantrell-gnome-3-16:default/linux/amd64/17.0/desktop/gnome/3.16 (stable)
  \[66\]  dantrell-gnome-3-16:default/linux/amd64/17.0/desktop/gnome/3.16/extended (stable)
... additional output suppressed ...
  \[91\]  dantrell-gnome-3-30:default/linux/amd64/13.0/desktop/gnome/3.30 (stable)
  \[92\]  dantrell-gnome-3-30:default/linux/amd64/13.0/desktop/gnome/3.30/extended (stable)
  \[93\]  dantrell-gnome-3-30:default/linux/amd64/17.0/desktop/gnome/3.30 (stable)
  \[94\]  dantrell-gnome-3-30:default/linux/amd64/17.0/desktop/gnome/3.30/extended (stable)
  \[91\]  dantrell-gnome-3-32:default/linux/amd64/13.0/desktop/gnome/3.32 (stable)
  \[92\]  dantrell-gnome-3-32:default/linux/amd64/13.0/desktop/gnome/3.32/extended (stable)
  \[93\]  dantrell-gnome-3-32:default/linux/amd64/17.0/desktop/gnome/3.32 (stable)
  \[94\]  dantrell-gnome-3-32:default/linux/amd64/17.0/desktop/gnome/3.32/extended (stable)

(The numbering etc. on the output on any given target machine may well differ from the above, and of course only dantrell-gnome-... profiles corresponding to overlays *installed* overlays will be displayed.)

`root #````
eselect profile set "dantrell-gnome-3-24:default/linux/amd64/17.0/desktop/gnome/3.24/extended"
```
`root #````
eselect profile show
```
Current /etc/portage/make.profile symlink:
  dantrell-gnome-3-24:default/linux/amd64/17.0/desktop/gnome/3.24/extended

With the desired profile set, re-emerge @world, to pick up the new `USE` flags, default packages etc.

`root #````
emerge --deep --with-bdeps=y --changed-use --update --ask --verbose @world
```
The various [dependencies](https://github.com/dantrell/gentoo-project-gnome-without-systemd#dependencies) for GNOME without systemd are now installed automatically, either as part of the above @world update under the custom profile (e.g., [sys-apps/dbus](https://packages.gentoo.org/packages/sys-apps/dbus) and [sys-auth/consolekit](https://packages.gentoo.org/packages/sys-auth/consolekit) (for \<= GNOME 3.22) / [sys-auth/elogind](https://packages.gentoo.org/packages/sys-auth/elogind) (for >= GNOME 3.24)) or during the [gnome-base/gnome](https://packages.gentoo.org/packages/gnome-base/gnome) emerge to follow shortly (e.g., [sys-power/pm-utils](https://packages.gentoo.org/packages/sys-power/pm-utils) and [sys-power/acpid](https://packages.gentoo.org/packages/sys-power/acpid)); nothing further needs to be emerged at this stage.

### Preparing USE Flags Prior to GNOME Emerge

If there are any additional applications which it is desired to install as part of the GNOME emerge, set their USE flags now, [as follows](https://github.com/dantrell/gentoo-project-gnome-without-systemd#configuration-optional):

| USE flag | Default | Description | 
|---|---|---|
| `anjuta` |  | Install the dev-util/anjuta IDE | 
| `bijiben` | Yes | Install the app-misc/bijiben note editor | 
| `boxes` |  | Install the gnome-extra/gnome-boxes remote and virtual system manager | 
| `builder` |  | Install the gnome-extra/gnome-builder IDE (only in GNOME 3.16 or greater) | 
| `california` |  | Install the gnome-extra/california calendar | 
| `dino` | Yes | Install the net-im/dino chat client (only in GNOME 3.32 or greater) | 
| `empathy` |  | Install the net-im/empathy chat client | 
| `epiphany` |  | Install the www-client/epiphany web browser | 
| `evolution` | Yes | Install the mail-client/evolution mail client | 
| `flashback` |  | Install the gnome-base/gnome-flashback (aka fallback mode; work-in-progress) | 
| `fonts` | Yes | Install media-fonts/{noto,symbola,unifont} | 
| `games` | Yes | Install Gnome Games | 
| `geary` |  | Install the mail-client/geary mail client | 
| `gnote` |  | Install the app-misc/gnote note editor | 
| `latexila` |  | Install the app-editors/latexila integrated LaTeX environment | 
| `multi-writer` |  | Install the gnome-extra/gnome-multi-writer USB device writer (only in GNOME 3.16 or greater) | 
| `recipes` | Yes | Install the gnome-extra/gnome-recipes live cookbook (only in GNOME 3.22 or greater) | 
| `share` | Yes | Install the gnome-extra/gnome-user-share personal file sharing tool | 
| `shotwell` | Yes | Install the media-gfx/shotwell photo manager | 
| `simple-scan` |  | Install the media-gfx/simple-scan document scanning utility | 
| `todo` | Yes | Install the gnome-extra/gnome-builder task manager (only in GNOME 3.18 or greater) | 
| `tracker` | Yes | Install the app-misc/tracker indexer and the GNOME packages that require it | 
| `usage` | Yes | Install the gnome-extra/gnome-usage system resources monitor (only in GNOME 3.28 or greater) | 

For example, to add the [epiphany](https://wiki.gentoo.org/wiki/Epiphany) browser to the default set of installed applications, do:

`root #````
mkdir -pv /etc/portage/package.use
```
`root #````
echo "gnome-base/gnome-extra-apps epiphany" >> /etc/portage/package.use/gnome-extra-apps
```
There are some additional GNOME 'quality of life' improvements in Dantrell's patchset (background resolution etc.), which are controlled by `USE` flags on certain packages. The defaults will probably be suitable for most users, but for more details please see [here](https://github.com/dantrell/gentoo-project-gnome-without-systemd#configuration-optional).

### Emerging GNOME

GNOME itself may now be emerged! Issue:

`root #````
emerge --ask --verbose --keep-going gnome-base/gnome
```
Assuming that completes successfully, **it is still important to check that the necessary X11 drivers have been properly emerged**: often, they will not have been, particularly if it proved necessary to run the emerge step more than once (due to build parallelism errors). To make sure, issue:

`root #````
emerge --ask --verbose --oneshot x11-base/xorg-drivers
```
## Configuring and running GNOME

Once GNOME is emerged, change the `DISPLAYMANGER` value in the xdm configuration file (/etc/conf.d/xdm), so that the [gdm](https://wiki.gentoo.org/wiki/GNOME/gdm) display manager is used:

**`/etc/conf.d/xdm`**

**Specify the GNOME display manager, as follows**

```
# What display manager do you use ?  [ xdm | gdm | kdm | gpe | entrance ]
# NOTE: If this is set in /etc/rc.conf, that setting will override this one.
DISPLAYMANAGER="gdm"
```
Leave the rest of the file as-is.

Then set dbus, xdm, [NetworkManager](https://wiki.gentoo.org/wiki/NetworkManager) and openrc-settingsd to come up on boot, and disable [dhcpcd](https://wiki.gentoo.org/wiki/Dhcpcd) if using it:

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
rc-update add openrc-settingsd default
```
`root #````
rc-update del dhcpcd default
```
If targeting X11 (rather than Wayland) *and* using \< GNOME 3.24, issue:

`root #````
rc-update add acpid default
```
`root #````
rc-update add consolekit default
```
Otherwise, if targeting Wayland, *or* using >= GNOME 3.24 issue:

`root #````
rc-update add elogind boot
```
Next, check if the machine has a plugdev group, and, if it does, add any regular users to it:

`root #````
getent group plugdev && gpasswd -a <regular_username> plugdev
```
Similarly, to allow regular users to play GNOME games (assuming these have been installed), add them to the games group:

`root #````
getent group games && gpasswd -a <regular_username> games
```
To allow the use of [direct rendering](https://wiki.gentoo.org/wiki/Xorg/Hardware_3D_acceleration_guide#Add_appropriate_user.28s.29_to_the_video_group), issue:

`root #````
getent group video && gpasswd -a <regular_username> video
```
Finally, start up GNOME!

`root #````
openrc
```
A GNOME login screen should now be visible (and this will also come up automatically on boot). On some machines, it may be necessary to move the mouse or press a key, for the login screen to appear.

## Usage

For more information about the GNOME interface (which is generally self-explanatory), see [https://gnome.org](https://gnome.org).
Some additional useful setup tips about GNOME may also be found [here](https://wiki.gentoo.org/wiki/User:Sakaki/Sakaki%27s_EFI_Install_Guide/Using_Your_New_Gentoo_System#Miscellaneous_GNOME_Points).

## Removal

To remove GNOME, begin by unmerging it:

`root #````
emerge --ask --verbose --depclean gnome-base/gnome
```
or

`root #````
emerge --ask --verbose --depclean gnome-base/gnome-light
```
Remove all of the project overlays. For example, if using eselect-repository:

`root #````
eselect repository delete dantrell-gnome
```
`root #````
eselect repository delete dantrell-gnome-3.24
```
Switch profile; for example:

`root #````
eselect profile set "default/linux/amd64/17.0"
```
Update @world:

`root #````
emerge --ask --update --deep --changed-use --with-bdeps=y @world
```
Clean dependencies:

`root #````
emerge --ask --depclean
```
Prevent unnecessary services starting up automatically; e.g.:

`root #````
rc-update del elogind boot
```
`root #````
rc-update del dbus default
```
`root #````
rc-update del xdm default
```
`root #````
rc-update del NetworkManager default
```
`root #````
rc-update del openrc-settingsd default
```
Add dhcpcd (if required, in the absence of NetworkManager):

`root #````
rc-update add dhcpcd default
```
Finally, reboot the system to complete the uninstall (to a textual login, in this case).

## See also

## External resources

- [Dantrell B.'s project homepage on GitHub](https://github.com/dantrell/gentoo-project-gnome-without-systemd)
- Dantrell B.'s support thread at Gentoo forums, [part 1](https://forums.gentoo.org/viewtopic-t-1022050.html) and [part 2](https://forums.gentoo.org/viewtopic-t-1082226.html).
