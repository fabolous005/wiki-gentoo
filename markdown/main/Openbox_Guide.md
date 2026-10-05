<!-- source: https://wiki.gentoo.org/wiki/Openbox/Guide | group: Gentoo Wiki (Main) | wiki-title: Openbox/Guide -->
---
title: Openbox/Guide
url: https://wiki.gentoo.org/wiki/Openbox/Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-06"
fingerprint: "54e8de5a0b9788dd"
license: CC BY-SA 4.0
---

# Openbox/Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This guide covers the basics on installing and configuring the OpenBox window manager. Many potential programs to be used in conjunction with OpenBox are referenced throughout this article. For a shorter, simpler install guide see [Openbox](https://wiki.gentoo.org/wiki/Openbox).

## Introduction

### What is Openbox?

After installing [The X Server](https://wiki.gentoo.org/wiki/Xorg/Guide) the default window manager (Tab Window Manager, or TWM) just isn't going to cut it. Most users will have some experience with big desktop environments like [KDE](https://wiki.gentoo.org/wiki/KDE), [GNOME](https://wiki.gentoo.org/wiki/GNOME), and [Xfce](https://wiki.gentoo.org/wiki/Xfce/Guide). One component of those larger desktop suites is called the window manager (or WM for short). A window manager is responsible for the appearance and placement of the containers (or "windows") inside which programs run. Openbox is a minimalistic, no-frills-attached window manager.

### Why should I use it?

Openbox, unlike the larger desktop environments, depends on very few libraries. For that reason, it can provide a lightweight graphic environment that runs very quickly, even on older hardware. Whether the system hardware is old or new, Openbox also provides a highly customizable and unobtrusive working environment. That means that if there is no need for a panel, taskbar, clock, or any other program, those choices are open to make!

## Installation and configuration

### Initial installation

After emerging and configuring [x11-base/xorg-server](https://packages.gentoo.org/packages/x11-base/xorg-server), emerge Openbox:

`root #``emerge --ask x11-wm/openbox`
Just like other window managers and desktop environments, in order to load Openbox automatically, the X Server needs to be told to start Openbox. This is done by adding Openbox to the \~/.xinitrc file in the user's home directory:

`user $``echo "exec openbox-session" >> ~/.xinitrc`
This will automatically start an Openbox session when startx is typed at a terminal.

Now that Openbox has been emerged and the proper command has been added to the user's .xinitrc file, issue the startx command to see Openbox in action. Beware! The desktop may appear as a cluttered mess! In following the Openbox philosophy, a bare bones environment is provided from which a user can build a desktop completely to their liking.

Upon typing the startx command, typically nothing more than the default Openbox black screen will appear. Where is the menu? Clicking the right mouse button, will generate a pop up menu in the location of the cursor. This menu is nothing more than an example to illustrate the *style* of an Openbox menu : none of the items in the menu will work unless the programs have been previously emerged.

### Menu configuration

Everything in the Openbox menu is written in the menu.xml file, which can be in the user-specific location of \~/.config/openbox/menu.xml, or in the system-wide location of /etc/xdg/openbox/menu.xml. By default, the only menu.xml file that is created is the system-wide one which applies to all users on the system.

An easy way to get a basic menu file which can be modified is to use MenuMaker, which will generate a menu.xml file based on the programs which are currently installed on the system. To do so, first install it:

`root #``emerge menumaker`
Once it is installed, make sure to logout of root, and back into the user account. Then instruct MenuMaker to create a menu specifically using the Openbox XML syntax:

`user $``mmaker -v OpenBox3`
The generated menu will be located at \~/.config/openbox/menu.xml. Users can then choose to leave it as the user-specific menu.xml, or to additionally copy it to the system-wide menu configuration as well:

`root #``cp .config/openbox/menu.xml /etc/xdg/openbox/menu.xml`
The menu.xml file is very human-readable and easily understandable. The basic syntax is as follows:

**`menu.xml`**

**Example XML syntax for menu**

```
<?xml version="1.0" encoding="UTF-8"?>
<openbox_menu>
<separator label="NAME_OF_SEPARATOR" />
<menu id="IDENTIFIER" label="NAME_OF_MENU">
  <item label="NAME_OF_PROGRAM">
    <action name="execute"><command>/LOCATION/OF/BINARY --OPTION1 --OPTION2</command></action>
  </item>
</menu>
</openbox_menu>
```
Simply replace anything in ALL\_CAPS in the above example with the right information. Alternatively, use obmenu, which is a graphical interface allowing users to create personalized menus without having to manually edit the menu.xml file.

### Openbox theme and behavior configuration

A user can easily change various settings related to theme, appearance, window placement, docking, and more. There are two options for configuring these settings within Openbox : either manually edit \~/.config/openbox/rc.xml, or use a GUI to help in quickly changing the settings.

To manually edit rc.xml, open up a text editor and start making changes. There are plenty of comments within the document itself that should help with editing. Alternatively, take a look at the [Openbox configuration guides](http://openbox.org/wiki/Help:Contents#Configuration).

If manually editing rc.xml isn't preferred, then try to use the GTK application to manage themes and behaviors in Openbox. The application that can be used is called ObConf, and can be installed on the system just as easily as was Openbox itself.

`root #``emerge obconf`
Next open the configurator by typing obconf in the terminal. Go and add an entry for ObConf into the menu.xml so it will show up in the Openbox menu. If the "editing the menu.xml file" code listing above seemed too vague to be helpful, we'll use ObConf as an example of a menu entry:

**`menu.xml`**

**Using obconf**

```
<?xml version="1.0" encoding="UTF-8"?>
<openbox_menu>
<menu id="1" label="Configuration">
  <item label="OpenBox Config">
    <action name="execute"><command>/usr/bin/obconf</command></action>
  </item>
</menu>
</openbox_menu>
```
While ObConf is a great GUI tool for editing many behavior-related settings for Openbox, it doesn't allow one to manipulate nearly as many settings as are presented in the rc.xml file itself. Please consult the [Openbox Wiki](http://openbox.org/wiki/Configuration) for more information.

### Autostart configuration

As mentioned above, not a whole lot is seen when running the startx command for the first time after installing Openbox. In addition to customizing menus and changing the behavior of the window manager, most users will probably want to have some programs automatically start with their Openbox session. There is an easily-editable autostart script that allows to do just that. Just like with the menu.xml file, there are two different locations of the autostart script--the system-wide (/etc/xdg/openbox/autostart), and the user-defined (\~/.config/openbox/autostart).

In the default autostart, notice a bunch of lines calling for programs like the gnome-settings-daemon, XDG, and others. These lines will generate errors upon logout if the programs are not installed and configured. The easiest thing to do when getting started with Openbox is to just comment out these lines by using the # symbol.

The commenting method is preferred to just deleting the lines because users may want to add support for some of those startup items at a later time. Thus, leaving the default lines in place could ease that process.

Adding programs to the autostart script is as easy as writing in the program name for many applications. For instance, if  [app-admin/conky](https://packages.gentoo.org/packages/app-admin/conky) is installed (a lightweight system monitor), and it needs to be started automatically with the Openbox session, simply add the following line to the autostart file:

**`autostart`**

**Adding Conky to autostart**

```
 &
```
The ampersand (&) after the command allows that application to load up in the background. Most users will likely want to load all the applications in their autostart script in the background because doing so will let Openbox and other programs load without the previous one finishing.

### Configuring PolKit

Many applications depend on the [PolKit](http://www.freedesktop.org/wiki/Software/polkit/) authentication framework. They may need a PolKit agent, such as polkit-gnome, running in the Openbox session.

First, install the agent:

`root #``emerge polkit-gnome`
Now configure PolKit to start automatically when logging in to Openbox. Add the following line to \~/.config/openbox/autostart (for a single user) or /etc/xdg/openbox/autostart (for all users):

**`autostart`**

**Start PolKit automatically**

```
 1 && /usr/libexec/polkit-gnome-authentication-agent-1 &
```
### Setting the background

Some things that users might take for granted in bigger desktop environments are not included by default in Openbox. One such thing is setting the desktop background. In order to place an image as the wallpaper, emerge a program like [media-gfx/feh](https://packages.gentoo.org/packages/media-gfx/feh) or [x11-misc/nitrogen](https://packages.gentoo.org/packages/x11-misc/nitrogen). feh is a simple image viewer that can also set the background, *and* it can easily be put into the autostart script. Once feh is emerged, issue the following command to set the background:

`user $``feh --bg-scale /path/to/image.jpg`
Once the background has been set manually, a file called .fehbg will be created in the user home directory. This file simply contains the above command that was just entered in the terminal, and is automatically updated when issuing a different background command. Now, to set the background automatically upon login, add the following line to the autostart script:

`user $``source $HOME/.fehbg &`
Alternatively use nitrogen. It will allow the user to set a folder for the background images, view thumbnails of those images, and fit, stretch, or tile them to the desktop.

Installing nitrogen and getting it into the Openbox menu requires a few more steps than are readily apparent. First, emerge nitrogen. Second, run nitrogen with the backgrounds folder as argument:

`user $``nitrogen /path/to/the/backgrounds/folder`
Third, set the background image, but keep in mind that it will not be there anymore after logout. Just as with feh, restore the background by editing the autostart script to have the following line:

`user $``nitrogen --restore &`
This will cause nitrogen to load automatically when starting the Openbox session, and that can lead to a slightly slower load time than using feh.

## Programs to use with Openbox

The following is a non-exhaustive list of some programs which might be of use within an Openbox environment. Please check the appropriate categories in Portage for more options.

### Terminal emulators

- [lxde-base/lxterminal](https://packages.gentoo.org/packages/lxde-base/lxterminal) is the default terminal emulator for LXDE. It is very lightweight, and based on VTE. While EvilVTE offers many more customization options (including transparency), LXterminal has a graphical interface for some of the more common options (font, colors, et cetera).

- [x11-terms/evilvte](https://packages.gentoo.org/packages/x11-terms/evilvte) is an extremely lightweight terminal emulator based on VTE. It supports tabs, multiple encodings, as well as an easy and extensible configuration file.

- [x11-terms/mrxvt](https://packages.gentoo.org/packages/x11-terms/mrxvt) is a multi-tabbed rxvt clone with XFT, transparent background and CJK support. It also features session support for each tab.

- [x11-terms/aterm](https://packages.gentoo.org/packages/x11-terms/aterm) supports transparency and backwards compatibility with rxvt. It was originally designed for the AfterStep window manager, but easily integrates with other environments.

- [x11-terms/eterm](https://packages.gentoo.org/packages/x11-terms/eterm) is a terminal based on vt102 and designed to be a more feature-rich replacement for xterm.

- [x11-terms/rxvt-unicode](https://packages.gentoo.org/packages/x11-terms/rxvt-unicode) is a clone of rxvt that supports Unicode, daemons, embedded perl, and multiple fonts simultaneously.

- [x11-terms/xfce4-terminal](https://packages.gentoo.org/packages/x11-terms/xfce4-terminal) is the VTE-based default for the Xfce desktop environment, so it does require some Xfce libraries to run. However, it is still fairly speedy, and supports transparency and is easily customized.

### File managers

- [x11-misc/pcmanfm](https://packages.gentoo.org/packages/x11-misc/pcmanfm) is the lightweight file manager from LXDE. It supports tabbed browsing, drag and drop, thumbnails for images, bookmarks, volume management, searching, and more. It also provides supports for managing the desktop background and drawing desktop icons (both optionally).

- [xfce-base/thunar](https://packages.gentoo.org/packages/xfce-base/thunar) is the standard file manager from Xfce. It features a bulk renamer, user-customizable actions, and an extension framework, along with many optional plug-ins, such as media tag editing. It depends on several Xfce libraries, but it's still slimmed down compared to other file managers like Nautilus (from GNOME), and Konqueror (from KDE).

- [gnome-base/nautilus](https://packages.gentoo.org/packages/gnome-base/nautilus) is the powerful file manager from the GNOME desktop environment. It features volume management, thumbnails for images, searching, and some system configuration. As it depends on many of the GNOME libraries for proper function, it can seem a bit heavy compared to some of the other file managers.

- [app-misc/gentoo](https://packages.gentoo.org/packages/app-misc/gentoo) (no relation to this glorious Linux distribution) is a two-pane style file manager. It is incredibly lightweight, but lacks a some features now prominent in modern file managers. It should definitely be considered for older hardware, or if a barebones setup is needed.

- [app-misc/emelfm2](https://packages.gentoo.org/packages/app-misc/emelfm2) is another file manager in the vein of Midnight Commander. It features a two-pane window. As with the Gentoo file manager (listed above), it is bare bones and does not include many features prevalent in newer file managers. However, it also offers a few features not found in other file managers, such as a built-in command line in a separate pane.

### Desktop management

- Though [x11-misc/pcmanfm](https://packages.gentoo.org/packages/x11-misc/pcmanfm) is mainly a file manager, it also gives the option to manage the desktop background (instead of using feh or nitrogen) and draw desktop icons.

- [x11-misc/idesk](https://packages.gentoo.org/packages/x11-misc/idesk) is a simple program used to draw desktop icons. It supports shadowed and anti-aliased fonts, PNG images, "snap-to-grid" placement, and changing the desktop background.

### Panels

- [x11-misc/tint2](https://packages.gentoo.org/packages/x11-misc/tint2) is a simple, lightweight panel and taskbar. It supports color, transparency, a clock, drag and drop between virtual desktops, a system tray, and comes with a battery monitor. One can even add a button to display the applications menu from the window manager.

- [x11-misc/pypanel](https://packages.gentoo.org/packages/x11-misc/pypanel) is an easily customized panel written in Python and C. It features transparency, shading, tinting, location and layout configuration, font type, auto-hiding, application launcher, clock, and more.

- [lxde-base/lxpanel](https://packages.gentoo.org/packages/lxde-base/lxpanel) is the default panel and taskbar from LXDE. It features a launcher, menu, clock, and a GUI-based configurator. It is feature-rich while depending on very few packages, making it a good choice for a lean system.

- [xfce-base/xfce4-panel](https://packages.gentoo.org/packages/xfce-base/xfce4-panel) is the default panel from the Xfce desktop environment. It supports application launchers, detachable menus, a pager, tasklist, clock, applets, and more. It does, however, require a few of the Xfce libraries which are not dependencies of some other panels.

- [x11-misc/fbpanel](https://packages.gentoo.org/packages/x11-misc/fbpanel) is a simple, extremely lightweight panel that supports window lists, launchers, a clock, and a few other goodies. It's not the most featureful panel, and it can be cumbersome to configure, but it needs only GTK to run.

### Pagers and systrays

- [x11-misc/netwmpager](https://packages.gentoo.org/packages/x11-misc/netwmpager) is an EWMH-compliant pager that integrates nicely into any of the \*box environments. It is not as obtrusive, and is much more readily customizable than many of the other available pagers.

- [x11-misc/bbpager](https://packages.gentoo.org/packages/x11-misc/bbpager) is a desktop pager that was originally written for BlackBox, but works nicely with Openbox as well. It does have some BlackBox dependencies though.

- [x11-plugins/docker](https://packages.gentoo.org/packages/x11-plugins/docker) is the system tray that is made especially for Openbox. It has no extra dependencies, and gives the ability to view and use tray icons for supported GTK and QT-based applications.

- [x11-misc/trayer](https://packages.gentoo.org/packages/x11-misc/trayer) is a system tray that was modified from the FBpanel code, and is often used with FVWM. One of its perks is that it supports transparency.

### Session management

- [lxde-base/lxsession](https://packages.gentoo.org/packages/lxde-base/lxsession) is the stripped down session manager from LXDE. It is designed to remember applications that the user was running at the last logout, and to automatically restart those programs. It also supports the HAL daemon.

- [xfce-base/xfce4-session](https://packages.gentoo.org/packages/xfce-base/xfce4-session) is the session manager from Xfce. It is capable of saving several sessions, and provides methods for logging out, rebooting, and suspending the computer. It does, however, have many Xfce dependencies.

### Configuration tools

- [x11-misc/obconf](https://packages.gentoo.org/packages/x11-misc/obconf) is a GUI application allowing to customize the Openbox window manager without manually editing \~/.config/openbox/rc.conf.

- [lxde-base/lxappearance](https://packages.gentoo.org/packages/lxde-base/lxappearance) is a GTK theme and icon configurator used with LXDE. It provides a nice graphical interface for setting the theme and icons, while depending on very few extra libraries.

- [x11-themes/gtk-chtheme](https://packages.gentoo.org/packages/x11-themes/gtk-chtheme) is a simple application allowing for easier switching of GTK themes and the font. Currently, it does not allow for the switching of icon themes.

- [x11-themes/gtk-theme-switch](https://packages.gentoo.org/packages/x11-themes/gtk-theme-switch) is another simple application that allows users to change their GTK theme.

### Miscellaneous

- [app-admin/conky](https://packages.gentoo.org/packages/app-admin/conky) is a lightweight system monitor that can display over 250 objects, including date and time, CPU usage, memory usage, IMAP/POP3 email, top processes, hardware sensor data, and even info from the music player. It is highly customizable both in appearance and data display. We also have a [Conky configuration guide](https://wiki.gentoo.org/wiki/Conky/Guide) available.

- [app-editors/leafpad](https://packages.gentoo.org/packages/app-editors/leafpad) is a simple text editor. It is very lightweight, but includes features like codeset options, and the ability to undo/redo without limits.

- [media-gfx/feh](https://packages.gentoo.org/packages/media-gfx/feh) is a simple image viewer that runs from the terminal, but it also has many other features. It can display a slideshow of images, create an index print, dynamically zoom, and set the desktop background (detailed instructions above).

- [media-gfx/gpicview](https://packages.gentoo.org/packages/media-gfx/gpicview) is a GUI-based image viewer. Though it has more dependencies than feh, it is incredibly quick to load and run.

- [x11-misc/slim](https://packages.gentoo.org/packages/x11-misc/slim) is the Simple Login Manager, which allows to login to the Openbox session via a graphical interface instead of the terminal. It has very few dependencies, and supports many themes, but should not be used on machines that require remote logins.

### LXDE

If installing each component of a working environment sounds like a little *too* much customization, but the flexibility of Openbox is still wanted, then take a look into a desktop environment that uses Openbox as its default window manager. That environment is [LXDE](https://wiki.gentoo.org/wiki/LXDE), the Lightweight X Desktop Environment. It is designed to require even fewer system resources than Xfce.

### Openbox inside GNOME

To use Openbox as the window manager of an installed [GNOME](https://wiki.gentoo.org/wiki/GNOME) environment, edit the \~/.xinitrc file, and put the following command inside it:

**`~/.xinitrc`**

**Adding an Openbox-GNOME session to .xinitrc**

```
exec openbox-gnome-session
```
Now, when using GDM or another graphic login manager, a new "GNOME/Openbox" option will appear in the session menu. Simply select that option instead of manually editing the \~/.xinitrc.

### Openbox inside KDE

To use Openbox as the window manager of an installed [KDE](https://wiki.gentoo.org/wiki/KDE) environment, edit the \~/.xinitrc file, and replace the current exec command with the following:

**`~/.xinitrc`**

**Adding an Openbox-KDE session to .xinitrc**

```
exec openbox-kde-session
```
Now, when using KDM or another graphic login manager, a new "KDE/Openbox" option will appear in the session menu. Simply select that option instead of manually editing the \~/.xinitrc.

### Openbox inside Xfce

To use Openbox as the window manager of an installed [Xfce](https://wiki.gentoo.org/wiki/Xfce) environment, first start a normal Xfce session, and issue the following command in a terminal:

`user $``killall xfwm4 ; openbox & exit`
Second, exit out of the Xfce session, and make sure to tick the checkbox that says "Save session for future login." This will keep Openbox as the default window manager. Third, notice that the default logout menu action no longer works. To fix this problem, open up the menu.xml file, and locate this line:

**`menu.xml`**

**Exit action**

```
<item label="Exit">
     <action name="Exit"/>
</item>
```
Change it to this:

**`menu.xml`**

**New exit action**

```
<item label="Exit">
  <action name="Execute">
    <command>xfce4-session-logout</command>
  </action>
</item>
```
## External resources

- [urukrama's guide to Openbox](http://urukrama.wordpress.com/openbox-guide/) — contains, along with the rest of the blog, a plethora of information about switching GTK themes, setting up keybindings, desktop effects, and other programs to use in conjunction with Openbox. Though the tutorial was originally written for use with Ubuntu, everything is applicable to other Linux distributions.

- [Box-Look](http://box-look.org/) — provides numerous themes, icons, wallpapers, fonts, and tools to be used with Openbox (as well as the other \*box window managers like Fluxbox, Blackbox, PekWM, etc.)
