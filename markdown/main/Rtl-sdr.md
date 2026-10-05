<!-- source: https://wiki.gentoo.org/wiki/Rtl-sdr | group: Gentoo Wiki (Main) | wiki-title: Rtl-sdr -->
---
title: Rtl-sdr
url: https://wiki.gentoo.org/wiki/Rtl-sdr
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-08-11"
fingerprint: bf480e1cf9cfb5a5
license: CC BY-SA 4.0
---

# Rtl-sdr

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**RTL-SDR** is a driver that enables the use of Realtek RTL2832-series based DVB-T tuners as cheap (\<$25 USD) Software Defined Radio hardware.

## Installation

### Kernel

RTL-SDR is incompatible with the rtl2832 kernel driver. If the driver has been built as a module it must be blacklisted:

**`/etc/modprobe.d/blacklist.conf`**

```
blacklist rtl2832
```
If the rtl2832 driver has been built into the kernel it must either be built as a module and blacklisted (if DVB functionality is desired), or not selected.

**Disable rtl2832 driver**

### Userspace

Install the [net-wireless/rtl-sdr](https://packages.gentoo.org/packages/net-wireless/rtl-sdr) driver:

`root #``emerge --ask net-wireless/rtl-sdr`
Create the sdr group to allow non-root users to access the device:

`root #``groupadd sdr`
Add any users that need to access the SDR device to the sdr group:

`root #``usermod -aG sdr larry`
Use lsusb to confirm the product and vendor IDs of the device:

`user $``lsusb`
...

Bus 001 Device 008: ID 0bda:2838 Realtek Semiconductor Corp. RTL2838 DVB-T

Add a udev rule to create the device /dev/rtl\_sdr:

**`/etc/udev/rules.d/20-rtlsdr.rules`**

```
SUBSYSTEM=="usb", ATTRS{idVendor}=="0bda", ATTRS{idProduct}=="2838", GROUP="sdr", MODE="0666", SYMLINK+="rtl_sdr"
```
Reload and trigger your udev rules:

`root #``udevadm control --reload-rules``root #``udevadm trigger`
Plug in the device and ensure that /dev/rdl\_sdr is created by udev.

`root #``ls -la /dev/rtl_sdr`
Test the device using rtl\_test.

`user $``rtl_test`
0:  Realtek, RTL2838UHIDIR, SN: 00000001

Using device 0: Generic RTL2832U OEM Found Rafael Micro R820T tuner

...

User cancel, exiting...

Samples per million lost (minimum): 0
## Software

Install a package such as [net-wireless/gqrx](https://packages.gentoo.org/packages/net-wireless/gqrx) to gain a graphical view of the radio spectrum and easily capture tuned frequencies; see [Gqrx](https://wiki.gentoo.org/wiki/Gqrx) for details.

The rtl\_power utility (installed as part of [net-wireless/rtl-sdr](https://packages.gentoo.org/packages/net-wireless/rtl-sdr)) can be used to scan the spectrum available to the rtl2832 device (\~64 - 1700MHz) for a period of time and output the results to a csv file. This can be converted into a waterfall graph that can be analysed to find interesting signals to investigate.

The example command below will scan from 13MHz to 1750MHz in bins of 200kHz, with a scan interval of 10 seconds for four hours:

`user $``rtl_power -f 13M:1750M:200k -i 100 -e 4h ~/sdr_data.csv`
This can be converted to a waterfall using the script available [here](https://github.com/keenerd/rtl-sdr-misc/tree/master/heatmap).
