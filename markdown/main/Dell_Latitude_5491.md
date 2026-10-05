<!-- source: https://wiki.gentoo.org/wiki/Dell_Latitude_5491 | group: Gentoo Wiki (Main) | wiki-title: Dell Latitude 5491 -->
---
title: Dell Latitude 5491
url: https://wiki.gentoo.org/wiki/Dell_Latitude_5491
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-20"
fingerprint: "7f4a8d2553aa1368"
license: CC BY-SA 4.0
---

# Dell Latitude 5491

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The Dell Latitude 5491 is a 14" laptop with 8th generation Intel Core i5/i7 CPU and a dedicated NVidia GeForce MX130 GPU.

## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | Intel Core i5/i7 8th gen. |  |  |  | 5.4.38 |  | 
| PCIe controller | Intel Corporation Xeon E3-1200 v5/E3-1500 v5/6th Gen Core Processor PCIe Controller |  |  | pcieport | 5.4.38 | Twice | 
| PCIe controller | Intel Corporation Cannon Lake PCH PCI Express Root Port |  |  | pcieport | 5.4.38 | Twice, once used for the Alps synaptics | 
| Video | Intel Corporation UHD Graphics 630 |  |  | i915 | 5.4.38 |  | 
| Video | NVidia GeForce MX130 (GM108M) |  |  | nouveau or nvidia | 5.4.38 | Tested with proprietary drivers | 
| Audio | Intel Corporation Cannon Lake PCH cAVS |  |  | snd\_hda\_intel | 5.4.38 |  | 
| Ethernet | Intel Corporation Ethernet Connection I219-LM |  |  | e1000e | 5.4.38 |  | 
| WiFi | Intel Corporation Wireless-AC 9560 \[Jefferson Peak\] |  |  | [iwlwifi](https://wiki.gentoo.org/wiki/Iwlwifi) | 5.4.38 | [Requires firmware](https://wireless.wiki.kernel.org/en/users/drivers/iwlwifi?s%5b%5d=AC%209560#firmware) | 
| WWAN | Sierra Wireless, Inc. [DW5811e](https://linux-hardware.org/index.php?id=usb:413c-81b6) Snapdragon™ X7 LTE |  | 413c:81b6 |  |  |  | 
| Bluetooth | Bluetooth |  | 8087:0aaa | btusb | 5.4.38 |  | 
| USB controller | Intel Corporation Cannon Lake PCH USB 3.1 xHCI Host Controller |  |  | xhci\_hcd | 5.4.38 |  | 
| Sata controller | Intel Corporation 82801 Mobile SATA Controller |  |  | ahci | 5.4.38 |  | 
| SD card reader | Realtek Semiconductor Co., Ltd. RTS525A PCI Express Card Reader |  |  | rtsx\_pci | 5.4.38 | Does not detect the insertion & removal of the SD card straight away | 
| Webcam | Sunplus Innovation Technology Inc. |  | 1bcf:2b96 | uvcvideo | 5.4.38 |  | 
| Fingerprint reader | Broadcom Corp 5880 |  | 0a5c:5834 |  |  |  | 
| Thermal | Intel Corporation Xeon E3-1200 v5/E3-1500 v5/6th Gen Core Processor Thermal Subsystem |  |  | proc\_thermal | 5.4.38 |  | 
| Thermal | Intel Corporation Cannon Lake PCH Thermal Controller |  |  | intel\_pch\_thermal | 5.4.38 |  | 
|  | Intel Corporation Xeon E3-1200 v5/v6 / E3-1500 v5 / 6th/7th/8th Gen Core Processor Gaussian Mixture Model |  |  |  | 5.7.38 | [Linux hardware status](https://linux-hardware.org/index.php?id=pci:8086-1911-8086-2064) | 
| I2C controllers | Intel Corporation Cannon Lake PCH Serial IO I2C Controller |  |  | intel-lpss | 5.4.38 | Twice | 
| SMBus | Intel Corporation Cannon Lake PCH SMBus Controller |  |  | i801\_smbus | 5.4.38 |  | 

For [hardware probes](https://wiki.gentoo.org/wiki/Hardware_probe) see [https://linux-hardware.org/index.php?view=computers&vendor=Dell&model=Latitude+5491](https://linux-hardware.org/index.php?view=computers&vendor=Dell&model=Latitude+5491)

## Installation

### Firmware

The [Linux firmware](https://wiki.gentoo.org/wiki/Linux_firmware) package will have to be installed, for certain components to work properly

`root #``emerge --ask sys-kernel/linux-firmware`
- iwlwifi-9000-pu-b0-jf-b0-34.ucode
- i915/skl\_dmc\_ver1\_27.bin


And add them space-separated in the kernel firmware selection:

**Enable firmware**

### Microcode

Be sure to follow the Wiki page on how to load the [Intel microcode](https://wiki.gentoo.org/wiki/Intel_microcode)

### Kernel

#### Touchpad

To set the touchpad and keyboard knob in the kernel, do the following.

**Enable support for touchpad and knob**

Do not forget to add the `libinput` in the `INPUT_DEVICE` variable of the /etc/portage/package.use file:

**`/etc/portage/package.use/00input`**

```
 INPUT_DEVICES: libinput
```
Here is what is excpected:

`user $``dmesg`
\[    2.075357\] mousedev: PS/2 mouse device common for all mice
\[    2.190180\] input: DELL0818:00 044E:121F Mouse as /devices/pci0000:00/0000:00:15.1/i2c\_designware.1/i2c-9/i2c-DELL0818:00/0018:044E:121F.0001/input/input7
\[    2.190347\] hid-multitouch 0018:044E:121F.0001: input,hidraw0: I2C HID v1.00 Mouse \[DELL0818:00 044E:121F\] on i2c-DELL0818:00

#### SD card reader

**Enable support for the SD card reader**

#### Webcam

**Enable support for the webcam**

And add a user to the video group to access the /dev/video0.

`root #``gpassword -a <user> video`
