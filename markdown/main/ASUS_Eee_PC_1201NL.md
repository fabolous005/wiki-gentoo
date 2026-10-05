<!-- source: https://wiki.gentoo.org/wiki/ASUS_Eee_PC_1201NL | group: Gentoo Wiki (Main) | wiki-title: ASUS Eee PC 1201NL -->
---
title: ASUS Eee PC 1201NL
url: https://wiki.gentoo.org/wiki/ASUS_Eee_PC_1201NL
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-29"
fingerprint: "7f3a192f7baae53c"
license: CC BY-SA 4.0
---

# ASUS Eee PC 1201NL

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



## Hardware Status

| Device | Works | Notes | 
|---|---|---|
| Intel Atom N270 |  |  | 
| Nvidia ION |  |  | 
| Intel HDA |  |  | 
| Ethernet Atheros AR8132 |  |  | 
| Wireless Atheros AR9285 |  |  | 
| Bluetooth |  |  | 
| Camera |  |  | 
| Card Reader |  |  | 

## Hardware

`root #``lspci -nn`
00:00.0 Host bridge \[0600\]: nVidia Corporation MCP79 Host Bridge \[10de:0a82\] (rev b1)
00:00.1 RAM memory \[0500\]: nVidia Corporation MCP79 Memory Controller \[10de:0a88\] (rev b1)
00:03.0 ISA bridge \[0601\]: nVidia Corporation MCP79 LPC Bridge \[10de:0aae\] (rev b3)
00:03.1 RAM memory \[0500\]: nVidia Corporation MCP79 Memory Controller \[10de:0aa4\] (rev b1)
00:03.2 SMBus \[0c05\]: nVidia Corporation MCP79 SMBus \[10de:0aa2\] (rev b1)
00:03.3 RAM memory \[0500\]: nVidia Corporation MCP79 Memory Controller \[10de:0a89\] (rev b1)
00:03.5 Co-processor \[0b40\]: nVidia Corporation MCP79 Co-processor \[10de:0aa3\] (rev b1)
00:04.0 USB Controller \[0c03\]: nVidia Corporation MCP79 OHCI USB 1.1 Controller \[10de:0aa5\] (rev b1)
00:04.1 USB Controller \[0c03\]: nVidia Corporation MCP79 EHCI USB 2.0 Controller \[10de:0aa6\] (rev b1)
00:06.0 USB Controller \[0c03\]: nVidia Corporation MCP79 OHCI USB 1.1 Controller \[10de:0aa7\] (rev b1)
00:06.1 USB Controller \[0c03\]: nVidia Corporation MCP79 EHCI USB 2.0 Controller \[10de:0aa9\] (rev b1)
00:08.0 Audio device \[0403\]: nVidia Corporation MCP79 High Definition Audio \[10de:0ac0\] (rev b1)
00:09.0 PCI bridge \[0604\]: nVidia Corporation MCP79 PCI Bridge \[10de:0aab\] (rev b1)
00:0b.0 IDE interface \[0101\]: nVidia Corporation MCP79 SATA Controller \[10de:0ab5\] (rev b1)
00:10.0 PCI bridge \[0604\]: nVidia Corporation MCP79 PCI Express Bridge \[10de:0aa0\] (rev b1)
00:16.0 PCI bridge \[0604\]: nVidia Corporation MCP79 PCI Express Bridge \[10de:0ac7\] (rev b1)
00:18.0 PCI bridge \[0604\]: nVidia Corporation MCP79 PCI Express Bridge \[10de:0ac7\] (rev b1)
02:00.0 VGA compatible controller \[0300\]: nVidia Corporation Device \[10de:0876\] (rev b1)
03:00.0 Network controller \[0280\]: Atheros Communications Inc. AR9285 Wireless Network Adapter (PCI-Express) \[168c:002b\] (rev 01)
05:00.0 Ethernet controller \[0200\]: Attansic Technology Corp. Atheros AR8132 / L1c Gigabit Ethernet Adapter \[1969:1062\] (rev c0)

`user $``lsusb`
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 003 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
Bus 004 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
Bus 001 Device 002: ID 13d3:5111 IMC Networks Integrated Webcam
Bus 004 Device 002: ID 0b05:1789 ASUSTek Computer, Inc.

## CPU

**`/etc/portage/make.conf`**

```
CHOST="i686-pc-linux-gnu"
CFLAGS="-march=prescott -O2 -fomit-frame-pointer -pipe"
CXXFLAGS="${CFLAGS}"
```
## Video

### Kernel

**nVidia**

### Install driver

`root #``emerge --ask x11-drivers/nvidia-drivers`
### Xorg

**`/etc/X11/xorg.conf`**

```
Section "Files"
 ModulePath   "/usr/lib/xorg/modules"
 FontPath     "/usr/share/fonts/misc/"
 FontPath     "/usr/share/fonts/TTF/"
 FontPath     "/usr/share/fonts/OTF/"
 FontPath     "/usr/share/fonts/Type1/"
 FontPath     "/usr/share/fonts/100dpi/"
 FontPath     "/usr/share/fonts/75dpi/"
EndSection
 
Section "Module"
 Load    "record"
 Load    "glx"
 Load    "dbe"
 Load    "extmod"
EndSection
 
Section "Monitor"
 Identifier "Monitor0"
 ModelName  "Asus 1201NL"
 Option "DPI" "96x96"
EndSection
 
Section "Device"
 Identifier "Card0"
 Driver     "nvidia"
 BusID      "PCI:2:0:0"
 Option     "NoLogo"                "True"
 Option     "AllowGLXwithComposite" "True"
 Option     "TripleBuffer"          "True"
 Option     "AddARGBGLXVisuals"     "True"
EndSection
 
Section "Screen"
 Identifier "Screen0"
 Device     "Card0"
 Monitor    "Monitor0"
 SubSection "Display"
  Viewport   0 0
  Depth     24
 EndSubSection
EndSection
 
Section "ServerFlags"
    Option      "AllowEmptyInput" "on"
    Option      "AutoAddDevices" "on"
    Option      "AutoEnableDevices" "on"
EndSection
 
Section "ServerLayout"
    Identifier  "Asus 1201NL"
    Screen 0    "Screen0" 0 0
EndSection
```
## Sound

**Intel HDA**

## Ethernet

**AR8132**

## Wireless

**AR9285**

## Bluetooth

**Bluetooth**

## Fn Keys

## Camera

`user $``dmesg | grep EasyCamera`
uvcvideo: Found UVC 1.00 device USB2.0 UVC VGA WebCam (13d3:5111)

**Test Camera**

`user $``mplayer -tv driver=v4l2:device=/dev/video0 tv://`
**Bug**
On new kernel (3.3.8) camera hangs the system, to avoid this, build driver as module.

And put below options to the module.

`root #``echo "options uvcvideo nodrop=1" > /etc/modprobe.d/uvcvideo.conf`
## Sleep

If after Fn-F2 laptop goes to suspend and wakes up immediately, run as root code below and try again.

`root #``echo US15 > /proc/acpi/wakeup`
If the problem was fixed, add this code to autostart. For example:

`root #````
echo "echo US15 > /proc/acpi/wakeup" > /etc/local.d/suspend_fix.start
```
`root #``chmod +x /etc/local.d/suspend_fix.start`
