<!-- source: https://wiki.gentoo.org/wiki/Dell_Latitude_5580 | group: Gentoo Wiki (Main) | wiki-title: Dell Latitude 5580 -->
---
title: Dell Latitude 5580
url: https://wiki.gentoo.org/wiki/Dell_Latitude_5580
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-29"
fingerprint: "3f430d75d5a3196c"
license: CC BY-SA 4.0
---

# Dell Latitude 5580

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | [Intel i5-7300U](https://ark.intel.com/products/97472/Intel-Core-i5-7300U-Processor-3M-Cache-up-to-3_50-GHz) | Works | N/A | N/A | 4.12.12 |  | 
| Controller | Intel Sunrise Point-LP Serial IO I2C Controller | Works |  | mfd\_intel\_lpss\_{acpi/pci} | 4.12.12 | required for touchpad | 
| Controller | Intel Sunrise Point-LP Thermal subsystem | Works |  | intel\_pch\_thermal | 4.12.12 |  | 
| Video | Intel Device 5916 | Works |  | i915 | 4.12.12 |  | 
| Audio | Intel Device 9d71 | Works |  | snd\_hda\_intel | 4.12.12 |  | 
| Ethernet | [Intel I219-LM](https://ark.intel.com/products/82185/Intel-Ethernet-Connection-I219-LM) | Works |  | e1000e | 4.12.12 |  | 
| Wireless | Intel Wireless 8265 / 8275 | Works |  | [iwlwifi](https://wiki.gentoo.org/wiki/Iwlwifi) | 4.12.12 | linux-firmware-2017 wifi-8265-27.ucode | 
| Touchpad | DLL07A8:01 044E:120B | Works |  | i2c\_designware\_{core,platform} | 4.12.12 | with alps/synaptics | 
| SD Card reader | Realtek RTS525A PCI Express Card Reader | Works |  | mfd\_rtsx\_pci, mmc\_realtek\_pci | 4.12.12 |  | 
| Bluetooth | Intel Bluetooth controller | Works | 8087:0a2b | intel/ibt-12-16.sfi intel/ibt-12-16.ddc | 4.19.97 5.4.80 | with regulatory.db regulatory.db.p7s, all in CONFIG\_EXTRA\_FIRMWARE | 
| Webcam | Realtek Integrated Webcam HD | Works | 0bda:568c | uvcvideo (usb\_video\_class) | 4.12.12 |  | 
| Smartcard | Broadcom 5880 | Not tested | 0a5c:5832 |  |  |  | 

## Installation

### Firmware

You will need some firmware from the Linux firmware package:

`root #``emerge --ask sys-kernel/linux-firmware`
- iwlwifi-8265-27.ucode
- i915/kbl\_dmc\_ver1\_01.bin (be sure to follow the [Intel](https://wiki.gentoo.org/wiki/Intel) manual)

And add them space-separated in the kernel firmware selection:

**Enable firmware**

```
Device Drivers  --->
    Generic Driver Options  --->
        (iwlwifi-8265-27.ucode i915/kbl_dmc_ver1_01.bin) External firmware blobs to build into the kernel binary
        (/lib/firmware) Firmware blobs root directory
```
### Kernel

#### Touchpad

To set the touchpad (and keyboard knob) in the kernel, do the following.

**Enable support for touchpad and knob**

```
Device Drivers  --->
     I2C support  ---> 
         I2C Hardware Bus support  --->
              <*> Synopsys DesignWare Platform
     Multifunction device drivers  --->
              <*> Intel Low Power Subsystem support in PCI mode
     HID support  --->
         Special HID drivers  --->
             <*> Alps HID device support
         I2C HID support  --->
             <*> HID over I2C transport layer
```
After that, the [Synaptics](https://wiki.gentoo.org/wiki/Synaptics) article can be followed.

NOTE! Altering acpi\_osi parameter may result in touchpad being not detected and as such, not working.

#### Nvidia dGPU and freeze on starting X with dGPU switched off by bbswitch

It may occour that Xorg freezes on start if prior to that dGPU was disabled using the bbswitch kernel module. The system may also randomely freeze after waking out of s2ram. Soluton to that is to enable CONFIG\_ACPI\_REV\_OVERRIDE\_POSSIBLE and add acpi\_rev\_override=5 to boot params. [https://github.com/Bumblebee-Project/Bumblebee/issues/764#issuecomment-395869314](https://github.com/Bumblebee-Project/Bumblebee/issues/764#issuecomment-395869314)

#### SD card reader

**Enable support for the SD card reader**

```
Device Drivers  --->
     Multifunction device drivers  --->
         <*> Realtek PCI-E card reader
     <*> MMC/SD/SDIO card support  --->
         <*>   Realtek PCI-E SD/MMC Card Interface Driver
```
#### Webcam

**Enable support for the webcam**

```
Device Drivers  --->
    <*> Multimedia support  --->
        [*]   Media USB Adapters  --->.
            <*>   USB Video Class (UVC)
```
And add a user to the video group to access the /dev/video0.

`root #``gpassword -a <user> video`
