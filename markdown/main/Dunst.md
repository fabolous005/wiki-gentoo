<!-- source: https://wiki.gentoo.org/wiki/Dunst | group: Gentoo Wiki (Main) | wiki-title: Dunst -->
---
title: Dunst
url: https://wiki.gentoo.org/wiki/Dunst
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-10-26"
tags: ['Release v1.5.0 · dunst-project/dunst']
fingerprint: "763ddc4e9fa75baa"
license: CC BY-SA 4.0
---

# Dunst

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**dunst** is a lightweight replacement for the notification daemons provided by most desktop environments. It is very customizable and does not depend on any toolkits.

## Installation

### Review the USE flags


### USE flags for
            [x11-misc/dunst](https://packages.gentoo.org/packages/x11-misc/dunst)
            
            Lightweight replacement for common notification daemons

| [+X](https://packages.gentoo.org/useflags/+X) | Add support for X11 | 
| [+completions](https://packages.gentoo.org/useflags/+completions) | Install shell completions (for bash, fish and zsh) | 
| [+dunstify](https://packages.gentoo.org/useflags/+dunstify) | Build dunstify (notify-send alternative) | 
| [+xdg](https://packages.gentoo.org/useflags/+xdg) | Install xdg-utils for opening links with xdg-open | 
| [wayland](https://packages.gentoo.org/useflags/wayland) | Enable dev-libs/wayland backend | 

### Emerge dunst

`root #``emerge --ask --verbose x11-misc/dunst`
### Unmerge other notification daemons

In order to avoid confusion other notification daemons could be removed, e.g. [x11-misc/notification-daemon](https://packages.gentoo.org/packages/x11-misc/notification-daemon):

`root #``emerge --ask --verbose --depclean x11-misc/notification-daemon`
### Start dunst

[D-Bus](https://wiki.gentoo.org/wiki/D-Bus) should start a notification daemon automatically, but if multiple are installed then it may just pick one. Starting dunst before any other notification daemons are fired up will make sure that dunst will handle your notifications. Review the used desktop setup on how to auto-start programs.

#### Start with Sway

**`~/.config/sway/config`**

**Start Dunst with Sway**

```
exec dunst &
```
#### Start with Hyprland

**`~/.config/hypr/hyprland.conf`**

**Start Dunst with Hyprland**

```
exec-once = dunst &
```
## Configuration

After the installation there is a working configuration file /etc/xdg/dunst/dunstrc. Edit this file to customize the settings for all users, or copy it to $XDG\_CONFIG\_HOME/dunst/dunstrc for setting for a single user.

`user $``cp -r /etc/xdg/dunst ~/.config/dunst`
## Usage

Test dunst by creating a notification with the dunstify command:

`user $``dunstify "Title" "Content"`
Dunst provides a client called dunstctl<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>. The dunstctl client supports passing commands to the running daemon.

`user $``dunstctl --help````
Commands:
  action                            Perform the default action, or open the
                                    context menu of the notification at the
                                    given position
  close                             Close the last notification
  close-all                         Close the all notifications
  context                           Open context menu
  count [displayed|history|waiting] Show the number of notifications
  history                           Display notification history (in JSON)
  history-pop [ID]                  Pop the latest notification from
                                    history or optionally the
                                    notification with given ID.
  is-paused                         Check if dunst is running or paused
  set-paused [true|false|toggle]    Set the pause status
  rule name [enable|disable|toggle] Enable or disable a rule by its name
  debug                             Print debugging information
  help                              Show this help
```
All currently displayed notification can be cleared as:

`user $``dunstctl close-all`
After modifying the configuration file use the killall dunst command, to apply new configuration:

`user $``killall dunst`
## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) [Release v1.5.0 · dunst-project/dunst](https://github.com/dunst-project/dunst/releases/tag/v1.5.0), GitHub. Retrieved on March 10, 2022
