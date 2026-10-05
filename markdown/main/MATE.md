<!-- source: https://wiki.gentoo.org/wiki/MATE | group: Gentoo Wiki (Main) | wiki-title: MATE -->
---
title: MATE
url: https://wiki.gentoo.org/wiki/MATE
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-01-28"
fingerprint: d015d67b088799c4
license: CC BY-SA 4.0
---

# MATE

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**MATE** (pronounced to rhyme with *latte*, not *late*) is a [fork](<https://en.wikipedia.org/wiki/Fork_(software_development)>) of the [GNOME 2](https://wiki.gentoo.org/wiki/GNOME) desktop environment designed to retain the look and feel of a 'traditional' desktop environment.

According to the MATE team's manifesto, they aim to keep a traditional desktop look and feel, maintain an open development model, have an open relationship with GNU/Linux distributions, and serve as a good alternative for lower-end hardware.

## Installation

### Architecture Support

MATE is available for **amd64** and **x86**. As of March 2019, testing versions are available for **\~arm**,**\~arm64**, **\~loong** and **\~riscv**. Support for other architectures will be considered with support of people testing and reporting issues, please reach out in [#gentoo-mate](ircs://irc.libera.chat/#gentoo-mate) ([webchat](https://web.libera.chat/#gentoo-mate)) on Libera Chat if interested in this.

### USE flags

First enable or disable desired [USE flags](https://wiki.gentoo.org/wiki/USE_flag) for [mate-base/mate](https://packages.gentoo.org/packages/mate-base/mate).


| [+base](https://packages.gentoo.org/useflags/+base) | Install base MATE Desktop applications that are recommended for the most common usage; for example, this installs the file manager. Disable this and other USE flags if you want a more minimal MATE Desktop. | 
| [+extras](https://packages.gentoo.org/useflags/+extras) | Install additional MATE Desktop applications that are recommended for extended usage of the MATE Desktop as upstream sees it; for example, this installs MATE Desktop's office related applications. Disable this if you plan to use your own non-MATE Desktop alternatives or a custom mixture of MATE and non-MATE packages. | 
| [+notification](https://packages.gentoo.org/useflags/+notification) | Force notification daemon to default to MATE's notification daemon. Enabled by default. Disable if it causes conflicts with other installed desktop environments. | 
| [+themes](https://packages.gentoo.org/useflags/+themes) | Install MATE Desktop's themes; if you use other themes, you can disable this to spare some space and time. | 
| [bluetooth](https://packages.gentoo.org/useflags/bluetooth) | Enable Bluetooth Support | 
| [help](https://packages.gentoo.org/useflags/help) | Install gnome-extra/yelp to handle in application help menus and documentation browsing | 

### Installing MATE

#### Profiles

##### OpenRC

It is highly recommended to run a desktop profile when running a [Desktop Environment](https://wiki.gentoo.org/wiki/Desktop_environment), using **amd64** as an example this can be done by running:

`root #``eselect profile set default/linux/amd64/23.0/desktop``root #``emerge -vauDU @world`
##### systemd =

It is highly recommended to run a desktop profile when running a [Desktop Environment](https://wiki.gentoo.org/wiki/Desktop_environment), using **amd64** as an example this can be done by running:

`root #``eselect profile set default/linux/amd64/23.0/desktop/systemd``root #``emerge -vauDU @world`
#### Combined hardened profiles

Users that run hardened profiles can also combine it with all the features of the desktop profile. For steps on doing this please follow [Hardened\_Desktop\_Profiles](https://wiki.gentoo.org/wiki/Hardened_Desktop_Profiles).

#### Emerge

First, you will need xorg-server:

`root #``emerge --ask x11-base/xorg-server`
To install the MATE desktop environment meta package run the following command:

`root #``emerge --ask --changed-use mate-base/mate`
## Usage

Either a [display manager](https://wiki.gentoo.org/wiki/Display_manager) ([SLiM](https://wiki.gentoo.org/wiki/SLiM), [GDM](https://wiki.gentoo.org/wiki/GNOME/gdm), [LightDM](https://wiki.gentoo.org/wiki/LightDM), etc.) or the `startx` command can be used to start MATE during system the system boot process.

### Display manager (DM)

To make the display manager work specify a MATE session (mate-session) in the configuration of the display manager; some perform this action interactively, others will need to have a configuration file modified. The default session can also often be changed by setting `XSESSION="Mate"` in /etc/env.d/90xsession:

**`/etc/env.d/90xsession`**

**Enabling MATE**

```
XSESSION="Mate"
```
The MATE team recommends [LightDM](https://wiki.gentoo.org/wiki/LightDM) as the display manager. Install [x11-misc/lightdm](https://packages.gentoo.org/packages/x11-misc/lightdm):

`root #``emerge --ask x11-misc/lightdm`
#### OpenRC

Set LightDM as the default display manager:

**`/etc/conf.d/display-manager`**

```
DISPLAYMANAGER="lightdm"
```
To start LightDM on boot, add dbus and display-manager to the default runlevel:

`root #````
rc-update add dbus default
```
`root #``rc-update add display-manager default`
To start LightDM now:

`root #````
rc-service dbus start
```
`root #``rc-service display-manager start`
#### systemd

To start LightDM on boot:

`root #``systemctl enable lightdm`
To start LightDM now:

`root #``systemctl start lightdm`
### Manual start

To start MATE manually create a \~/.xinitrc file in a user's home directory. Make its contents as follows:

**`~/.xinitrc`**

```
exec mate-session
```
Note that dbus-launch may be needed between exec mate-session for [D-Bus](https://wiki.gentoo.org/wiki/D-Bus) communication to work, for example:

**`~/.xinitrc`**

```
exec dbus-launch mate-session
```
### Desktop portal

Because apps are trying to support Wayland, some functionality in MATE relies on the XDG Desktop Portal. For example, Firefox installed from Flatpak will fail to open a user's "Downloads" directory without desktop portal implementation. Therefore xdg-desktop-portal-xapp is needed as it works with MATE.

`root #``emerge --ask sys-apps/xdg-desktop-portal-xapp`
The user should then logout and log back into their graphical session to ensure that everything works correctly.

## Configuration

### Compositing

Compositing is *not* enabled by default. To enable compositing run System → Preferences → Windows and click the tick box alongside **Enable software compositing window manager** in the General tab.

### Window centering

Window centering is *not* enabled by default. To enable window centering run System → Preferences → Windows and click the tick box alongside **Center new windows** in the Placement tab.

### Window snapping

Window snapping is *not* enabled by default. To enable window snapping run System → Preferences → Windows and click on the tick box alongside **Enable side by side tiling** in the Placement tab.

The applications menu icon is set to start-here by default. To use a different icon, copy the user's icon to a folder such as /usr/local/share/pixmaps and execute the following:

`user $``gsettings set org.mate.panel.menubar icon-name <icon>`
Where `icon` is the name of the icon without the file extension. Restart MATE panel.

### Show or hide desktop icons

Desktop icons are enabled by default. They can be hidden or shown individually using dconf.

#### Hide all desktop icons

`user $``dconf write /org/mate/desktop/background/show-desktop-icons false`
#### Hide individual icons

To hide the computer icon:

`user $``dconf write /org/mate/caja/desktop/computer-icon-visible false`
To hide the user directory icon:

`user $``dconf write /org/mate/caja/desktop/home-icon-visible false`
To hide the network icon:

`user $``dconf write /org/mate/caja/desktop/network-icon-visible false`
To hide the trash icon:

`user $``dconf write /org/mate/caja/desktop/trash-icon-visible false`
To hide mounted volumes:

`user $``dconf write /org/mate/caja/desktop/volumes-visible false`
Alternatively, dconf Editor may be used to show or hide desktop icons. Navigate to org → mate → caja → desktop.

### Show volume control

To control pulseaudio and see the volume control icon in the notification area, make sure to compile [media-libs/libmatemixer](https://packages.gentoo.org/packages/media-libs/libmatemixer) with `pulseaudio` USE enabled.

### Applications

This is a list of GNOME 2 applications that have been renamed and included in the MATE desktop environment.

| Icon | GNOME 2 | MATE | Type | 
|---|---|---|---|
| ![Mozo.png](https://wiki.gentoo.org/images/5/52/Mozo.png) | Alacarte (alacarte) | Mozo (mozo) | Menu editor | 
| ![Desktop.png](https://wiki.gentoo.org/images/5/5a/Desktop.png) | About GNOME (gnome-about) | About MATE (mate-about) | About Desktop Environment | 
| ![Mate-disk-usage-analyzer.png](https://wiki.gentoo.org/images/d/d0/Mate-disk-usage-analyzer.png) | Baobab (baobab) | Disk Usage Analyzer (mate-disk-usage-analyzer) | Disk usage analyzer | 
| ![Preferences-desktop.png](https://wiki.gentoo.org/images/e/e2/Preferences-desktop.png) | Control Center (gnome-control-center) | Control Center (mate-control-center) | Example | 
| ![Gnome-mime-application-pdf.png](https://wiki.gentoo.org/images/5/50/Gnome-mime-application-pdf.png) | Evince (evince) | Atril (atril) | Document Viewer | 
| ![Mate-image.png](https://wiki.gentoo.org/images/6/69/Mate-image.png) | Eye of GNOME (eog) | Eye of MATE (eom) | Image Viewer | 
| ![Mate-zip.png](https://wiki.gentoo.org/images/5/52/Mate-zip.png) | File Roller (file-roller) | Engrampa (engrampa) | File Archive Manager | 
| ![Mateconf.png](https://wiki.gentoo.org/images/d/d5/Mateconf.png) | GConf (gconftool-2, gconf-editor) | MateConf (mateconftool-2, mateconf-editor) | DE Configuration System | 
| ![Pluma-text-editor.png](https://wiki.gentoo.org/images/e/e6/Pluma-text-editor.png) | Gedit (gedit) | Pluma (pluma) | Text Editor | 
| ![Marco.png](https://wiki.gentoo.org/images/a/a5/Marco.png) | Metacity (metacity) | Marco (marco) | Window Manager | 
| ![Caja-file-manager.png](https://wiki.gentoo.org/images/a/ad/Caja-file-manager.png) | Nautilus (nautilus) | Caja (caja) | File Manager | 
| ![Mate-applets-screenshooter.png](https://wiki.gentoo.org/images/f/ff/Mate-applets-screenshooter.png) | Take Screenshot (gnome-screenshot) | Take Screenshot (mate\_screenshot) | Screen Capture Tool | 
| ![Gnome-terminal.png](https://wiki.gentoo.org/images/7/77/Gnome-terminal.png) | Terminal (gnome-terminal) | Terminal (mate-terminal) | Terminal | 
| ![Matedialog.png](https://wiki.gentoo.org/images/9/92/Matedialog.png) | Zenity (zenity) | MateDialog (matedialog) | GTK Dialog Boxes | 

#### Applets

This is a list of GNOME 2 panel applets that have been renamed and included in the MATE desktop environment.

| Icon | GNOME 2 | MATE | Type | 
|---|---|---|---|
| ![Mateweather.png](https://wiki.gentoo.org/images/8/87/Mateweather.png) | gweather | mateweather | Panel Weather Applet | 
| ![Mateinvest.png](https://wiki.gentoo.org/images/a/a8/Mateinvest.png) | gnome-invest-applet | mate-invest-applet | Stock Tracking Applet | 
| ![Mate-netspeed-applet.png](https://wiki.gentoo.org/images/6/67/Mate-netspeed-applet.png) | gnome-netspeed-applet | mate-netspeed-applet | View Internet Speed | 
| ![Mate-inhibit-applet.png](https://wiki.gentoo.org/images/5/56/Mate-inhibit-applet.png) | gnome-inhibit-applet | mate-inhibit-applet | Inhibit Power Saving | 
| ![User-trash-full.png](https://wiki.gentoo.org/images/7/7b/User-trash-full.png) | gnome-trash-applet | mate-trash-applet | Shortcut to Trash | 
| ![Mate-panel-notification-area.png](https://wiki.gentoo.org/images/2/2b/Mate-panel-notification-area.png) | gnome-panel-notification area | mate-panel-notification-area | Notification Area | 
| ![Mate-panel-window-list.png](https://wiki.gentoo.org/images/8/84/Mate-panel-window-list.png) | gnome-panel-window-list | mate-panel-window-list | Switch Windows using the Taskbar | 
| ![Mate-panel-window-menu.png](https://wiki.gentoo.org/images/2/21/Mate-panel-window-menu.png) | gnome-panel-window-menu | mate-panel-window-menu | Switch Windows using a Menu | 
| ![Mate-panel-workspace-switcher.png](https://wiki.gentoo.org/images/d/d6/Mate-panel-workspace-switcher.png) | gnome-panel-workspace-switcher | mate-panel-workspace-switcher | Switch Workspaces | 
| ![Mate-sticky-notes-applet.png](https://wiki.gentoo.org/images/1/1f/Mate-sticky-notes-applet.png) | gnome-sticky-notes-applet | mate-sticky-notes-applet | Create, View, Manage Sticky Notes | 

### Autostart

MATE is capable of automatically running binaries or scripts on a per-user basis.

Autostart entries can be added via System → Preferences → Start Applications.

From the command-line, entries can be added in the \~/.config/autostart directory as XDG formatted .desktop files. Be sure to include a line that says `X-MATE-Autostart-enabed=true`. For example:

**`~/.config/autostart/example.desktop`**

**MATE autostart example**

```
[Desktop Entry]
Type=Application
Exec=/path/to/executable
Hidden=false
Name=Autostart example
Comment=This file shows that the line below is needed for MATE to autostart an executable
X-MATE-Autostart-enabled=true
```
## FAQ

### Does MATE rely on a specific service manager or init system?

No, MATE has been tested to work with both [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) and [systemd](https://wiki.gentoo.org/wiki/Systemd) and might work on other service managers and init systems too (untested, but no known reason for it to break); systemd support was added in [release 1.6](http://mate-desktop.org/fr/blog/2013-04-02-mate-1-6-released/).

### Can MATE be installed side-by-side GNOME packages or do they block?

As the MATE packages use their own categories, it is possible to have MATE and GNOME 3 installed at the same time which allows the user to test either; taking it even a step further, if the user change MATE to not have a top panel (as it gets hidden under the GNOME 3 shell) the user can even start mate-session within GNOME 3 and run MATE and GNOME 3 at the same time.

### How do I enable the panel shadow?

Due to a race condition, the panel shadow does not appear after logging in to the MATE desktop, even with compositing enabled. The user must first copy /usr/share/applications/mate-panel.desktop to \~/.local/share/applications/mate-panel.desktop. Then set X-MATE-Autostart-Phase to Applications, and add a delay:

**`/usr/share/applications/mate-panel.desktop`**

```
=Applications
X-MATE-Autostart-Delay=2
X-MATE-Provides=windowmanager
X-MATE-Autostart-Notify=true
```
The user may need to adjust the delay as needed. Finally, restart Marco with the following command:

`user $``marco --replace`
### Using MATE with dual screens

When using MATE desktop with multiple screens, it must be emerged with the `xinerama` USE flag enabled. Specifically, the 
window manager that is powering mate-desktop, which is [x11-wm/marco](https://packages.gentoo.org/packages/x11-wm/marco). This will solve issues like [windows being maximized over both screens in MATE desktop](https://forums.gentoo.org/viewtopic-p-7583398.html).

### Using MATE with Android phones

To connect Android devices and open them in [mate-base/caja](https://packages.gentoo.org/packages/mate-base/caja) the user need to compile [gnome-base/gvfs](https://packages.gentoo.org/packages/gnome-base/gvfs) with the `mtp` USE flag.

### Can I replace the default screen-shot tool with X?

mate-screenshot is provided in [mate-extra/mate-utils](https://packages.gentoo.org/packages/mate-extra/mate-utils) provides basic screenshot capabilities. If the user don't like it and need a more advanced tool, like [x11-misc/shutter](https://packages.gentoo.org/packages/x11-misc/shutter) the user can replace the default behavior of the Print Screen button by editing the following configurations option with dconf-edtior:

org.mate.marco.keybinding-commands.command-screenshot $user-command

If the user feel comfortable doing this with the command line the user can do:

`user $``dconf write /org/mate/marco/keybinding-commands/command-screenshot \"foo\"`
or with:

`user $``gsettings set org.mate.Marco.keybinding-commands command-screenshot 'foo'`
If only "Restart Cancel Shutdown" buttons appear in the dialog, make sure both [mate-base/mate-session-manager](https://packages.gentoo.org/packages/mate-base/mate-session-manager) and [mate-base/mate-power-manager](https://packages.gentoo.org/packages/mate-base/mate-power-manager) were built with `elogind` flag.

### Wayland Support

As of 1.28 all MATE programs have Wayland support enabled with [wayland](https://packages.gentoo.org/useflags/wayland) [however](https://wiki.gentoo.org/wiki/USE_flag) [mate-wayland-session](https://packages.gentoo.org/packages/mate-wayland-session) is currently only available in the mate-overlay as early testing has deemed it too unstable for a wide release. If the user are interested in helping with testing please visit the IRC channel [#gentoo-mate](ircs://irc.libera.chat/#gentoo-mate) ([webchat](https://web.libera.chat/#gentoo-mate)) on Libera Chat.

#### Prepare system

Add the USE flag [wayland](https://packages.gentoo.org/useflags/wayland) [as a global USE flag:](https://wiki.gentoo.org/wiki/USE_flag)

**`/etc/portage/package.use/wayland`**

**wayland USE flag**

```
 wayland
```
Next, preform a system update with:

`root #``emerge -vauDU @world`
#### Enable overlay

`root #``emerge -va eselect-repository dev-vcs/git``root #``eselect repository enable mate-overlay``root #``emerge --sync`
#### mate-wayland-session

`root #``emerge --ask mate-base/mate-wayland-session`
#### Desktop Manager

Currently SDDM is being used by upstream and the Gentoo MATE team to test wayland support with Gentoo. More testing with other DMs and feedback given would be very helpful.

#### Switching to PipeWire

By default, mate-base ships a pre-configured PulseAudio server. As the transition to Wayland continues, users may wish to install PipeWire for screen capture.

Switching to PipeWire is very simple. First, remove pulseaudio-daemon:

`root #``emerge --deselect media-sound/pulseaudio-daemon`
Then, depclean:

`root #``emerge --depclean`
Add the pipewire and pulseaudio USE flag globally:

**`/etc/portage/make.conf`**

Add the sound-server USE flag to PipeWire

**`/etc/portage/package.use/pipewire`**

Then, emerge PipeWire:

`root #``emerge --ask media-video/pipewire`
To start PipeWire on systemd, follow the instructions at [pipewire](https://wiki.gentoo.org/wiki/Pipewire#systemd)

For OpenRC, add gentoo-pipewire-launcher to wayfire's autostart.

**`~/.config/wayfire.ini`**

#### dbus

Users of Openrc may need dbus. Unlike systemd, this will require some manual configuration.

If using a Display Manager, edit the wayfire desktop file.

**`/usr/share/wayland-sessions/wayfire.desktop`**

#### polkit

Systemd also handles polkit, but users of OpenRC will need to add polkit to their wayfire configuration. This allows graphical file managers to access mountable drives and the recycle bin.

First, emerge polkit:

`root #``emerge --ask sys-auth/polkit`
Then, add polkit to the wayfire configuration file:

**`~/.config/wayfire.ini`**

## Removal

`root #``emerge --ask --depclean mate-base/mate $(qlist -IC 'mate-base/*')`
To remove all packages with the name MATE ([eix](https://wiki.gentoo.org/wiki/Eix) required):

`root #``emerge --ask --depclean $(eix -I -# mate)`
## Troubleshooting

### GLib-GObject-ERROR: object GsmAutostartApp 0x73ca40 finalized while still in-construction

When the user get this error (see \~/.materc-errors), it is usually preceded by a warning, fixing the warning could fix the problem; for example, when I get to see:

In this case, the user can resolve this by moving away the desktop file or fixing it up by adding the Name key. If the user want a clean start, the user can move those files out of the way by backing them up:

`user $``for f in ~/.config/autostart/*.desktop ; do mv "${f}" "${f}.bak" ; done`
### Failure to emerge due to conflicts with x11-libs/gtk+:3 and x11-themes/mate-themes{,-meta}

Unfortunately, due to some packaging requirements, there is the potential for users to have an issue with proper dependency resolution when installing mate-themes{,-meta}. Generally speaking, the simplest solution is to oneshot the appropriate mate-themes package, allowing subsequent emerges to happen without issue. For example, if the user have x11-libs/gtk+-3.18 installed, the user will want to

`root #``emerge -1av '=mate-themes-3.18*'`
or if the user have gtk+-3.20 installed, the user will want to

`root #``emerge -1av '=mate-themes-3.20*'`
### Failed to acquire org.freedesktop.timedate1.set-time: GDBus.Error:org.freedesktop.PolicyKit1.Error.Failed:

In order to set the system time through the calendar applet or mate-control center (or through mate-admin) the user will have to compile:

`root #``emerge --ask app-admin/openrc-settingsd`
And then add it to the default run level:

`root #``rc-update add openrc-settingsd default`
Logout and re-login and the user will be able to set the system time.

## See also

- [Desktop environment](https://wiki.gentoo.org/wiki/Desktop_environment) — provides a list of desktop environments available in Gentoo.
- [KDE](https://wiki.gentoo.org/wiki/KDE) — a free software community, producing a wide range of applications including the popular Plasma desktop environment.
- [Gnome](https://wiki.gentoo.org/wiki/Gnome) — a feature-rich desktop environment provided by the [GNOME project](https://www.gnome.org).
- [Xfce](https://wiki.gentoo.org/wiki/Xfce)
