<!-- source: https://wiki.gentoo.org/wiki/Lenovo_IdeaPad_yoga_13_(i5) | group: Gentoo Wiki (Main) | wiki-title: Lenovo IdeaPad yoga 13 (i5) -->
---
title: Lenovo IdeaPad yoga 13 (i5)
url: https://wiki.gentoo.org/wiki/Lenovo_IdeaPad_yoga_13_(i5)
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "7f4b871cdb6a322a"
license: CC BY-SA 4.0
---

# Lenovo IdeaPad yoga 13 (i5)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Still working on installation, wiki will come soon.

## Hardware

### Laptop Specifications

| Device | Model | Works | Notes | 
|---|---|---|---|
| Intel® Core™ i5 | 3317U, 1.7G/2.6G, 4T, 3M (Ivy Bridge) |  |  | 
| Intel® HD Graphics | 4000 |  | i915 kernel driver | 
| LG 13.3"1600×900 | LP133WD2 (SL)(B1) (IPS) |  | intel\_backlight | 
| Wireless | Realtek RTL8723AU |  | Support Added To Linux 3.15 through r8723au kernel driver | 
| Bluetooth | Realtek usb |  |  | 
| Camera | Lenovo |  |  | 
| Card Reader |  |  |  | 
| Touchscreen |  |  | Singletouch working | 

#### Hardware

`root #``lspci`
00:00.0 Host bridge: Intel Corporation 3rd Gen Core processor DRAM Controller (rev 09)
00:02.0 VGA compatible controller: Intel Corporation 3rd Gen Core processor Graphics Controller (rev 09)
00:04.0 Signal processing controller: Intel Corporation 3rd Gen Core Processor Thermal Subsystem (rev 09)
00:14.0 USB controller: Intel Corporation 7 Series/C210 Series Chipset Family USB xHCI Host Controller (rev 04)
00:16.0 Communication controller: Intel Corporation 7 Series/C210 Series Chipset Family MEI Controller #1 (rev 04)
00:1a.0 USB controller: Intel Corporation 7 Series/C210 Series Chipset Family USB Enhanced Host Controller #2 (rev 04)
00:1b.0 Audio device: Intel Corporation 7 Series/C210 Series Chipset Family High Definition Audio Controller (rev 04)
00:1d.0 USB controller: Intel Corporation 7 Series/C210 Series Chipset Family USB Enhanced Host Controller #1 (rev 04)
00:1f.0 ISA bridge: Intel Corporation QS77 Express Chipset LPC Controller (rev 04)
00:1f.2 SATA controller: Intel Corporation 7 Series Chipset Family 6-port SATA Controller \[AHCI mode\] (rev 04)
00:1f.3 SMBus: Intel Corporation 7 Series/C210 Series Chipset Family SMBus Controller (rev 04)
00:1f.6 Signal processing controller: Intel Corporation 7 Series/C210 Series Chipset Family Thermal Management Controller (rev 04)

## Configuration details

### Graphics

See [Intel](https://wiki.gentoo.org/wiki/Intel).

#### Feature support

| Chipset | OpenGL | OpenGL ES | OpenCL | [VAAPI](https://wiki.gentoo.org/wiki/VAAPI) | USE | 
|---|---|---|---|---|---|
| Ivy Bridge |  | <sup>1</sup> | <sup>2</sup> |  | VIDEO\_CARDS="intel i965" | 

### Display

Backlight control through brightness buttons is enabled by adding the `acpi_backlight=vendor` into the /etc/default/grub, i.e. `GRUB_CMDLINE_LINUX_DEFAULT="acpi_backlight=vendor"`

### Wireless

#### Method 1: Using the kernel driver

The r8723au driver provides support for the Realtek RTL8723AU WiFi chipset. The RTL8723AU is the USB version of the Realtek 8723A chipset, with ID of 0bda:1724. Since the version 3.15 of the kernel, the RTL8723AU is supported in the staging drivers.

**Step 1ː** Configure and compile kernel

**Step 2:** The gentoo package [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) is required.

`root #``emerge --ask sys-kernel/linux-firmware`
#### Method 2: Using the lwfinger driver

`root #````
git clone git@github.com:lwfinger/rtl8723au.git
```
`root #````
cd rtl8723au/
```
`root #````
make
```
`root #````
make install
```
`root #````
modprobe rtl8723au
```
