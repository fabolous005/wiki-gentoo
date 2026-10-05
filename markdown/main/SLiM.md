<!-- source: https://wiki.gentoo.org/wiki/SLiM | group: Gentoo Wiki (Main) | wiki-title: SLiM -->
---
title: SLiM
url: https://wiki.gentoo.org/wiki/SLiM
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-05-02"
fingerprint: bc98b63b098483b4
license: CC BY-SA 4.0
---

# SLiM

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**SLiM** (**S**imple **L**og**i**n **M**anager) is a desktop-independent graphical [display manager](https://wiki.gentoo.org/wiki/Display_manager). Being fast and having only a few dependencies, it is a popular choice among users of lightweight window managers such as [Openbox](https://wiki.gentoo.org/wiki/Openbox).

## Installation

### USE flags


### Emerge

Install [x11-misc/slim](https://packages.gentoo.org/packages/x11-misc/slim):

`root #``emerge --ask x11-misc/slim`
## Configuration

### Files

Most global configuration is done in the /etc/slim.conf file. However, this references several helper scripts which may contain further configuration.

### Service

#### OpenRC

##### With display-manager

Set SLiM as the default display manager:

**`/etc/conf.d/display-manager`**

```
DISPLAYMANAGER="slim"
```
To start SLiM on boot, add display-manager to the default runlevel:

`root #``rc-update add display-manager default`
To start SLiM now:

`root #``rc-service display-manager start`
##### With the deprecated xdm init script

Set SLiM as the default display manager:

**`/etc/conf.d/xdm`**

```
DISPLAYMANAGER="slim"
```
To start SLiM on boot, add xdm to the default runlevel:

`root #``rc-update add xdm default`
To start SLiM now:

`root #``/etc/init.d/xdm start`
#### systemd

To start SLiM on boot:

`root #``systemctl enable slim`
To start SLiM now:

`root #``systemctl start slim`
### Session selection

SLiM starts the user's preferred desktop session through a helper. There are several options for how this is done.

#### Upstream-style configuration

This method provides full flexibility per user, at the cost of requiring all users to have the helper script in their home directory.

Make the config section look like this:

**`/etc/slim.conf`**

**Modifying slim.conf**

Each user must setup a window manager of choice in their \~/.xinitrc. If the user chooses a different session from the login page, that will be passed as an argument to the script. If $1 is empty it should run the user's default session.

#### Default configuration (Gentoo-style)

By default, SLiM is configured to make all sessions in /etc/X11/Sessions/ accessible — they can be cycled through by pressing the `F1` key. This behavior is acquired by setting the options below:

**`/etc/slim.conf`**

**Editing session behavior**

If no changes are made to /etc/X11/Sessions/ then users will need to press `F1` while logging-in to select the desired session.

In following sections several methods of setting one session as the default are described.

##### Setting a global default session

Set a default session for all users of the computer by modifying the `XSESSION` variable. To do this, create and edit file /etc/env.d/90xsession file. The example below sets the Awesome window manager as the default session.

**`/etc/env.d/90xsession`**

```
XSESSION="awesome"
```
After saving the file run env-update:

`root #``env-update`
##### Setting a default session for one user

Besides (or instead of) setting *global default* sessions, it is possible to let each user choose a default session.

There are three approaches to accomplishing the task:

- Use the SLiM session script (/usr/share/slim/Xsession) to trigger session script from /etc/X11/Sessions/
- Use the SLiM session script to trigger user-created session script
- Force users to set everything by themselves

The first and second possibilities are generally preferred. However, if for any reason you do not want to set any defaults, you may choose the third approach.

###### Per-user default session using bundled session files

The simplest way to set a default session for one user is to make a symbolic link from the session file to \~/.xsession

`user $``ln -s /etc/X11/Sessions/awesome ~/.xsession`
An alternative is to put session command in the \~/.xsession file:

**`~/.xsession`**

You will only need to make sure the file is executable:

`user $``chmod u+x ~/.xsession`
###### Per-user default session using the customized \~/.xsession file

Sometimes you need to customize the launcher script, e.g. to run other programs before starting the window manager or to start a window manager with a customized command. The method above does not allow such modifications, but it can be achieved by using default session script (instead of the one provided by window manager) and launching WM through the \~/.xsession file.

To run your sessions this way, make sure you have no global default session set (or set it to `XSESSION="custom"` in /etc/env.d/90xsession like described above). Then edit the \~/.xsession file located in the user's home directory, putting the window manager launcher at the end.

The file may look like this:

**`~/.xsession`**

```
#!/bin/sh
# Run urxvt daemon
urxvtd -q -o -f
# Set xserver parameters
xset s 0
xset dpms 0 0 1800
# Launch awesome
exec /usr/bin/awesome
```
Finally, remember to make sure the file is executable:

`user $``chmod u+x ~/.xsession`
###### Per-user default session without using any default session files

If you do not like **any** defaults from /usr/share/slim/Xsession and /etc/X11/Sessions/Xsession, you can let your users set everything by themselves. It is generally not a good idea, but sometimes you may need it.

Start by editing /etc/slim.conf in the following way:

**`/etc/slim.conf`**

In the *sessions* line you may put the names of window managers you are planning to use.

Then create and edit \~/.xinitrc file, which may look as below:

**`~/.xinitrc`**

```
#!/bin/sh
#
# Custom xinitrc file for Gentoo
DEFAULT_SESSION="awesome"
# Redirect errors to a file in user's home directory if we can
for errfile in "$HOME/.wm-errors" "${TMPDIR-/tmp}/wm-$USER" "/tmp/wm-$USER"
do
    if ( cp /dev/null "$errfile" 2> /dev/null )
    then
        chmod 600 "$errfile"
        exec > "$errfile" 2>&1
        break
    fi
done
# Define Xresources
userresources=$HOME/.Xresources
# Merge what is available
if [ -f "$userresources" ]; then
    xrdb -merge "$userresources"
fi
# Run urxvt daemon
urxvtd -q -o -f
# Set xserver parameters
xset s 0
xset dpms 0 0 1800
# Start WM
case $1 in 
awesome|i3)
        exec $1
        ;;
*)
        exec $DEFAULT_SESSION
        ;;
esac
```
Finally make sure the file is executable:

`user $``chmod u+x ~/.xinitrc`
### More tweaks

#### Theme selection

Theme selection is done by changing the following line:

**`/etc/slim.conf`**

You can easily find what themes are available on your system:

`user $``ls /usr/share/slim/themes/`
You can preview a theme by running the following command while Xorg is running:

`user $``slim -p /usr/share/slim/themes/<theme name>`
#### NumLock state on login

The NumLock key can be turned on or off by default:

**`/etc/slim.conf`**

#### Autologin

Change `<USER>` in the example below to an appropriate user name:

**`/etc/slim.conf`**

#### Unlock keyrings

The following section will describe how to make SLiM automatically unlock different keychains automatically when you log in.

##### GNOME Keyring

First make sure you have [gnome-base/gnome-keyring](https://packages.gentoo.org/packages/gnome-base/gnome-keyring) (include [app-crypt/seahorse](https://packages.gentoo.org/packages/app-crypt/seahorse) if you want a GUI). To configure SLiM to unlock your GNOME Keyring automatically you have to edit its PAM configuration file. To do so open /etc/pam.d/slim and modify it to look similar to the text below. Lines ending with comment `#keyring` should be added.

**`/etc/pam.d/slim`**

Once this change has been made your keyring should be automatically unlocked next time you log in.

## Troubleshooting

### Failed to connect to socket /var/run/dbus/system\_bus\_socket

This error is caused when [D-Bus](https://wiki.gentoo.org/wiki/D-Bus) hasn't been started. It is possible that it wasn't added to the default run level. To fix this type the following:

`root #``rc-update add dbus default`
This will ensure that D-Bus is running when you boot up your computer. It may be that this has already been added by other window manager, but I had to do this when using awesome WM.

### SLiM starts but leaves a blank screen after login

This error is caused when SLiM is built with the `elogind` USE flag, and required alongside

`root #``rc-update add xdm default`
the use of

`root #``rc-update add elogind default`
### On fresh installs, slim starts but fails to load any sessions

In this error, slim will start and be responsive (you can type the username for example) but after the login no sessions will be loaded and you will get a blank screen. Pressing F1 to change the session will result in nothing. This can occur when dbus is not being loaded when the system starts. To add dbus to the default runlevel, use:

`root #``rc-update add dbus default`
## See also

- [SDDM](https://wiki.gentoo.org/wiki/SDDM) — a modern [display manager](https://wiki.gentoo.org/wiki/Display_manager) that supports both the [X server](https://wiki.gentoo.org/wiki/X_server) and the [Wayland](https://wiki.gentoo.org/wiki/Wayland) protocol.
- [LightDM](https://wiki.gentoo.org/wiki/LightDM)
