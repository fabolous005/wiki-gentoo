<!-- source: https://wiki.gentoo.org/wiki/Droidcam | group: Gentoo Wiki (Main) | wiki-title: Droidcam -->
---
title: Droidcam
url: https://wiki.gentoo.org/wiki/Droidcam
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-16"
fingerprint: d3811c5a31babbe8
license: CC BY-SA 4.0
---

# Droidcam

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


Droidcam is a tool to use a smartphone's cameras as webcam on a computer.

## Installation

### Kernel

Droidcam brings its own Kernel module. For it to work, the Kernel must be configured appropriately.

**Enable support for \<Software\_title>**

```
Device Drivers --->
  <M> Multimedia support --->
    [*] Cameras/video grabbers support
    [*] Media Controller API
```
The module is available as `v4l2loopback-dc` and must be loaded before starting Droidcam, for example using `modprobe v4l2loopback-dc`

### USE flags


### USE flags for
            [media-video/droidcam](https://packages.gentoo.org/packages/media-video/droidcam)
            
            Use your phone or tablet as webcam with a v4l device driver and app

| [+strip](https://packages.gentoo.org/useflags/+strip) | Allow symbol stripping to be performed by the ebuild for special files | 
| [dist-kernel](https://packages.gentoo.org/useflags/dist-kernel) | Enable subslot rebuilds on Distribution Kernel upgrades | 
| [gtk](https://packages.gentoo.org/useflags/gtk) | Build the dev-cpp/gtkmm:3.0 client. | 
| [modules-compress](https://packages.gentoo.org/useflags/modules-compress) | Install compressed kernel modules (if kernel config enables module compression) | 
| [modules-sign](https://packages.gentoo.org/useflags/modules-sign) | Cryptographically sign installed kernel modules (requires CONFIG\_MODULE\_SIG=y in the kernel) | 

## Usage

To use Droidcam, a smartphone with the Droidcam app installed is needed. The smartphone and the computer need to be able to reach each other over the network. Alternatively, a USB connection can be used. Depending on the operating system, this might require setting further options on the smartphone (e.g. enabling USB debugging).

### Connect via Wi-Fi/LAN

After starting Droidcam on the smartphone, the smartphone app shows the IP address and port for the computer to connect to.
After starting the app on the computer, `WiFi / LAN` needs to be selected and the WiFi IP and port, which are shown on the smartphone, need to be entered in the appropriate field on the computer.
After that, the two apps should be able to establish a connection.
The phone's camera is then available like any other webcam connected to the computer.

## Troubleshooting

### Droidcam/v4l2loopback device not found

If droidcam shows a message like this on startup, it is very likely that the kernel hasn't been configured properly or that the required modules haven't been loaded.
Please make sure the kernel is configured properly and `v4l2loopback-dc` been loaded.
Note that this module is not the same as `v4l2loopback`.

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose media-video/droidcam`
### Remove user configuration

User configuration is stored as a simple text file in the user's config directory. To remove the configuration:

`user $``rm ~/.config/droidcam`
