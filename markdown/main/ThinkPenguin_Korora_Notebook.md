<!-- source: https://wiki.gentoo.org/wiki/ThinkPenguin_Korora_Notebook | group: Gentoo Wiki (Main) | wiki-title: ThinkPenguin Korora Notebook -->
---
title: ThinkPenguin Korora Notebook
url: https://wiki.gentoo.org/wiki/ThinkPenguin_Korora_Notebook
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "3f4c896cdfaa566b"
license: CC BY-SA 4.0
---

# ThinkPenguin Korora Notebook

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



## Hardware Support

### Summary

| Device | Works? | Description | 
|---|---|---|
| Processor |  | 4th Gen Intel Core i5-4200U 1.6Ghz | 
| Screen |  | 14.1" 1366x768 LED Wide Screen | 
| Wireless |  | 802.11N Atheros Wifi (freedom compatible chipset) | 
| Webcam |  | Built-in 1.3 Mega Pixel Camera | 
| Card Reader |  | SD/MMC Card Reader | 
| Optical Drive |  | Super-Multi Drive (DVD-RAM/R/RW/+/-/CD-R/RW) | 
| Graphics |  | Intel® HD Graphics 4400 | 
| Built-in Audio & Mic |  | Intel HD | 
| LAN |  | 10/100/1000 Mbps Gigabit Ethernet | 
| VGA |  |  | 
| USB 2.0 x 3 |  |  | 
| Microphone-in |  | TRRS jack (i.e. uses same socket as Headphone-out) | 
| Headphone-out |  |  | 

### Extra Hardware Information

`root #``lspci`
00:00.0 Host bridge: Intel Corporation Haswell-ULT DRAM Controller (rev 09)
00:02.0 VGA compatible controller: Intel Corporation Haswell-ULT Integrated Graphics Controller (rev 09)
00:03.0 Audio device: Intel Corporation Device 0a0c (rev 09)
00:14.0 USB controller: Intel Corporation Lynx Point-LP USB xHCI HC (rev 04)
00:16.0 Communication controller: Intel Corporation Lynx Point-LP HECI #0 (rev 04)
00:1b.0 Audio device: Intel Corporation Lynx Point-LP HD Audio Controller (rev 04)
00:1c.0 PCI bridge: Intel Corporation Lynx Point-LP PCI Express Root Port 3 (rev e4)
00:1c.3 PCI bridge: Intel Corporation Lynx Point-LP PCI Express Root Port 4 (rev e4)
00:1d.0 USB controller: Intel Corporation Lynx Point-LP USB EHCI #1 (rev 04)
00:1f.0 ISA bridge: Intel Corporation Lynx Point-LP LPC Controller (rev 04)
00:1f.2 SATA controller: Intel Corporation Lynx Point-LP SATA Controller 1 \[AHCI mode\] (rev 04)
00:1f.3 SMBus: Intel Corporation Lynx Point-LP SMBus Controller (rev 04)
01:00.0 Unassigned class \[ff00\]: Realtek Semiconductor Co., Ltd. Device 5289 (rev 01)
01:00.2 Ethernet controller: Realtek Semiconductor Co., Ltd. RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller (rev 0a)
02:00.0 Network controller: Qualcomm Atheros AR9285 Wireless Network Adapter (PCI-Express) (rev 01)

`root #``lsusb`
Bus 001 Device 004: ID 174f:14a1 Syntek 
Bus 001 Device 002: ID 8087:8000 Intel Corp. 
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

## Configuration

### Touchpad

The Touchpad takes a lot of physical physical space under the keyboard, and it's easy to accidentally move the mouse cursor while typing. One can address this by enabling the Synaptics palm detection:

`user $``synclient PalmDetect=1``user $``synclient PalmMinWidth=5`
`synclient` is installed by the [x11-drivers/xf86-input-synaptics](https://packages.gentoo.org/packages/x11-drivers/xf86-input-synaptics); see [synaptics driver installation instructions](https://wiki.gentoo.org/wiki/Synaptics#Driver).

### Disk Drives

### Graphics

### Network Devices

### Sound

### Webcam

### Backlight

The kernel parameter `acpi_backlight=vendor` must be added to make the backlight adjustable.

The backlight setting is not persistent between reboots, but it is persistent between suspend. Create a local service to save and apply the backlight setting:

**`/etc/local.d/backlight.stop`**

**Save backlight setting**

```
#!/bin/sh
einfo "Saving backlight brightness"
cat /sys/class/backlight/intel_backlight/brightness > /var/tmp/backlight_brightness
```
**`/etc/local.d/backlight.start`**

**Restore backlight setting**

```
#!/bin/sh
brightness=`cat /var/tmp/backlight_brightness`
max_brightness=`cat /sys/class/backlight/intel_backlight/max_brightness`
percent() {
    # Returns string in percent.
    # Example:
    #     $(percent 2 3) -> "66%"
    a=$1
    b=$2
    tens=$((a * 100 / b))
    echo "${tens}%"
}
# The COMPAL display brightness allows very low intensity values which
# black out the screen!  We don't want this to happen, so ignore low
# brightness values.
if [ $brightness > 4 ]; then
    einfo "Restoring backlight brightness to $(percent brightness max_brightness)"
    echo $brightness > /sys/class/backlight/intel_backlight/brightness
fi
```
**`/etc/pm/sleep.d/xscreensaver`**

**Unblank screen on resume**

```
                                                                     
case "$1" in
     suspend|hibernate)
        # No need to lock screen as XFCE does that for you.                     
        ;;
    resume|thaw)
        # Restore brightness.                                                   
        brightness=`cat /var/tmp/backlight_brightness`
        echo $brightness > /sys/class/backlight/intel_backlight/brightness
        xscreensaver-command -deactivate
        ;;
    *)
        exit 1
        ;;
esac
exit 0
```
### CPU Frequency scaling

### SD Card Reader
