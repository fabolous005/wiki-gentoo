<!-- source: https://wiki.gentoo.org/wiki/Openbox | group: Gentoo Wiki (Main) | wiki-title: Openbox -->
---
title: Openbox
url: https://wiki.gentoo.org/wiki/Openbox
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-20"
fingerprint: "3ea0fc7e431795d0"
license: CC BY-SA 4.0
---

# Openbox

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



**Openbox** is a highly configurable stacking [window manager](https://wiki.gentoo.org/wiki/Window_manager) for [X11](https://wiki.gentoo.org/wiki/X11) with extensive standards support. It was originally forked from [Blackbox](https://wiki.gentoo.org/wiki/Blackbox). It is used by the [LXDE](https://wiki.gentoo.org/wiki/LXDE) as its default window manager and [KDE](https://wiki.gentoo.org/wiki/KDE) can also use Openbox as its window manager instead of its default window manager, KWin.

## Installation

### USE flags


| [branding](https://packages.gentoo.org/useflags/branding) | Enable Gentoo specific branding | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [imlib](https://packages.gentoo.org/useflags/imlib) | Add support for imlib, an image loading and rendering library | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [session](https://packages.gentoo.org/useflags/session) | Enables support for session managers | 
| [startup-notification](https://packages.gentoo.org/useflags/startup-notification) | Enable application startup event feedback mechanism | 
| [svg](https://packages.gentoo.org/useflags/svg) | Add support for SVG (Scalable Vector Graphics) | 
| [xdg](https://packages.gentoo.org/useflags/xdg) | Install the python xdg files for xdg autostart | 

### Emerge

Emerge Openbox:

`root #``emerge --ask x11-wm/openbox`
## Configuration

There are four configuration files:

/etc/xdg/openbox/ contains the system-wide default version of these files. \~/.config/openbox/ is used to store the user-specific custom version of these files, though it needs to be created and populated.

For example:

`user $````
mkdir -p ~/.config/openbox/
```
`user $````
cp /etc/xdg/openbox/* ~/.config/openbox/
```
`user $``vim ~/.config/openbox/autostart`
### Starting

To start Openbox it is possible to use a [display manager](https://wiki.gentoo.org/wiki/Display_manager) or the startx command.

To use startx, set the `XSESSION` variable your shell to `openbox`, for example:

**`~/.bashrc`**

**Openbox with startx**

```
# ...other .bashrc content...
export XSESSION=openbox
```
Note that there are an infinite number of ways of achieving the above and this example only applies to bash for users using \~/.bashrc and those who wish to start X manually from the command line.

Also note that a common misconception is to create a \~/.xinitrc file - please do not do that, as it will prevent X from executing all the automatically provided scripts in /etc/X11/xinit/xinitrc.d/ - if you wish to add anything in particular, you can create an executable file in that directory.

### autostart

The autostart script is the place to execute specific commands and programs when Openbox starts. It makes no difference if Openbox is started using the startx command or a display manager, autostart will be executed either way.

**`~/.config/openbox/autostart`**

**Openbox autostart example**

```
# Sets the wallpaper to an image of choice.
wallpaper="planet.jpeg"
# Sets the DESKTOP_ENV variable to "OPENBOX"
DESKTOP_ENV="OPENBOX"
# The following are examples on how to call programs;
# unless programs are called in a daemon mode like the 
# conky example below an ampersand (&) will need to be included 
# in order to start the command and fork it to the background
# See job control for more information.
conky --daemonize
# Waits one second then starts the NetworkManager applet.
sleep 1 && nm-applet --sm-disable &
# Starts a light-weight X11 desktop panel.
fbpanel &
# Starts a lightweight image viewer to display the wallpaper.
feh --bg-scale ~/Artwork/${wallpaper} &
# Starts a simple battery monitor for the system tray.
batti &
# Starts a program that turns on numlock in X (after the X server starts).
numlockx &
```
### environment

The environment script is the place to export global variables and configure the Openbox environment.

**`~/.config/openbox/environment`**

**Openbox environment example**

```
eval $(gpg-agent --daemon)
eval $(gnome-keyring-daemon)
if which dbus-launch >/dev/null && test -z "$DBUS_SESSION_BUS_ADDRESS"; then
       eval `dbus-launch --sh-syntax --exit-with-session`
fi
LANG="el_GR.UTF8"
```
The menu.xml file defines the right-click Openbox menus. By default the right-click menu is predefined with some common applications, so unless the applications are installed on the system most of the default links on the menu will not be operational.

#### Dynamic generation

openbox-menu can dynamically generate new Openbox right-click menus based off information defined in the /etc/xdg/menus/ directory.

Install [x11-misc/openbox-menu](https://packages.gentoo.org/packages/x11-misc/openbox-menu):

`root #``emerge --ask x11-misc/openbox-menu`
To create a new menu for the present user:

`user $``openbox-menu`
obmenu-generator can generate either static or dynamic (pipe) menus based on a user-modifiable schema and the .desktop files installed by most applications. It supports themed icons and arbitrarily nested submenus.

Install [x11-misc/obmenu-generator](https://packages.gentoo.org/packages/x11-misc/obmenu-generator):

`root #``emerge --ask x11-misc/obmenu-generator`
To generate a static menu with icons:

`user $``obmenu-generator -i -s`
To generate a pipe menu with icons:

`user $``obmenu-generator -i -p`
Add '-c' to automatically reconfigure a running instance of Openbox:

`user $``obmenu-generator -i -s -c`
##### MenuMaker

maker can automatically configure your menu, and also works with JWM, IceWM, BlackBox, FluxBox, PekWM and others.

Install  [x11-misc/menumaker](https://packages.gentoo.org/packages/x11-misc/menumaker):

`root #``emerge --ask x11-misc/menumaker`
To generate a static menu for OpenBox with sakura terminal:

`user $``mmaker -vf OpenBox -t Sakura`
If no terminal is specified, xtem will be used by default. You can check the availiable terminal emulators, terminals and otehr options by running:

`user $``mmaker --help`
#### Manual configuration

**`~/.config/openbox/menu.xml`**

**Openbox menu.xml example**

```
<?xml version="1.0" encoding="UTF-8"?>
<openbox_menu>
    <menu id="root-menu" label="OpenBox 3">
        <menu id="2" label="Editors">
            <item label="Mousepad"> <action name="Execute">
                <execute>mousepad</execute>
            </action> </item>
            <item label="Xournal"> <action name="Execute">
                <execute>xournal</execute>
            </action> </item>
        </menu>
        <menu id="18" label="Games">
            <item label="Nibbles"> <action name="Execute">
                <execute>gnome-nibbles</execute>
            </action> </item>
            <item label="DeSmuME (Gtk)"> <action name="Execute">
                <execute>desmume</execute>
            </action> </item>
        </menu>
        <menu id="5" label="Network">
            <item label="FireFox"> <action name="Execute">
                <execute>firefox</execute>
            </action> </item>
        </menu>
        <menu id="19" label="File Managers">
            <item label="File Manager PCManFM"> <action name="Execute">
                <execute>pcmanfm</execute>
            </action> </item>
        </menu>
        <separator/>
        <menu id="40" label="OpenBox">
            <menu id="client-list-menu"/>
            <item label="Reconfigure"> <action name="Reconfigure"/> </item>
            <separator/>
            <item label="Exit"> <action name="Exit"/> </item>
        </menu>
    </menu>
</openbox_menu>
```
In this example shown is a couple of applications in different categories. Each submenu goes inside the root `<menu>` which must go inside the `<openbox_menu>` tag. Specify a name for the subcategory to be displayed with the label "attribute", also specify an `"id"` for each submenu inside the main menu tag, which must be unique, but can be any positive number aside from 1 which is reserved for the parent `<menu tag>`. Items inside menus are specified with the `<item>` tag, and the name to be displayed is as in the `<menu>` tag case specified with the `"label"` attribute. The actions executed by the item are specified with the tag `<action>` which will likely want to used with the `label name="Execute"`, in that case, inside the `<action>` tag to define an `<execute>` tag and inside that tag place a terminal instruction to be called when the user clicks that button, for example `"firefox"` to start Firefox or `"loginctl poweroff"` to turn off the system, to give some examples.

On each `<item>`tag there's also the possibility to specify an icon using the `"icon"` attribute with must be filled with a path pointing to the icon wanting to display in the menu.

### rc.xml

The rc.xml file defines Openbox behavior, keyboard bindings, and mouse bindings.

The following is a list of special key 'modifiers':

| Key | Description | 
|---|---|
| S | `Shift` key | 
| C | `Ctrl` key | 
| A | `Alt` key | 
| W | Super (windows) key | 
| M | Meta key | 
| H | Hyper key | 

To make a key binding, combine modifiers and a key. They are separated with the `-` (dash) sign.

#### Key binding examples

| Keybinding | Modifier | 
|---|---|
| `Alt`+`o` | A-o | 
| `Ctrl`+`Alt`+`x` | C-A-x | 
| `Ctrl`+`Alt`+`Shift`+`y` | C-A-S-y | 
| `Shift`+`s` | S-s | 

To open a xterm window with **Super+`t`**, add the following to the rc.xml file:

**`~/.config/openbox/rc.xml`**

**XTerm key bind example**

```
<keybind key="W-t">
  <action name="Execute">
    <command>xterm</command>
  </action>
</keybind>
```
To open Openbox's right-click menu with **Super+`x`**, and Openbox's middle-click menu with **Super+`z`**, add the following to the rc.xml file:

**`~/.config/openbox/rc.xml`**

**Root-Menu key bind example**

```
<keybind key="W-x">
  <action name="ShowMenu">
    <menu>root-menu</menu>
  </action>
</keybind>
<keybind key="W-z">
  <action name="ShowMenu">
    <menu>client-list-combined-menu</menu>
  </action>
</keybind>
```
## Tips

Openbox is able to have icons next to menu entries.

1. To emerge Openbox with imlib support add `imlib` USE flag to x11-wm/openbox in /etc/portage/package.use `root #``echo "x11-wm/openbox imlib" >> /etc/portage/package.use`
2. Re-emerge Openbox so that support for the `imlib` USE flag is considered: `root #``emerge --ask --changed-use x11-wm/openbox`
3. Add a \<showIcons>yes\</showIcons> line to the \<menu> section of the rc.xml file.
4. Add in menu.xml icon="\<path>" like this: FILE**`~/.config/openbox/menu.xml`****Manually add icons to Openbox example** \<menu label="Shells" icon="/usr/share/icons/shell.png"> \<item label="xterm" icon="/usr/share/icons/xterm.png"> \<action name="Execute"> \<execute>xterm\</execute> \</action> \</item>

## See also

- [Tint2](https://wiki.gentoo.org/wiki/Tint2) — a lightweight panel/taskbar specifically made for [Openbox], but it can also work with other window managers.
- [Openbox/Guide](https://wiki.gentoo.org/wiki/Openbox/Guide) — covers the basics on installing and configuring the OpenBox window manager.
- [LXDE](https://wiki.gentoo.org/wiki/LXDE) — a free desktop environment with comparatively low resource requirements.
