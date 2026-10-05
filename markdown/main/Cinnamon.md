<!-- source: https://wiki.gentoo.org/wiki/Cinnamon | group: Gentoo Wiki (Main) | wiki-title: Cinnamon -->
---
title: Cinnamon
url: https://wiki.gentoo.org/wiki/Cinnamon
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-21"
fingerprint: fa927b77a235818c
license: CC BY-SA 4.0
---

# Cinnamon

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

**Cinnamon** is a contemporary [desktop environment](https://wiki.gentoo.org/wiki/Desktop_environment) with a traditional graphical interface. GNOME 2, [LXDE](https://wiki.gentoo.org/wiki/LXDE), or [Xfce](https://wiki.gentoo.org/wiki/Xfce) users will find it familiar. Cinnamon has very good graphical and functional features. Forked from GNOME 3's GNOME Shell and developed for [Linux Mint](https://linuxmint.com/), it is available on Gentoo for the **amd64** and **x86** architectures. Wayland support is experimental.[\[1\]](https://wiki.gentoo.org#cite_note-1)

## Installation

### Prerequisites

Cinnamon works great both with and without systemd. To install without systemd dependencies, use eselect to switch to a profile without Gnome, such as `default/linux/amd64/23.0/desktop`. To install with systemd, use a Gnome-enabled profile such as `default/linux/amd64/23.0/desktop/gnome/systemd`. The architecture and version can be different in both cases.

To get a list of available system profiles:

`root #``eselect profile list`
...
  \[21\]  default/linux/amd64/23.0 (stable) \*
  \[22\]  default/linux/amd64/23.0/systemd (stable)
  \[23\]  default/linux/amd64/23.0/desktop (stable)
  \[24\]  default/linux/amd64/23.0/desktop/systemd (stable)
  \[25\]  default/linux/amd64/23.0/desktop/gnome (stable)
  \[26\]  default/linux/amd64/23.0/desktop/gnome/systemd (stable)
...

#### OpenRC

When using OpenRC, set the right system profile:

`root #``eselect profile set default/linux/amd64/23.0/desktop`
It is also possible to use the associated profile number according to the profiles list above:

`root #``eselect profile set 20`
Read up on the instructions available in the [Gentoo without systemd](https://wiki.gentoo.org/wiki/Gentoo_without_systemd) article.

#### systemd

For Cinnamon with systemd support select the GNOME desktop profile ending in systemd:

`root #``eselect profile set default/linux/amd64/23.0/desktop/systemd`
#### Combined hardened profiles

Users that run hardened profiles can also combine it with all the features of the desktop profile. For steps on doing this please follow [Hardened\_Desktop\_Profiles](https://wiki.gentoo.org/wiki/Hardened_Desktop_Profiles).

#### Xorg

Of course for Cinnamon to work [Xorg must be installed and configured](https://wiki.gentoo.org/wiki/Xorg/Guide). Make sure an X server is set up before proceeding with this article. If needed, test Xorg as detailed in the [Using startx](https://wiki.gentoo.org/wiki/Xorg/Guide#Using_startx) section of the Xorg Gentoo guide.

#### Internationalization

Cinnamon (and other applications) are internationalized, supporting a number of languages (Linguas). If this was not done during the initial Gentoo installation, add the appropriate language value to the `LINGUAS` variable in /etc/portage/package.use:

**`/etc/portage/package.use/00localization`**

```
 LINGUAS: en
```
### USE flags


| [+eds](https://packages.gentoo.org/useflags/+eds) | Enable support for Evolution-Data-Server (EDS) | 
| [+gstreamer](https://packages.gentoo.org/useflags/+gstreamer) | Add support for media-libs/gstreamer (Streaming media) | 
| [+networkmanager](https://packages.gentoo.org/useflags/+networkmanager) | Enable net-misc/networkmanager support | 
| [+nls](https://packages.gentoo.org/useflags/+nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [gtk-doc](https://packages.gentoo.org/useflags/gtk-doc) | Build and install gtk-doc based developer documentation for dev-util/devhelp, IDE and offline use | 
| [wayland](https://packages.gentoo.org/useflags/wayland) | Enable dev-libs/wayland backend | 

Find more information on Python targets by visiting the Python project's [PYTHON TARGETS](https://wiki.gentoo.org/wiki/Project:Python/PYTHON_TARGETS) article.

The `icu`, `pulseaudio`, and `gnome-keyring` USE flags are needed by Cinnamon's dependencies. If they are not set already, either allow emerge --ask gnome-extra/cinnamon to create or update package.use (or package.use/cinnamon, or set the USE flags manually using another method.

### Emerge

Finally, after performing all the steps above, ask Portage to emerge Cinnamon:

`root #``emerge --ask gnome-extra/cinnamon`
## System configuration

Before looking at the Cinnamon theming, configure the system to properly start Cinnamon at boot or upon user request, and enable the right privilege delegation settings so that end users can interact with the system in an intuitive manner.

### systemd services

dbus must be activated with systemd, use systemctl as detailed in the [systemd page](https://wiki.gentoo.org/wiki/Systemd#Enabling.2C_disabling.2C_starting.2C_and_stopping_services).

### OpenRC services

#### dbus, openrc-settingsd, elogind

Xorg server and Cinnamon need the dbus, openrc-settingsd and elogind services, make sure that they will be started at system boot time:

`root #````
rc-update add dbus default
```
`root #````
rc-update add openrc-settingsd default
```
`root #````
rc-update add elogind boot
```
Next, start the services (there is no need to restart the system):

`root #````
rc-service dbus start
```
`root #````
rc-service openrc-settingsd start
```
`root #````
rc-service elogind start
```
#### NetworkManager

[NetworkManager](https://wiki.gentoo.org/wiki/NetworkManager) is a Cinnamon dependency and is not needed if the network was configured during the initial Gentoo installation. It should not run concurrently with another network service. If NetworkManager is desired be sure to disable other network management programs or services before starting it. For example, if the system was originally configured to use Gentoo's netifrc scripts to manage the network connections, disable these network services:

`root #````
rc-service net.eth0 stop
```
`root #````
rc-update del net.eth0 default
```
After removing the other network services, add NetworkManager:

`root #````
rc-service NetworkManager start
```
`root #````
rc-update add NetworkManager default
```
#### Disabling NetworkManager applet

If NetworkManager is not used, the Network Manager applet will fail to start up, which will show up as a problem during startup.

To disable the applet, go to System Settings -> Preferences -> Applets, find "Network Manager" and click the minus button.

### Sudo

For an ordinary user to reboot or shutdown the system from Cinnamon, sudo is needed with some settings.

First install [app-admin/sudo](https://packages.gentoo.org/packages/app-admin/sudo):

`root #``emerge --ask app-admin/sudo`
Next modify the sudo configuration with the visudo command:

`root #``visudo`
Visudo runs the default text editor to generate or update a sudoers file. Replace `username` in the text below with the username of each user that will be using Cinnamon:

**`/etc/sudoers`**

```
  ALL=(root) NOPASSWD: /sbin/reboot
username  ALL=(root) NOPASSWD: /sbin/halt
username  ALL=(root) NOPASSWD: /sbin/poweroff
username  ALL=(root) NOPASSWD: /sbin/shutdown
```
Or for the wheel group:

**`/etc/sudoers`**

```
  ALL=(root) NOPASSWD: /sbin/reboot
%wheel  ALL=(root) NOPASSWD: /sbin/halt
%wheel  ALL=(root) NOPASSWD: /sbin/poweroff
%wheel  ALL=(root) NOPASSWD: /sbin/shutdown
```
### Polkit rules and actions

For an ordinary user to perform additional highly privileged actions from Cinnamon, policy kit must be configured. Use pkaction to get a list of actions; the names should be self-explanatory:

`root #``pkaction`
org.cinnamon.settings-daemon.plugins.power.backlight-helper
org.cinnamon.settings-users
org.cinnamon.settingsdaemon.datetimemechanism.configure
org.freedesktop.ModemManager1.Contacts
org.freedesktop.ModemManager1.Control
org.freedesktop.ModemManager1.Device.Control
org.freedesktop.ModemManager1.Firmware
org.freedesktop.ModemManager1.Location
org.freedesktop.ModemManager1.Messaging
org.freedesktop.ModemManager1.USSD
org.freedesktop.NetworkManager.enable-disable-network
org.freedesktop.NetworkManager.enable-disable-wifi
etc.

The simplest way is to authorize all actions by only testing wheel group membership. Put a JavaScript file with a .rules extension in /etc/polkit-1/rules.d, named for example 55-allowing-all-actions.rules:

**`/etc/polkit-1/rules.d/55-allowing-all-actions.rules`**

**Setting general polkit rule**

```
polkit.addRule (function (action, subject)
{
  if (subject.isInGroup ("wheel"))
  {
    return polkit.Result.YES;
  }
});
```
To only authorize some actions, the action names must be iterated. For a wheel group user to suspend, hibernate, shutdown and restart the system, make a 55-allowing-actions.rules file like so:

**`/etc/polkit-1/rules.d/55-allowing-actions.rules`**

**Setting polkit rules**

```
polkit.addRule (function (action, subject)
{
  if (action.id == "org.freedesktop.upower.hibernate" ||
      action.id == "org.freedesktop.upower.suspend" ||
      action.id == "org.freedesktop.consolekit.system.stop" ||
      action.id == "org.freedesktop.consolekit.system.restart" &&
      subject.isInGroup ("wheel"))
      {
        return polkit.Result.YES;
      }
});
```
Various actions can be added: those to change color profiles, use Nemo (Cinnamon files manager) as root, mount and eject media, set screen backlight, use network manager, change wallpaper, etc. Several `polkit.addRule (function (action, subject){ … });` blocks can be used next to each other as well, and several files can be added.

### Starting Cinnamon

When not using a login/display manager, make a .xinitrc file in the user's home directory, starting Cinnamon (D-bus will be launched too automatically):

**`~/.xinitrc`**

**Cinnamon launch directive**

```
exec cinnamon-session
```
Then to start Xorg and Cinnamon, execute startx after logging in:

`user $``startx`
With a display manager follow the [display manager](https://wiki.gentoo.org/wiki/Display_manager) instructions.

## Theming

### Mint-X icons

By default Cinnamon comes with Gnome icons. [Mint-X icons are on GitHub](https://github.com/linuxmint/mint-x-icons). Download, unzip, and put the Mint-X icon set in /usr/share/icons:

`root #``mv pathToUnzippedArchive/mint-x-icons-master/usr/share/icons/Mint-X /usr/share/icons/Mint-X`
Next, make a cache for it:

`root #``gtk-update-icon-cache /usr/share/icons/Mint-X`
The new icon set can be selected in Cinnamon Control Center. Mint-X icons are green, but other icon sets with different colors can be installed too, like Mint-X-Grey or Mint-X-Orange. Make sure to create a cache for these as well.

### Desktop icons text color

An icon's text color in Cinnamon desktop is black by default, which is not always readable. The instructions to change this color depends on the GTK 3 version that the system is using. Get the currently used version with:

`user $``emerge --info x11-libs/gtk+`
- In GTK versions less than 3.13.3, this setting can only be changed system-wide. Change the text color by adding CSS rules to the /usr/share/themes/Adwaita/gtk-3.0/gtk.css file (Adwaita is the default theme coming with GTK), after the `@import` line:

**`/usr/share/themes/Adwaita/gtk-3.0/gtk.css`**

**CSS rules for GTK \< 3.13.3**

```
@import url("resource:///org/gnome/adwaita/gtk-main.css");
 
/* added css rules */
.nemo-desktop.nemo-canvas-item {
  color: #fff;  /* #fff is white web color indication, #333 should be dark grey */
}
 
.nemo-desktop.nemo-canvas-item:selected {
  background-color: alpha (#fff, 0.2);  /* 0.2 is transparency level */
}
```
- The 2<sup>nd</sup> rule concerns the selected desktop icon and can be modified to get another background color or set the transparency.

- In more recent GTK versions (3.13.3 onward), this must be tuned through a user-specific configuration file. Adwaita is now included in GTK as the theme replacing Raleigh, so rules must be set in the user's home directories. The file to edit is \~/.config/gtk-3.0/gtk.css (create \~/.config/gtk-3.0/ directory if needed), and the change is now without the `@import` line. Additionally in this example, the left Nemo side bar is gray colored:

**`~/.config/gtk-3.0/gtk.css`**

**CSS rules for GTK ≥ 3.13.3**

```
.nemo-desktop.nemo-canvas-item {
  color: #fff;
}
 
.nemo-desktop.nemo-canvas-item:selected {
  background-color: alpha (#fff, 0.2);
}
 
NemoWindow .sidebar, NemoWindow .sidebar .view {
  background-color: #444;
  color: #eee; /* light grey */
}
```
### Mint-X themes

Cinnamon comes with a default cinnamon theme, obviously... The Mint-X themes are [available for download](https://github.com/linuxmint/mint-themes) on GitHub. Download them, unzip, and copy to /usr/share/themes if needed. They can be selected in the Cinnamon Control Center.

### Playing with themes

Almost all theme settings can be changed in the cinnamon.css file in the theme directory.

For example with the Cinnamon theme, which is not in /usr/share/themes but in /usr/share/cinnamon/theme, first copy the theme's directory to a user .themes sub-directory, named for example other-cinnamon, which will be the new theme name:

`root #````
mkdir /home/userName/.themes/other-cinnamon
```
`root #````
cp -R /usr/share/cinnamon/theme /home/userName/.themes/other-cinnamon/cinnamon
```
`root #````
chown -R userName:userName /home/userName/.themes/other-cinnamon
```
Edit \~/.themes/other-cinnamon/cinnamon/cinnamon.css. The results of the changes can be viewed immediately by selecting another theme in Cinnamon Control Center, then reselecting the theme being changed.

## Common applications

### Terminals

- [lxde-base/lxterminal](https://packages.gentoo.org/packages/lxde-base/lxterminal) - Requires no dependencies.
- [x11-terms/xfce4-terminal](https://packages.gentoo.org/packages/x11-terms/xfce4-terminal) - Requires some dependencies but is better featured than [lxde-base/lxterminal](https://packages.gentoo.org/packages/lxde-base/lxterminal).
- [x11-terms/gnome-terminal](https://packages.gentoo.org/packages/x11-terms/gnome-terminal) - Doesn't require systemd if `-nautilus` and `-gnome-shell` USE flags are set for this package.

### Gnome popular applications

A number of popular applications are available as well, built for Gnome and working flawlessly in Cinnamon:

- standard calculator ([gnome-extra/gnome-calculator](https://packages.gentoo.org/packages/gnome-extra/gnome-calculator))
- screen copy utility ([media-gfx/gnome-screenshot](https://packages.gentoo.org/packages/media-gfx/gnome-screenshot))
- image viewer ([media-gfx/eog](https://packages.gentoo.org/packages/media-gfx/eog))
- PDF reader ([app-text/evince](https://packages.gentoo.org/packages/app-text/evince))
- system monitor ([gnome-extra/gnome-system-monitor](https://packages.gentoo.org/packages/gnome-extra/gnome-system-monitor))
- archive manager ([app-arch/file-roller](https://packages.gentoo.org/packages/app-arch/file-roller))
- disk burning ([app-cdr/brasero](https://packages.gentoo.org/packages/app-cdr/brasero))
- text editor ([app-editors/gedit](https://packages.gentoo.org/packages/app-editors/gedit))



`root #``emerge --ask gnome-extra/gnome-calculator media-gfx/gnome-screenshot media-gfx/eog app-text/evince gnome-extra/gnome-system-monitor app-arch/file-roller app-cdr/brasero app-editors/gedit`
### Applications without Gnome

Also working flawlessly in Cinnamon:

- PDF readers [app-text/mupdf](https://packages.gentoo.org/packages/app-text/mupdf) and [app-text/qpdfview](https://packages.gentoo.org/packages/app-text/qpdfview)
- light text editor [app-editors/leafpad](https://packages.gentoo.org/packages/app-editors/leafpad)
- mp3 and video readers [media-video/mpv](https://packages.gentoo.org/packages/media-video/mpv) and [media-video/vlc](https://packages.gentoo.org/packages/media-video/vlc)


Compile [app-text/qpdfview](https://packages.gentoo.org/packages/app-text/qpdfview) and [media-video/vlc](https://packages.gentoo.org/packages/media-video/vlc) with `qt5` USE flags to get a GUI.

## Troubleshooting

If problems occur, try to get access to the live error messages. These can be displayed on a separate (virtual) terminal or in a log file. If the messages cannot be found, try starting Cinnamon from the command line rather than through a display manager. Alternatively have a look at /var/log/Xorg.0.log.

When the error or warning messages are not sufficient to troubleshoot Cinnamon, start it with the `--debug` parameter to get more detailed Xorg messages:

**`~/.xinitrc`**

**Cinnamon debug launch**

```
exec cinnamon-session --debug
```
If Cinnamon cannot launch, deleting (or renaming) .cinnamon in the home directory is not enough: other Cinnamon or Gnome items must be deleted (or renamed) too before X restarts. These can be found in the .cache, .config and .local XDG directories. .gnome2 can be deleted (or renamed) entirely.

### Refresh rate

It's possible to set custom refresh rate via \~/.config/monitors.xml file. Replace `<rate>60</rate>` with preferred setting.

## See also

- [Desktop environment](https://wiki.gentoo.org/wiki/Desktop_environment) — provides a list of desktop environments available in Gentoo.
- [Xfce](https://wiki.gentoo.org/wiki/Xfce)
- [MATE](https://wiki.gentoo.org/wiki/MATE) — a [fork](<https://en.wikipedia.org/wiki/Fork_(software_development)>) of the [GNOME 2](https://wiki.gentoo.org/wiki/GNOME) desktop environment designed to retain the look and feel of a 'traditional' desktop environment.
- [GNOME](https://wiki.gentoo.org/wiki/GNOME) — a feature-rich desktop environment provided by the [GNOME project](https://www.gnome.org).
- [KDE](https://wiki.gentoo.org/wiki/KDE) — a free software community, producing a wide range of applications including the popular Plasma desktop environment.
