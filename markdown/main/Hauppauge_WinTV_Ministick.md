<!-- source: https://wiki.gentoo.org/wiki/Hauppauge_WinTV_Ministick | group: Gentoo Wiki (Main) | wiki-title: Hauppauge WinTV Ministick -->
---
title: Hauppauge WinTV Ministick
url: https://wiki.gentoo.org/wiki/Hauppauge_WinTV_Ministick
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-20"
fingerprint: f6be8eb0ed236d2e
license: CC BY-SA 4.0
---

# Hauppauge WinTV Ministick

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This tutorial show how to use the Hauppauge WinTV Ministick for DVB-T TV. I will also show how to use the remote control with lirc.

## Installation

### Kernel

You need [USB](https://wiki.gentoo.org/wiki/USB) and [Evdev](https://wiki.gentoo.org/wiki/Evdev) support.

### Firmware

The firmware is included in the [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) package from version 20150206.

### Check Kernel Configuration

When you have compiled the needed modules and the firmware was correct loaded so you see something like the output here from my dmesg.

**`/var/log/dmesg`**

To check the event devices you can use the **lsinput** tool out of the [sys-apps/input-utils](https://packages.gentoo.org/packages/sys-apps/input-utils) package.

`root #``lsinput`
/dev/input/event4
   bustype : (null)
   vendor  : 0x0
   product : 0x0
   version : 0
   name    : "SMS IR (Hauppauge WinTV MiniStic"
   phys    : "usb-0000:00:1f.2-2/ir0"
   bits ev : EV\_SYN EV\_KEY EV\_MSC EV\_REP
/dev/input/event5
   bustype : (null)
   vendor  : 0x0
   product : 0x0
   version : 0
   name    : "MCE IR Keyboard/Mouse (smsmdtv)"
   phys    : "/input0"
   bits ev : EV\_SYN EV\_KEY EV\_REL EV\_MSC EV\_REP

**Udevadm**

`root #``udevadm info -q all -n /dev/input/event4`
P: /devices/pci0000:00/0000:00:1f.2/usb1/1-2/rc/rc0/input4/event4
N: input/event4
S: input/by-path/pci-0000:00:1f.2-usb-0:2-event-ir
E: DEVLINKS=/dev/input/by-path/pci-0000:00:1f.2-usb-0:2-event-ir
E: DEVNAME=/dev/input/event4
E: DEVPATH=/devices/pci0000:00/0000:00:1f.2/usb1/1-2/rc/rc0/input4/event4
E: ID\_INPUT=1
E: ID\_INPUT\_KEY=1
E: ID\_PATH=pci-0000:00:1f.2-usb-0:2
E: ID\_PATH\_TAG=pci-0000\_00\_1f\_2-usb-0\_2
E: ID\_SERIAL=noserial
E: MAJOR=13
E: MINOR=68
E: SUBSYSTEM=input
E: UDEV\_LOG=3
E: USEC\_INITIALIZED=20837104
E: XKBLAYOUT=de
E: XKBMODEL=pc105
E: XKBVARIANT=nodeadkeys

### Gentoo

#### USE flags

**`/etc/portage/make.conf`**

```
USE="lirc zvbi"
```
`root #``emerge --ask --newuse --deep @world`
#### LIRCd

**`/etc/portage/package.use/lirc`**

Install [app-misc/lirc](https://packages.gentoo.org/packages/app-misc/lirc):

`root #``emerge --ask --oneshot lirc`
## Setting up TV with VLC

To use the dvb device you have to be member of the **video** group:

`root #``usermod -a -G video username`
### Scan for Channels

The **scan** utility is comming with the [media-tv/linuxtv-dvb-apps](https://packages.gentoo.org/packages/media-tv/linuxtv-dvb-apps) package. The next step is to find the correct initial-tuning file for your location.

`user $``scan -x 0 /usr/share/dvb/dvb-t/de-Saarland > channels.conf`
scanning /usr/share/dvb/dvb-t/de-Saarland
using '/dev/dvb/adapter0/frontend0' and '/dev/dvb/adapter0/demux0'
initial transponder 546000000 0 2 9 1 1 3 0
initial transponder 642000000 0 2 9 1 1 3 0
initial transponder 658000000 0 2 9 1 1 3 0
initial transponder 698000000 0 2 9 1 1 3 0
>>> tune to: 546000000:INVERSION\_AUTO:BANDWIDTH\_8\_MHZ:FEC\_2\_3:FEC\_AUTO:QAM\_16:TRANSMISSION\_MODE\_8K:GUARD\_INTERVAL\_1\_4:HIERARCHY\_NONE
0x0000 0x0202: pmt\_pid 0x0000 ZDFmobil -- ZDF (running)
0x0000 0x0203: pmt\_pid 0x0000 ZDFmobil -- 3sat (running)
0x0000 0x0204: pmt\_pid 0x0000 ZDFmobil -- ZDFinfo (running)
0x0000 0x0205: pmt\_pid 0x0000 ZDFmobil -- neo/KiKA (running)
Network Name 'ZDF'
>>> tune to: 642000000:INVERSION\_AUTO:BANDWIDTH\_8\_MHZ:FEC\_2\_3:FEC\_AUTO:QAM\_16:TRANSMISSION\_MODE\_8K:GUARD\_INTERVAL\_1\_4:HIERARCHY\_NONE
0x0000 0x0002: pmt\_pid 0x1120 ARD -- Arte (running)
0x0000 0x0003: pmt\_pid 0x1130 ARD -- Phoenix (running)
0x0000 0x00d0: pmt\_pid 0x1110 ARD -- Das Erste (running)
0x0000 0x00d1: pmt\_pid 0x1140 ARD -- SR-Fernsehen (running)
Network Name 'ARD SR'
>>> tune to: 658000000:INVERSION\_AUTO:BANDWIDTH\_8\_MHZ:FEC\_2\_3:FEC\_AUTO:QAM\_16:TRANSMISSION\_MODE\_8K:GUARD\_INTERVAL\_1\_4:HIERARCHY\_NONE
0x0000 0x0022: pmt\_pid 0x0400 SWR -- Bayerisches FS (running)
0x0000 0x0041: pmt\_pid 0x0200 SWR -- hr-fernsehen (running)
0x0000 0x00e2: pmt\_pid 0x0100 SWR -- SWR Fernsehen RP (running)
0x0000 0x0106: pmt\_pid 0x0300 SWR -- WDR Fernsehen (running)
Network Name 'SWR RP'
>>> tune to: 698000000:INVERSION\_AUTO:BANDWIDTH\_8\_MHZ:FEC\_2\_3:FEC\_AUTO:QAM\_16:TRANSMISSION\_MODE\_8K:GUARD\_INTERVAL\_1\_4:HIERARCHY\_NONE
0x0000 0x401d: pmt\_pid 0x01d0 BetaDigital -- TELE 5 (running)
0x0000 0x4014: pmt\_pid 0x0140 MEDIA BROADCAST -- QVC (running)
0x0000 0x402a: pmt\_pid 0x02a0 MEDIA BROADCAST -- Bibel TV (running)
0x0000 0x4101: pmt\_pid 0x1010 MEDIA BROADCAST -- Multithek (running)
Network Name 'MEDIA BROADCAST'
>>> tune to: 570000000:INVERSION\_AUTO:BANDWIDTH\_8\_MHZ:FEC\_2\_3:FEC\_1\_2:QAM\_16:TRANSMISSION\_MODE\_8K:GUARD\_INTERVAL\_1\_4:HIERARCHY\_NONE
WARNING: >>> tuning failed!!!
>>> tune to: 570000000:INVERSION\_AUTO:BANDWIDTH\_8\_MHZ:FEC\_2\_3:FEC\_1\_2:QAM\_16:TRANSMISSION\_MODE\_8K:GUARD\_INTERVAL\_1\_4:HIERARCHY\_NONE (tuning failed)
WARNING: >>> tuning failed!!!
dumping lists (16 services)
Done.

### Using Teletext

Using the teletext function in vlc is pretty easy. Compile [media-video/vlc](https://packages.gentoo.org/packages/media-video/vlc) with **zvbi** useflag and it works like a sharm. Start VLC and load the channels.conf file, open a channel and you should see a button for teletext on the OSD.

### EPG

VLC also knows to handle EPG data. You find the **Programm Guide** under **Extras**. The problem is that VLC can only handle the current channel, other programms like me-tv get the data from all channels.

## Remote Control PT# R-005 (LIRC)

Here you will find the steps to use the remote control PT# R-005.

### Keymap

**`/etc/rc_keymaps/hauppauge_novaTD`**

### Load the keymap at boot

Install media-tv/v4l-utils to get ir-keytable.

#### Option1: With udev over /etc/rc\_maps.cfg

Normally it should be enough to add CONFIG\_RC\_MAP support in your kernel. This point I already added to the kernel installation section of this howto.

The trigger to load our new IR keymap comes from udev when the CONFIG\_RC\_MAP support is enabled.

**`/lib/udev/rules.d/40-ir-keytable.rules`**

Here we simply add or edit the **rc-hauppauge** line as shown in the box below.

**`/etc/rc_maps.conf`**

Replace **/lib/udev/rc\_keymaps/hauppauge** with **/etc/rc\_keymaps/hauppauge\_novaTD**.

#### Option2: Manual with /etc/local.d/ir-keytable.start

**`/etc/local.d/ir-keytable.start`**

```
 -c --protocol=rc-5 -w /etc/rc_keymaps/hauppauge_novaTD
ir-keytable --delay=500
ir-keytable --period=50
/etc/init.d/lircd start
```
### Xorg

Ignore the remote control devices in Xorg.

**`/etc/X11/xorg.conf`**

```
Section "InputClass"
	Identifier	"Ignore Hauppauge WinTV MiniStick IR Devices"
	MatchProduct	"SMS IR (Hauppauge WinTV MiniStic|MCE IR Keyboard/Mouse (smsmdtv)"
	Option		"Ignore"	"true"
EndSection
```
### LIRC

**`/etc/lirc/hardware.conf`**

**`/etc/lirc/lircd.conf`**

**`/usr/share/lirc/extras/more_remotes/hauppauge/lircd.conf.hauppauge_novaTD_usb`**

### Test the LIRCd

To test your current lircd configuration you can use the **irw** tool.

### Example .lircrc

**`~/.lircrc`**

**`~/.lirc/irexec`**

**`~/.lirc/vlc`**

**`~/.lirc/audacious`**
