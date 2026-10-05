<!-- source: https://wiki.gentoo.org/wiki/Flatpak | group: Gentoo Wiki (Main) | wiki-title: Flatpak -->
---
title: Flatpak
url: https://wiki.gentoo.org/wiki/Flatpak
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-07"
fingerprint: b40758785da3f9c4
license: CC BY-SA 4.0
---

# Flatpak

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Flatpak** is a package management framework aiming to provide support for sandboxed, distro-agnostic binary packages for Linux desktop applications. Just as [chroot](https://wiki.gentoo.org/wiki/Chroot), [Docker](https://wiki.gentoo.org/wiki/Docker), and [LXD](https://wiki.gentoo.org/wiki/LXD) provide a means to isolate primarily *server-based* applications from the underlying operating system, Flatpak provides a mechanism to isolate primarily *desktop-based* applications from the underlying operating system. When combined with features like [systemd-homed](https://wiki.gentoo.org/wiki/Systemd-homed), it becomes possible to contain a user and all of that user's applications within a single directory, the user's `$HOME`, in a manner that is portable across systems of the same CPU architecture.

## Installation

### Kernel

**Required**

```
File systems  --->
   [*] FUSE (Filesystem in Userspace) support 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_FUSE_FS</code> to find this item.
 Security options  --->
   \[\*\] Landlock support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SECURITY\_LANDLOCK\</code> to find this item.
   (landlock,yama) Ordered list of enabled LSMs [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_LSM\</code> to find this item.

### USE flags


| [X](https://packages.gentoo.org/useflags/X) | Add support for X11 | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [introspection](https://packages.gentoo.org/useflags/introspection) | Add support for GObject based introspection | 
| [policykit](https://packages.gentoo.org/useflags/policykit) | Enable PolicyKit (polkit) authentication support | 
| [seccomp](https://packages.gentoo.org/useflags/seccomp) | Enable seccomp (secure computing mode) to perform system call filtering at runtime to increase security of programs | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

Xorg session users will want X enabled.

### Emerge

`root #``emerge --ask sys-apps/flatpak`
### Add flathub repository

## Configuration

### Files

- /var/lib/flatpak — global flatpak state (system-wide installed apps and repos)
- $HOME/.local/share/flatpak — per-user flatpak state (locally installed apps and repos)
- $HOME/.var/app/ — per application state (configuration files and cache)

### Permissions

In some instances, it may be necessary to edit the sandbox permissions of a flatpak application. The most convenient way of doing this is via the GUI tool Flatseal.

`user $``flatpak install com.github.tchx84.Flatseal`
## Basic usage

To install an application, e.g. Thunderbird, run:

`user $``flatpak search Thunderbird`
Get the **Application ID**: *org.mozilla.Thunderbird* and install the application:

`user $``flatpak --user install org.mozilla.Thunderbird`
To run the application, use created .desktop file or run:

`user $``flatpak run org.mozilla.Thunderbird`
To update installed applications and runtimes:

`user $``flatpak update`
To remove the application:

`user $``flatpak uninstall org.mozilla.Thunderbird`
## Theming

Flatpak documentiation offers [a good guide](https://docs.flatpak.org/en/latest/desktop-integration.html) about desktop integration and theming.

### GTK

Flatpak applications don't follow the system's GTK theme by default. First find out what's the current GTK theme, e.g. Materia-dark-compact, and then install it for Flatpak applications to use. [\[1\]](https://wiki.gentoo.org#cite_note-1)

`user $``gsettings get org.gnome.desktop.interface gtk-theme``user $``flatpak install flathub org.gtk.Gtk3theme.Materia-dark-compact`
## Desktop integration for Wayland

When using WMs such as [Sway](https://wiki.gentoo.org/wiki/Sway), installing an [xdg-desktop-portal](https://wiki.gentoo.org/wiki/Xdg-desktop-portal) implementation is needed for full integration. Available implementations include:

- [GNOME](https://wiki.gentoo.org/wiki/GNOME) backend: [sys-apps/xdg-desktop-portal-gnome](https://packages.gentoo.org/packages/sys-apps/xdg-desktop-portal-gnome)
- [GTK](https://wiki.gentoo.org/wiki/GTK) backend: [sys-apps/xdg-desktop-portal-gtk](https://packages.gentoo.org/packages/sys-apps/xdg-desktop-portal-gtk)
- [KDE](https://wiki.gentoo.org/wiki/KDE) backend: [kde-plasma/xdg-desktop-portal-kde](https://packages.gentoo.org/packages/kde-plasma/xdg-desktop-portal-kde) (in development)
- [Wayland](https://wiki.gentoo.org/wiki/Wayland)/wlroots backend: [gui-libs/xdg-desktop-portal-wlr](https://packages.gentoo.org/packages/gui-libs/xdg-desktop-portal-wlr) (in development)
- [LXQt](https://wiki.gentoo.org/wiki/LXQt) backend [gui-libs/xdg-desktop-portal-lxqt](https://packages.gentoo.org/packages/gui-libs/xdg-desktop-portal-lxqt) (in development)
- Flatpak backend: 'flatpak-portal' (included in the [sys-apps/flatpak](https://packages.gentoo.org/packages/sys-apps/flatpak) package)

Please note that these are separate entities that do not substitute each other and some of them may not be run at the same time as some of the others.

### Installation

First, emerge [sys-apps/xdg-desktop-portal](https://packages.gentoo.org/packages/sys-apps/xdg-desktop-portal):

`root #``emerge --ask sys-apps/xdg-desktop-portal`
Then emerge any needed backends:

`root #``emerge --ask sys-apps/xdg-desktop-portal-gtk gui-libs/xdg-desktop-portal-wlr gui-libs/xdg-desktop-portal-lxqt sys-apps/xdg-desktop-portal-gnome`
### Ensuring portals are running

Please note that sometimes these libraries aren't pulled automatically by the OS and need to be run by the user, for example they can be pulled in [Sway](https://wiki.gentoo.org/wiki/Sway) configuration:

**`~/.config/sway/config`**

**Running xdg portals**

```
exec /usr/libexec/xdg-desktop-portal-gtk -r
exec /usr/libexec/xdg-desktop-portal-wlr -r
exec /usr/libexec/flatpak-portal -r
exec "sh -c 'sleep 5;exec /usr/libexec/xdg-desktop-portal -r'"
```
## Troubleshooting

### Installed applications' desktop entries do not show in launchers

It is important to reboot the system after first installing Flatpak. Otherwise, installed applications' desktop entries may not show in launchers.

### After updating nvidia-drivers 3D applications crash or become slow

Make sure to update the flatpak nvidia platform.

`user $``flatpak update`
### Flatpaked GTK apps under Wayland and jagged fonts

Some users [report jagged fonts](https://github.com/flatpak/flatpak/issues/2861) on Wayland. This happens because if GTK apps can't detect whether they should perform font antialiasing, they disable ones by default. It obtain info ether from the system or via `xdg-desktop-portal-gtk` if flatpaked. It also requires setting up the proper wayland scheme for it from `gnome-base/gsettings-desktop-schemas`, but that package already in list of flatpak dependencies.

So a workaround is to install `xdg-desktop-portal-gtk` and reboot/restart the desktop:

`root #``emerge --ask sys-apps/xdg-desktop-portal-gtk`
To make sure if it is launched, see **"Ensuring portals are running"** topic above.

Since in early 2022 GTK wayland schemas are moved from `gnome-base/gnome-settings-daemon` to `gnome-base/gsettings-desktop-schemas`, the gnome settings daemon is no more required and can be uninstalled.

### Certain flatpak applications failing to access proper cursor

Some flatpaks such as `com.discordapp.Discord` or `com.spotify.Client` have an issue where they cannot find the systems cursor, and so default to the ugly default cursor that is used when no proper replacement is found.

A solution to this is to copy the systems icon directory to a location in the users home directory. In this example `~/.local/share/icons` will be used:

`user $``cp -r /usr/share/icons ~/.local/share/`
Next, use `flatpak-override` to give the flatpak in question (com.discordapp.Discord in this example) access to the home directory in which the cursors are inside:

`root #``flatpak override --filesystem=home com.discordapp.Discord`
To remove the filesystem override, run:

`user $``flatpak override --nofilesystem=home com.discordapp.Discord`
After the filesystem override is set, the `XCURSOR_PATH` and `XCURSOR_THEME` variables must be set, where `XCURSOR_PATH` is the path to the theme and `XCURSOR_PATH` is the name of the theme like so:

`user $``flatpak override --env=XCURSOR_PATH=/home/$USER/.local/share/icons com.discordapp.Discord``user $``flatpak override --env=XCURSOR_THEME=Adwaita-dark com.discordapp.Discord`
Finally, run the flatpak to see the applied changes:

`user $``flatpak run com.discordapp.Discord`
### File Chooser or similar Dialogues not opening

File Chooser, App Chooser, Email, Print, or Notification dialogues (and more) are provided by an XDG Desktop Portal, as per [Desktop Integration for Wayland](https://wiki.gentoo.org/wiki/Flatpak#Desktop_integration_for_Wayland).
Check also whether your `XDG_CURRENT_DESKTOP` environment variable corresponds to the `UseIn` attribute for your XDG Desktop Portal.
Flatpak's logic for this has been changed[\[1\]](https://github.com/flatpak/flatpak/commit/8ca4addc7352df428ee1888632f63083ba862dc2) to mimic that of *xdg-desktop-portal* more closely and thus **requires** the environment variable to be set, otherwise matching interfaces will be ignored even if there is only one implementation.

For instance *xdg-desktop-portal-gtk* has its `UseIn` defined in `/usr/share/xdg-desktop-portal/portals/gtk.portal` as `UseIn=gnome`.
Therefore your `XDG_CURRENT_DESKTOP` environment variable should be set to `gnome` if not automatically done so by your desktop environment (users without DE may need to set this in `~/.xinitrc` or another appropriate location) for the GTK portal to be used as a file chooser (and similar).



### Searching for any package returns "No matches found"

See "Add flathub repository" section of this documentation

### flatpak: /usr/lib64/libxmlb.so.2: no version information available (required by /usr/lib64/libappstream.so.5)

Issue:

`root #``emerge --oneshot dev-libs/libxmlb dev-libs/appstream`
### ebuild selected to satisfy has unmet requirements.

This is caused by setting [dracut](https://packages.gentoo.org/useflags/dracut) [as a global USE flag rather than in /etc/portage/package.use for](https://wiki.gentoo.org/wiki/USE_flag) [sys-kernel/installkernel](https://packages.gentoo.org/packages/sys-kernel/installkernel).

Fix by removing [dracut](https://packages.gentoo.org/useflags/dracut) [from /etc/portage/make.conf and following](https://wiki.gentoo.org/wiki/USE_flag) [Handbook:AMD64/Installation/Kernel#Initramfs](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Kernel#Initramfs)

## See also

- [Docker](https://wiki.gentoo.org/wiki/Docker) — a [container](<https://en.wikipedia.org/wiki/Container_(virtualization)>)-based [virtualization](https://wiki.gentoo.org/wiki/Virtualization) system
- [LXD](https://wiki.gentoo.org/wiki/LXD) — a system container manager
- [systemd/systemd-nspawn](https://wiki.gentoo.org/wiki/Systemd/systemd-nspawn) — a lightweight, loosely [chroot](https://wiki.gentoo.org/wiki/Chroot)-like, OS-level [OCI container](https://opencontainers.org/) environment native to [systemd](https://wiki.gentoo.org/wiki/Systemd).
