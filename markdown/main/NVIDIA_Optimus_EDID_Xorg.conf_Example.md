<!-- source: https://wiki.gentoo.org/wiki/NVIDIA/Optimus/EDID_Xorg.conf_Example | group: Gentoo Wiki (Main) | wiki-title: NVIDIA/Optimus/EDID Xorg.conf Example -->
---
title: NVIDIA/Optimus/EDID Xorg.conf Example
url: https://wiki.gentoo.org/wiki/NVIDIA/Optimus/EDID_Xorg.conf_Example
hostname: gentoo.org
sitename: NVIDIA/Optimus/EDID Xorg.conf Example
date: "2019-01-12"
fingerprint: "83851abc3f3def5d"
license: CC BY-SA 4.0
---

# NVIDIA/Optimus/EDID Xorg.conf Example

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Below is an example configuration of the file /etc/X11/xorg.conf from a working NVIDIA Optimus configuration on a Clevo W670SR notebook. Note the "CustomEDID" portions:

FILE **`/etc/X11/xorg.conf`****Xorg Configuration example for a Clevo W670SR Notebook**

```
Section "ServerLayout"
    Identifier     "Layout0"
    Screen      0  "Screen0"
    Inactive       "intel"
    InputDevice    "Keyboard0" "CoreKeyboard"
    InputDevice    "Mouse0" "CorePointer"
EndSection
Section "Files"
EndSection
Section "InputDevice"
    # generated from data in "/etc/conf.d/gpm"
    Identifier     "Mouse0"
    Driver         "evdev"
    Option         "Protocol"
    Option         "Device" "/dev/input/mice"
    Option         "Emulate3Buttons" "no"
    Option         "ZAxisMapping" "4 5"
EndSection
Section "InputDevice"
    # generated from default
    Identifier     "Keyboard0"
    Driver         "evdev"
EndSection
Section "Monitor"
    Identifier     "Monitor0"
    VendorName     "Unknown"
    ModelName      "Unknown"
EndSection
Section "Device"
    Identifier     "Device0"
    Driver         "nvidia"
    VendorName     "NVIDIA Corporation"
    # Stuff on the net says to use BusID "PCI:1:0:0" but the manual says to use "1@0:0:0"
    BusID          "1@0:0:0"
    Option         "ConnectedMonitor" "DFP-0"
    Option         "CustomEDID" "DFP-0:/lib/firmware/edid/1920x1080_Clevo_W670SR.bin"
    Option         "UseEDID" "True"
    Option         "ModeValidation" "NoVirtualSizeCheck"
    # Option       "ModeDebug" "True"
EndSection
Section "Screen"
    Identifier     "Screen0"
    Device         "Device0"
    Monitor        "Monitor0"
    Option         "RenderAccel" "True"
    Option         "NoRenderExtension" "False"
    Option         "AllowGLXWithComposite" "True"
    Option         "AddARGBGLXVisuals" "True"
    Option         "DamageEvents" "True"
    Option         "ConnectToAcpid" "True"
    SubSection     "Display"
        Depth       24
        Modes      "1920x1080"
    EndSubSection
EndSection
Section "Device"
    Identifier     "intel"
    Driver         "modesetting"
    Option         "CustomEDID" "DFP-0:/lib/firmware/edid/1920x1080_Clevo_W670SR.bin"
EndSection
Section "Screen"
    Identifier    "intel"
    Device        "intel"
    SubSection     "Display"
        Depth       24
        Modes      "1920x1080"
    EndSubSection
    Monitor        "Monitor0"
EndSection
Section "Extensions"
    Option         "Composite" "Enable"
EndSection
```
