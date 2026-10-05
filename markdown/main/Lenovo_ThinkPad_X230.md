<!-- source: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_X230 | group: Gentoo Wiki (Main) | wiki-title: Lenovo ThinkPad X230 -->
---
title: Lenovo ThinkPad X230
url: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_X230
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "7f4e9f15d32ad46a"
license: CC BY-SA 4.0
---

# Lenovo ThinkPad X230

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

## Hardware

`root #``lspci`
00:00.0 Host bridge: Intel Corporation 3rd Gen Core processor DRAM Controller (rev 09)
00:02.0 VGA compatible controller: Intel Corporation 3rd Gen Core processor Graphics Controller (rev 09)
00:14.0 USB controller: Intel Corporation 7 Series/C210 Series Chipset Family USB xHCI Host Controller (rev 04)
00:16.0 Communication controller: Intel Corporation 7 Series/C210 Series Chipset Family MEI Controller #1 (rev 04)
00:19.0 Ethernet controller: Intel Corporation 82579LM Gigabit Network Connection (rev 04)
00:1a.0 USB controller: Intel Corporation 7 Series/C210 Series Chipset Family USB Enhanced Host Controller #2 (rev 04)
00:1b.0 Audio device: Intel Corporation 7 Series/C210 Series Chipset Family High Definition Audio Controller (rev 04)
00:1c.0 PCI bridge: Intel Corporation 7 Series/C210 Series Chipset Family PCI Express Root Port 1 (rev c4)
00:1c.1 PCI bridge: Intel Corporation 7 Series/C210 Series Chipset Family PCI Express Root Port 2 (rev c4)
00:1c.2 PCI bridge: Intel Corporation 7 Series/C210 Series Chipset Family PCI Express Root Port 3 (rev c4)
00:1d.0 USB controller: Intel Corporation 7 Series/C210 Series Chipset Family USB Enhanced Host Controller #1 (rev 04)
00:1f.0 ISA bridge: Intel Corporation QM77 Express Chipset LPC Controller (rev 04)
00:1f.2 SATA controller: Intel Corporation 7 Series Chipset Family 6-port SATA Controller \[AHCI mode\] (rev 04)
00:1f.3 SMBus: Intel Corporation 7 Series/C210 Series Chipset Family SMBus Controller (rev 04)
02:00.0 System peripheral: Ricoh Co Ltd PCIe SDXC/MMC Host Controller (rev 07)
03:00.0 Network controller: Realtek Semiconductor Co., Ltd. RTL8188CE 802.11b/g/n WiFi Adapter (rev 01)

`root #``lsusb`
Bus 004 Device 002: ID 8087:0024 Intel Corp. Integrated Rate Matching Hub
Bus 004 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 003 Device 003: ID 0a5c:21e6 Broadcom Corp. BCM20702 Bluetooth 4.0 \[ThinkPad\]
Bus 003 Device 002: ID 8087:0024 Intel Corp. Integrated Rate Matching Hub
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 001 Device 002: ID 0bdb:1926 Ericsson Business Mobile Networks BV 
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

### TrackPoint Scrolling

When using [x11-drivers/xf86-input-evdev](https://packages.gentoo.org/packages/x11-drivers/xf86-input-evdev), scrolling with the TrackPoint can be enabled via [x11-apps/xinput](https://packages.gentoo.org/packages/x11-apps/xinput) and the following settings:

**`~/.xinitrc`**

```
 set-prop "TPPS/2 IBM TrackPoint" "Evdev Wheel Emulation" 1
xinput set-prop "TPPS/2 IBM TrackPoint" "Evdev Wheel Emulation Button" 2
xinput set-prop "TPPS/2 IBM TrackPoint" "Evdev Wheel Emulation Timeout" 200
xinput set-prop "TPPS/2 IBM TrackPoint" "Evdev Wheel Emulation Axes" 6 7 4 5
xinput set-prop "TPPS/2 IBM TrackPoint" "Device Accel Constant Deceleration" 0.95
```
Then press down the middle button and move the TrackPoint in the scrolling direction. It works vertically and horizontally.

### External USB keyboard and mouse issues

See:

- [http://hamwaves.com/usb.autosuspend/en/](http://hamwaves.com/usb.autosuspend/en/)
- [http://www.thinkwiki.org/wiki/How\_to\_reduce\_power\_consumption](http://www.thinkwiki.org/wiki/How_to_reduce_power_consumption) (loosely related)
