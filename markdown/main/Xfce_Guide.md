<!-- source: https://wiki.gentoo.org/wiki/Xfce/Guide | group: Gentoo Wiki (Main) | wiki-title: Xfce/Guide -->
---
title: Xfce/Guide
url: https://wiki.gentoo.org/wiki/Xfce/Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-03-31"
fingerprint: d699165a08850956
license: CC BY-SA 4.0
---

# Xfce/Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

- Deduplicate this page with the main Xfce article.

This guide provides an extensive introduction to Xfce, a fast, lightweight, full-featured desktop environment.

## Introduction

### The Xfce desktop environment

[Xfce](https://www.xfce.org/) is a fast, lightweight desktop environment for Unix-like operating systems. It is designed for productivity, and is quite configurable while still adhering to the [Freedesktop](https://www.freedesktop.org/) specifications.

Unlike heavier desktop environments, such as [Gnome](https://wiki.gentoo.org/wiki/Gnome) and [KDE](https://wiki.gentoo.org/wiki/KDE), Xfce uses far fewer system resources. Additionally, it offers greater modularity and fewer dependencies; it takes up less space on the hard disk and takes less time to install.

This guide will not only show how to install and configure a minimal Xfce environment, but will also explore options to create a full-featured desktop in keeping with the Xfce philosophy: light, fast, and modular.

The last part of this guide lists a few commands to run after upgrading to a new Xfce release, so be sure to follow them when upgrading from an older version.

## Installation

### The basics

First, make sure Xorg has been configured as instructed in the [Xorg Guide](https://wiki.gentoo.org/wiki/Xorg/Guide). If these steps have not been completed the rest of this guide will not apply as expected.

In order to have the best experience, the basic desktop profile should be chosen which has many of the common settings XFCE needs.

An example for the **amd64** architecture:

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
  \[29\]  default/linux/amd64/23.0/no-multilib (stable)
  \[30\]  default/linux/amd64/23.0/no-multilib/systemd (stable)
  \[31\]  default/linux/amd64/23.0/no-multilib/hardened (stable)
  \[32\]  default/linux/amd64/23.0/no-multilib/hardened/systemd (stable)
  \[33\]  default/linux/amd64/23.0/no-multilib/hardened/selinux (stable)
...

### Emerge

Now that the USE variables have been set in /etc/portage/make.conf, it is time to install Xfce.

`root #``emerge --ask xfce-base/xfce4-meta`
If desired add regular user(s) to the `cdrom`, `cdrw`, and `usb` groups, so that they can mount and use devices such as cameras, optical drives, and USB sticks.

`root #``for x in cdrom cdrw usb ; do gpasswd -a username $x ; done`
Update the system's environment variables:

`root #``env-update && . /etc/profile`
A graphical terminal will be needed to continue working in the new desktop environment. The [x11-terms/xfce4-terminal](https://packages.gentoo.org/packages/x11-terms/xfce4-terminal) package is a good choice; it is made specifically for Xfce. Install Terminal as shown:

`root #``emerge --ask x11-terms/xfce4-terminal`
## Configuration

### Starting Xfce

Now that Xfce is now installed, configure it to be the default desktop environment when we issue the startx command. Exit the root shell and log on as a regular user.

`user $``echo "exec startxfce4" > ~/.xinitrc`
Now start the graphical environment by typing startx:

`user $``startx`


### Sessions and startup

After installing popular applications such as k3b, nautilus, kmail, evolution, etc., then make sure that Xfce launches the appropriate services for these at startup. Navigate to Menu --> Settings --> Sessions & Startup. On the Advanced tab, select the appropriate checkbox. This might slightly increase Xfce startup times, but it decreases load times for certain applications.

Xfce has the ability to save the session settings and running programs from the General tab in the Sessions & Startup menu. They can be automatically saved on logout, or Xfce can ask each time. This feature is particularly useful for undoing configuration mistakes. Accidentally killed a panel? Just select "No" when prompted to save the current session, and the next time Xfce is started, the old desktop is restored. Want to automatically launch the open web browser, terminal, and email client on the next login? Just save the session before logging out.

### Additional applications

Every user should consider installing some or all of the following useful applications and utilities:

`root #``emerge --ask xfce-extra/xfce4-pulseaudio-plugin xfce-extra/xfce4-taskmanager x11-themes/xfwm4-themes app-editors/mousepad xfce-base/xfce4-power-manager x11-terms/xfce4-terminal xfce-base/thunar`
| Package | Description | 
|---|---|
| [xfce-extra/xfce4-pulseaudio-plugin](https://packages.gentoo.org/packages/xfce-extra/xfce4-pulseaudio-plugin) | Pulseaudio volume control (Others can install [media-sound/volumeicon](https://packages.gentoo.org/packages/media-sound/volumeicon) or similar tray audio mixer). | 
| [xfce-extra/xfce4-taskmanager](https://packages.gentoo.org/packages/xfce-extra/xfce4-taskmanager) | Displays a list of all running programs, and the CPU and memory consumption each one takes up. By right-clicking an item, you can kill a misbehaving application, pause and restart it, or even alter its run time priority, which will fine-tune how much of a demand it puts on the system's resources. | 
| [x11-themes/xfwm4-themes](https://packages.gentoo.org/packages/x11-themes/xfwm4-themes) | Adds several window manager themes. Additional icon themes, such as [x11-themes/tango-icon-theme](https://packages.gentoo.org/packages/x11-themes/tango-icon-theme), are available to round out the desktop. | 
| [app-editors/mousepad](https://packages.gentoo.org/packages/app-editors/mousepad) | Is a bare bones text editor that starts up extremely quickly. | 
| [xfce-base/xfce4-power-manager](https://packages.gentoo.org/packages/xfce-base/xfce4-power-manager) | An application to monitor and manage power usage. This is especially important for laptops! The power manager allows user to adjust screen brightness, choose maximum performance or battery-saving modes, and setup hibernate, suspend, and shutdown actions when the lid is shut or buttons are pressed. [xfce4-power-manager](https://docs.xfce.org/xfce/xfce4-power-manager/start) can be set to provide warning when the battery reaches certain levels, or even turn off the machine. The application comes with a couple of helpful panel plugins to display battery/charging status, and a brightness control. | 
| [x11-terms/xfce4-terminal](https://packages.gentoo.org/packages/x11-terms/xfce4-terminal) | An X11 terminal emulator, far more configurable and useful than the barebones xterm. xfce4-terminal supports Unicode text, color schemes, pseudo-transparency and hardware-accelerated transparency via Xfce's built-in compositor, all out-of-the-box. Just make sure that the default action on the terminal launcher of the panel runs /usr/bin/Terminal instead of xterm. Right-click the launcher and choose Properties to change the command. | 
| [xfce-base/thunar](https://packages.gentoo.org/packages/xfce-base/thunar) | Xfce's default graphical file manager. It is fast yet quite powerful, can support several plugins for even more functionality; just install them with emerge. These plugins include: [xfce-extra/thunar-archive-plugin](https://packages.gentoo.org/packages/xfce-extra/thunar-archive-plugin) which lets users create and extract archive files using the right-click menu. It provides a handy [front-end](https://docs.xfce.org/xfce/thunar/archive) for graphical archiving applications such as xarchiver and file-roller. | 
| [xfce-base/tumbler](https://packages.gentoo.org/packages/xfce-base/tumbler) | Lets users preview certain types of files from within Thunar, such as images and fonts. | 
| [xfce-base/thunar-volman](https://packages.gentoo.org/packages/xfce-base/thunar-volman) | Automatically [manages](https://docs.xfce.org/xfce/thunar/thunar-volman) removable media and drives. | 
| [media-video/parole](https://packages.gentoo.org/packages/media-video/parole) | A simple Xfce4 media player using GStreamer. | 

#### Desktop applications

Though [app-editors/mousepad](https://packages.gentoo.org/packages/app-editors/mousepad) is nice enough as a basic text editor, when a full-featured word processor is required but don't want the bloat of [LibreOffice](https://wiki.gentoo.org/wiki/LibreOffice), try emerging [app-office/abiword](https://packages.gentoo.org/packages/app-office/abiword). AbiWord is lighter, faster, and is completely inter-operable with industry-standard document types.

Need a nice email client/newsreader that is not as demanding as [mail-client/thunderbird](https://packages.gentoo.org/packages/mail-client/thunderbird) or [mail-client/evolution](https://packages.gentoo.org/packages/mail-client/evolution)? Try emerging [mail-client/claws-mail](https://packages.gentoo.org/packages/mail-client/claws-mail).

For internet chat needs, [net-irc/irssi](https://packages.gentoo.org/packages/net-irc/irssi) is an excellent, tiny, incredibly configurable [IRC](https://wiki.gentoo.org/wiki/IRC) client that runs in a terminal. For a compact all-in-one client that handles nearly all chat protocols, consider [net-im/pidgin](https://packages.gentoo.org/packages/net-im/pidgin).

For movie and music players, look no further than [media-video/mplayer](https://packages.gentoo.org/packages/media-video/mplayer) and [media-sound/exaile](https://packages.gentoo.org/packages/media-sound/exaile). They can play most every media format available quite nicely.

Finally, nearly all graphical web browsers require more resources than most of the other desktop applications. Still, [Firefox](https://wiki.gentoo.org/wiki/Firefox) and [Falkon](https://wiki.gentoo.org/wiki/Falkon) are always good choices. Alternatively, [Opera](https://wiki.gentoo.org/wiki/Opera) can be quite fast. However, Opera is not available on as many processor architectures as Firefox.

`root #``emerge --ask www-client/firefox``root #``emerge --ask www-client/falkon``root #``emerge --ask www-client/opera`
#### Panel plugins

There are many plugins for the panel available in Portage; see with emerge --search xfce. Though for the most part their names are self-explanatory, a few deserve extra attention, as they are quite helpful. To use them, simply emerge them. They'll be added to the list of available items in the Add New Items menu shown when right-clicking on the panel.

| Package | Description | 
|---|---|
| [xfce-extra/xfce4-battery-plugin](https://packages.gentoo.org/packages/xfce-extra/xfce4-battery-plugin) | Perfect for laptop users. It displays battery percentage, time remaining, power source (AC or battery), fan status, warnings, and can even be configured to execute commands at certain power levels. This feature can be used to put the laptop into hibernate mode when the battery is almost exhausted. | 
| [xfce-extra/xfce4-verve-plugin](https://packages.gentoo.org/packages/xfce-extra/xfce4-verve-plugin) | A small command line embedded into the panel. It is quicker than opening up a terminal to run a command. | 
| [xfce-extra/xfce4-mount-plugin](https://packages.gentoo.org/packages/xfce-extra/xfce4-mount-plugin) | Provides a handy method of mounting devices listed in /etc/fstab just by clicking the mouse. | 
| [xfce-extra/xfce4-sensors-plugin](https://packages.gentoo.org/packages/xfce-extra/xfce4-sensors-plugin) | Lets users monitor hardware sensors, such as CPU temperature, fan RPM, hard drive temp, motherboard voltage, and more. | 

### Graphical login

Remember when the startxfce4 line was added to the \~/.xinitrc file? To get into the desktop, type startx after logging in. To have a GUI login instead, use a display manager that will automatically start Xfce after booting.

First, make sure Xfce loads at boot:

`root #````
rc-update add dbus default
```
`root #``rc-update add display-manager default`
Now, pick a [display manager](https://wiki.gentoo.org/wiki/Display_manager) from the list and configure it to start in /etc/conf.d/display-manager.

Most display managers can automatically start the Xfce session by adding `XSESSION="Xfce4"` to /etc/env.d/90xsession:

`root #````
echo XSESSION=\"Xfce4\" > /etc/env.d/90xsession
```
`root #``env-update && source /etc/profile`
### Beautifying the desktop

A little customization of the desktop's appearance can go a long way. Xfce has all the options one would expect from a modern desktop environment, font anti-aliasing settings, color schemes, dozens of window decorations, themes, and more. If these are not enough, it is easy to install third-party themes, icon sets, mouse cursor themes, and wallpapers.

A selection of nice Gentoo wallpapers in a variety of resolutions are hosted on the [Gentoo website](https://www.gentoo.org/inside-gentoo/artwork/#wallpapers). When looking for icon sets and complete Xfce themes, [Xfce-Look](https://www.xfce-look.org/) has a huge collection. The important thing to remember about any third-party eye candy is that it will usually need to be unpacked and then installed to the proper directory. Icon sets go in /usr/share/icons/, and themes go to /usr/share/themes/; use these directories for all users to be able to access themes and icon sets. Individual users can install themes and icon sets to their home directories in \~/.themes/ and \~/.icons/.

Finally, Xfce has its own built-in compositor to manage window transparency. This option can be found in Menu --> Settings --> Window Manager. For best performance, a graphics card with drivers that support hardware-accelerated rendering is required. Make sure the [xfce-base/xfwm4](https://packages.gentoo.org/packages/xfce-base/xfwm4) package has been emerged with the `xcomposite` USE flag.

This is the bare minimum configuration required for Xfce and Xorg-X11. However, setting up hardware-accelerated rendering depends on the system's graphics card, and is beyond the scope of this guide. Various articles exist in our wiki that assist in the setup of hardware-accelerated rendering for various graphic cards.

Once finished setting up a beautiful Xfce desktop, the next thing to do is take a screenshot of it to share with other folks! Install [xfce-extra/xfce4-screenshooter](https://packages.gentoo.org/packages/xfce-extra/xfce4-screenshooter) and post the pictures somewhere for all to admire.

## Upgrading

When upgrading Xfce from earlier major versions (4.x), the old cached sessions and profiles will need to be removed; they are incompatible with new releases. For each user, run the following commands to remove the old incompatible cached sessions and profile:

`root #````
rm -r ~/.cache/sessions
```
`root #````
rm -r ~/.config/xfce*
```
`root #````
rm -r ~/.config/Thunar
```
Users will be greeted with a new and shiny interface, but will lose many of their individual settings. Sadly, no migration of configuration(s) exist...

## External resources

Need additional help on configuring and using Xfce? Need more lightweight application suggestions? Try checking out:

- [The Gentoo forums](https://forums.gentoo.org/)
- The installed help files and other documentation provided by Xfce: /usr/share/xfce4/doc/C/index.html. Just point a browser at it and start reading. There are even a lot of "hidden" configuration options detailed in the help files.
