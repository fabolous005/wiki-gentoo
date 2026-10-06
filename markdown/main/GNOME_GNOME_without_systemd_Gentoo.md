<!-- source: https://wiki.gentoo.org/wiki/GNOME/GNOME_without_systemd/Gentoo | group: Gentoo Wiki (Main) | wiki-title: GNOME/GNOME without systemd/Gentoo -->
---
title: GNOME/GNOME without systemd/Gentoo
url: https://wiki.gentoo.org/wiki/GNOME/GNOME_without_systemd/Gentoo
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-04-27"
fingerprint: da75937b0fa38984
license: CC BY-SA 4.0
---

# GNOME/GNOME without systemd/Gentoo

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

![](https://wiki.gentoo.org/images/thumb/9/94/Openrc_gnome3.jpg/500px-Openrc_gnome3.jpg)

The fully-featured GNOME desktop environment is directly supported in Gentoo for both systemd *and* OpenRC, as of [gnome-base/gnome](https://packages.gentoo.org/packages/gnome-base/gnome)-3.30 <sup>[\[1\]](https://wiki.gentoo.org#cite_note-gnome_openrc-1)</sup>.

This article briefly covers a native OpenRC install; for an alternative (OpenRC-based) approach, please see [Dantrell's overlays](https://wiki.gentoo.org/wiki/GNOME/GNOME_without_systemd/Dantrell).

## Prerequisites

It is assumed that:

- following completion of the normal ["Installing Gentoo" process from the official Handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64#Installing_Gentoo), a stock **amd64**, **\~ppc**, **\~ppc64** or **x86** Gentoo system is running, with working internet access etc.
- at least one 'regular' (non-root) user has already been set up;
- the kernel and /etc/portage/make.conf file has been prepared for X-server installation (`VIDEO_CARDS` and `INPUT_DEVICES` variables set), as described [here](https://wiki.gentoo.org/wiki/Xorg/Guide#Installation). While the X-server itself does not *have* to actually be installed prior to emerging GNOME, this *is* recommended (since X-related problems are some of the most commonly encountered);
  - if targeting Wayland, an appropriate [Direct Rendering Manager](https://en.wikipedia.org/wiki/Direct_Rendering_Manager) ('DRM') kernel driver has been installed (see e.g. [these notes](https://wiki.gentoo.org/wiki/User:Sakaki/Sakaki%27s_EFI_Install_Guide/Setting_up_the_GNOME_3_Desktop_under_OpenRC#Enabling_the_Kernel_DRM_Driver_for_your_Graphics_Card) for further details);
- a UTF-8 locale has been selected (as described [here](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Configure_locales)).

## Installation

### Keywording

At the time of writing (May 2019), official support for GNOME on OpenRC has been stabilized for **amd64** and **x86** users. Other supported architectures (**\~ppc** and **\~ppc64**) still require use of the testing branch.

### Setting global USE flags

If the bindist `USE` flag is set in /etc/portage/make.conf, it is recommended to **unset** it now, to avoid issues with [dev-libs/openssl](https://packages.gentoo.org/packages/dev-libs/openssl), [net-misc/openssh](https://packages.gentoo.org/packages/net-misc/openssh) and dependencies later.

If it is desired to install GNOME on [Wayland](<https://en.wikipedia.org/wiki/Wayland_(display_server_protocol)>) (rather than the default X11), then add `wayland egl` to the global `USE` flags, in /etc/portage/make.conf (note that it *will* still be possible to log in to an old-school GNOME-on-X11 session when needed, even when Wayland is used).

If it is desired to run Xorg without root/suid (which is far more secure) then add `elogind` to the global USE flags.

To disable GNOME's tracker software (this is optional), add `-tracker` to the global USE flags in /etc/portage/make.conf.

Then, ensure everything is up-to-date, before proceeding further:

`root #````
emerge --sync
```
`root #````
emerge --deep --with-bdeps=y --changed-use --update --ask --verbose @world
```
### Setting the GNOME profile, and updating

To ease installation under OpenRC, select the appropriate profile now (this will ensure appropriate package-specific USE flags, masks etc are set to ensure a painless GNOME emerge):

`root #````
eselect profile set "default/linux/amd64/23.0/desktop/gnome"
```
`root #````
eselect profile show
```
Current /etc/portage/make.profile symlink:
  default/linux/amd64/23.0/desktop/gnome

With the desired profile set, re-emerge @world, to pick up the new `USE` flags, default packages etc.

`root #``emerge --ask --deep --changed-use --update --verbose @world`
### Emerging GNOME

GNOME itself may now be emerged! Issue:

`root #``emerge --ask --verbose --keep-going gnome-base/gnome`
Assuming that completes successfully, **it is still important to check that the necessary X11 drivers have been properly emerged**: often, they will not have been, particularly if it proved necessary to run the emerge step more than once (due to build parallelism errors). To make sure, issue:

`root #``emerge --ask --verbose --oneshot x11-base/xorg-drivers`
## Configuration

Once GNOME is emerged, change the `DISPLAYMANAGER` value in the display-manager configuration file (/etc/conf.d/display-manager), so that the [gdm](https://wiki.gentoo.org/wiki/GNOME/gdm) display manager is used:

**`/etc/conf.d/display-manager`**

**Specify the GNOME display manager, as follows**

```
CHECKVT=7
DISPLAYMANAGER="gdm"
```
Leave the rest of the file as-is.

Then, set dbus, display-manager, and openrc-settingsd to come up in the default runlevel:

`root #````
rc-update add dbus default
```
`root #````
rc-update add display-manager default
```
`root #````
rc-update add openrc-settingsd default
```
Also, ensure that the [sys-auth/elogind](https://packages.gentoo.org/packages/sys-auth/elogind) service starts up at boot:

`root #``rc-update add elogind boot`
Next, check if the machine has a plugdev group, and, if it does, add any regular users to it:

`root #``getent group plugdev && gpasswd -a <regular_username> plugdev`
To allow the use of [direct rendering](https://wiki.gentoo.org/wiki/Xorg/Hardware_3D_acceleration_guide#Add_appropriate_user.28s.29_to_the_video_group), issue:

`root #``getent group video && gpasswd -a <regular_username> video`
Finally, start up GNOME!

`root #``openrc`
A GNOME login screen should now be visible (and this will also come up automatically on boot). On some machines, it may be necessary to move the mouse or press a key, for the login screen to appear.

#### Troubleshooting:

- rebooting the system may be required before elogind works properly, though  `rc-service display-manager start` may suffice
- check the docs on how to set up [Non root Xorg](https://wiki.gentoo.org/wiki/Non_root_Xorg) instead of using suid for a more secure system (also has helpful troubleshooting suggestions)



## Usage

For more information about the GNOME interface (which is generally self-explanatory), see [https://gnome.org](https://gnome.org).
Some additional useful setup tips about GNOME may also be found [here](https://wiki.gentoo.org/wiki/User:Sakaki/Sakaki%27s_EFI_Install_Guide/Using_Your_New_Gentoo_System#Miscellaneous_GNOME_Points).

## Removal

To remove GNOME, begin by unmerging it:

`root #``emerge --deselect gnome-base/gnome`
Switch profile; for example:

`root #``eselect profile set "default/linux/amd64/23.0"`
Update @world:

`root #``emerge --ask --update --deep --changed-use @world`
Clean dependencies:

`root #``emerge --ask --depclean`
Prevent unnecessary services starting up automatically; e.g.:

`root #````
rc-update del elogind boot
```
`root #````
rc-update del dbus default
```
`root #````
rc-update del display-manager default
```
`root #````
rc-update del openrc-settingsd default
```
Finally, reboot the system to complete the uninstall (to a textual login, in this case).

## See also

- [Project:GNOME](https://wiki.gentoo.org/wiki/Project:GNOME) — aims to bring the current and complete GNOME desktop environment to Gentoo.
- [GNOME Display Manager](https://wiki.gentoo.org/wiki/GNOME/gdm) — is the daemon responsible for launching graphical display sessions via the [Xorg](https://wiki.gentoo.org/wiki/Xorg) display server or the gnome-shell directly via [Wayland](https://wiki.gentoo.org/wiki/Wayland) display protocol.
- [Sakaki's Unofficial EFI Install Guide](https://wiki.gentoo.org/wiki/User:Sakaki/Sakaki%27s_EFI_Install_Guide)

## External resources

- The project's [sticky support thread](https://forums.gentoo.org/viewtopic-t-1094796.html) on the Gentoo Forums.

## References

1. [↑](https://wiki.gentoo.org#cite_ref-gnome_openrc_1-0) Raudsepp, Mart. Gentoo Blogs: ["Gentoo GNOME 3.30 for all init systems"](https://blogs.gentoo.org/leio/2019/03/26/gnome-3-30/), March 26th, 2019. Retrieved April 26th 2019.
