<!-- source: https://wiki.gentoo.org/wiki/NVIDIA/Optimus/xorg.conf | group: Gentoo Wiki (Main) | wiki-title: NVIDIA/Optimus/xorg.conf -->
---
title: NVIDIA/Optimus/xorg.conf
url: https://wiki.gentoo.org/wiki/NVIDIA/Optimus/xorg.conf
hostname: gentoo.org
sitename: NVIDIA/Optimus/xorg.conf
date: "2022-10-01"
fingerprint: "43259ffd831fed3d"
license: CC BY-SA 4.0
---

# NVIDIA/Optimus/xorg.conf

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Intel / AMD and Nvidia

For a laptop with Intel integrated graphics and Nvidia discrete graphics, the following xorg.conf should be sufficient:

FILE **`/etc/X11/xorg.conf.d/10-nvidia.conf`**

```
Section "ServerLayout"
    Identifier "layout"
    Screen 0 "nvidia"
    Inactive "intel"
EndSection
Section "Device"
    Identifier "nvidia"
    Driver "nvidia"
    BusID "01:00:0"
    Option "RegistryDwords" "EnableBrightnessControl=1"
EndSection
Section "Screen"
    Identifier "nvidia"
    Device "nvidia"
    Option "AllowEmptyInitialConfiguration"
EndSection
Section "Device"
    Identifier "intel"
    Driver "modesetting"
EndSection
Section "Screen"
    Identifier "intel"
    Device "intel"
EndSection
```
For a laptop with AMD integrated graphics and Nvidia discrete graphics, the following xorg.conf works for me:

FILE **`/etc/X11/xorg.conf`**

```
Section "ServerLayout"
    Identifier     "Layout0"
    Screen      0  "Screen-nvidia"
EndSection
Section "Device"
    Identifier     "Device-nvidia"
    Driver         "nvidia"
    VendorName     "NVIDIA Corporation"
    BoardName      "NVIDIA GeForce GTX 1650 Ti"
    BusID          "PCI:1:0:0"
EndSection
Section "Screen"
    Identifier     "Screen-nvidia"
    Device         "Device-nvidia"
    GPUDevice      "Device-amd"
    Option "AllowEmptyInitialConfiguration"
EndSection
Section "Device"
    Identifier     "Device-amd"
    Driver         "modesetting"
    BusID          "PCI:5:0:0"
EndSection
```
