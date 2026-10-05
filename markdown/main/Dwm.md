<!-- source: https://wiki.gentoo.org/wiki/Dwm | group: Gentoo Wiki (Main) | wiki-title: Dwm -->
---
title: dwm
url: https://wiki.gentoo.org/wiki/Dwm
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-15"
fingerprint: "8606aa3b258f0bc4"
license: CC BY-SA 4.0
---

# dwm

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**dwm** (shortened from **d**ynamic **w**indow **m**anager) is a dynamic [window manager](https://wiki.gentoo.org/wiki/Window_manager) for [X11](https://wiki.gentoo.org/wiki/X11) from [suckless.org](https://suckless.org/). dwm is a single binary, and its source code is intended to never exceed 2000 [SLOC](https://en.wikipedia.org/wiki/Source_lines_of_code).

dwm is configured by editing the [C](<https://en.wikipedia.org/wiki/C_(programming_language)>) source code, and recompiling it. The suckless website states that the project ***focuses on advanced and experienced computer users***, and - perhaps tongue in cheek - that customization through editing source code "keeps its userbase small and elitist".

dwm is a dynamic window manager, as such it manages windows in tiled, monocle and floating layouts. All of the layouts can be applied dynamically, optimizing the environment for the application in use and the task performed.

Launch a few terminals with `Shift`+`Alt`+`Enter` and dwm will tile the windows between the master and stack. A new terminal appears on the master window. Existing windows are pushed upon a stack to the right of the screen. `Alt`+`Enter` toggles windows between master and stack.

+------+----------------------------------+--------+
   | tags | title                            | status +
   +------+---------------------+------------+--------+
   |                            |                     |
   |                            |                     |
   |                            |                     |
   |                            |                     |
   |          master            |        stack        |
   |                            |                     |
   |                            |                     |
   |                            |                     |
   |                            |                     |
   +----------------------------+---------------------+


| [savedconfig](https://packages.gentoo.org/useflags/savedconfig) | Use this to restore your config from /etc/portage/savedconfig ${CATEGORY}/${PN}. Make sure your USE flags allow for appropriate dependencies | 
| [xinerama](https://packages.gentoo.org/useflags/xinerama) | Add support for querying multi-monitor screen geometry through the Xinerama API | 

Users should consider enabling the [savedconfig](https://wiki.gentoo.org/wiki/Savedconfig)

`root #``euse --enable savedconfig`
Users with multiple monitors should enable the `xinerama` USE flag regardless of whether or not Xinerama will be used.

`root #``euse --enable xinerama`
Install [x11-wm/dwm](https://packages.gentoo.org/packages/x11-wm/dwm):

`root #``emerge --ask x11-wm/dwm`
To start dwm use a [display manager](https://wiki.gentoo.org/wiki/Display_manager) or the startx command.

Those choosing to go the startx route need to create the following file:

**`~/.xinitrc`**

```
exec dbus-launch --sh-syntax --exit-with-session dwm
```
As stated previously, the main dwm configuration file is the /etc/portage/savedconfig/x11-wm/dwm-6.5 file and after each change, dwm needs to be recompiled for any changes to take effect.

In order for the editor to properly display syntax highlighting for C code, create a symlink using a C header filename extension.

`root #``ln -s /etc/portage/savedconfig/x11-wm/dwm-6.5 /etc/portage/savedconfig/x11-wm/dwm-6.5.h`
or consult the documentation of your editor of choice on how to change the syntax highlighting used for a file independent of its extension.

To use a new configuration after recompilation, if already within a dwm session, quit dwm (`Mod`+`Shift`+`Q`) then reload it, to replace the currently executing binary in memory.

The default xsession file provided by the Gentoo Ebuild (/etc/X11/Sessions/dwm) provides for a default status box that displays system load and the date/time or whatever shell code the user has inside \~/.dwm/dwmrc. The present mechanism (as of dwm-6.0) for sending text to a status box in the window manager's bar is to use 'xsetroot', as illustrated by the default xsession mentioned above. With a few lines of shell code, one can use this mechanism to send arbitrary text to the status bar (for example, the CPU temperature, the current track on the music player, number of unread emails, etc.)

dmenu is a dynamic menu for X, originally designed for dwm.

Install dmenu:

`root #``emerge --ask x11-misc/dmenu`
dmenu's options can be customized using the dwm.h file, such as displaying the menu at the bottom of the display.

**`/etc/portage/savedconfig/x11-misc/dmenu-6.5.h`**

```
static const char *dmenucmd[] = { "dmenu_run", "-b", "-fn", font, "-nb", normbgcolor, "-nf", normfgcolor, "-sb", selbgcolor, "-sf", selfgcolor, NULL };
```
By default, `Alt` + `P` key sequence enables the menu.

###### Flatpaks

To get Flatpaks listed on dmenu, the user will have to make a symbolic link for the app in /usr/bin. Flatpak exports binaries to /var/lib/flatpak/exports/bin for system-wide Flatpaks and to \~/.local/share/flatpak/exports/bin for per-user Flatpaks. In this example, Mission Center will be used. It is a system resource usage monitor.

(system-wide):

`root #``ln -s /var/lib/flatpak/exports/bin/io.missioncenter.MissionCenter /usr/bin/MissionCenter`
(per-user):

`root #``ln -s ~/.local/share/flatpak/exports/bin/io.missioncenter.MissionCenter /usr/bin/MissionCenter`
To display additional status information on dwm's menu bar, one should use [x11-apps/xsetroot](https://packages.gentoo.org/packages/x11-apps/xsetroot), which sets text information into the upper right corner.

First of all, install [x11-apps/xsetroot](https://packages.gentoo.org/packages/x11-apps/xsetroot) if it's not installed yet.

Then, use a script or side program to loop current information in dwm status.

`root #``emerge --ask x11-apps/xsetroot`
For example, try [Conky](https://wiki.gentoo.org/wiki/Conky) to display current information about the system. Prefer installing with `-X` USE flag as only text information is piped through to the dwm instance (USE flags for consideration are `-X hddtemp iostats wifi`").

`root #``emerge --ask app-admin/conky`
An example \~/.config/conky/conky.conf file. The configuration file is divided into two sections: `conky.config` and `conky.text`. The `conky.config` section contains options related to Conky, while the `conky.text` section defines what and how information is displayed. To use Conky on your system, you may need to adjust some settings in the `conky.text` section, such as the path to network interfaces or hard drives, as those can vary depending on your system.

**`~/.config/conky/conky.conf`**

```
 = {
 
background = no,
format_human_readable = yes,
out_to_console = yes,
temperature_unit = celsius,
total_run_times = 0,
update_interval = 1,
#use_spacer = left,
use_spacer = none,
 
};
 
conky.text = [[
 
M ${memperc}%/${swapperc}% | \
/sda ${diskio sda} /sdb ${diskio sdb} \
/sdc ${diskio sdc} | \
${if_existing /proc/net/route ppp0}P0 U ${upspeed ppp0} D ${downspeed ppp0} |${endif}\
${if_existing /proc/net/route eth0}E0 U ${upspeed eth0} D ${downspeed eth0} |${endif}\
${if_existing /proc/net/route wlan0}W0 U ${upspeed eth0} D ${downspeed eth0}\
${wireless_ap wlan0} ${wireless_link_qual_perc wlan0} ${endif}\
CPU ${hwmon 1 temp 1}F \
/sda ${hddtemp /dev/sda}F \
/sdb ${hddtemp /dev/sdb}F \
     ${time %a, %b %d %Y  %H:%M (%z)}
 
]]
```
Add a line in the \~/.xinitrc file before the dwm execution command, mentioned earlier.

**`~/.xinitrc`**

```
 | while read -r; do xsetroot -name "$REPLY"; done &
exec ck-launch-session dbus-launch --sh-syntax --exit-with-session dwm
```
Instead of emerging side programs, create a simple loop to show date, time, weather and other system information.

For example, to show weather, date and time create a shell script file, in \~/.scripts/:

**`~/.scripts/xsetloop.sh`**

```
#!/bin/sh
 
let loop=0
while true; do
	if [[ $loop%300 -eq 0 ]]; then
		weather="$(curl 'https://wttr.in?format=1')"
		let loop=0
	fi
	xsetroot -name " $weather | $(date '+%b %d %a') | $(date '+%H:%M') "
	let loop=$loop+1
	sleep 1
done
```
Since the script is already looped, we just need to set it within xroot in our \~/.xinitrc file.

**`~/.xinitrc`**

```
 ~/.scripts/xsetloop.sh &
exec ck-launch-session dbus-launch --sh-syntax --exit-with-session dwm
```
All (default) dwm key bindings work with a certain `MODKEY`, which is defined in dwm.h. The default `MODKEY` value is `Mod1Mask`, which means `Alt` key for PC keyboards. In the rest of this article, `Mod` is used to represent `MODKEY`.

To move a window to another window tag manually, hold down the `Mod` key and left click anywhere on the window. Then, while still holding down `Mod`, click again on the window tag to move the window to.

Those shortcuts are used by default in x11-wm/dwm.

- `Mod`+`2` - Display window tag number two
- `Mod`+`Shift`+`1-9` - Hover mouse over window and press keys.  Puts window on tag number specified.
- `Mod`+`Shift`+`0` - Hover mouse over window and press keys.  Puts window on all tags.

- `Mod`+`Shift`+`Enter` - Launch a terminal
- `Mod`+`Shift`+`C` - Kills a window
- `Mod`+`P` - dmenu
- `Mod`+`J` or `Mod`+`K` - Move to another terminal.
- `Mod`+`Enter` - Toggles Windows between stack and master.
- `Mod`+`Shift`+`Q` - Quit dwm

- `Mod`+`F` - Change layout on floating.
- `Mod`+`T` - Change layout on tiled.

Add the following lines to the config file and re-emerge dwm:

**`/etc/portage/savedconfig/x11-wm/dwm-6.5`**

```
#include <X11/XF86keysym.h>
 
...
 
/* commands */
static const char *upvol[] = { "amixer", "set", "Master", "2%+", NULL };
static const char *downvol[] = { "amixer", "set", "Master", "2%-", NULL };
 
// for muting/unmuting //
static const char *mute[] = { "amixer", "-q", "set", "Master", "toggle", NULL };
 
// for pulse compatible //
static const char *upvol[] = { "amixer", "-q", "sset", "Master", "1%+", NULL };
static const char *downvol[] = { "amixer", "-q", "sset", "Master", "1%-", NULL };
static const char *mute[] = { "amixer", "-q", "-D", "pulse", "sset", "Master", "toggle", NULL };
 
...
 
static Key keys[] = {
        /* modifier                     key        function        argument */
        { 0,              XF86XK_AudioRaiseVolume, spawn,          {.v = upvol } },
        { 0,              XF86XK_AudioLowerVolume, spawn,          {.v = downvol } },
        { 0,              XF86XK_AudioMute,        spawn,          {.v = mute } },
```
(Put user customization tricks & tips here.)

### GTK Theme

Those wishing to customize the GTK theme of applications may use lxappearance:

`root #``emerge --ask lxde-base/lxappearance`
### Wallpaper

There are a few ways to change the wallpaper in dwm. One of the mainstays is feh:

`root #``emerge --ask media-gfx/feh`
Those using a Display Manager will configure feh in their \~/.xprofile:

**`~/.xprofile`**

```
 --bg-scale /path/to/wallpaper
```
If startx is used, place that in \~/.xinitrc.

Gentoo has a specific way of patching dwm. If the patches are ready to be merged with dwm source, there is special function called `eapply_user` that can be called during the emerge process. This function allows user patches to be applied to the source.  Move the necessary patches one of the two locations:

- [/etc/portage/patches](https://wiki.gentoo.org/wiki//etc/portage/patches)/category/application
- An [ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository)

First create the following directory:

`root #``mkdir -p /etc/portage/patches/x11-wm/dwm`
Copy the dwm patches to /etc/portage/patches/x11-wm/dwm/ and make sure each patch is prefixed with a number, like so: 01-name\_of\_patch.patch. Also the filename needs to end with `.patch` or `.diff` otherwise Portage will not apply it. For this example we will assume that the patch is located in the home directory (/home/larry) of the user called larry.

`root #``cp /home/larry/01-dwm.6.0-xft.patch /etc/portage/patches/x11-wm/dwm/`
Now just install dwm, emerge will take care of applying patches:

`root #``emerge --ask x11-wm/dwm`
Creating an ebuild repository for the dwm patches can help when wanting to share them, either on another machine, or publicly.

Copy x11-wm/dwm from /var/db/repos/gentoo/ to a [new ebuild repository](https://wiki.gentoo.org/wiki/Creating_an_ebuild_repository), or to whatever repository is appropriate.

Place patches in the files directory (for this example, the repository created to hold these files is named "dwm\_patches"):

`root #``cp /home/larry/01-dwm-6.5-xft.diff /var/db/repos/dwm_patches/x11-wm/dwm/files/`
Open the ebuild file in a text editor, /var/db/repos/dwm\_patches/x11-wm/dwm/dwm-6.5.ebuild, and list them in the PATCHES variable array for automatic patching:

**`/var/db/repos/dwm_patches/x11-wm/dwm-6.5.ebuild`**

```
PATCHES=( "${FILESDIR}/01-dwm-6.5-xft.diff"
          "${FILESDIR}/02-dwm-6.5-x11.diff"
)
```
Fire up emerge and enjoy:

`root #``emerge --ask x11-wm/dwm`
A user can have their favorite applications start on a different window tag, such as starting [MPlayer](https://wiki.gentoo.org/wiki/MPlayer) on window tag number five.

First, know the *name* of the application recorded by Xorg so dwm can be aware of this window on startup. To find this, start the target application (MPlayer in this example) and then further execute the xprop command ([x11-apps/xprop](https://packages.gentoo.org/packages/x11-apps/xprop)). Click on the MPlayer window and xprop will report Xorg's data on the MPlayer window. Use the second window name identified on the `WM_CLASS(STRING)` line. Now we have the name of the window dwm needs to be aware of.

**`/etc/portage/savedconfig/x11-wm/dwm-6.5.h`**

```
static const Rule rules[] ={
    { "MPlayer",    NULL,       NULL,   1 << 4,     True,        0 },
};
```
### Cursor is the wrong size

Cursor size is set via \~/.Xresources. You can configure it to 8, 16, 24, 32, or 64.

**`~/.Xresources`**

Upgrading from dwm-5.9 to dwm-6.0 incorporated many changes making the previous config.h a likely problem for compiling dwm-6.0. Likely problems displayed might be compiler error messages "'nmaster' undeclared". To resolve, compile and install dwm-6.0 without using the custom config.h file and then find the default dwm-6.0 config.h file and diff against the old config.h file. (Or, decompress the dwm-6.0 tarball to acquire the default dwm-6.0 config.h file.)

A logind provider, like systemd or elogind, must be running in order to start a X session as non privileged user. If a logind provider is not running and the user issues startx dwm fails to start and message similar to this appears:

(EE) parse\_vt\_settings: Cannot open /dev/tty0 (Permission denied)

If this is the case and the system is [OpenRC](https://wiki.gentoo.org/wiki/OpenRC)-based, add elogind to boot and start the service:

`root #``rc-update add elogind boot``root #``/etc/init.d/elogind start`
for more information, please visit [Non root Xorg](https://wiki.gentoo.org/wiki/Non_root_Xorg) wiki page.

If there are conflicts with the default dwm `Alt` conflicting with other console interface applications, use the `Esc` while within the console application. The `Esc` is an immediate usable fall back escape key. Another option, redefine the Mod key to use the keyboard `Super` (Windows) or other additional keys near the `Space`.

**`/etc/portage/savedconfig/x11-wm/dwm-6.5.h`**

```
#define MODKEY Mod4Mask         /* Use Super Key */
```
To assign a second Mod key allowing a user to have a Mod key on both sides of the keyboard, mimic or copy this keys activity to another key on the keyboard. The Microsoft Menu key (or context menu key) on Microsoft keyboards is directly opposite of the `Super` (Windows). The [x11-apps/xmodmap](https://packages.gentoo.org/packages/x11-apps/xmodmap) package is required for this. (For reference, the two key's values are: `showkey 125/127` and `xev 133/135` respectively - on MS NEK4000 keyboard.)

**`$HOME/.xinitrc`**

```
# Top of $HOME/.xinitrc file is a good place for this.
# This reassigns MS NEK4000 right Menu key to simulate DWM Mod4Key as well.
xmodmap -e "keycode 135 = Super_L" # reassign MS Menu Keypress to Super_L
xmodmap -e "remove mod1 = Super_L" # make sure X keeps it out of the mod1 group
```
Now, a user should have a non-conflicting and easily accessible Mod key on both sides of the keyboard!

[Java](https://wiki.gentoo.org/wiki/Java)-based applications are known to misbehave as Java doesn't know the WM being used. This result in GUI of specific Java applications to not work properly. To solve this we need to set the window manager name property of the root window. This can be done using the wmname tool and set it to `LG3D`.

Install the tool:

`root #``emerge --ask x11-misc/wmname`
and set the property name:

`user $``wmname LG3D`
To make this setting permanent add this command to \~/.xinitrc.

Java-based applications, such as [Apache NetBeans](https://en.wikipedia.org/wiki/NetBeans), does not render properly. To mitigate this problem set the `AWT_TOOLKIT` variable as:

`user $``AWT_TOOLKIT=MToolkit; export AWT_TOOLKIT`
To make the action permanent is is required to add the command to the startup script, for example \~/.xinitrc.

Sometimes the background may not properly redraw when the current view is switched. For example, some terminal emulators such as st don't draw the entirety of their allocated window space. In these cases, X root window must have a properly defined color. This can be done with the xsetroot command. For example:

`user $``xsetroot -solid black`
