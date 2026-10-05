<!-- source: https://wiki.gentoo.org/wiki/Lenovo_Thinkpad_E540 | group: Gentoo Wiki (Main) | wiki-title: Lenovo Thinkpad E540 -->
---
title: Lenovo Thinkpad E540
url: https://wiki.gentoo.org/wiki/Lenovo_Thinkpad_E540
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "7f5e0f35db2a166a"
license: CC BY-SA 4.0
---

# Lenovo Thinkpad E540

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | N/A |  | N/A | N/A | N/A |  | 
| GPU | Intel Corporation 4th Gen Core Processor Integrated Graphics Controller |  | N/A | N/A | N/A |  | 
| Wi-Fi | Intel Corporation Wireless 7260 |  | N/A | N/A | N/A |  | 
| Bluetooth | Intel Corporation Wireless 7260 |  | N/A | N/A | N/A |  | 
| Trackpoint | N/A |  | N/A | N/A | N/A | Since the mouse buttons got integrated into the touchpad, it is awkward to use. To enable middle button scrolling use x11-drivers/xf86-input- [libinput](https://wiki.gentoo.org/wiki/Libinput) (>=0.8.0) | 
| LCD Backlight control | N/A |  | N/A | N/A | N/A |  | 
| Hotkeys | N/A |  | N/A | N/A | N/A |  | 
| Webcam | N/A |  | N/A | N/A | N/A |  | 

`root #``lspci`
00:00.0 Host bridge: Intel Corporation Xeon E3-1200 v3/4th Gen Core Processor DRAM Controller (rev 06)
00:02.0 VGA compatible controller: Intel Corporation 4th Gen Core Processor Integrated Graphics Controller (rev 06)
00:03.0 Audio device: Intel Corporation Xeon E3-1200 v3/4th Gen Core Processor HD Audio Controller (rev 06)
00:14.0 USB controller: Intel Corporation 8 Series/C220 Series Chipset Family USB xHCI (rev 04)
00:16.0 Communication controller: Intel Corporation 8 Series/C220 Series Chipset Family MEI Controller #1 (rev 04)
00:1a.0 USB controller: Intel Corporation 8 Series/C220 Series Chipset Family USB EHCI #2 (rev 04)
00:1b.0 Audio device: Intel Corporation 8 Series/C220 Series Chipset High Definition Audio Controller (rev 04)
00:1c.0 PCI bridge: Intel Corporation 8 Series/C220 Series Chipset Family PCI Express Root Port #1 (rev d4)
00:1c.2 PCI bridge: Intel Corporation 8 Series/C220 Series Chipset Family PCI Express Root Port #3 (rev d4)
00:1c.3 PCI bridge: Intel Corporation 8 Series/C220 Series Chipset Family PCI Express Root Port #4 (rev d4)
00:1c.4 PCI bridge: Intel Corporation 8 Series/C220 Series Chipset Family PCI Express Root Port #5 (rev d4)
00:1d.0 USB controller: Intel Corporation 8 Series/C220 Series Chipset Family USB EHCI #1 (rev 04)
00:1f.0 ISA bridge: Intel Corporation HM87 Express LPC Controller (rev 04)
00:1f.2 SATA controller: Intel Corporation 8 Series/C220 Series Chipset Family 6-port SATA Controller 1 \[AHCI mode\] (rev 04)
00:1f.3 SMBus: Intel Corporation 8 Series/C220 Series Chipset Family SMBus Controller (rev 04)
02:00.0 Unassigned class \[ff00\]: Realtek Semiconductor Co., Ltd. RTS5227 PCI Express Card Reader (rev 01)
03:00.0 Ethernet controller: Realtek Semiconductor Co., Ltd. RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller (rev 10)
04:00.0 Network controller: Intel Corporation Wireless 7260 (rev 73)

`root #``lsusb`
Bus 002 Device 002: ID 8087:8000 Intel Corp. 
Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 002: ID 8087:8008 Intel Corp. 
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 003 Device 003: ID 04f2:b398 Chicony Electronics Co., Ltd 
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

## Installation

### Firmware

Wi-fi and Bluetooth firmware:

`root #``emerge --ask sys-firmware/linux-firmware`
### Kernel

### Emerge

If you are using [Xfce](https://wiki.gentoo.org/wiki/Xfce), you can install [xfce-extra/xfce4-kbdleds-plugin](https://packages.gentoo.org/packages/xfce-extra/xfce4-kbdleds-plugin) to see the status of your modifier keys (Caps-Lock, Num-Lock):

`root #``emerge --ask xfce-extra/xfce4-kbdleds-plugin`
For [GNOME](https://wiki.gentoo.org/wiki/GNOME) there is the "Lock keys" extension, which you can find on [https://extensions.gnome.org](https://extensions.gnome.org)

## Troubleshooting

### No resume possible (UEFI problem)

When you open the lid or press a button to resume, the device will not resume. The fan seems to come on, but thats it. You have to hard reset (push the power button for about 5 seconds) to make it work again. To make resume work, you have to turn off "USB 3 Mode" in the UEFI settings (pushing Enter when the laptop starts). The drawback of this method is, of course, that your USB ports will only work in USB 2 mode, which means much slower data transfers.

See [this link](https://bugs.launchpad.net/ubuntu/+source/linux/+bug/1331077) for more information.

### No Wi-Fi after resume

When the device resumes from suspend, the Wifi will no longer work. This seems to be a problem with wpa\_supplicant, because restarting it fixes it. On Ubuntu distributions, this problem does not occur, so they probably have a patch for it.

See [this link](https://bugs.launchpad.net/ubuntu/+source/gnome-nettool/+bug/1311257) for more information.

With this script in your "/etc/pm/sleep.d" folder, you can restart wpa\_supplicant after suspend:

After saving the script file, you have to make it executable:

`root #``chmod +x fix_wpa_supplicant.sh`
### Blinking power LED after resume

Sometimes, when the device resumes, the power LED will blink instead of stay lit. This can be fixed by resetting the power LED after suspend. To do this, you have to issue the following command as root:

`root #``echo '0 on' > /proc/acpi/ibm/led`
See "Documentation/laptops/thinkpad-acpi.txt" in your kernel source directory for more information.

### Hotkeys do not work after resume

Update BIOS to the latest version.
