<!-- source: https://wiki.gentoo.org/wiki/Awesome | group: Gentoo Wiki (Main) | wiki-title: Awesome -->
---
title: awesome
url: https://wiki.gentoo.org/wiki/Awesome
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-04-04"
fingerprint: "8e14a67d4291a798"
license: CC BY-SA 4.0
---

# awesome

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**awesome** is a highly configurable, next generation, dynamic [window manager](https://wiki.gentoo.org/wiki/Window_manager) for [X](https://wiki.gentoo.org/wiki/X). It is primarily targeted at power users, developers and any people dealing with every day computing tasks and who want to have fine-grained control on their graphical environment. It is extended using the [Lua](https://wiki.gentoo.org/wiki/Lua) programming language.

Choose exactly one of:

- [elogind](https://wiki.gentoo.org/wiki/Elogind): Standalone logind package, extracted from the systemd project for use with [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) or other init systems.
- [systemd](https://wiki.gentoo.org/wiki/Systemd): Uses the session tracker part of systemd. Users of systemd do not need to take any other initiative here.

- [D-Bus](https://wiki.gentoo.org/wiki/D-Bus): Enables use of the D-Bus message bus system.
- [polkit](https://wiki.gentoo.org/wiki/Polkit): Enables the polkit framework for controlling privileges for system-wide services.
- [udisks](https://wiki.gentoo.org/wiki/Udisks): Enables support for some storage related services.

Follow the instructions on [Xorg/Guide](https://wiki.gentoo.org/wiki/Xorg/Guide) to set up the [X](https://wiki.gentoo.org/wiki/X) environment.

One of the following methods can be used to start [X](https://wiki.gentoo.org/wiki/X):

- [Display manager](https://wiki.gentoo.org/wiki/Display_manager): Presents the user with a graphical login screen.
- [X without Display Manager](https://wiki.gentoo.org/wiki/X_without_Display_Manager): When running a single-user system, one may find display managers an unnecessary waste of resources.


| [dbus](https://packages.gentoo.org/useflags/dbus) | Enable dbus support for anything that needs it (gpsd, gnomemeeting, etc) | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [gnome](https://packages.gentoo.org/useflags/gnome) | Add GNOME support | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

Install [x11-wm/awesome](https://packages.gentoo.org/packages/x11-wm/awesome):

`root #``emerge --ask x11-wm/awesome`
To start **awesome**, refer to [Starting the X server](https://wiki.gentoo.org/wiki/Awesome#Starting_the_X_server) or use the command startx.

To use startx with [elogind](https://wiki.gentoo.org/wiki/Elogind) support, setup [elogind](https://wiki.gentoo.org/wiki/Elogind) and create the following file:

**`~/.xinitrc`**

```
exec dbus-launch --sh-syntax --exit-with-session awesome
```
The default configuration file of awesome is located in \~/.config/awesome/rc.lua. If such a directory or file does not exist then it needs to be created. A default, out of the box, configuration is distributed with awesome and can be found at /etc/xdg/awesome/rc.lua. Copy that configuration file to the user's home directory.

First create the awesome/ directory:

`user $``mkdir -p ~/.config/awesome/`
Next copy the rc.lua configuration file:

`user $``cp /etc/xdg/awesome/rc.lua ~/.config/awesome/rc.lua`
If [x11-terms/xterm](https://packages.gentoo.org/packages/x11-terms/xterm) is not installed, either install it or change the default terminal emulator to the terminal emulator available on the system. Below, the default terminal emulator is set to konsole, part of [kde-apps/konsole](https://packages.gentoo.org/packages/kde-apps/konsole).

**`~/.config/awesome/rc.lua`**

```
-- This is used later as the default terminal and editor to run.
terminal = "konsole"
```
After making changes it is useful to check the configuration file for errors:

`user $``awesome -k`
✔ Configuration file syntax OK

Add wallpaper support through the [media-gfx/feh](https://packages.gentoo.org/packages/media-gfx/feh) package:

`root #``emerge --ask media-gfx/feh`
For instance, to set the wallpaper, edit \~/.config/awesome/theme/theme.lua:

**`~/.config/awesome/theme/theme.lua`**

**Setting a specific background using the wallpaper property**

```
theme.wallpaper = ".config/awesome/themes/awesome-wallpaper.png"
```
In awesome, tags are the name given to virtual desktops under which one or more applications are running. It is possible to assign custom symbols to these tags:

**`~/.config/awesome/rc.lua`**

```
-- {{{ Tags
tags = {}
for s = 1, screen.count() do
    tags[s] = awful.tag({ "➊", "➋", "➌", "➍" }, s, layouts[1])
end
-- }}}
```
Below is an example of a custom awesome menu:

**`~/.config/awesome/rc.lua`**

```
-- {{{ Menu
myawesomemenu = {
   { "manual", terminal .. " -e man awesome" },
   { "edit config", editor_cmd .. " " .. awesome.conffile },
   { "reload", awesome.restart },
   { "quit", awesome.quit },
   { "reboot", "reboot" },
   { "shutdown", "shutdown" }
}
 
appsmenu = {
   { "urxvt", "urxvt" },
   { "sakura", "sakura" },
   { "ncmpcpp", terminal .. " -e ncmpcpp" },
   { "luakit", "luakit" },
   { "uzbl", "uzbl-browser" },
   { "firefox", "firefox" },
   { "chromium", "chromium" },
   { "thunar", "thunar" },
   { "ranger", terminal .. " -e ranger" },
   { "gvim", "gvim" },
   { "leafpad", "leafpad" },
   { "htop", terminal .. " -e htop" },
   { "sysmonitor", "gnome-system-monitor" }
}
 
gamesmenu = {
   { "warsow", "warsow" },
   { "nexuiz", "nexuiz" },
   { "xonotic", "xonotic" },
   { "openarena", "openarena" },
   { "alienarena", "alienarena" },
   { "teeworlds", "teeworlds" },
   { "frozen-bubble", "frozen-bubble" },
   { "warzone2100", "warzone2100" },
   { "wesnoth", "wesnoth" },
   { "supertuxkart", "supertuxkart" },
   { "xmoto" , "xmoto" },
   { "flightgear", "flightgear" },
   { "snes9x" , "snes9x" }
}
 
mymainmenu = awful.menu({ items = { { "awesome", myawesomemenu },
                                    { "apps", appsmenu },
				    { "games", gamesmenu },
                                    { "terminal", terminal },
				    { "web browser", browser },
				    { "text editor", geditor }
                                  }
                        })
 
mylauncher = awful.widget.launcher({ image = image(beautiful.awesome_icon),
                                     menu = mymainmenu })
-- }}}
```
Below is an example use of a custom date format. The format syntax used is `%Y-%m-%d %H:%M`. The second option, `60`, is the update interval in seconds.

**`~/.config/awesome/rc.lua`**

**Creating a text-clock widget**

```
-- {{{ Wibox
-- Create a text-clock widget
mytextclock = wibox.widget.textclock(" %Y-%m-%d %H:%M ", 60)
-- }}}
```
[media-sound/volumeicon](https://packages.gentoo.org/packages/media-sound/volumeicon) can be used to handle volume keys automatically, and to show the volume level through a tray icon.

`root #``emerge --ask media-sound/volumeicon`
Autostart volumeicon from within \~/.xinitrc:

**`~/.xinitrc`**

**Launching volumeicon in the background when starting X**

```
 &
exec [...]
```
Alternatively, a lightweight method is to add volume keys straight into the awesome configuration:

**`~/.config/awesome/rc.lua`**

**Volume keys**

```
awful.key({ }, "XF86AudioLowerVolume", function () awful.util.spawn("amixer -q sset Master 5%-") end)
awful.key({ }, "XF86AudioRaiseVolume", function () awful.util.spawn("amixer -q sset Master 5%+") end)
```
Install [media-sound/mpc](https://packages.gentoo.org/packages/media-sound/mpc) to add multimedia key bindings for [MPD](https://wiki.gentoo.org/wiki/MPD):

`root #``emerge --ask media-sound/mpc`
Next update the awesome configuration to assign the multimedia keys to the proper command:

**`~/.config/awesome/rc.lua`**

**Volume key bindings**

```
awful.key({ }, "XF86AudioNext",function () awful.util.spawn( "mpc next" ) end),
awful.key({ }, "XF86AudioPrev",function () awful.util.spawn( "mpc prev" ) end),
awful.key({ }, "XF86AudioPlay",function () awful.util.spawn( "mpc play" ) end),
awful.key({ }, "XF86AudioStop",function () awful.util.spawn( "mpc pause" ) end),
```
Gaps between windows can be visible, most noticeably between terminal windows. These can be removed by inserting the `size_hints_honor = false` property in the `awful.rules.rules` table like this:

**`~/.config/awesome/rc.lua`**

**Setting size\_hints\_honor property**

```
awful.rules.rules = {
    { rule = { },
      properties = { size_hints_honor = false, -- Remove gaps
                     border_width = beautiful.border_width,
                     border_color = beautiful.border_normal,
      ...
```
Xephyr is a useful tool for debugging new configuration files as it creates an instance of an X server within a client window.

`user $``Xephyr -ac -nolisten tcp -br -noreset -screen 800x600 :1`
This will open an 800x600 window. To run awesome within it open a new terminal and run the following:

`user $``DISPLAY=:1.0 awesome`
This will run awesome within a window.

These are the most useful default shortcuts:

- `Super`+`Mouse1` = move client with mouse
- `Super`+`Mouse2` = resize client with mouse

- `Super`+`Enter` = open terminal
- `Super`+`r` = run command
- `Super`+`Shift`+`c` = kill
- `Super`+`m` = maximize
- `Super`+`n` = minimize
- `Super`+`Ctrl`+`n` = restore minimized clients
- `Super`+`f` = full screen
- `Super`+`Tab` = switch to previous client
- `Super`+`Ctrl`+`Space` = float

- `Super`+`j` = highlight left client
- `Super`+`k` = highlight right client
- `Super`+`Shift`+`j` = move client right
- `Super`+`Shift`+`k` = move client left

- `Super`+`l` = resize tiled client
- `Super`+`h` = resize tiled client

- `Super`+`left / right` = change tag
- `Super`+`1-9` = change tag
- `Super`+`Shift`+`1-9` = send client to tag

Custom key bindings, like `Alt`+`Tab`, can be mapped to make the awesome experience even better. For instance, to use `Alt`+`Tab` to switch to the previous window:

**`~/.config/awesome/rc.lua`**

**Alt-TAB key binding**

```
-- {{{ Key bindings
globalkeys = awful.util.table.join(
...
    -- alt + tab
    awful.key({ "Mod1", }, "Tab",
        function ()
            awful.client.focus.history.previous()
            if client.focus then
                client.focus:raise()
            end
        end),
-- }}}
```
- [User Configuration Files](https://web.archive.org/web/20160701200046/https://awesome.naquadah.org/wiki/Main_Page#User_configuration_files) at awesome wiki (web.archive.org)
- [Desktop profile switching USE default to elogind](https://gentoo.org/support/news-items/2020-04-14-elogind-default.html)
