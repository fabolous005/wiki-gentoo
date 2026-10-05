<!-- source: https://wiki.gentoo.org/wiki/Hprofile | group: Gentoo Wiki (Main) | wiki-title: Hprofile -->
---
title: Hprofile
url: https://wiki.gentoo.org/wiki/Hprofile
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-03-31"
fingerprint: ad03587f367fb7ae
license: CC BY-SA 4.0
---

# Hprofile

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **deprecated (obsolete)**. Contents are <u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

hprofile is an application that can be used to manage multiple profiles be it hardware or software. The following subsection are just examples on what could be done with hprofile.

## Installation

### Emerge

`root #``emerge --ask sys-apps/hprofile`
## Hardware profiles

### Prelude

**`hprofile.initd`**

```
#!/sbin/openrc-run
# Copyright 1999-2014 Gentoo Foundation
# Distributed under the terms of the 2-clause BSD or GPL-2 license
# $Header: hprofile.initd,v 1.2 2014/08/01 13:05:23 -tclover Exp $
config_file="/etc/conf.d/${SVCNAME}"
description="initialize hprofile boot profiles"
depend() {
	after modules
}
start() {
	local profile value
	for profile in $HPROFILE; do
	    value=$(hpdet $profile)
	    ebegin "Selecting $profile profile $value"
	    hprofile $profile.$value
	    eend "$?" "Failed to start $profile.$value profile"
	done
	return 0
}
stop() {
	local profile
	for profile in $HPROFILE; do
	    ebegin "Stoping $profile profile"
	    hprofile -s $profile
	    eend "$?" "Failed to stop $profile profile"
	done
	return 0
}
# vim:fenc=utf-8:ft=gentoo-init-d:ci:pi:sts=0:sw=4:ts=4:
```
**`hprofile.confd`**

```
# $Header: hprofile.confd,v 1.2 2014/08/01 13:05:23 -tclover Exp $
# This variable is a list of profiles to start when starting the service
HPROFILE="vga power"
# vim:fenc=utf-8:ft=gentoo-conf-d:ci:pi:sts=0:sw=4:ts=4:
```
### Power

When thinking about a better way to have dynamic power profiles that could be dynamically switched depending on hardware or software state is not straightforward especially for power management which could depend on many software e.g. [sys-power/powertop](https://packages.gentoo.org/packages/sys-power/powertop), [sys-apps/hdparm](https://packages.gentoo.org/packages/sys-apps/hdparm) to name a few. So, how could you get hardware profiles and switch between them dynamically? And how to get everything together? How many daemons to start depending on what hardware or software state?

That sum up quite a few considerations to what a user can face when trying to build a nice setup with power management in mind. \[S\]he can remember of Windows(tm) power management profiles if \[s\]he ever used it. Or else, \[s\]he did not thought about it because \[s\]he used to run a GNU Linux or BSD based distribution with everything putted together and does not know how to begin with if \[s\]he is not satisfied with what \[s\]he gets.

Now [sys-power/powertop](https://packages.gentoo.org/packages/sys-power/powertop)-2 grew up to be quite a good piece of software. If you're running an Intel based platform, you can get access to pretty much everything one would expect if trying to build power profiles. But the package does not ship with a daemon-ish like software nor with a configuration or profile file which PowerTOP could pick up when launched in the next restart. One has to launch it again and again in a terminal and enable power management for every power manageable hardware... quite the redundant an manual way every time the system boot up.

This basic profile will try to implement [Disk](https://wiki.gentoo.org#Hard_disk), CPU (see [External resources](https://wiki.gentoo.org#External_resources)), network interfaces and [ALSA](https://wiki.gentoo.org/wiki/ALSA) drivers power saving capabilities.

One would need the following files to set up basic power management profiles.

**`/etc/hprofile/profiles/power/profiles`**

**`/etc/hprofile/profiles/power/default`**

**`/etc/hprofile/profiles/power/ptest`**

```
#!/bin/sh
if [ -e "/sys/class/power_supply/A*/online" ]; then
	status=$(cat /sys/class/power_supply/A*/online)
	case ${status} in
		1) profile="adp";;
		0) profile="bat";;
	esac
fi
echo "${profile:-adp}"
# vim:fenc=utf-8:ci:pi:sts=0:sw=4:ts=4:
```
**`/etc/hprofile/profiles/power/start`**

```
#!/bin/sh
# $Id: /etc/hprofile/profiles/power/base-start, 2014/07/31 -tclover $
CPU=${1:-powersave}
PCI=${2:-auto}
USB=${3:-auto}
HDA=${5:-1}
NMI=${6:-0}
VMW=${7:-1000}
ASPM=${8:-default}
SCSI=${4:-min_power}
for cpu in $(ls -d /sys/devices/system/cpu/cpu[0-9]*); do
	echo $CPU >$cpu/cpufreq/scaling_governor
done
[ -e /sys/module/pcie_aspm/parameters/policy ] &&
	echo $ASPM >/sys/module/pcie_aspm/parameters/policy
for scsi in $(ls -d /sys/class/scsi_host/host[0-9]*); do
	[ -e $scsi/link_power_management_policy ] || continue
	case $(cat $scsi/proc_name) in
		ahci|usb-storage) echo $SCSI >$scsi/link_power_management_policy;;
	esac
done
# set usb host to auto powersave
for usb in $(ls -d /sys/bus/usb/devices/{usb[0-9],[0-9]-[0-9]}); do
	echo $USB >$usb/power/control
done
# pci power control
for pci in $(ls -d /sys/bus/pci/devices/0000:0*); do 
	echo $PCI >$pci/power/control
done
# snd-hda-intel powersave
[ -e /sys/module/snd_hda_intel/parameters/power_save ] &&
	echo $HDA >/sys/module/snd_hda_intel/parameters/power_save
# turn off NMI watchdog
[ -e /proc/sys/kernel/nmi_watchdog ] &&
	echo $NMI >/proc/sys/kernel/nmi_watchdog
# VM write back timeout
[ -e /proc/sys/vm/dirty_writeback_centisecs ] &&
	echo $VMW >/proc/sys/vm/dirty_writeback_centisecs
# vim:fenc=utf-8:ci:pi:sts=0:sw=4:ts=4:
```
**`/etc/hprofile/profiles/power/scripts/adp.start`**

```
#!/bin/sh
script=/etc/hprofile/profiles/power/start
$script "powersave" "auto" "auto" "min_power" "1" "0" "default"
hprofile disk.adp
if [ -n "$DISPLAY" ]; then
    xbacklight -set 25
    xset dpms 300 600 800
fi
# vim:fenc=utf-8:ci:pi:sts=0:sw=4:ts=4:
```
**`/etc/hprofile/profiles/power/scripts/bat.start`**

```
#!/bin/sh
script=/etc/hprofile/profiles/power/start
$script "powersave" "auto" "auto" "min_power" "1" "0" "1500" "powersave"
hprofile disk.bat
if [ -n "$DISPLAY" ]; then
    p
    xbacklight -set 20
    xset dpms 300 400 600
fi
# vim:fenc=utf-8:ci:pi:sts=0:sw=4:ts=4:
```
**`/etc/hprofile/profiles/power/scripts/dyn.start`**

```
#!/bin/sh
script=/etc/hprofile/profiles/power/start
$script "performance" "on" "on" "max_power" "0" "1" "500" "performane"
hprofile disk.dyn
if [ -n "$DISPLAY" ]; then
    xbacklight -set 35
    xset dpms 600 800 1200
fi
# vim:fenc=utf-8:ci:pi:sts=0:sw=4:ts=4:
```
**`/etc/hprofile/profiles/power/scripts/med.start`**

```
#!/bin/sh
script=/etc/hprofile/profiles/power/start
$script "performance" "on" "on" "min_power" "0" "1" "500" "performance"
hprofile disk.adp
if [ -n "$DISPLAY" ]; then
    xbacklight -set 30
    xset dpms 450 600 900
fi
# vim:fenc=utf-8:ci:pi:sts=0:sw=4:ts=4:
```
**`/etc/hprofile/profiles/power/scripts/quiet.start`**

```
#!/bin/sh
script=/etc/hprofile/profiles/power/start
$script "powersave" "auto" "auto" "min_power" "1" "0" "2500" "powersave"
hprofile disk.quiet
if [ -n "$DISPLAY" ]; then
    xbacklight -set 20
    xset dpms 200 400 600
fi
# vim:fenc=utf-8:ci:pi:sts=0:sw=4:ts=4:
```
I have a */etc/acpi/defaults.sh* script that make use of those scripts, at least *battery* and *AC Adapter*. Those two power profiles are dynamically switched if AC power is un/plugged. See [ACPI](https://wiki.gentoo.org/wiki/ACPI) article for more info about the acpid *default.sh* script. It's not enough? Do you want more tunings? Expand the article then!

### RFKill

This hardware profile subsection could be beneficial to the previous section, however software RF killing/switching can be quite troublesome which require cold reboot and removing battery/AC power for a few seconds to be able to get back usable wireless (WiFi/Bluetooth) interfaces again. This happen on my Intel GM45 based laptop. Luckily there is a physical switch to kill Bluetooth and WiFi radio.

There's no much consideration for this section, if your hardware support RFKill, add it to your hardware profile!

**`/etc/hprofile/profiles/rfkill/ptest`**

**`/etc/hprofile/profiles/rfkill/default`**

**`/etc/hprofile/profiles/rfkill/profiles`**

**`/etc/hprofile/profiles/rfkill/scripts/bsw.start`**

```
#!/bin/sh
echo 1 >/sys/class/rfkill/rfkill1/state
# vim:fenc=utf-8:ft=sh:ci:pi:sts=0:sw=4:ts=4:
```
**`/etc/hprofile/profiles/rfkill/scripts/bsw.stop`**

```
#!/bin/sh
echo 0 >/sys/class/rfkill/rfkill1/state
# vim:fenc=utf-8:ft=sh:ci:pi:sts=0:sw=4:ts=4:
```
**`/etc/hprofile/profiles/rfkill/scripts/wsw.start`**

```
#!/bin/sh
echo 1 >/sys/class/rfkill/rfkill0/state
echo 1 >/sys/class/rfkill/rfkill2/state
# vim:fenc=utf-8:ft=sh:ci:pi:sts=0:sw=4:ts=4:
```
**`/etc/hprofile/profiles/rfkill/scripts/wsw.stop`**

```
#!/bin/sh
echo 0 >/sys/class/rfkill/rfkill0/state
echo 0 >/sys/class/rfkill/rfkill2/state
# vim:fenc=utf-8:ft=sh:ci:pi:sts=0:sw=4:ts=4:
```
**`/etc/hprofile/profiles/rfkill/scripts/ksw.start`**

```
#!/bin/sh
hprofile rfkill.bsw
hprofile rfkill.wsw
# vim:fenc=utf-8:ft=sh:ci:pi:sts=0:sw=4:ts=4:
```
**`/etc/hprofile/profiles/rfkill/scripts/ksw.stop`**

```
#!/bin/sh
hprofile rfkill.bsw
hprofile rfkill.wsw
# vim:fenc=utf-8:ft=sh:ci:pi:sts=0:sw=4:ts=4:
```
### VGA

I use [sys-apps/hprofile](https://packages.gentoo.org/packages/sys-apps/hprofile) to manage VGA profile to my laptop and my desktop. On my desktop, I use hprofile to switch between [nvidia-drivers](https://wiki.gentoo.org/wiki/NVIDIA/nvidia-drivers), [nouveau](https://wiki.gentoo.org/wiki/Nouveau) and nv ([x11-drivers/xf86-video-nv](https://packages.gentoo.org/packages/x11-drivers/xf86-video-nv)) when the previous profiles do no work; and to switch [radeon](https://wiki.gentoo.org/wiki/Radeon) and [intel](https://wiki.gentoo.org/wiki/Intel) on my laptop which has switchable graphics via VGASwitcheroo. Now I have a new laptop with NVIDIA Optimus, VGASwitcheroo/PRIME does work quite bit avoiding to get a hot potato because that thing has a GT750M GPU! although I have to wait linux-3.16 and Mesa-10.3 to make any use of a GT750M and go through the black screen of death (right, BSOD(tm)).

Just be sure to built kernel modules about everything — at least i915, nouveau, radeon, ttm, drm, and optionally (?) ac, button, video, i2c-algo-bit — to avoid useless hassles and be able to boot with a single kernel be it with vesa/nvidia-drivers, or i915, or nouveau.

Create the necessaries folder /etc/hprofile/profiles/vga/{scripts,files/etc/X11/xorg.conf.d} and add the following files.

**`/etc/hprofile/profiles/vga/profiles`**

**`/etc/hprofile/profiles/vga/post-start`**

```
#!/bin/sh
echo "Finished starting profile ${1}"
/etc/init.d/consolefont restart
# vim:fenc=utf-8:ft=sh:ci:pi:sts=0:sw=4:ts=4:
```
**`/etc/hprofile/profiles/vga/stop`**

```
#!/bin/sh
if [ -e /sys/kernel/debug/vgaswitcheroo/switch ]; then
	case ${1} in
		intel|radeon|nouveau) echo ON >/sys/kernel/debug/vgaswitcheroo/switch;;
	esac
fi
echo "Finished Stopping profile ${1} and moving back xorg.conf"
# vim:fenc=utf-8:ft=sh:ci:pi:sts=0:sw=4:ts=4:
```
**`/etc/hprofile/profiles/vga/ptest`**

```
#!/bin/sh
n=/dev/null
s=/sys/kernel/debug/vgaswitcheroo/switch
if [  ! -e $s ]; then
	grep -q i915    /proc/modules || modprobe i915    >$n 2>&1
	grep -q radeon  /proc/modules || modprobe radeon  >$n 2>&1
	grep -q nouveau /proc/modules || modprobe nouveau >$n 2>&1
fi
if [  -e $s ]; then
    vga=$(grep + $s)
    pci=$(sed -nre 's/^[0-9].*\+.*[a-zA-Z]:([0-9].*$)/\1/p' $s)
    if [ -d /sys/bus/pci/drivers/nouveau/$pci ]; then
	    drv=nouveau
    elif [ -d /sys/bus/pci/drivers/radeon/$pci ]; then
	    drv=radeon
    fi
    case $vga in
	    *:IGD:+*) echo "intel"; exit;;
	    *:DIS:+*) echo "$drv" ; exit;;
    esac
fi
unset n pci s vga
drv="$(lspci -k | sed -nre 's/Kernel driver in use: ((i915|fglrx|radeon|nouveau|nvidia))/\1/p')"
echo "${drv:-invalid}"
# vim:fenc=utf-8:ft=sh:ci:pi:sts=0:sw=4:ts=4:
```
**`/etc/hprofile/profiles/vga/scripts/fglrx.start`**

```
#!/bin/sh
echo "Starting vga profile to fglrx"
if ! modprobe fglrx; then
	echo "failed to load fglrx module"
	exit
fi
[ "$(eselect opengl show)" != "ati" ] && eselect opengl set ati
[ "$(eselect xvmc show)" != "ati" ] && eselect xvmc set ati
# vim:fenc=utf-8:ft=sh:ci:pi:sts=0:sw=4:ts=4:
```
**`/etc/hprofile/profiles/vga/scripts/fglrx.stop`**

```
#!/bin/sh
echo "Stopping nvidia profile."
rmmod fglrx || echo "failed to remove fglrx module." && exit 1
# vim:fenc=utf-8:ft=sh:ci:pi:sts=0:sw=4:ts=4:
```
**`/etc/hprofile/profiles/vga/scripts/intel.start`**

```
#!/bin/sh
echo "Starting intel vga profile"
modprobe i915 || exit $?
if [ -e /sys/kernel/debug/vgaswitcheroo/switch ]; then
	echo IGD >/sys/kernel/debug/vgaswitcheroo/switch &&
	echo OFF >/sys/kernel/debug/vgaswitcheroo/switch
fi
[ "$(eselect opengl show)" != "xorg-x11" ] && eselect opengl set xorg-x11
[ "$(eselect xvmc show)" != "intel-i915/i965" ] && eselect xvmc set 'intel-i915/i965'
# vim:fenc=utf-8:ft=sh:ci:pi:sts=0:sw=4:ts=4:
```
**`/etc/hprofile/profiles/vga/scripts/nouveau.start`**

```
#!/bin/sh
echo "Starting nouveau vga profile"
modprobe nouveau || exit$?
if [ -e /sys/kernel/debug/vgaswitcheroo/switch ]; then
	echo DIS >/sys/kernel/debug/vgaswitcheroo/switch
#	echo OFF >/sys/kernel/debug/vgaswitcheroo/switch
fi
[ "$(eselect opengl show)" != "xorg-x11" ] && eselect opengl set xorg-x11
[ "$(eselect xvmc show)" != "xorg-x11" ] && eselect opengl set xorg-x11
# vim:fenc=utf-8:ft=sh:ci:pi:sts=0:sw=4:ts=4:
```
**`/etc/hprofile/profiles/vga/scripts/nouveau.stop`**

```
#!/bin/sh
echo "Stopping nouveau vga profile"
# uncomment the following line and last one if one need
# to force remove nouveau/kms driver for e.g. nvidia
if [ ! -e sys/kernel/debug/vgaswitcheroo/switch ]; then
	echo 0 >/sys/class/vtconsole/vtcon1/bind
	if ! rmmod nouveau; then
		echo "failed to remove nouveau module"
	else
		/etc/init.d/consolefont restart
		rmmod ttm
		rmmod drm_kms_helper
		rmmod dri
	fi
fi
# vim:fenc=utf-8:ft=sh:ci:pi:sts=0:sw=4:ts=4:
```
**`/etc/hprofile/profiles/vga/scripts/radeon.start`**

```
#!/bin/sh
echo "Starting radeon vga profile"
modprobe radeon || exit $?
if [ -e /sys/kernel/debug/vgaswitcheroo/switch ]; then
	echo DIS >/sys/kernel/debug/vgaswitcheroo/switch
#	echo OFF >/sys/kernel/debug/vgaswitcheroo/switch
fi
echo low >/sys/class/drm/card0/device/power_profile
[ "$(eselect opengl show)" != "xorg-x11" ] && eselect opengl set xorg-x11
[ "$(eselect xvmc show)" != "xorg-x11" ] && eselect xvmc set xorg-x11
echo profile >/sys/class/drm/card0/device/power_method  # or 'dynpm'
echo    auto >/sys/class/drm/card0/device/power_profile # or 'low|mid'
# vim:fenc=utf-8:ft=sh:ci:pi:sts=0:sw=4:ts=4:
```
**`/etc/hprofile/profiles/vga/scripts/radeon.stop`**

```
#!/bin/sh
echo "Stopping fglrx vga profile."
# uncomment the following line and last one if one need
# to force remove nouveau/kms driver for e.g. fglrx
if [ ! -e sys/kernel/debug/vgaswitcheroo/switch ]; then
#	echo 0 >/sys/class/vtconsole/vtcon1/bind
	if ! rmmod nouveau; then
		echo "failed to remove fglrx module"
	else
		/etc/init.d/consolefont restart
#		rmmod ttm
#		rmmod drm_kms_helper
#		rmmod dri
	fi
fi
# vim:fenc=utf-8:ft=sh:ci:pi:sts=0:sw=4:ts=4:
```
**`/etc/hprofile/profiles/vga/scripts/nvidia.start`**

```
#!/bin/sh
echo "Starting nvidia vga profile"
if ! modprobe nvidia; then
	echo "failed to load nvidia module"
	exit $?
fi
[ "$(eselect opengl show)" != "nvidia" ] && eselect opengl set nvidia
# vim:fenc=utf-8:ft=sh:ci:pi:sts=0:sw=4:ts=4:
```
**`/etc/hprofile/profiles/vga/scripts/nvidia.stop`**

```
#!/bin/sh
echo "Stopping nvidia vga profile"
if ! rmmod nvidia; then
	ret=$?
	echo "failed to remove nvidia module"
	exit $?
fi
# vim:fenc=utf-8:ft=sh:ci:pi:sts=0:sw=4:ts=4:
```
**`/etc/hprofile/profiles/vga/files/etc/X11/xorg.conf.d/40-monitor.conf.fglrx`**

```
Section "Monitor"
	Identifier  "Default"
EndSection
Section "Device"
	Identifier  "ATI"
	Driver      "fglrx"
EndSection
Section "Screen"
	Identifier "FGLRX"
	Device     "ATI"
	Monitor    "Default"
EndSection
```
**`/etc/hprofile/profiles/vga/files/etc/X11/xorg.conf.d/40-monitor.conf.intel`**

```
Section "Monitor"
	Identifier  "Default"
EndSection
Section "Device"
	Option      "DRI" "True"
	Option      "XvMCSurfaces" "6"
	Identifier  "GMAX"
	Driver      "intel"
EndSection
Section "Screen"
	Identifier "INTEL"
	Device     "GMAX"
	Monitor    "Default"
EndSection
```
**`/etc/hprofile/profiles/vga/files/etc/X11/xorg.conf.d/40-monitor.conf.nouveau`**

```
Section "Monitor"
    Identifier  "Default"
EndSection
Section "Device"
    Option      "EXAVsync"  "True"
    Option      "GLXVBlank" "True"
    Identifier  "nVidia"
    Driver      "nouveau"
EndSection
Section "Screen"
    Identifier "NOUVEAU"
    Device     "nVidia"
    Monitor    "Default"
EndSection
```
**`/etc/hprofile/profiles/vga/files/etc/X11/xorg.conf.d/40-monitor.conf.nvidia`**

```
Section "Monitor"
    Identifier "Default"
EndSection
Section "Device"
    Option     "AddARGBGLXVisuals"  "on"
    Option     "NoLogo"
    Identifier "nVidia"
    Driver     "nvidia"
EndSection
Section "Screen"
    Identifier "NVIDIA"
    Device     "nVidia"
    Monitor    "Default"
EndSection
```
**`/etc/hprofile/profiles/vga/files/etc/X11/xorg.conf.d/40-monitor.conf.radeon`**

```
Section "Monitor"
	Identifier   "Default"
EndSection
Section "Device"
	Option      "AccelMethod" "EXA"
	Option      "ClockGating" "On"
	Option      "ForceLowPowerMode" "On"
	Option      "VGAAccess" "On"
	Identifier  "RadeonHD"
	Driver      "radeon"
	Option	    "MonitorLayout" "LVDS, AUTO"
EndSection
Section "Screen"
	Identifier "RADEON"
	Device     "RadeonHD"
	Monitor    "Default"
EndSection
```
And with that, you should be able to switch vga profile as you like without needing an extra kernel and setup. Just close your X session and type hprofile vga.$profile ENTER.

The ptest script will just look if VGASwitcheroo (Intel HD Graphics/AMD ATI Radeon hybrid graphics) or PRIME (Intel HD Graphicss/NVIDIA Optimus) is available and which driver is loaded and then start the appropriate profile. One can black list modules and leave the other un-black listed to be able to chose ...a default VGA profile. Or else, use of /etc/hprofile/profiles/vga/default file.

**`/etc/hprofile/profiles/vga/default`**

### Hard disk

A disk profile is even straightforward because there fewer issue to keep in mind. So this section will be short and straightforward. Additionally, one need [sys-apps/hdparm](https://packages.gentoo.org/packages/sys-apps/hdparm).

**`/etc/hprofile/profiles/disk/default`**

**`/etc/hprofile/profiles/disk/ata`**

```
#!/bin/sh
echo "$(ls -d /sys/devices/pci*/*/ata*/host*/target*/*/block/sd[a-z] 2>/dev/null)"
# vim:fenc=utf-8:ft=sh:ci:pi:sts=0:sw=4:ts=4:
```
**`/etc/hprofile/profiles/disk/usb`**

```
#!/bin/sh
for dev in $(ls -d /sys/devices/pci*/*/usb*/*/*/host*/target*/*/block/sd[a-z] 2>/dev/null)
do
	echo noop  >$dev/queue/scheduler
done
# vim:fenc=utf-8:ft=sh:ci:pi:sts=0:sw=4:ts=4:
```
**`/etc/hprofile/profiles/disk/post-start`**

```
#!/bin/sh
for dev in $(/etc/hprofile/profiles/disk/ata); do
    echo cfq  >$dev/queue/scheduler
    echo 0    >$dev/queue/iosched/slice_idle
    echo 64   >$dev/queue/iosched/quantum
    # more opitmizations with ncq
    echo 1024 >$dev/queue/nr_requests
    echo 2    >$dev/device/queue_depth
done
# vim:fenc=utf-8:ft=sh:ci:pi:sts=0:sw=4:ts=4:
```
**`/etc/hprofile/profiles/disk/ptest`**

```
#!/bin/sh
echo "adp"
# vim:fenc=utf-8:ft=sh:ci:pi:sts=0:sw=4:ts=4:
```
**`/etc/hprofil/profiles/disk/profiles`**

**`/etc/hprofile/profiles/disk/stop`**

```
#!/bin/sh
ata="$(/etc/hprofile/profiles/disk/ata)"
[ -n "$ata" ] && hdparm -q -S120 -B254 -M254 $ata
# vim:fenc=utf-8:ft=sh:ci:pi:sts=0:sw=4:ts=4:
```
**`/etc/hprofile/profiles/disk/scripts/adp.start`**

```
#!/bin/sh
/etc/hprofile/profiles/disk/usb
ata="$(/etc/hprofile/profiles/disk/ata)"
[ -n "$ata" ] && hdparm -q -S120 -B230 -M254 $ata
```
**`/etc/hprofile/profiles/disk/scripts/bat.start`**

```
#!/bin/sh
/etc/hprofile/profiles/disk/usb
ata="$(/etc/hprofile/profiles/disk/ata)"
[ -n "$ata" ] && hdparm -q -S120 -B210 -M230 $ata
```
**`/etc/hprofile/profiles/disk/scripts/dyn.start`**

```
#!/bin/sh
/etc/hprofile/profiles/disk/usb
ata="$(/etc/hprofile/profiles/disk/ata)"
[ -n "$ata" ] && hdparm -q -S180 -B254 -M254 $ata
```
**`/etc/hprofile/profiles/disk/scripts/quiet.start`**

```
#!/bin/sh
/etc/hprofile/profiles/disk/usb
ata="$(/etc/hprofile/profiles/disk/ata)"
[ -n "$ata" ] && hdparm -q -S120 -B200 -M200 $ata
```
## See also

- [Power management/Processor](https://wiki.gentoo.org/wiki/Power_management/Processor) — describes the setup of [power management](https://wiki.gentoo.org/wiki/Power_management) for processors.
- [USB Power Saving](https://wiki.gentoo.org/wiki/USB_Power_Saving) - See that article for USB power management insight.
