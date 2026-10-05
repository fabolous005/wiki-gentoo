<!-- source: https://wiki.gentoo.org/wiki/DisplayLink | group: Gentoo Wiki (Main) | wiki-title: DisplayLink -->
---
title: DisplayLink
url: https://wiki.gentoo.org/wiki/DisplayLink
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-01-13"
categories: ['Linux plugable.com']
fingerprint: "7a53f5f9f05f7fd"
license: CC BY-SA 4.0
---

# DisplayLink

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**DisplayLink** is a technology that enables monitors to work via [USB](https://wiki.gentoo.org/wiki/USB).

## Installation

### Kernel

Activate the following kernel options:

After booting into the new kernel the external monitor should show a green background image. That means the kernel module is loaded and the device works, it also creates the device in /dev/fb0.

### X driver

For X11 drivers, [x11-drivers/xf86-video-fbdev](https://packages.gentoo.org/packages/x11-drivers/xf86-video-fbdev):

**`/etc/portage/package.use/00video`**

```
 VIDEO_CARDS: -* fbdev
```
After setting or altering `VIDEO_CARDS` values remember to update the system using the following command so the changes take effect:

`root #``emerge --ask --changed-use --deep @world`
## One X server

TODO

## Two X server

This method is failsafe and should work with any graphics card installed. Start two instances of [X server](https://wiki.gentoo.org/wiki/X_server) for each device and then use a software called x2x to move the input devices between them.

- two independent instances and desktops
- Input devices follow the mouse pointer

### Software

For this method, another input device driver called [x11-drivers/xf86-input-void](https://packages.gentoo.org/packages/x11-drivers/xf86-input-void) is necessary:

**`/etc/portage/package.use/00input`**

```
 INPUT_DEVICES: void
```


`root #``emerge --ask --changed-use --deep @world`
Also install [x11-misc/x2x](https://packages.gentoo.org/packages/x11-misc/x2x):

`root #``emerge --ask x11-misc/x2x`
### xorg.conf.DL

Configure two independent [xorg.confs](https://wiki.gentoo.org/wiki/Xorg.conf) for each device and initialize the desktop using \~/.xinitrc scripts.

Create the file /etc/X11/xorg.conf.DL:

**`/etc/X11/xorg.conf.DL`**

```
Section "Device"
    Identifier "DisplayLinkDevice"
    driver "fbdev"
    Option "fbdev" "/dev/fb0"    # You have to use the correct framebuffer device here
EndSection
Section "Monitor"
    Identifier "DisplayLinkMonitor"
EndSection
Section "Screen"
    Identifier "Default Screen"
    Device "DisplayLinkDevice"
    Monitor "DisplayLinkMonitor"
    SubSection "Display"
        Depth 16         # 24bit works fine but for USB 2.0 a lot of data
        Modes "1280x1024"
    EndSubSection
EndSection
Section "ServerLayout"
    Identifier "Server Layout"
    Screen 0 "Default Screen" 0 0
    Option "AllowMouseOpenFail" "True"
    InputDevice "Keyboard0" "CoreKeyboard"
    InputDevice "Mouse0" "CorePointer"
EndSection
Section "ServerFlags"
    Option "AllowEmptyInput" "false"
    Option "AutoAddDevices" "false"
    Option "AutoEnableDevices" "false"
EndSection
Section "InputDevice"
    Identifier "Keyboard0"
    Driver "void"
EndSection
Section "InputDevice"
    Identifier "Mouse0"
    Driver "void"
EndSection
```
### .xinitrc2

Next, create the \~/.xinitrc2 for the external display. Create and customize the file to as needed, here is an example:

**`~/.xinitrc2`**

```
# DPMS stuff
## turn on monitor
xset dpms force on
## disable sleep modes etc.
xset -dpms
## disable screensaver
xset s off
# turn off beep
xset -b
# activate zapping (ctrl+alt+Bksp killall X)
setxkbmap -option terminate:ctrl_alt_bksp
# Set the background using feh
feh --bg-scale /usr/share/slim/themes/capernoited/background.jpg
# compositoring
xcompmgr -c -t-5 -l-5 -r4.2 -o.55 &
# start programs
wicd-client &
mrxvt &
# start the actual window manager
exec /usr/bin/awesome
```
### displaylink.sh

This is the actual script that starts the second instance of X server. Make it executable and save it somewhere in your home folder, in this example we save it to \~/.displaylink.sh:

**`~/.displaylink.sh`**

```
#!/bin/sh
xinit ~/.xinitrc2 -- /usr/bin/X :1 -xf86config xorg.conf.DL -novtswitch -sharevts -audit 0 -layout "Screen Layout" vt12 &
sleep 5
x2x -west -from :0 -to :1 &
```
## DisplayLink 4-in-1 Adapter

It is a USB 3.0 adapter comes with 4 ports:

- One USB 3.0 port
- One Ethernet port
- One HDMI port
- One VGA port

The USB 3.0 port should work if you already have USB 3.0 related kernel configured. To get the Ethernet port work, you need to activate the following kernel options：

The Ethernet port will be seen as **usb0** network device.
