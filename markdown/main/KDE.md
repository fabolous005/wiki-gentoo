<!-- source: https://wiki.gentoo.org/wiki/KDE | group: Gentoo Wiki (Main) | wiki-title: KDE -->
---
title: KDE
url: https://wiki.gentoo.org/wiki/KDE
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-10"
categories: ['kde-apps', 'kde-misc']
fingerprint: ab1ba24909a3b18c
license: CC BY-SA 4.0
---

# KDE

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**KDE** is a free software community, producing a wide range of applications including the popular Plasma desktop environment.

Gentoo support for the KDE project is excellent, with comprehensive packaging of KDE Frameworks, Plasma, and Applications, as well as a wide array of other miscellaneous KDE-based software.

## Prerequisites

### Profile

Choosing an appropriate [profile](<https://wiki.gentoo.org/wiki/Profile_(Portage)>), although not required, is recommended as it sets a number of global and package-specific USE flags to ease installation and ensure a smooth KDE experience.

In order to choose the most suitable profile, first list what's available:

`root #``eselect profile list`
...
  \[21\]  default/linux/amd64/23.0 (stable)
  \[22\]  default/linux/amd64/23.0/systemd (stable)
  \[23\]  default/linux/amd64/23.0/desktop (stable)
  \[24\]  default/linux/amd64/23.0/desktop/systemd (stable)
  \[25\]  default/linux/amd64/23.0/desktop/gnome (stable)
  \[26\]  default/linux/amd64/23.0/desktop/gnome/systemd (stable)
  \[27\]  default/linux/amd64/23.0/desktop/plasma (stable)
  \[28\]  default/linux/amd64/23.0/desktop/plasma/systemd (stable)
  ...

Then, select the right profile, substituting `X` with the appropriate profile number:

`root #``eselect profile set X`
For Plasma desktop environment choose `desktop/plasma` with [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) or `desktop/plasma/systemd` with [systemd](https://wiki.gentoo.org/wiki/Systemd). Note that other USE flag combinations than set by the profile may technically be possible (especially if selected applications are run instead of a full KDE Plasma desktop environment), but may be unsupported, untested, or lead to unexpected loss of functionality.

#### Combined hardened profiles

Users that run hardened profiles can also combine it with all the features of the plasma desktop profile. For steps on doing this please follow [KDE/Hardened KDE Plasma profile](https://wiki.gentoo.org/wiki/KDE/Hardened_KDE_Plasma_profile).

### Services

KDE Plasma as well as many applications (not just limited to KDE) rely on proper setup of various system services. Default choices of these services will be pulled in automatically - by the installation steps in the following chapters. For deviating from the defaults, it is recommended to install them in advance of KDE Plasma or KDE Gear via emerge --oneshot so that [Portage](https://wiki.gentoo.org/wiki/Portage) can take them into account. Follow the links for information how to set up these services.

- [elogind](https://wiki.gentoo.org/wiki/Elogind) (with desktop/plasma profile) or [systemd](https://wiki.gentoo.org/wiki/Systemd) (with desktop/plasma/systemd profile) for session tracking.
- [udev](https://wiki.gentoo.org/wiki/Udev) for device management.
- [D-Bus](https://wiki.gentoo.org/wiki/D-Bus) for interprocess communication.
- [polkit](https://wiki.gentoo.org/wiki/Polkit) for controlling privileges for system-wide services.
- [udisks](https://wiki.gentoo.org/wiki/Udisks) for some storage related services.
- [PipeWire](https://wiki.gentoo.org/wiki/PipeWire) for audio and video handling - it serves as default sound server for [PulseAudio](https://wiki.gentoo.org/wiki/PulseAudio) and is used for screensharing and window previews.

## Plasma

Plasma 6 is the current generation of KDE's desktop environment, based on Qt 6 and KDE Frameworks 6, and using Wayland (exclusively, starting with Plasma 6.8).

Make sure to have configured applicable `VIDEO_CARDS` USE expand settings and kernel with DRMs (Direct Rendering Manager) enabled for Mesa. KWin, the window manager and Wayland compositor, uniquely falls back to low performance software Rendering if unsatisfied.

### Available versions

| KDE | Gentoo | Ebuild repository | Status | 
|---|---|---|---|
| KDE Plasma 6.6.6 | kde-plasma/plasma-meta-6.6.6 | gentoo | Stable for **amd64** and **arm64**; testing for **loong**, **ppc64**, **riscv** and **x86** | 
| KDE Plasma 6.7.5 | kde-plasma/plasma-meta-6.7.5 | gentoo | Stable for **amd64** and **arm64**; testing for **loong**, **ppc64**, **riscv** and **x86** | 
| KDE Plasma 6.8 beta 2 | kde-plasma/plasma-meta-6.7.91 | [KDE](https://wiki.gentoo.org/wiki/KDE/Ebuild_repository) | Masked; testing for **amd64**, **arm64**, **ppc64**, **riscv** and **x86** | 
| KDE Plasma 6.7 stable branch | kde-plasma/plasma-meta-6.7.49.9999 | [KDE](https://wiki.gentoo.org/wiki/KDE/Ebuild_repository) | Live version | 
| KDE Plasma 6.8 stable branch | kde-plasma/plasma-meta-6.8.49.9999 | [KDE](https://wiki.gentoo.org/wiki/KDE/Ebuild_repository) | Live version | 
| KDE Plasma 6 master branch | kde-plasma/plasma-meta-9999 | [KDE](https://wiki.gentoo.org/wiki/KDE/Ebuild_repository) | Live version | 

### Installation

#### USE flags

The [kde-plasma/plasma-meta](https://packages.gentoo.org/packages/kde-plasma/plasma-meta) package provides the full Plasma desktop, configurable by a wealth of USE flags:


### USE flags for
            [kde-plasma/plasma-meta](https://packages.gentoo.org/packages/kde-plasma/plasma-meta)
            
            Merge this to pull in all Plasma 6 packages

| [+browser-integration](https://packages.gentoo.org/useflags/+browser-integration) | Enable integration with Chrome/Firefox with browser extensions | 
| [+crash-handler](https://packages.gentoo.org/useflags/+crash-handler) | Pull in kde-plasma/drkonqi for assisted upstream crash reports | 
| [+display-manager](https://packages.gentoo.org/useflags/+display-manager) | Pull in a graphical display manager | 
| [+elogind](https://packages.gentoo.org/useflags/+elogind) | Enable session tracking via sys-auth/elogind | 
| [+firewall](https://packages.gentoo.org/useflags/+firewall) | Pull in kde-plasma/plasma-firewall for system firewall administration | 
| [+kwallet](https://packages.gentoo.org/useflags/+kwallet) | Enable support for KWallet auto-unlocking via kde-plasma/kwallet-pam | 
| [+networkmanager](https://packages.gentoo.org/useflags/+networkmanager) | Enable net-misc/networkmanager support | 
| [+sddm](https://packages.gentoo.org/useflags/+sddm) | Pull in the x11-misc/sddm display manager and system settings module | 
| [+smart](https://packages.gentoo.org/useflags/+smart) | Pull in kde-plasma/plasma-disks for disk health monitoring | 
| [+wallpapers](https://packages.gentoo.org/useflags/+wallpapers) | Install wallpapers for the Plasma Workspace | 
| [+xwayland](https://packages.gentoo.org/useflags/+xwayland) | Enable Wayland windows screensharing to XWayland applications via gui-apps/xwaylandvideobridge | 
| [X](https://packages.gentoo.org/useflags/X) | Enable X window manager support via kde-plasma/kwin-x11 (only until and including Plasma 6.7) | 
| [accessibility](https://packages.gentoo.org/useflags/accessibility) | Add support for accessibility (eg 'at-spi' library) | 
| [bluetooth](https://packages.gentoo.org/useflags/bluetooth) | Enable Bluetooth Support | 
| [crypt](https://packages.gentoo.org/useflags/crypt) | Pull in kde-plasma/plasma-vault for encrypted vaults integration | 
| [cups](https://packages.gentoo.org/useflags/cups) | Add support for CUPS (Common Unix Printing System) | 
| [discover](https://packages.gentoo.org/useflags/discover) | Pull in resources management GUI; a centralised GHNS alternative and optional sys-apps/fwupd frontend | 
| [flatpak](https://packages.gentoo.org/useflags/flatpak) | Pull in kde-plasma/flatpak-kcm for flatpak permissions administration | 
| [grub](https://packages.gentoo.org/useflags/grub) | Pull in Breeze theme for sys-boot/grub | 
| [gtk](https://packages.gentoo.org/useflags/gtk) | Enable Breeze widget style and system settings module for GTK+ | 
| [ocr](https://packages.gentoo.org/useflags/ocr) | Enable Optical Character Recognition support via app-text/tesseract | 
| [oxygen-theme](https://packages.gentoo.org/useflags/oxygen-theme) | Pull in Oxygen icons, sound theme and visual style for KDE Plasma | 
| [plymouth](https://packages.gentoo.org/useflags/plymouth) | Pull in Breeze theme for sys-boot/plymouth | 
| [pulseaudio](https://packages.gentoo.org/useflags/pulseaudio) | Install Plasma applet for PulseAudio volume management | 
| [rdp](https://packages.gentoo.org/useflags/rdp) | Enables RDP/Remote Desktop support | 
| [sdk](https://packages.gentoo.org/useflags/sdk) | Pull in kde-plasma/plasma-sdk for Plasma development | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [thunderbolt](https://packages.gentoo.org/useflags/thunderbolt) | Pull in kde-plasma/plasma-thunderbolt control center module | 
| [unsupported](https://packages.gentoo.org/useflags/unsupported) | Allow packages that are known to ruin runtime experience \*\* DO NOT FILE BUGS WITH THIS ENABLED \*\* | 
| [virtualkeyboard](https://packages.gentoo.org/useflags/virtualkeyboard) | Pull in kde-plasma/plasma-keyboard | 
| [wacom](https://packages.gentoo.org/useflags/wacom) | Pull in kde-plasma/wacomtablet control center module | 
| [webengine](https://packages.gentoo.org/useflags/webengine) | Use kde-apps/khelpcenter to access the locally installed KDE Help System Handbook | 

#### Emerge

`root #``emerge --ask kde-plasma/plasma-meta`
Alternatively, [kde-plasma/plasma-desktop](https://packages.gentoo.org/packages/kde-plasma/plasma-desktop) provides a very basic desktop, leaving users free to install only the extra packages they require - or rather, figure out missing features on their own.

### Starting Plasma

#### Display manager

[SDDM](https://wiki.gentoo.org/wiki/SDDM) (Simple Desktop Display Manager) is the recommended login manager and is pulled in automatically via [kde-plasma/plasma-meta](https://packages.gentoo.org/packages/kde-plasma/plasma-meta) by default. This is the preferred option. Alternatively, [LightDM](https://wiki.gentoo.org/wiki/LightDM) can be used and pulled in by setting USE flag `-sddm` for [kde-plasma/plasma-meta](https://packages.gentoo.org/packages/kde-plasma/plasma-meta). Change the setting accordingly in /etc/conf.d/display-manager. Also, be sure to read through the [SDDM](https://wiki.gentoo.org/wiki/SDDM) page if further issues appear.

Wayland is the default session.


### USE flags for
            [kde-plasma/plasma-login-sessions](https://packages.gentoo.org/packages/kde-plasma/plasma-login-sessions)
            
            KDE Plasma login sessions

Users can unset [X](https://packages.gentoo.org/useflags/X) [or](https://wiki.gentoo.org/wiki/USE_flag) [wayland](https://packages.gentoo.org/useflags/wayland) [on this package if they wish to control available login sessions. This will go away in Plasma 6.8 which will be supporting Wayland only.](https://wiki.gentoo.org/wiki/USE_flag)

#### No display manager

Plasma can be launched with dbus-run-session startplasma-wayland.

This can be added to a user's profile file which will be executed when logging in:

**`~/.profile`**

```
#!/bin/sh
dbus-run-session startplasma-wayland
```
#### X server

Read and follow the instructions in the [X server](https://wiki.gentoo.org/wiki/X_server) article to setup the X environment.

With X, Plasma can be started the old-fashioned way with startx, but extra care needs to be taken to ensure it gets a valid session.

**`~/.xinitrc`**

```
#!/bin/sh
exec dbus-launch --exit-with-session startplasma-x11
```
### KWallet

Many users will be introduced to [kde-frameworks/kwallet](https://packages.gentoo.org/packages/kde-frameworks/kwallet), Plasma's encrypted password storage, while adding a (wireless) network connection after login or adding E-Mail accounts in [kde-apps/kmail](https://packages.gentoo.org/packages/kde-apps/kmail).

For managing KWallets, importing and exporting passwords, there is [kde-apps/kwalletmanager](https://packages.gentoo.org/packages/kde-apps/kwalletmanager):

`root #``emerge --ask kde-apps/kwalletmanager`
#### KWallet auto-unlocking

[kde-plasma/kwallet-pam](https://packages.gentoo.org/packages/kde-plasma/kwallet-pam) provides a mechanism to avoid being subsequently asked for access to kwallet after login.

`root #``emerge --ask kde-plasma/kwallet-pam`
It requires the following setup:

- For KWallet security, use classic blowfish encryption instead of GPG
- Choose same password for login and kwallet
- Configure a display manager with support for PAM - both [x11-misc/sddm](https://packages.gentoo.org/packages/x11-misc/sddm) and [x11-misc/lightdm](https://packages.gentoo.org/packages/x11-misc/lightdm) fulfill that requirement:

**`/etc/pam.d/sddm`**

**Config lines for KWallet PAM unlocking via SDDM**

```
-auth           optional        pam_kwallet5.so
-session        optional        pam_kwallet5.so auto_start
```
For unlocking on tty login (no display manager, or like [gui-apps/tuigreet](https://packages.gentoo.org/packages/gui-apps/tuigreet)), edit /etc/pam.d/login accordingly. The user will need to specify the **force\_run** parameter.

**`/etc/pam.d/greetd`**

**Config lines for KWallet PAM unlocking via Greetd**

```
-auth           optional        pam_kwallet5.so
-session        optional        pam_kwallet5.so auto_start force_run
```
#### Disabling KWallet

To disable the KWallet subsystem completely, edit the following file:

**`~/.config/kwalletrc`**

```
[Wallet]
Enabled=false
```
### SSH/GPG Agent startup/shutdown scripts

ssh-agent scripts are located in /etc/xdg/plasma-workspace/env and /etc/xdg/plasma-workspace/shutdown. Shutdown scripts require executable bit set because they are not sourced. The [Keychain](https://wiki.gentoo.org/wiki/Keychain) article provides more information about this.

### Non-root user authentication for dialogs

Some KDE dialogs such as printers, adding wireless networks and adding users require administrator authentication. This is handled through [sys-auth/polkit](https://packages.gentoo.org/packages/sys-auth/polkit) and operates independently from [app-admin/sudo](https://packages.gentoo.org/packages/app-admin/sudo). By default in Gentoo, the root account is the only administrator, and so even if a user account can run root commands through sudo, authentication in these KDE dialogs will fail.

Adding wireless networks using [net-misc/networkmanager](https://packages.gentoo.org/packages/net-misc/networkmanager) is allowed by a polkit rule which is part of the Gentoo package and already allows access for every user in the group `plugdev`. For other dialogs the behavior must be configured manually: If all users of the group `wheel` are required to be administrators, create a copy of /usr/share/polkit-1/rules.d/50-default.rules starting with a number lower than 50, and edit the line return \["unix-user:0"\] to the following:

**`/etc/polkit-1/rules.d/49-wheel.rules`**

**Administrator wheel group**

```
polkit.addAdminRule(function(action, subject) {
    return ["unix-group:wheel"];
});
```
The [Polkit](https://wiki.gentoo.org/wiki/Polkit) wiki page provides more details on rules configuration.

### Files

XDG standard directories are being used for KDE Plasma and KDE applications:

- $XDG\_CONFIG\_HOME (defaults to $HOME/.config) - Configuration files
- $XDG\_DATA\_HOME (defaults to $HOME/.local/share) - Application data

## Applications

KDE Gear consists of various applications and supporting libraries based on Qt/KDE Frameworks.

### Available versions

| KDE | Gentoo | Ebuild repository | Status | 
|---|---|---|---|
| KDE Gear 26.04.3 | kde-apps/kde-apps-meta-26.04.3 | gentoo | Stable for **amd64** and **arm64**; testing for **x86** | 
| KDE Gear 26.08.2 | kde-apps/kde-apps-meta-26.08.2 | gentoo | Testing for **amd64**, **arm64** and **x86** | 
| KDE Gear 26.08 stable branch | kde-apps/kde-apps-meta-26.08.49.9999 | [KDE](https://wiki.gentoo.org/wiki/KDE/Ebuild_repository) | Live version | 
| KDE Gear master branch | kde-apps/kde-apps-meta-9999 | [KDE](https://wiki.gentoo.org/wiki/KDE/Ebuild_repository) | Live version | 

KDE Gear is divided in the following meta packages:

| Package name | Description | 
|---|---|
| [kde-apps/kdeaccessibility-meta](https://packages.gentoo.org/packages/kde-apps/kdeaccessibility-meta) | Accessibility applications and utilities. | 
| [kde-apps/kdeadmin-meta](https://packages.gentoo.org/packages/kde-apps/kdeadmin-meta) | Administrative utilities, which help in managing the system. | 
| [kde-apps/kdecore-meta](https://packages.gentoo.org/packages/kde-apps/kdecore-meta) | Basic applications such as file browser, editor, terminal emulator. | 
| [kde-apps/kdeedu-meta](https://packages.gentoo.org/packages/kde-apps/kdeedu-meta) | Educational applications and games. | 
| [kde-apps/kdegames-meta](https://packages.gentoo.org/packages/kde-apps/kdegames-meta) | Standard desktop games. | 
| [kde-apps/kdegraphics-meta](https://packages.gentoo.org/packages/kde-apps/kdegraphics-meta) | Graphics applications such as image viewers, color pickers, etc. | 
| [kde-apps/kdemultimedia-meta](https://packages.gentoo.org/packages/kde-apps/kdemultimedia-meta) | Audio and video playback applications and services. | 
| [kde-apps/kdenetwork-meta](https://packages.gentoo.org/packages/kde-apps/kdenetwork-meta) | Network applications and VNC services. | 
| [kde-apps/kdepim-meta](https://packages.gentoo.org/packages/kde-apps/kdepim-meta) | PIM applications such as emailer, addressbook, organizer, etc. | 
| [kde-apps/kdesdk-meta](https://packages.gentoo.org/packages/kde-apps/kdesdk-meta) | Various development tools. | 
| [kde-apps/kdeutils-meta](https://packages.gentoo.org/packages/kde-apps/kdeutils-meta) | Standard desktop utilities such as an archiver, a calculator, etc. | 

### Installation

The [kde-apps/kde-apps-meta](https://packages.gentoo.org/packages/kde-apps/kde-apps-meta) package provides the full KDE Gear bundle:

`root #``emerge --ask kde-apps/kde-apps-meta`
If not all the packages are required, one or several smaller meta packages from the list above may be picked instead. Alternatively, it is possible to set [USE flags](https://wiki.gentoo.org/wiki/USE_flag) to reduce the number of applications installed by [kde-apps/kde-apps-meta](https://packages.gentoo.org/packages/kde-apps/kde-apps-meta).

### Localization

Plasma and Applications are shipping their [localization](https://wiki.gentoo.org/wiki/Localization) per-package. Enable desired localization in System Settings.

### KDE PIM

KDE PIM is a whole suite of applications to manage personal information including mail, calendar, contacts and more. It has several optional runtime dependencies to extend its functionality:

- Virus detection: [app-antivirus/clamav](https://packages.gentoo.org/packages/app-antivirus/clamav)
- Spam filtering: [mail-filter/bogofilter](https://packages.gentoo.org/packages/mail-filter/bogofilter) or [mail-filter/spamassassin](https://packages.gentoo.org/packages/mail-filter/spamassassin)

### Run GUI applications with root privileges

[kde-plasma/kdesu-gui](https://packages.gentoo.org/packages/kde-plasma/kdesu-gui) is a utility to start graphical programs with root privileges.

`root #``emerge --ask kde-plasma/kdesu-gui`
It can be used by invoking kdesu either from KRunner or a terminal emulator:

`user $``kdesu <program-name>`
A message dialog will be displayed prompting for the root password.

## Frameworks

KDE Frameworks is a collection of libraries and software frameworks that provide the foundation for KDE Plasma and KDE Gear (applications), but may be leveraged by any Qt application.

As Frameworks are mostly libraries and provide little user functionality, it's not necessary to install them manually - the required packages will be pulled in automatically as dependencies.

### Available versions

| KDE | Gentoo | Ebuild repository | Status | 
|---|---|---|---|
| KDE Frameworks 6.29.0 | kde-frameworks/\*-6.29.0 | gentoo | Stable for **amd64**, **arm64** and **ppc64**; testing for **loong**, **riscv** and **x86** | 
| KDE Frameworks 6.30.0 | kde-frameworks/\*-6.30.0 | gentoo | Testing for **amd64**, **arm64**, **loong**, **ppc64**, **riscv** and **x86** | 
| KDE Frameworks 6.31.0 | kde-frameworks/\*-6.31.0 | gentoo | Testing for **amd64**, **arm64**, **loong**, **ppc64**, **riscv** and **x86** | 
| KDE Frameworks 6 (master) branch | kde-frameworks/\*-9999 | [KDE](https://wiki.gentoo.org/wiki/KDE/Ebuild_repository) | Live version | 

## More KDE software

The most important KDE applications are in the Gentoo ebuild repository and many are located in the [kde-apps](https://packages.gentoo.org/category/kde-apps) and [kde-misc](https://packages.gentoo.org/category/kde-misc) categories.

## Removal

Before attempting to remove KDE Plasma or KDE Gear, first of all get an overview of currently non-registered (or not being depended on) packages in @world. These packages would be part of the cleanup being performed in the follow-up steps.

`root #``emerge --depclean -p`  
Carefully read through the list of packages to be removed before continuing.

The next step is to unmerge any respective KDE related meta packages registered in @world. This will not yet remove any files from the installation, so the desktop environment and/or applications will keep running. Examples:

`root #``emerge --ask --depclean --verbose kde-plasma/plasma-meta``root #``emerge --ask --depclean --verbose kde-apps/kde-apps-meta`
In a next step it can be useful to scan /etc/portage directory for any KDE specific entries in package.mask, package.unmask and package.accept\_keywords and clean them up.

Finally, run the command to uninstall any obsolete packages and their dependencies. If Plasma is about to be gone, it would make sense to quit any running Plasma session beforehand.

`root #``emerge --ask --depclean --verbose`  
## Troubleshooting

Refer to the [Troubleshooting](https://wiki.gentoo.org/wiki/KDE/Troubleshooting) sub-article.

## See also

- [KDE/Ebuild repository](https://wiki.gentoo.org/wiki/KDE/Ebuild_repository) — provides instructions on adding Gentoo's KDE ebuild development repository to a system.
- [kde-sunset ebuild repository](https://wiki.gentoo.org/wiki/Overlay:Kde-sunset) - For old KDE software that has been removed from the main ebuild repository.
- [Desktop environment](https://wiki.gentoo.org/wiki/Desktop_environment) — provides a list of desktop environments available in Gentoo.
