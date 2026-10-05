<!-- source: https://wiki.gentoo.org/wiki/Sway | group: Gentoo Wiki (Main) | wiki-title: Sway -->
---
title: Sway
url: https://wiki.gentoo.org/wiki/Sway
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-02"
fingerprint: ae159e7b5997fb9c
license: CC BY-SA 4.0
---

# Sway

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Sway** (contracted from **S**irCmpwn's **Way**land compositor) is an open-source [wlroots](https://wiki.gentoo.org/wiki/Wlroots)-based [Wayland compositor](https://wiki.gentoo.org/wiki/Wayland_compositor) that is designed to be compatible with the [i3](https://wiki.gentoo.org/wiki/I3) window manager.

## Installation

### USE flags


| [+drm](https://packages.gentoo.org/useflags/+drm) | Enable support for gui-libs/wlroots compiled with USE=drm | 
| [+filecaps](https://packages.gentoo.org/useflags/+filecaps) | Use Linux file capabilities to control privilege rather than set\*id (this is orthogonal to USE=caps which uses capabilities at runtime e.g. libcap) | 
| [+libinput](https://packages.gentoo.org/useflags/+libinput) | Enable support for gui-libs/wlroots compiled with USE=libinput | 
| [+session](https://packages.gentoo.org/useflags/+session) | Enable support for gui-libs/wlroots compiled with USE=session | 
| [+swaybar](https://packages.gentoo.org/useflags/+swaybar) | Install 'swaybar': sway's status bar component | 
| [+swaynag](https://packages.gentoo.org/useflags/+swaynag) | Install 'swaynag': shows a message with buttons | 
| [X](https://packages.gentoo.org/useflags/X) | Enable support for X11 applications (XWayland) | 
| [tray](https://packages.gentoo.org/useflags/tray) | Enable support for StatusNotifierItem tray specification | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 
| [wallpapers](https://packages.gentoo.org/useflags/wallpapers) | Install sway's default wallpaper image | 
| [x11-backend](https://packages.gentoo.org/useflags/x11-backend) | Enable support for gui-libs/wlroots compiled with USE=x11-backend | 

### Emerge

`root #``emerge --ask gui-wm/sway`
## Configuration

To view all available configuration options:

`user $``man 5 sway`
### Files

Each user running sway can edit the default configuration file in order to run a customized sway session. Gentoo stores this file at its default /etc/sway/config location:

`user $````
mkdir -p ~/.config/sway/
```
`user $````
cp /etc/sway/config ~/.config/sway/
```
### Terminal emulator

By default the Sway configuration file uses the [foot](https://wiki.gentoo.org/wiki/Foot) [terminal emulator](https://wiki.gentoo.org/wiki/Terminal_emulator) (found in the [gui-apps/foot](https://packages.gentoo.org/packages/gui-apps/foot) package). It is a good idea to emerge this terminal emulator so that a terminal will be available once Sway is running:

`root #``emerge --ask gui-apps/foot`
Other popular choices include [x11-terms/alacritty](https://packages.gentoo.org/packages/x11-terms/alacritty) or [x11-terms/kitty](https://packages.gentoo.org/packages/x11-terms/kitty), which works natively with Wayland if the `KITTY_ENABLE_WAYLAND` environment variable is set to `1`.

Another very lightweight alternative is [st](https://wiki.gentoo.org/wiki/St), but it isn't Wayland native.

### Display configuration

Display options can be queried with:

`user $``swaymsg -t get_outputs````
Output DP-1 'HP Inc. HP X34 6CM2261GK2' (focused)
  Current mode: 3440x1440 @ 165.000 Hz
  Position: 0,0
  Scale factor: 1.000000
  Scale filter: nearest
  Subpixel hinting: unknown
  Transform: normal
  Workspace: 1
  Max render time: off
  Adaptive sync: disabled
  Available modes:
    3440x1440 @ 165.000 Hz
 
...
 
Output DP-2 'LG Electronics LG HDR QHD 110NTTQ0U193'
  Current mode: 2560x1440 @ 59.951 Hz
  Position: 3440,0
  Scale factor: 1.000000
  Scale filter: nearest
  Subpixel hinting: unknown
  Transform: normal
  Workspace: 2
  Max render time: off
  Adaptive sync: disabled
  Available modes:
    2560x1440 @ 59.951 Hz
    2560x1440 @ 74.971 Hz
 
...
 
Output DP-3 'Ancor Communications Inc VE247 E3LMQS103610'
  Current mode: 1920x1080 @ 60.000 Hz
  Position: 6000,0
  Scale factor: 1.000000
  Scale filter: nearest
  Subpixel hinting: unknown
  Transform: normal
  Workspace: 3
  Max render time: off
  Adaptive sync: disabled
  Available modes:
    1920x1080 @ 60.000 Hz
```
The results have been shortened to only contain the desired resolution. The default positions are not configured properly, and can be adjusted by modifying \~/.config/sway/config. Once the file is saved, the configuration can be reloaded with `$mod`+`Shift`+`C`

**`~/.config/sway/config`**

**Configure the left display which is physically slightly larger than the primary display**

**`~/.config/sway/config`**

**Configure primary display which is centered**

**`~/.config/sway/config`**

**Configure alternate display which is vertical**

### Input Devices

Input devices can be queried with:

`user $``swaymsg -t get_inputs`
Input device: Logitech G502 HERO Gaming Mouse Keyboard
  Type: Mouse
  Identifier: 1133:49291:Logitech\_G502\_HERO\_Gaming\_Mouse\_Keyboard
  Product ID: 49291
  Vendor ID: 1133
  Libinput Send Events: enabled
 
Input device: Logitech G502 HERO Gaming Mouse Keyboard
  Type: Keyboard
  Identifier: 1133:49291:Logitech\_G502\_HERO\_Gaming\_Mouse\_Keyboard
  Product ID: 49291
  Vendor ID: 1133
  Active Keyboard Layout: English (US)
  Libinput Send Events: enabled
 
Input device: Logitech G502 HERO Gaming Mouse
  Type: Mouse
  Identifier: 1133:49291:Logitech\_G502\_HERO\_Gaming\_Mouse
  Product ID: 49291
  Vendor ID: 1133
  Libinput Send Events: enabled

**`~/.config/sway/config`**

**Disable mouse acceleration, decrease pointer speed**

**`~/.config/sway/config`**

**Enable touchpad tap to click**

### Application launcher

Sway works with a variety of application launchers. By default it attempts to use [gui-apps/wmenu](https://packages.gentoo.org/packages/gui-apps/wmenu), it's a wayland native launcher.

[dev-libs/bemenu](https://packages.gentoo.org/packages/dev-libs/bemenu) is a dynamic menu library and client program inspired by dmenu. to configure sway to use  bemenu:

`root #``emerge --ask dev-libs/bemenu`
**`~/.config/sway/config`**

**Configure sway to use bemenu.**

**`~/.config/sway/config`**

**Configure sway to use bemenu, with an empty prompt.**

By default sway tries to use [gui-apps/wmenu](https://packages.gentoo.org/packages/gui-apps/wmenu), which can be installed with:

`root #``emerge --ask gui-apps/wmenu`
To configure sway to simply use wmenu:

**`~/.config/sway/config`**

**Configure Sway to use wmenu**

**`~/.config/sway/config`**

**Configure Sway to use wmenu with no prompt**

### Status bar

In addition to Sway's own status bar, [Waybar](https://wiki.gentoo.org/wiki/Waybar) can be used as a highly customizable status bar for Sway:

`root #``emerge --ask gui-apps/waybar`
There are two simple ways to enable Waybar in Sway: executing Waybar as a bar subcommand, or as a regular command. Both will behave the same when Sway starts up (and when Sway exits and starts up again), but they differ in the reloading of Sway. Executing Waybar, or any other status bar, as a bar subcommand will restart the status bar on Sway reload. Executing Waybar as a regular command will *not* restart the status bar on Sway reload; executing with `exec_always` instead of `exec` does not solve this, it only makes more status bars on each Sway reload.

Users that wish to quickly test their status bar configurations via a Sway reload should use the bar subcommand method.

**`~/.config/sway/config`**

**Enable Waybar via a bar subcommand**

**`~/.config/sway/config`**

**Enable Waybar via a regular command**

### Brightness

There are several options for adjusting the backlight brightness, it can even be done by writing to /sys/class/backlight/\<device>/brightness.

#### acpilight

Alternatively, [sys-power/acpilight](https://packages.gentoo.org/packages/sys-power/acpilight) can also accomplish the same brightness changes via a xbacklight compatible command:

**`~/.config/sway/config`**

**Set the keyboard shortcuts for screen brightness support**

```
 XF86MonBrightnessDown exec xbacklight -dec 2
bindsym XF86MonBrightnessUp exec xbacklight -inc 4
```
To permit non-root users to update brightness values, [Udev](https://wiki.gentoo.org/wiki/Udev) rules can be used to permit non-root access to /sys/class/backlight/%k/brightness. Here is an example:

**`/etc/udev/rules.d/90-backlight.rules`**

**Allow non-root users to access backlights**

```
ACTION=="add", SUBSYSTEM=="backlight", KERNEL=="intel_backlight", RUN+="/bin/sh -c '/bin/chmod 666 /sys/class/backlight/%k/brightness'"
ACTION=="add", SUBSYSTEM=="leds", KERNEL=="intel_backlight", RUN+="/bin/sh -c '/bin/chmod 666 /sys/class/backlight/%k/brightness'"
```
#### brightnessctl

[app-misc/brightnessctl](https://packages.gentoo.org/packages/app-misc/brightnessctl) is in **::guru** and can be used to simply adjust the backlight:

**`~/.config/sway/config`**

**Add brightnessctl keybinds**

```
 XF86MonBrightnessDown exec brightnessctl set 5%-
bindsym XF86MonBrightnessUp exec brightnessctl set 5%+
```


#### light

[dev-libs/light](https://packages.gentoo.org/packages/dev-libs/light) can be used to adjust backlights and brightness. Here is an example config:

**`~/.config/sway/config`**

**Set the keyboard shortcuts for screen brightness support**

```
 XF86MonBrightnessDown exec light -U 2
bindsym XF86MonBrightnessUp exec light -A 4
```
#### ddcutil

For external monitors, you can use [app-misc/ddcutil](https://packages.gentoo.org/packages/app-misc/ddcutil) to control the brightness via [I<sup>2</sup>C](https://wiki.gentoo.org/wiki/I2C):

**`~/.config/sway/config`**

**Add ddcutil keybinds**

```
 XF86MonBrightnessDown exec ddcutil setvcp 10 - 5
bindsym XF86MonBrightnessUp exec ddcutil setvcp 10 + 5
```
### Notification

[gui-apps/mako](https://packages.gentoo.org/packages/gui-apps/mako) can be used as a notification daemon.

**`~/.config/sway/config`**

**Launch mako daemon when Sway starts**

```
exec mako
```
### Sound volume

The `XF86AudioRaiseVolume` and `XF86AudioLowerVolume` keycode are generally present and used to adjust the system volume. This must be bound and set depending on the audio backend.

#### Pipewire

If [PipeWire](https://wiki.gentoo.org/wiki/PipeWire) is being used, the following configuration can be used for changing sound volume (with [Wireplumber](https://wiki.gentoo.org/wiki/Wireplumber)):

**`~/.config/sway/config`**

**Set the keyboard shortcuts to change sound volume for PipeWire**

```
 XF86AudioMute exec wpctl set-mute @DEFAULT_SINK@ toggle
# tip: you might consider adding `--limit 1.0` to avoid going over 100% volume
bindsym XF86AudioRaiseVolume exec wpctl set-volume @DEFAULT_SINK@ 5%+
bindsym XF86AudioLowerVolume exec wpctl set-volume @DEFAULT_SINK@ 5%-
bindsym XF86AudioMicMute exec wpctl set-mute @DEFAULT_SOURCE@ toggle
```
#### Pulseaudio

If [pulseaudio](https://wiki.gentoo.org/wiki/Pulseaudio) is being used, the following configuration can be used for changing sound volume:

**`~/.config/sway/config`**

**Set the keyboard shortcuts to change sound volume for pulseaudio**

```
 XF86AudioMute exec pactl set-sink-mute @DEFAULT_SINK@ toggle
bindsym XF86AudioRaiseVolume exec pactl set-sink-volume @DEFAULT_SINK@ +5%
bindsym XF86AudioLowerVolume exec pactl set-sink-volume @DEFAULT_SINK@ -5%
bindsym XF86AudioMicMute exec pactl set-source-mute @DEFAULT_SOURCE@ toggle
```
#### ALSA

If [ALSA](https://wiki.gentoo.org/wiki/ALSA) is being used, the following configuration can be used for changing the sound volume:

**`~/.config/sway/config`**

**Set the keyboard shortcuts to change sound volume for ALSA**

```
 XF86AudioRaiseVolume exec amixer -Mq set Speaker 5%+
bindsym XF86AudioLowerVolume exec amixer -Mq set Speaker 5%-
```
#### sndio

If [media-sound/sndio](https://packages.gentoo.org/packages/media-sound/sndio) is being used, the following configuration can be used for changing the sound volume:

**`~/.config/sway/config`**

**Set the keyboard shortcuts to change sound volume for sndio**

```
 XF86AudioRaiseVolume exec sndioctl -f snd/default output.level=+0.05
bindsym XF86AudioLowerVolume exec sndioctl -f snd/default output.level=-0.05
```
### Taking screenshots

#### Simple approach: use slurpshot

([Slurpshot](https://github.com/de-arl/slurpshot)) is a script to simplify taking screenshots. It uses native wayland apps only and enables selecting specific windows only, as well as previewing and printing screenshots withous saving them. First install dependencies:

`root #``emerge --ask gui-apps/grim gui-apps/slurp app-misc/jq dev-libs/bemenu`
Put the slurpshot script somewhere in your PATH, for example to \~/bin, make it executable and just set one keybind:

**`~/.config/sway/config`**

**Set the keyboard shortcuts for slurpshot support**

```
#
# Screen capture
#
bindsym Print exec slurpshot
```
#### Manual approach

To add screenshot support, use the grim utility (found in the [gui-apps/grim](https://packages.gentoo.org/packages/gui-apps/grim) package). The abbreviation `grim` is defined as **Gr**ab **Im**ages. This utility is tailored to the specifics of the Wayland protocol. In order to install grim, use the following command:

`root #``emerge --ask gui-apps/grim`
To add support for determining the boundaries of the selected screen area, the slurp utility, found in the [gui-apps/slurp](https://packages.gentoo.org/packages/gui-apps/slurp) package, is used in combination with the grim utility. To install slurp, use the command:

`root #``emerge --ask gui-apps/slurp`
To add clipboard support wl-clipboard is used, found in [gui-apps/wl-clipboard](https://packages.gentoo.org/packages/gui-apps/wl-clipboard).
To install wl-clipboard, use the command:

`root #``emerge --ask gui-apps/wl-clipboard`
Next, edit the configuration file to add support for keyboard shortcuts to perform a screenshot operation:

**`~/.config/sway/config`**

**Set the keyboard shortcuts for screenshot support**

```
#
# Screen capture
#
set $ps1 Print
set $ps2 Control+Print
set $ps3 Alt+Print
set $ps4 Alt+Control+Print
set $psf $(xdg-user-dir PICTURES)/ps_$(date +"%Y%m%d%H%M%S").png
 
bindsym $ps1 exec grim - | wl-copy
bindsym $ps2 exec grim -g "$(slurp)" - | wl-copy
bindsym $ps3 exec grim $psf
bindsym $ps4 exec grim -g "$(slurp)" $psf
```
Please note that the `Print` or `Ctrl` + `Print` keys combination creates a screenshot in the `wl-copy` buffer. This allows pasting the image directly from the clipboard, without having to save to a file on disk.

For the `Alt` + `Print` or `Alt` + `Ctrl` + `Print` keyboard shortcuts, the method of automatically saving the image file in the Pictures user directory is used.

#### Snipping tool like behavior

The following captures an area of the screen to the clipboard when `mod`+`shift`+`S` is pressed:

**`~/.config/sway/config`**

**Similar function to the snipping tool**

```
 $mod+shift+s exec grim -g "$(slurp)" - | wl-copy
```
### Set a random wallpaper

Default configuration uses [gui-apps/swaybg](https://packages.gentoo.org/packages/gui-apps/swaybg) for wallpapers, other packages are available too. A random wallpaper can be pulled from a folder using **swaybg** and be set: [\[1\]](https://wiki.gentoo.org#cite_note-1)

**`~/.config/sway/config`**

**Set a random wallpaper from a folder**

```
set $wallpapers_path $HOME/Pictures/Wallpapers
output * bg $(find $wallpapers_path -type f | shuf -n 1) fill
```
### Swaylock

[gui-apps/swaylock](https://packages.gentoo.org/packages/gui-apps/swaylock) can be used to lock the current session.

`root #``emerge --ask gui-apps/swaylock`
**`~/.config/sway/config`**

**Lock the session when`$mod`+`l` is pressed**

```
 $mod+l exec swaylock --ignore-empty-password --show-failed-attempts --color 1e1e1e
```
**`~/.config/sway/config`**

**With colors man swaylock for more info**

```
 $mod+l exec swaylock --ignore-empty-password --show-failed-attempts \
    --color 1e1e1e --inside-color cccccc --ring-color ffffff \
    --inside-clear-color 11a8cd --ring-clear-color 29b8db \
    --inside-ver-color 2472c8 --ring-ver-color 3b8eea \
    --inside-wrong-color cd3131 --ring-wrong-color f14c4c
```
### Swayidle

[gui-apps/swayidle](https://packages.gentoo.org/packages/gui-apps/swayidle) runs a command after a certain idle time, typically to lock and/or power off the screen.

`root #``emerge --ask gui-apps/swayidle`
**`~/.config/sway/config`**

**Power off all displays after 15 minutes of idle**

```
exec swayidle -w \
  timeout 900 'swaymsg "output * power off"' \
  resume 'swaymsg "output * power on"'
```
### HiDPI

To adjust sway's rendering for HiDPI displays (4K and above), the name of the display to be adjusted must be obtained. After a sway session is running, issue the following:

`user $``swaymsg -t get_outputs`
The `output` statement in the sway configuration file will accept a `scale` parameter to adjust the scaling of the high resolution display.

### Xresources

**`~/.config/sway/config`**

**Reload`~/.Xresources` on sway reload**

### GTK configuration

#### Dark Mode

##### GTK4

GTK4 dark mode can be enabled by setting:

**`~/.config/gtk-4.0/settings.ini`**

**Enable gtk4 dark mode**

##### GTK3

GTK3 dark mode can be enabled by setting:

**`~/.config/gtk-3.0/settings.ini`**

**Enable gtk3 dark mode**

##### GTK2

GTK2 does not have a dark mode toggle, a dark theme must be selected:

**`~/.gtkrc-2.0`**

**Enable gtk2 dark mode**

#### GTK3 Themes and Fonts

Currently setting a GTK font and theme should be done by editing sway's configuration file (see [Sway's wiki](https://github.com/swaywm/sway/wiki/GTK-3-settings-on-Wayland) as well):

**`~/.config/sway/config`**

**Set the font and theme for GTK applications**

```
set $gnome-schema org.gnome.desktop.interface
exec_always {
    gsettings set $gnome-schema gtk-theme 'theme name'
    gsettings set $gnome-schema icon-theme 'icon theme name'
    gsettings set $gnome-schema cursor-theme 'cursor theme name'
    gsettings set $gnome-schema font-name 'Sans 10'
}
```
If encountering problems setting the mouse cursor with certain applications (including sway), this may help:

**`~/.config/sway/config`**

**Set the cursor theme**

```
 seat0 xcursor_theme custom_cursor_theme custom_cursor_size
```
Replace *custom\_cursor\_theme* and *custom\_cursor\_size*. **Adwaita** and **24** are pretty much default on all Linux distributions.

### Automatic floating windows

By default, Sway opens new windows in tiling mode. The following configuration snippet makes many common windows which should float, float:

```
 [window_role = "pop-up"] floating enable
for_window [window_role = "bubble"] floating enable
for_window [window_role = "dialog"] floating enable
for_window [window_type = "dialog"] floating enable
for_window [window_role = "task_dialog"] floating enable
for_window [window_type = "menu"] floating enable
for_window [app_id = "floating"] floating enable
for_window [app_id = "floating_update"] floating enable, resize set width 1000px height 600px
for_window [class = "(?i)pinentry"] floating enable
for_window [title = "Administrator privileges required"] floating enable
```
#### Firefox Tweaks

```
 [title = "About Mozilla Firefox"] floating enable
for_window [window_role = "About"] floating enable
for_window [app_id="firefox" title="Library"] floating enable, border pixel 1, sticky enable
```
```
 [title = "Firefox - Sharing Indicator"] kill
for_window [title = "Firefox — Sharing Indicator"] kill
```
#### Steam Tweaks

```
 [class="^(steam$)" title="^(?!Steam$)"] floating enable
```
### Service

#### OpenRC

On [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) based systems, [elogind](https://wiki.gentoo.org/wiki/Elogind) must be added to the *boot* runlevel:

`root #``rc-update add elogind boot`
### Switching Keyboard Layouts

Sway uses [gui-libs/wlroots](https://packages.gentoo.org/packages/gui-libs/wlroots) and supports [x11-libs/libxkbcommon](https://packages.gentoo.org/packages/x11-libs/libxkbcommon) keyboard layouts. Configure the keyboard layout in the `~/.config/sway/config` file or change it dynamically using:

`user $``swaymsg`
#### Temporarily change keyboard layout

To switch the layout of the keyboard for the current session, use the following command:

`user $``swaymsg input type:keyboard xkb_layout "fr"`
Replace `"fr"` with the desired layout code (e.g., `"us"`, `"de"`, `"es"`). This change is immediate, but it will reset when Sway is restarted.

To make the layout persistent across sessions, add the following to your Sway configuration file:

**`~/.config/sway/config`**

**Change the keyboard layout**

```
 type:keyboard {
    xkb_layout "fr"
}
```
#### Switching between layouts with a keybinding

Define multiple layouts and enable a keyboard shortcut to toggle between them. Add this section to the Sway config:

**`~/.config/sway/config`**

**Enable a keyboard shortcut to switch between multiple keyboard layouts**

```
 type:keyboard {
    xkb_layout "us,fr"
    xkb_options "grp:alt_shift_toggle"
}
```
This sets up US and French layouts and allows toggling between them using `Alt`+`Shift`. You can customize the toggle shortcut by changing `xkb_options`.

A full list of options can be found by running:

`root #``cat /usr/share/x11/xkb/rules/base.lst | less`
## Usage

### Starting Sway

#### Launching Sway automatically with TTY login

To start Sway on login to the first TTY:

**`~/.bashrc`**

**Launch Sway after logging into the first TTY**

##### Automatic login on tty1

To enable automatic login on tty1, `--skip-login` and `--login-options` can be added to tty1's agetty instance defined in /etc/inittab:

**`/etc/inittab`**

**Automatically login as Larry on tty1**

#### Starting Sway manually

`user $``dbus-run-session sway`
#### Launching Sway from a script

This method uses a script to forcibly take over a virtual terminal and launch Sway in it. The typical use case is to launch Sway automatically on boot.

**`/usr/sbin/sway_launcher`**

**Sway Launcher**

```
#!/bin/sh
 
# Launch sway with a specific user, from a specific Virtual Terminal (vt)
# Two arguments are expected: a username (e.g., larry) and the id of a free vt (e.g., 7)
 
# prepare the tty for the user. vtX uses /dev/ttyX
chown "$1" "/dev/tty${2}"
chmod 600 "/dev/tty${2}"
 
# setup a clean environment for the user, take over the target vt, then launch sway
su --login --command "openvt --switch --console ${2} -- sway >\${HOME}/.sway_autolauncher.log 2>&1" "$1"
# this script returns immediately
```
This script has a few limitations:

- `XDG_RUNTIME_DIR` is expected to be defined and valid, see the section above.
- Without the `--switch` option for openvt, sway will freeze when trying to switch to a different VT (`Ctrl`+`Alt`+`Fn`), whether this is a bug or not is unknown.
- The VT is not cleared when Sway exits, clear it by calling deallocvt.
- Similarly the TTY's owner and mode are not changed back to their default values when Sway exits.

Launching this script on boot can be done with the [*local*](https://wiki.gentoo.org/wiki//etc/local.d) service:

**`/etc/local.d/sway.start`**

**Launch Sway on boot**

```
#!/bin/sh
sway_launcher larry 7
```
#### Starting Sway without elogind or systemd

Systems that are configured with neither systemd nor elogind will need to create a shell script (or use some other means) to set the `XDG_RUNTIME_DIR` variable.

The environment variable can be defined in the usual configuration files. For example, if  [Larry the cow (Larry)](https://wiki.gentoo.org/wiki/User:Larry)  sets the `XDG_RUNTIME_DIR` variable in his shell's configuration file and he has chosen that the directory will be in /tmp:

**`/home/larry/.bash_profile`**

**Set the`XDG_RUNTIME_DIR` variable**

```
#!/bin/sh
if test -z "${XDG_RUNTIME_DIR}"; then
  export XDG_RUNTIME_DIR=/tmp/"${UID}"-runtime-dir
    if ! test -d "${XDG_RUNTIME_DIR}"; then
        mkdir "${XDG_RUNTIME_DIR}"
        chmod 0700 "${XDG_RUNTIME_DIR}"
    fi
fi
```
With the `XDG_RUNTIME_DIR` defined, sway can be launched as usual:

`user $``dbus-run-session sway`
If issues are encountered, check [Sway issues on GitHub](https://github.com/swaywm/sway/issues) before contacting the Sway community on IRC ([#sway](ircs://irc.libera.chat/#sway) ([webchat](https://web.libera.chat/#sway))) or opening a [new Gentoo bug](https://bugs.gentoo.org/).

### Movement

All key combinations will be defined in the \~/.config/sway/config configuration file.

The `Super` key is defined as the `$mod` value by default. On most keyboards this will be the Windows key.

Sway has a [Vi](https://wiki.gentoo.org/wiki/Vim)-like interface. `h` (left), `j` (down), `k` (up), and `l` (right) can be used for movement, in addition to the arrow keys.

Focus can be moved with `mod`+`direction key`, windows can be moved with `mod`+`shift`+`direction key`:

**`~/.config/sway/config`**

**Default movement definitions**

See man 5 sway-input for more information.

#### Useful binds

**`~/.config/sway/config`**

**Cycle between workspaces with`mod`+`control`+`left arrow` and `mod`+`control`+`right arrow`**

### Layouts

By default, Sway uses a tiling layout. Layout modes can be switched with the following default binds:

- `mod`+`b` - Horizontal split
- `mod`+`v` - Vertical split
- `mod`+`s` - Stacking
- `mod`+`w` - Tabbed
- `mod`+`e` - Toggle split
- `mod`+`shift`+`space` - Toggle floating

**`~/.config/sway/config`**

**Default layout definitions**

### Terminal

The default key combination to open a terminal emulator is `$mod`+`Enter`.

#### Foot Server

[Foot](https://wiki.gentoo.org/wiki/Foot) is a minimal Wayland terminal emulator that can be configured to run as a server, reducing resource usage.

**`~/.config/sway/config`**

**Start the Foot server with Sway**

**`~/.config/sway/config`**

**Set the default terminal to be a foot client**

### Adding features

Sway is designed to be extended, adding additional features is easy:

**`~/.config/sway/config`**

**Start htop when`control`+`shift`+`esc` is pressed**

If using foot, the **app\_id**, which is set with **-a**, can be set to make it float automatically:

**`~/.config/sway/config`**

**Start htop floating when`control`+`shift`+`esc` is pressed**

#### Moving left and right with non-existing workspaces

Sway can switch to the left (`prev`) or right (`next`) workspace as long as there exists a workspace to switch to in that direction; this includes moving containers to those workspaces:

**`~/.config/sway/config`**

To be able to switch to non-existing workspaces, we can create a script to tell Sway to switch to a specific workspace:

**`~/.config/sway/config`**

**`~/.config/sway/workspace.gawk`**

```
#!/bin/gawk -f
 
$3 == "(focused)" {
	switch(move_type) {
	case "left":
		if ($2 == 1)
			$2=num_of_workspaces+1
		system("sway workspace "$2-1)
		exit
	
	case "right":
		if ($2 == num_of_workspaces)
			$2=0
		system("sway workspace "$2+1)
		exit
	
	case "container_left":
		if ($2 == 1)
			$2=num_of_workspaces+1
		system("sway move container to workspace "$2-1", workspace "$2-1)
		exit
	
	case "container_right":
		if ($2 == num_of_workspaces)
			$2=0
		system("sway move container to workspace "$2+1", workspace "$2+1)
		exit
	}
}
```
### Font size adjustment

**`~/.config/sway/config`**

To get current Sway font use

`user $``swaymsg -t get_config | grep font`
For foot terminal:

**`~/.config/foot/foot.ini`**

## Troubleshooting

### Screen sharing does not work

Ensure your Sway configuration isn't setting outputs with render\_bit\_depth 10, as only render\_bit\_depth 8 is supported for screen sharing.

Make sure the package [gui-libs/xdg-desktop-portal-wlr](https://packages.gentoo.org/packages/gui-libs/xdg-desktop-portal-wlr) is installed. By default, it is autostarted by D-Bus but it fails to run because it needs environment variables exported by Sway, and the D-Bus session is started before Sway. To fix, update the D-Bus environment by adding the following line to the beginning of Sway's config:

**`~/.config/sway/config`**

```
exec --no-startup-id dbus-update-activation-environment --all
```
Also see [this link](https://github.com/emersion/xdg-desktop-portal-wlr/wiki/Troubleshooting-Checklist) to see if [PipeWire](https://wiki.gentoo.org/wiki/PipeWire) is working properly. Ensure XDG\_CURRENT\_DESKTOP=sway. If using systemd and wireplumber, you may have to enable wireplumber.**service** ; wireplumber.**socket** may not be enough.

### Failed to connect to user bus

`[swaybar/tray/tray.c:42] Failed to connect to user bus: No such file or directory`
- Forum topic [\[swaybar/tray/tray.c:42\] Failed to connect to user bus: No such file or directory](https://forums.gentoo.org/viewtopic-p-8552581.html#8552581) => Use dbus-run-session sway
- Forum topic [sway(bar) with tray support](https://forums.gentoo.org/viewtopic-p-8364686.html#8364686)
- [https://github.com/swaywm/sway/issues/1415](https://github.com/swaywm/sway/issues/1415)

### Warning: no icon themes loaded

`[swaybar/tray/icon.c:348] Warning: no icon themes loaded`
It is looking for [x11-themes/hicolor-icon-theme](https://packages.gentoo.org/packages/x11-themes/hicolor-icon-theme)

### No backend was able to open a seat

`[ERROR] [wlr] [libseat] [libseat/libseat.c:78] No backend was able to open a seat`
Make sure you have done everything in [Usage](https://wiki.gentoo.org/wiki/Sway#Usage) correctly.

You will need to install a seat management daemon such as [sys-auth/seatd](https://packages.gentoo.org/packages/sys-auth/seatd) or [sys-auth/elogind](https://packages.gentoo.org/packages/sys-auth/elogind) and enable the corresponding service. Also check whether setting `XDG_RUNTIME_DIR` is required. If using the `elogind` package, it should set the `XDG_RUNTIME_DIR` variable automatically with no further configuration needed. This is not the case with the `seatd` package, and you will have to set the variable yourself, refer to [Starting Sway without elogind or systemd](https://wiki.gentoo.org/wiki/Sway#Starting_Sway_without_elogind_or_systemd) for instructions. Also check whether the user needs to be in the `seat` group.

### Applications forget logins

Some applications (e. g. [net-misc/nextcloud-client](https://packages.gentoo.org/packages/net-misc/nextcloud-client)) use a Secret-Service-Agent to save credentials for login. If applications ask for account credentials every run, an incorrectly configured Secret-Service-Agent might be the reason.

First, emerge [gnome-base/gnome-keyring](https://packages.gentoo.org/packages/gnome-base/gnome-keyring).

`root #``emerge --ask gnome-base/gnome-keyring`
Then, enable the `gnome-keyring` USE flag.

**`/etc/portage/package.use`**

```
# Sway Secret-Service-Agent
*/* gnome-keyring
```
Update the system to apply the new USE flag.

`root #``emerge -avuDN @world`
To run and unlock the Agent's storage when logging into a Sway session, the following sections need to be added to the corresponding files:

**`~/.config/sway/config`**

**`/etc/pam.d/login`**

## See also

- [i3](https://wiki.gentoo.org/wiki/I3) — a minimalist [tiling](https://en.wikipedia.org/wiki/Tiling_window_manager) [window manager](https://wiki.gentoo.org/wiki/Window_manager), completely written from scratch.
- [List of software for Wayland](https://wiki.gentoo.org/wiki/List_of_software_for_Wayland) — various desktop related packages for Wayland
- [Wayland](https://wiki.gentoo.org/wiki/Wayland) — a [communication protocol](https://en.wikipedia.org/wiki/communication_protocol) between a [display server](https://en.wikipedia.org/wiki/display_server) and its clients
- [Weston](https://wiki.gentoo.org/wiki/Weston) — a reference implementation of a [Wayland compositor](https://wiki.gentoo.org/wiki/Wayland_compositor).
