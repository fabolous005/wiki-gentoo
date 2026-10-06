<!-- source: https://wiki.gentoo.org/wiki/ASUS_Chromebook_C201/Configuration_examples | group: Gentoo Wiki (Main) | wiki-title: ASUS Chromebook C201/Configuration examples -->
---
title: ASUS Chromebook C201/Configuration examples
url: https://wiki.gentoo.org/wiki/ASUS_Chromebook_C201/Configuration_examples
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-20"
fingerprint: "1d742845a95e818a"
license: CC BY-SA 4.0
---

# ASUS Chromebook C201/Configuration examples

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

These configuration examples might be outdated but nevertheless provide helpful hints.

### package.use

**`/etc/portage/package.use/00video`**

```
*/* VIDEO_CARDS: -* fbdev
```
**`/etc/portage/package.use/00input`**

```
*/* INPUT_DEVICES: libinput
```
### ACPI

**`/etc/acpi/default.sh`**

```
#!/bin/sh
# /etc/acpi/default.sh
# Default acpi script that takes an entry for all actions
set $*
group=${1%%/*}
action=${1#*/}
device=$2
id=$3
value=$4
log_unhandled() {
	logger "ACPI event unhandled: $*"
}
case "$group" in
	jack)
		case "$id" in
			'plug')
				amixer -D hw:0 cset name='Left Speaker Mixer Left DAC Switch' off
				amixer -D hw:0 cset name='Right Speaker Mixer Right DAC Switch' off
				amixer -D hw:0 cset name='Headphone Switch Left' on
                                amixer -D hw:0 cset name='Headphone Switch Right' on
				;;
			'unplug')
				amixer -D hw:0 cset name='Left Speaker Mixer Left DAC Switch' on
                                amixer -D hw:0 cset name='Right Speaker Mixer Right DAC Switch' on
				amixer -D hw:0 cset name='Headphone Switch Left' off
                                amixer -D hw:0 cset name='Headphone Switch Right' off
				;;
			*) uhd $+;;
		esac
		log_unhandled $*
	;;
	button)
		case "$action" in
			#power)
					#pm-suspend
			#		log_unhandled $*
			#	;;
			lid)
				case "$id" in
					close) if [ $(cat /sys/class/power_supply/gpio-charger/online) -eq 0 ]; then
                                                        pm-suspend
                                                fi;;
					open) :;;
					*) uhd $*;;
				esac
				log_unhandled $*
				;;
			*)	log_unhandled $* ;;
		esac
		;;
	*)	log_unhandled $* ;;
esac
```
This handles alternating between speaker and headphone jack when one is plugged in, as well as lid suspend when the charger is unplugged. Power button support can be added if desired, though keep in mind the device will simply turn on again when the lid is closed.

### Pulseaudio configuration

Add the following if using Pulseaudio:

**`/etc/pulse/default.pa`**

```
load-module module-alsa-sink device=sysdefault
load-module module-alsa-source device=sysdefault
```
See [InstallingDebianOn/Asus/C201#Audio](https://wiki.debian.org/InstallingDebianOn/Asus/C201#Audio) for further information. VEYRON-I2S shipped with alsa, so it is unlikely to need to add Google's UCM files.

HDMI is not tested. It appeared under 3.14 as RockchipHDMI, but is missing under 4.13. Possibly a kernel config or UCM issue.

### Keybinds

xbacklight doesn't work, however, a backlight device exists. This script can be used in tandem with xbindkeys as a replacement for xbacklight:

**`c201_backlight.sh`**

```
#!/bin/bash
if [ "$(($(cat /sys/devices/platform/backlight/backlight/backlight/brightness)+$1))" -gt "255" ];then
        echo 255 > /sys/devices/platform/backlight/backlight/backlight/brightness
        exit
fi
if [ "$(($(cat /sys/devices/platform/backlight/backlight/backlight/brightness)+$1))" -lt "0" ]; then
        echo 0 > /sys/devices/platform/backlight/backlight/backlight/brightness
        exit
fi
echo $(($(cat /sys/devices/platform/backlight/backlight/backlight/brightness)+$1)) > /sys/devices/platform/backlight/backlight/backlight/brightness
```
Provide the delta for the backlight as an argument, positive or negative.

This file is read-only by default, add this script to the local service if desired:

**`/etc/local.d/backlight-permissions.start`**

```
chmod 666 /sys/devices/platform/backlight/backlight/backlight/brightness
```
This script needs to be marked executable by doing chmod +x

If reassigning the function keys for another purpose is desired, edit the /usr/share/X11/xkb/symbols/pc file.

In the following example, the search key (LWIN) has been remapped to Caps and set some of the function keys to much-needed movement keys. Also, the power button was remapped to the Delete key:

**`/usr/share/X11/xkb/symbols/pc`**

**keys added to "pc105" xkb\_symbols**

```
    key <LWIN>   {      [ Caps_Lock             ]       };
    key <FK01>   {      [ Home                  ]       };
    key <FK02>   {      [ End                   ]       };
    key <FK03>   {      [ Page_Up               ]       };
    key <FK04>   {      [ Page_Down             ]       };
```
**`/usr/share/X11/xkb/symbols/inet`**

**replaced items**

```
    key <POWR>   {      [ Delete                ]       };
```
Restart X for these changes to take effect.

To remove old files in /var/lib/xkb:

`root #``rm -rf /var/lib/xkb/*`
