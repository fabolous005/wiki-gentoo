<!-- source: https://wiki.gentoo.org/wiki/Unity | group: Gentoo Wiki (Main) | wiki-title: Unity -->
---
title: Unity
url: https://wiki.gentoo.org/wiki/Unity
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-21"
fingerprint: f21d724a0996b9ee
license: CC BY-SA 4.0
---

# Unity

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Unity** is an alternative shell for the [GNOME](https://wiki.gentoo.org/wiki/GNOME) desktop environment, developed by Canonical in its Ayatana project.

It consists of several components including the Launcher, Dash, Lenses, Panel and Indicators.

More info about its individual components can be found at [Ubuntu's Unity article](https://wiki.ubuntu.com/Unity)

Unity is currently a plugin of the *[Compiz](https://wiki.gentoo.org/wiki/Compiz)* window manager.

Under Canonical's developing convergence plans it will soon be a QT5 QML Desktop using their Mir [display server](https://wiki.gentoo.org/wiki/Display_server).

Mir is being developed as an alternative display server to X or Wayland, more info about it can be found at [Ubuntu's Mir article](https://wiki.ubuntu.com/Mir)

## Installation

### unity-gentoo

You can install Unity by adding the *unity-gentoo*[ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository) via [eselect repository](https://wiki.gentoo.org/wiki/Eselect/Repository):

`root #``eselect repository enable unity-gentoo`
Select one of the 'unity-gentoo' profiles:

Once 'unity-gentoo' profile has been installed, make sure you sync it up.

`root #``emerge --sync`
or

`root #``emerge-webrsync`
Then verify its installation:

`root #``eselect profile list`
Emerge *unity-base/unity-build-env*:

`root #``emerge -av unity-build-env`
Finally, emerge *unity-base/unity-meta*:

`root #``emerge -uDNavt unity-meta`
### gentoo-unity7

Alternatively, you can install Gentoo Unity⁷ Desktop from [*gentoo-unity7*](https://github.com/c4pp4/gentoo-unity7) overlay.

## Usage

You should be starting it via a [display manager](https://wiki.gentoo.org/wiki/Display_manager) or by setting `XSESSION=unity` for startx:

`root #``systemctl start lightdm``user $``XSESSION=unity startx`
## Technical

At its core Unity uses a lot of Gnome, and since >=gnome-3.8 systemd is required.

GNOME 3 requires systemd to be running to get it working properly, otherwise, you will get broken power management and multiseat handling, and also some more problems because upstream has moved away from the obsolete consolekit to logind (that needs systemd to be running to work).

### Startup files

1. The display manager (lightdm) or XSESSION=unity and 'startx' uses the *sys-apps/upstart* desktop services daemon to start 'gnome-session --session=unity'
  - This executes /usr/share/gnome-session/sessions/unity.session which starts compiz as the window manager.
2. /etc/X11/xinit/xinitrc.d/65compiz\_profile-on-session is sourced at Xsession startup and if $DESKTOP\_SESSION=unity, it sets the COMPIZ\_CONFIG\_PROFILE=ubuntu variable.
  - When compiz starts it checks the value of $COMPIZ\_CONFIG\_PROFILE and uses the 'ubuntu' entry located in /etc/compizconfig/config
  - This 'ubuntu' entry sets the compiz profile to 'unity' and sets the compizconfig backend to use gsettings (dconf) which reads the compiz settings from the dconf registry.
3. Default compizconfig settings find their way into the desktop user's dconf registry by way of /etc/xdg/autostart/compiz-migrate-to-dconf.desktop
  - This is auto started at Xsession startup and actually copies the settings from the global gconf registry to the desktop user's dconf registry.
4. Default compizconfig settings find their way into the global gconf registry when compiz is emerged.
  - At src\_install() time the compiz ebuild uses the 'update-gconf-defaults' tool to read all files in /usr/share/gconf/defaults/ and write them out to a gconf registry file located in /etc/gconf/gconf.xml.unity/
  - It then updates /etc/gconf/2/local-defaults.path to include the new /etc/gconf/gconf.xml.unity/ directory
