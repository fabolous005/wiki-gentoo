<!-- source: https://wiki.gentoo.org/wiki/LXQt | group: Gentoo Wiki (Main) | wiki-title: LXQt -->
---
title: LXQt
url: https://wiki.gentoo.org/wiki/LXQt
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-20"
fingerprint: aa70e61d108641d6
license: CC BY-SA 4.0
---

# LXQt

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**LXQt** is a lightweight desktop environment based on the [Qt](https://wiki.gentoo.org/wiki/Qt) toolkit. It is the result of the merge between the LXDE-Qt and the Razor-qt projects.

LXQt has been stable for **amd64** and **x86** since version 0.13.0, so normally no keywords are needed.

However, for other architectures, it may be necessary to add a few packages to package.accept\_keywords:

**`/etc/portage/package.accept_keywords/lxqt`**

Additional packages may need keywording; emerge will provide current package keywording information if that is the case. Refer to [accepting a keyword for a single package](https://wiki.gentoo.org/wiki/Knowledge_Base:Accepting_a_keyword_for_a_single_package) for more information on making packages from the testing branch available.


| [+about](https://packages.gentoo.org/useflags/+about) | Install lxqt-base/lxqt-about | 
| [+archiver](https://packages.gentoo.org/useflags/+archiver) | Install app-arch/lxqt-archiver | 
| [+desktop-portal](https://packages.gentoo.org/useflags/+desktop-portal) | Enable the LXQt sys-apps/xdg-desktop-portal backend implementation | 
| [+display-manager](https://packages.gentoo.org/useflags/+display-manager) | Install a graphical display manager | 
| [+filemanager](https://packages.gentoo.org/useflags/+filemanager) | Install x11-misc/pcmanfm-qt file manager | 
| [+icons](https://packages.gentoo.org/useflags/+icons) | Install kde-frameworks/breeze-icons | 
| [+lximage](https://packages.gentoo.org/useflags/+lximage) | Install media-gfx/lximage-qt image viewer | 
| [+policykit](https://packages.gentoo.org/useflags/+policykit) | Enable PolicyKit (polkit) authentication support | 
| [+processviewer](https://packages.gentoo.org/useflags/+processviewer) | Install x11-misc/qps package | 
| [+screenshot](https://packages.gentoo.org/useflags/+screenshot) | Install x11-misc/screengrab package | 
| [+sddm](https://packages.gentoo.org/useflags/+sddm) | Install x11-misc/sddm display manager | 
| [+sudo](https://packages.gentoo.org/useflags/+sudo) | Install lxqt-base/lxqt-sudo | 
| [+terminal](https://packages.gentoo.org/useflags/+terminal) | Install x11-terms/qterminal package | 
| [+trash](https://packages.gentoo.org/useflags/+trash) | Install gnome-base/gvfs to enable 'trash:///', 'computer:///' and other such "places" in x11-misc/pcmanfm-qt | 
| [+window-manager](https://packages.gentoo.org/useflags/+window-manager) | Install kde-plasma/kwin window manager | 
| [admin](https://packages.gentoo.org/useflags/admin) | Install lxqt-base/lxqt-admin | 
| [nls](https://packages.gentoo.org/useflags/nls) | Install dev-qt/qttranslations to better support different locales | 
| [powermanagement](https://packages.gentoo.org/useflags/powermanagement) | Install lxqt-base/lxqt-powermanagement package | 
| [ssh-askpass](https://packages.gentoo.org/useflags/ssh-askpass) | Install lxqt-base/lxqt-openssh-askpass user password prompt tool | 
| [wayland](https://packages.gentoo.org/useflags/wayland) | Install lxqt-base/lxqt-wayland-session to support Wayland sessions | 

Get the complete LXQt desktop environment by installing the [lxqt-base/lxqt-meta](https://packages.gentoo.org/packages/lxqt-base/lxqt-meta) package:

`root #``emerge --ask lxqt-base/lxqt-meta`
The LXQt appearance settings GUI has no support for changing [GTK](https://wiki.gentoo.org/wiki/GTK) themes; [lxde-base/lxappearance](https://packages.gentoo.org/packages/lxde-base/lxappearance) may be used instead.

Alternatively, it's possible to specify the theme manually: refer to the [GTK](https://wiki.gentoo.org/wiki/GTK) page for further information.

By default, LXQt tries to automount every disk attached at startup, and every disk plugged in. It may result in unwanted popup windows asking for the root password to mount disks.

The automount options are located in the [PCManFM](https://wiki.gentoo.org/wiki/PCManFM) file manager settings:

- In the applications menu, open **Accessories->PCManFM File Manager**.
- Select the **Edit->Preferences** menu item.
- Click on **Volume**.
- Disable or enable the desired features.

PCManFM-Qt supports the desktop file specification extension, so it's possible to add [custom actions](https://github.com/lxqt/pcmanfm-qt/wiki/custom_actions) to the contextual menu for files and directories.

[x11-libs/libfm](https://packages.gentoo.org/packages/x11-libs/libfm) and [x11-libs/libfm-extra](https://packages.gentoo.org/packages/x11-libs/libfm-extra) v1.2.4 or above are required, and the [vala](https://packages.gentoo.org/useflags/vala) [USE flag must be enabled:](https://wiki.gentoo.org/wiki/USE_flag)

**`/etc/portage/package.use/lxqt`**

Rebuilding the package can be done with:

`root #``emerge -1a libfm`
Once this is done, special .desktop files can be created in \~/.local/share/file-manager/actions/. For example here is a simple action that will pop up a notification with the path to the selected file or folder:

**`~/.local/share/file-manager/actions/test.desktop`**

PCManFM-Qt needs to be restarted to become aware of any change made to the custom actions. The safest and simplest way is to log out and log in again (i.e. restart LXQt).

Alternatively, it's possible to exit all instances of PCManFM-Qt with pcmanfm-qt -q before launching it again. However, be aware that it will likely switch to a different set of settings (it can use two different config directories, one "default" and one "lxqt", as in \~/.config/pcmanfm-qt/), and e.g. automount volumes, which may cause issues.

**Todo:**

- Confirm whether or not this method of launching works under systemd, and update the following accordingly.

To launch LXQt using the startx command instead of a [display manager](https://wiki.gentoo.org/wiki/Display_manager), the following file can be used with or without [elogind](https://wiki.gentoo.org/wiki/Elogind) (and possibly works with [systemd](https://wiki.gentoo.org/wiki/Systemd) as well):

**`~/.xinitrc`**

```
exec startlxqt
```
In many setups, a D-Bus session bus should be started for the GUI session, in addition to the `dbus` system service, which provides a D-Bus system bus. Refer to the [D-Bus](https://wiki.gentoo.org/wiki/D-Bus) page for details.

To disable energy saving, which powers off the display after 10 minutes of inactivity (even when watching YouTube) add these two lines *before* the `exec` line:

**`~/.xinitrc`**

```
 s off
xset -dpms
```
It's also possible to configure the user's login [shell](https://wiki.gentoo.org/wiki/Shell) to [run startx on login](https://wiki.gentoo.org/wiki/X_without_Display_Manager#Starting_X11_automatically).

A [display manager](https://wiki.gentoo.org/wiki/Display_manager) (DM) presents the user with a graphical login screen after boot, to log into a GUI session. Some may prefer this to using startx at the console or automatically launching LXQt. A DM may also be useful if multiple [desktop environments](https://wiki.gentoo.org/wiki/Desktop_environment) or [window managers](https://wiki.gentoo.org/wiki/Window_manager) are installed on the same machine.

Examples of display managers that will work with LXQt are [SDDM](https://wiki.gentoo.org/wiki/SDDM), [GDM](https://wiki.gentoo.org/wiki/GDM), and [LightDM](https://wiki.gentoo.org/wiki/LightDM).

Refer to the [display manager](https://wiki.gentoo.org/wiki/Display_manager) article for a list of available display managers for Gentoo, and each DM's wiki page for installation and setup instructions.

Any [Qt](https://wiki.gentoo.org/wiki/Qt) application can be used with LXQt, but if Qt applications that do not depend on any component of KDE are preferred, refer to the [Qt Desktop applications](https://wiki.gentoo.org/wiki/Qt_Desktop_applications) page.

If using OpenRC, add [NetworkManager](https://wiki.gentoo.org/wiki/NetworkManager) to the `default` runlevel:

`root #``rc-update add NetworkManager default`
In order to have a [GUI for Wi-Fi networks](https://wiki.gentoo.org/wiki/NetworkManager#GTK_GUIs), install [nm-applet(1)](https://man.archlinux.org/man/nm-applet.1.en)[:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`root #``emerge --ask gnome-extra/nm-applet`
If the LXQt panel is set to autohide and [mouse hover on Wi-Fi icon hides the panel](https://wiki.gentoo.org/wiki/File:Lxqt-tray-widget-obsolote.webp):

1. Emerge [gnome-extras/nm-applet](https://packages.gentoo.org/packages/gnome-extras/nm-applet) with the [appindicator](https://packages.gentoo.org/useflags/appindicator)2. Emerge [lxqt-base/lxqt-panel](https://packages.gentoo.org/packages/lxqt-base/lxqt-panel) with the [statusnotifier](https://packages.gentoo.org/useflags/statusnotifier)3. Edit autostart in LXQt: change the call to nm-applet to nm-applet --indicator.

The relevant Gentoo support channels are [#gentoo-qt](ircs://irc.libera.chat/#gentoo-qt) ([webchat](https://web.libera.chat/#gentoo-qt)) on Libera.Chat, and lxqt@gentoo.org.  Bugs should be reported at [bugs.gentoo.org](https://bugs.gentoo.org/) (b.g.o).

Upstream's IRC channels are on [OFTC](https://www.oftc.net/): #lxqt for user support, and #lxqt-dev for development.

- [Desktop environment](https://wiki.gentoo.org/wiki/Desktop_environment) — provides a list of desktop environments available in Gentoo.
- [Xfce](https://wiki.gentoo.org/wiki/Xfce)
