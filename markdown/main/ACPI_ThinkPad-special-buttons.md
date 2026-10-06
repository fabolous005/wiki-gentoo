<!-- source: https://wiki.gentoo.org/wiki/ACPI/ThinkPad-special-buttons | group: Gentoo Wiki (Main) | wiki-title: ACPI/ThinkPad-special-buttons -->
---
title: ACPI/ThinkPad-special-buttons
url: https://wiki.gentoo.org/wiki/ACPI/ThinkPad-special-buttons
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-01"
fingerprint: ac7f1ced646ba950
license: CC BY-SA 4.0
---

# ACPI/ThinkPad-special-buttons

[ACPI](https://wiki.gentoo.org/wiki/ACPI)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Many modern computer keyboards contain special media keys or key combinations to control functions such as monitor brightness, keyboard backlight brightness, volume (sound) level, suspend (sleep mode), and more. Some special keys need additional configuration in order to control what they are intended to control. This additional configuration can be done through [ACPI](https://wiki.gentoo.org/wiki/ACPI).

It is important to note that most users to not need ACPI to handle the buttons if using a major desktop environment; desktop environments like [Gnome](https://wiki.gentoo.org/wiki/Gnome), [KDE](https://wiki.gentoo.org/wiki/KDE), and [Xfce](https://wiki.gentoo.org/wiki/Xfce) should be capable of handling special buttons. If the desktop environment has not detected special keys automatically, most of the time it can be configured to properly handle the special key events.

This article describes how to configure ACPI events and actions for the Lenovo ThinkPad T410 laptop.

## Preparation

To have a functional ACPI make sure the kernel includes support for `THINKPAD_ACPI`. The zgrep command can be used to search through the /proc/config.gz file if .config support was built into the previous kernel:

`user $``zgrep -i thinkpad /proc/config.gz`
CONFIG\_THINKPAD\_ACPI=m
CONFIG\_THINKPAD\_ACPI\_ALSA\_SUPPORT=y
# CONFIG\_THINKPAD\_ACPI\_DEBUGFACILITIES is not set
# CONFIG\_THINKPAD\_ACPI\_DEBUG is not set
CONFIG\_THINKPAD\_ACPI\_UNSAFE\_LEDS=y
CONFIG\_THINKPAD\_ACPI\_VIDEO=y
CONFIG\_THINKPAD\_ACPI\_HOTKEY\_POLL=y

On boot it might be necessary to append the `acpi_backlight=vendor` kernel command-line option. Do not add this parameter to your kernel unless your ACPI events are not handled properly; one way to test this is by pressing the brightness up/down keys and looking for feedback from the kernel (see acpi\_listen below). Most Lenovo ThinkPads should work without this setting and indeed it is encouraged by the kernel documentation (5.9 onwards).

To check what names ACPI uses on a specific button, start acpi\_listen (provided by [sys-power/acpid](https://packages.gentoo.org/packages/sys-power/acpid)) and press `Fn`+special key buttons to see the output from each key:

`root #``acpi_listen`
button/fnf1 FNF1 00000080 00000000 K
button/screenlock SCRNLCK 00000080 00000000 K
^\[\[36\~button/battery BAT 00000080 00000000 K
^\[\[37\~button/sleep SBTN 00000080 00000000 K

| Lenovo ThinkPad T410 selected ACPI key events |  |  |  |  | 
|---|---|---|---|---|
| Keys | Event name | Short name | Numerical part | Note | 
|---|---|---|---|---|
| `Fn`+`F1` | button/fnf1 | FNF1 | 00000080 00000000 K |  | 
| `Fn`+`F2` | button/screenlock | SCRNLCK | 00000080 00000000 K |  | 
| `Fn`+`F3` | button/battery | BAT | 00000080 00000000 K |  | 
| `Fn`+`F4` | button/sleep | SBTN | 00000080 00000000 K |  | 
| `Fn`+`F5` | button/wlan | WLAN | 00000080 00000000 K |  | 
| `Fn`+`F6` | (not reported to handler) |  |  |  | 
| `Fn`+`F7` | video/switchmode | VMOD | 00000080 00000000 K |  | 
| `Fn`+`F8` | (not reported to handler) |  |  | Touchpad toggle | 
| `Fn`+`F9` | button/f24 | F24 | 00000080 00000000 K |  | 
| `Fn`+`F10` | (not reported to handler) |  |  |  | 
| `Fn`+`F11` | button/fnf11 | FF11 | 00000080 00000000 K |  | 
| `Fn`+`F12` | button/suspend | SUSP | 00000080 00000000 K |  | 
| `Fn`+`Space` | button/zoom | ZOOM | 00000080 00000000 K |  | 
| `Mute` | button/f20 | F20 | 00000080 00000000 K |  | 
| `VolumeDown` | button/volumedown | VOLDN | 00000080 00000000 K |  | 
| `VolumeUp` | botton/volumeup | VOLUP | 00000080 00000000 K |  | 
| `MuteMic` | button/f20 | F20 | 00000080 00000000 K |  | 
| `ThinkVantage` | button/prog1 | PROG1 | 00000080 00000000 K |  | 
| `Fn`+`Home` | video/brightnessup | BRTUP | 00000086 00000000 |  | 
| `Fn`+`End` | video/brightnessdown | BRTDN | 00000087 00000000 |  | 
| `Fn`+`PgUp` | (not reported to handler) |  |  | hardware light switch | 
| `Fn`+`PrtSc` | (not reported to handler) |  |  |  | 
| `Fn`+`ScrLk` | (not reported to handler) |  |  |  | 
| `Fn`+`Pause` | (not reported to handler) |  |  |  | 
| `Fn`+`ArrowUp` | cd/stop | CDSTOP | 00000080 00000000 K |  | 
| `Fn`+`ArrowLeft` | cd/prev | CDPREV | 00000080 00000000 K |  | 
| `Fn`+`ArrowRight` | cd/next | CDNEXT | 00000080 00000000 K |  | 
| `Fn`+`ArrowDown` | cd/play | CDPLAY | 00000080 00000000 K |  | 

Collect the names printed in the file above. They will vary from system to system.

By default, system store key assignment can be found in the /lib/udev/hwdb.d/60-keyboard.hwdb file:

**`/lib/udev/hwdb.d/60-keyboard.hwdb`**

```
...
# ThinkPad Keyboard with TrackPoint
keyboard:usb:v17EFp6009*
 KEYBOARD_KEY_090012=screenlock                         # Fn+F2
 KEYBOARD_KEY_090013=battery                            # Fn+F3
 KEYBOARD_KEY_090014=wlan                               # Fn+F5
 KEYBOARD_KEY_090016=switchvideomode                    # Fn+F7
 KEYBOARD_KEY_090017=f21                                # Fn+F8  touchpad toggle
 KEYBOARD_KEY_090019=suspend                            # Fn+F12
 KEYBOARD_KEY_09001a=brightnessup                       # Fn+Home
 KEYBOARD_KEY_09001b=brightnessdown                     # Fn+End
 KEYBOARD_KEY_09001d=zoom                               # Fn+Space
 KEYBOARD_KEY_090011=prog1                              # ThinkVantage button
 KEYBOARD_KEY_090015=camera                             # Fn+F6 headset/camera VoIP key  ??
 KEYBOARD_KEY_090010=f20                                # Microphone mute button; should be micmute
...
```
By default some buttons are mapped to key codes that X can not handle or has no keysym for. So the following might be useful. It modifies the udev defaults slightly to fix the key combinations that otherwise would not work in X:

**`/etc/udev/hwdb.d/thinkpad_keyboard.hwdb`**

```
# To debug key presses and access scan code mapping data of 
# an input device use the commonly available tool: evtest(1). 
# A list of possible keycodes is available under 
# https://github.com/torvalds/linux/blob/master/include/uapi/linux/input.h 
### common 
keyboard:name:ThinkPad Extra Buttons:dmi:bvn*:bvr*:bd*:svnLENOVO*:pn* 
 KEYBOARD_KEY_00=prog2          # Fn+F1 
 KEYBOARD_KEY_08=media          # Fn+F9; should be undock, but X has no keysym 
 KEYBOARD_KEY_0a=prog3          # Fn+F11 
 KEYBOARD_KEY_13=search         # Fn+Space; should be zoom, but X can't handle keycode 
# KEYBOARD_KEY_18=              # Fn+1 (might need hotkey unmasking) 
# KEYBOARD_KEY_19=              # Fn+2 (same) 
### X61t display buttons 
keyboard:dmi:bvn*:bvr*:bd*:svnLENOVO*:pnThinkPad*X6*:pvr* 
 KEYBOARD_KEY_67=screenlock     # small unlabelled button 
 KEYBOARD_KEY_6C=f21            # rotate button (f21 = XF86TouchpadToggle) 
 KEYBOARD_KEY_68=config         # rectangle thing button (config = XF86Tools; was:screenlock)
```
## Recipes

Create an *event* for each button in /etc/acpi/events/, the following example names them by location-function; use whatever naming scheme works best. After creating the events create corresponding actions for each event in /etc/acpi/actions/ See examples below for more information.

**`/etc/acpi/events/FnF2-screenlock`**

**Event creation for screen lock button**

```
#Fn+F2 button/screenlock SCRNLCK 00000080 00000000 K
event=button/screenlock
action=/etc/acpi/actions/FnF2-screenlock.sh
```
**`/etc/acpi/actions/FnF2-screenlock.sh`**

**Action script for screen lock event**

```
#!/bin/sh
logger "[ACPI] Fn+F2 pressed, start Slimlock for logged-in user"
XUSER=$(ps aux | grep xinit | awk '{print $1}' | head -n1)
sudo -u $XUSER /usr/bin/slimlock&
```
`root #``chmod +x /etc/acpi/actions/FnF2-screenlock.sh`
**`/etc/acpi/events/FnF3-battery`**

**Event creation for battery button**

```
#Fn+F3 button/battery BAT 00000080 00000000 K
event=button/battery
action=/etc/acpi/actions/FnF3-battery.sh
```
**`/etc/acpi/actions/FnF3-battery.sh`**

**Action script for battery button event**

```
#!/bin/sh
# Tried on Gentoo
# cpu throttling is in here it is the cpufreq-set lines
# turning swap off is only for those that feel comfortable
# doing something this nasty.
# comment out the laptop_mode line if it is not installed
# pcfe, 2008-10-28
# spindown time for HD (man hdparm for valid values)
# I prefer 2 hours for acad and 2 min for batt
ACAD_HD=244
BATT_HD=24
                                                                                                                                   
# Power management level
# 255 (off) on AC
# 128 (medium) on batt
# lowered to 32, pcfe, 2004-06-23
# upped to 64, pcfe, 2004-07-14
# upped to 96, pcfe, 2004-10-20
ACAD_PM=255
BATT_PM=96
logger "[ACPI] Fn+F3 pressed to toggle battery state"
# ac/battery event handler 
status=`awk '/^state: / { print $2 }' /proc/acpi/ac_adapter/AC/state` 
if [ "$status" = "off-line" ]
then
	logger "Running /sbin/laptop_mode start"
	/sbin/laptop_mode start
	logger "Setting HD spindown for BATT mode with hdparm -S $BATT_HD /dev/hda."
	/sbin/hdparm -S $BATT_HD /dev/hda > /dev/null 2>&1
	logger "Setting HD powersaving for BATT mode with hdparm -B $BATT_PM /dev/hda."
	/sbin/hdparm -B $BATT_PM /dev/hda > /dev/null 2>&1
	logger "Turning off swap."
	/sbin/swapoff -a
	/usr/bin/cpufreq-set -g conservative         
else   
	logger "Turning on swap."
	/sbin/swapon -a
	logger "Running /sbin/laptop_mode start"
	/sbin/laptop_mode stop
	logger "Setting HD spindown for AC mode with hdparm -S $ACAD_HD /dev/hda."
	/sbin/hdparm -S $ACAD_HD /dev/hda > /dev/null 2>&1
	logger "Setting HD powersaving for AC mode with hdparm -B $ACAD_PM /dev/hda."
	/sbin/hdparm -B $ACAD_PM /dev/hda > /dev/null 2>&1
	/usr/bin/cpufreq-set -g ondemand
fi
```
Be sure to make the action script executable:

`root #``chmod +x /etc/acpi/actions/FnF3-battery.sh`
Source: [ThinkWiki.org](https://www.thinkwiki.org/wiki/ACPI_action_script_for_battery_events)

**`/etc/acpi/events/FnF4-sleep`**

**Event creation for Sleep button**

```
#Fn+F4 button/sleep SBTN 00000080 00000000 K
event=button/sleep
action=/etc/acpi/actions/FnF4-sleep.sh
```
**`/etc/acpi/actions/FnF4-sleep.sh`**

**Action script for Sleep button event**

```
#!/bin/sh
logger "[ACPI] Fn+F4 pressed, start Slimlock for current user and suspend to ram"
XUSER=$(ps aux | grep xinit | awk '{print $1}' | head -n1)
sudo -u $XUSER /usr/bin/slimlock&
echo -n mem >/sys/power/state
```
`root #``chmod +x /etc/acpi/actions/FnF4-sleep.sh`
**`/etc/acpi/events/FnF5-wifi`**

**Event creation for Wi-Fi button**

```
#Fn+F5 button/wlan WLAN 00000080 00000000 K
event=button/wlan
action=/etc/acpi/actions/FnF5-wlan.sh
```
**`/etc/acpi/actions/FnF5-wifi.sh`**

**Action script for Wi-Fi button event**

```
#!/bin/sh
logger "[ACPI] Fn+F5 pressed, WiFi rfkill state toggled"
rf=/sys/class/rfkill/rfkill0
case $(< $rf/state) in
    0) echo 1 >$rf/state;;
    1) echo 0 >$rf/state;;
esac
```
`root #``chmod +x /etc/acpi/actions/FnF5-wifi.sh`
### Backlight control

##### Brightness up

**`/etc/acpi/events/FnHome-brightnessup`**

**Event creation for brightness up button**

```
#FnHome video/brightnessup BRTUP 00000086 00000000
event=video/brightnessup
action=/etc/acpi/actions/FnHome-brightnessup.sh
```
**`/etc/acpi/actions/FnHome-brightnessup.sh`**

**Action script for brightness up button event**

```
#!/bin/bash 
# Set the static increment value.  Keep in mind that this will 
# be done twice. 
IncVal=20 
# Get the Maximum value for use. 
#MaxVal=$(cat /sys/class/backlight/intel_backlight/max_brightness); 
read -r MaxVal < "/sys/class/backlight/intel_backlight/max_brightness"
# Get the current brightness value. 
#CurrVal=$(cat /sys/class/backlight/intel_backlight/brightness); 
read -r CurrVal < "/sys/class/backlight/intel_backlight/brightness"
# Set the new value minus the decrement value. 
NewVal=$(($CurrVal + $IncVal)); 
echo $NewVal 
# Set it to the threshold of the max value. 
ThresholdVal=$(($NewVal<$MaxVal?$NewVal:$MaxVal)) 
echo $ThresholdVal 
# Set the new value directly. 
echo -n $ThresholdVal > /sys/class/backlight/intel_backlight/brightness 
logger "[ACPI] brightnessup |$CurrVal| |$NewVal| |$ThresholdVal|"
```
Remember to make the script executable:

`root #``chmod +x /etc/acpi/actions/FnHome-brightnessup.sh`
##### Brightness down

**`/etc/acpi/events/FnEnd-brightnessdown`**

**Event creation for brightness down button**

```
#FnEnd video/brightnessdown BRTDN 00000087 00000000
event=video/brightnessdown
action=/etc/acpi/actions/FnEnd-brightnessdown.sh
```
**`/etc/acpi/actions/FnEnd-brightnessdown.sh`**

**Action script for brightness down button event**

```
#!/bin/bash 
# Set the static decrement value.  Keep in mind that this will 
# be done twice. 
DecVal=20 
# Set the Minimum we will accept. 
MinVal=0 
# Get the current brightness value. 
#CurrVal=$(cat /sys/class/backlight/intel_backlight/brightness); 
read -r CurrVal < "/sys/class/backlight/intel_backlight/brightness"
# Set the new value minus the decrement value. 
NewVal=$(($CurrVal - $DecVal)); 
echo $NewVal 
# Set it to the threshold of the min value. 
ThresholdVal=$(($NewVal>$MinVal?$NewVal:$MinVal)) 
echo $ThresholdVal 
# Set the new value directly. 
echo -n $ThresholdVal > /sys/class/backlight/intel_backlight/brightness 
logger "[ACPI] brightnessdown |$CurrVal| |$NewVal| |$ThresholdVal|"
```
Be sure to make the script executable:

`root #``chmod +x /etc/acpi/actions/FnEnd-brightnessdown.sh`
Source: [forums.gentoo.org](https://forums.gentoo.org/viewtopic-p-7578948.html)

### CD control

Instead of CD I would like to control [media-sound/moc](https://packages.gentoo.org/packages/media-sound/moc).

Initially I start mocp manually to populate playlist, quit qui and than use special keys to control mocp.

##### Stop

**`/etc/acpi/events/FnArrowUp-stop`**

```
#Fn+ArrowUp cd/stop CDSTOP 00000080 00000000 K
event=cd/stop
action=/etc/acpi/actions/FnArrowUp-stop.sh
```
**`/etc/acpi/actions/FnArrowUp-stop.sh`**

```
#!/bin/sh
XUSER=$(ps -eo uname,args --sort start_time | grep mocp | awk '{print $1}' | head -n1)
sudo -u $XUSER /usr/bin/mocp -s
```
`root #``chmod +x /etc/acpi/actions/FnArrowUp-stop.sh`
##### Prev track

**`/etc/acpi/events/FnArrowLeft-prev`**

```
#Fn+ArrowUp cd/prev CDPREV 00000080 00000000 K
event=cd/prev
action=/etc/acpi/actions/FnArrowLeft-prev.sh
```
**`/etc/acpi/actions/FnArrowLeft-prev.sh`**

```
#!/bin/sh
XUSER=$(ps -eo uname,args --sort start_time | grep mocp | awk '{print $1}' | head -n1)
sudo -u $XUSER /usr/bin/mocp -r
```
`root #``chmod +x /etc/acpi/actions/FnArrowLeft-prev.sh`
##### Next track

**`/etc/acpi/events/FnArrowRight-next`**

```
#Fn+ArrowUp cd/next CDNEXT 00000080 00000000 K
event=cd/next
action=/etc/acpi/actions/FnArrowRight-next.sh
```
**`/etc/acpi/actions/FnArrowRight-next.sh`**

```
#!/bin/sh
XUSER=$(ps -eo uname,args --sort start_time | grep mocp | awk '{print $1}' | head -n1)
sudo -u $XUSER /usr/bin/mocp -f
```
`root #``chmod +x /etc/acpi/actions/FnArrowRight-next.sh`
##### Play/pause toggle

**`/etc/acpi/events/FnArrowDown-play`**

```
#Fn+ArrowUp cd/play CDPLAY 00000080 00000000 K
event=cd/play
action=/etc/acpi/actions/FnArrowDown-play.sh
```
**`/etc/acpi/actions/FnArrowDown-play.sh`**

```
#!/bin/sh
XUSER=$(ps -eo uname,args --sort start_time | grep mocp | awk '{print $1}' | head -n1)
sudo -u $XUSER /usr/bin/mocp -G
```
`root #``chmod +x /etc/acpi/actions/FnArrowDown-play.sh`
## Troubleshooting

To see output when a key is pressed:

`root #``acpid -c /etc/acpi/events/ -s /var/run/acpid.socket -f -d -l | tee tmp`
## Questions

- Do I need to place all events in the same file, something like /etc/acpi/events/default.thinkpad-t410 and make it downloadable?

- How to choose vlock vs slimlock screenlock action depending on console vs Xorg or for both altogether?

Another option is to use ScrLk key to lock console with [app-misc/vlock](https://packages.gentoo.org/packages/app-misc/vlock), or it can be done through the same acpi action? dunno
