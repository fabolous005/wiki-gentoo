<!-- source: https://wiki.gentoo.org/wiki/Raspberry_Pi_Install_Guide/HintsandTips | group: Gentoo Wiki (Main) | wiki-title: Raspberry Pi Install Guide/HintsandTips -->
---
title: Raspberry Pi Install Guide/HintsandTips
url: https://wiki.gentoo.org/wiki/Raspberry_Pi_Install_Guide/HintsandTips
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-04"
fingerprint: "1211f5dd5db6eb62"
license: CC BY-SA 4.0
---

# Raspberry Pi Install Guide/HintsandTips

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Unreliable USB Attached SCSI

If you have a Raspberry Pi 4 and are getting bad speeds transferring data to/from USB3.0 SSDs or seeing USB disconnects/resets with USB3.0 to SATA adapters (`uas_eh_device_reset_handler` in dmesg), this could be due to your device not properly implementing the [USB Attached SCSI (UAS)](https://en.wikipedia.org/wiki/USB_Attached_SCSI)[USB Attached SCSI](https://en.wikipedia.org/wiki/USB_Attached_SCSI)  specification. Refer to [STICKY: If you have a Raspberry Pi 4 and are getting bad speeds transferring data to/from USB3.0 SSDs, read this](https://forums.raspberrypi.com/viewtopic.php?t=245931) and [#3070 USB3.0 to SATA adapter causes problems](https://github.com/raspberrypi/linux/issues/3070).

## Enable discard over USB

[SSD/NVMe over USB](https://wiki.gentoo.org/wiki/Discard_over_USB) users only. Trimming SD cards works by default, provided the SD card supports trim.

## www-client/chromium

Given at least 1G of swap, its possible to emerge www-client/chromium on an 8G Pi 4.

`root #``genlop -t chromium````
 * www-client/chromium
     Thu Oct 26 23:08:54 2023 >>> www-client/chromium-119.0.6045.21
       merge time: 3 days, 10 hours, 26 minutes and 57 seconds.
```
but it will probably be out of date by the time the emerge completes.

## Widevine DRM

Digital Rights Management for Chromium and Firefox on arm64. Not required on 32 bit installs.

Install [sys-fs/squashfs-tools](https://packages.gentoo.org/packages/sys-fs/squashfs-tools) as the widevine-installer script needs it.

`root #``emerge sys-fs/squashfs-tools`
as it has to be run as root anyway.

`root #``cd widevine-installer`
and read the widevine-installer script. Satisfy yourself that it will not do anything nasty beyond downloading a widewine squashfs image, unpacking and installing it for both Chromium and Firefox.

`root #``./widevine-installer`
to install widevine and configure both Chromium and Firefox to use it.

If the browser(s) are already running, log out and back in again.

## Default kernel configuration

`root #``modprobe configs`
will provide /proc/config.gz which is the configuration file for the running kernel.

## Zram

Users with small SD cards may want to consider [zram](https://wiki.gentoo.org/wiki/Raspberry_Pi4_64_Bit_Install#Zram) which is a compressed area of ram for swap and other frequently written things. The idea being to avoid SD card writes.

## GPIO

For most things related to the GPIO pins, please see [Raspberry\_Pi\_Install\_Guide/Raspberry\_Pi\_GPIO](https://wiki.gentoo.org/wiki/Raspberry_Pi_Install_Guide/Raspberry_Pi_GPIO).
