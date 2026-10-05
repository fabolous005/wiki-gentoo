<!-- source: https://wiki.gentoo.org/wiki/Gqrx | group: Gentoo Wiki (Main) | wiki-title: Gqrx -->
---
title: Gqrx
url: https://wiki.gentoo.org/wiki/Gqrx
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-08-11"
categories: ['Official documentation']
fingerprint: a6821e16cb323990
license: CC BY-SA 4.0
---

# Gqrx

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**gqrx** is an open source software defined radio (SDR) receiver powered by GNU Radio and the Qt graphical toolkit. It supports multiple SDR hardware devices, including (but not limited to) the RTL-SDR, HackRF, and Airspy. A complete list of supported hardware can be found at [https://gqrx.dk/supported-hardware](https://gqrx.dk/supported-hardware).

Notable features include AM, SSB, CW, and (wide) FM demodulation, spectrum plots and waterfalls, and the ability to record and stream audio output. A more complete list of features can be found on the gqrx website: [https://gqrx.dk/](https://gqrx.dk/)

## Installation

### Kernel

If using an RTL-SDR, the kernel must be configured to blacklist the **rtl2832** module, as the normal DVB-T driver is not compatible for its use as an SDR. More information regarding this can be found on the [RTL-SDR](https://wiki.gentoo.org/wiki/Rtl-sdr) wiki page.

### USE flags

USE flags for gqrx itself are as follows:

It is important to specify the correct USE flags for the net-wireless/gr-osmosdr dependency, or else gqrx will not find your SDR device:


| [airspy](https://packages.gentoo.org/useflags/airspy) | Build with Airspy support through net-wireless/airspy | 
| [bladerf](https://packages.gentoo.org/useflags/bladerf) | Build with Nuand BladeRF support through net-wireless/bladerf | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [hackrf](https://packages.gentoo.org/useflags/hackrf) | Build with Great Scott Gadgets HackRF support through net-libs/libhackrf | 
| [iqbalance](https://packages.gentoo.org/useflags/iqbalance) | Enable support for I/Q balancing using gr-iqbal through net-wireless/gr-iqbal | 
| [rtlsdr](https://packages.gentoo.org/useflags/rtlsdr) | Build with Realtek RTL2832U support through net-wireless/rtl-sdr | 
| [sdrplay](https://packages.gentoo.org/useflags/sdrplay) | Enable support for SDRplay devices through net-wireless/sdrplay | 
| [soapy](https://packages.gentoo.org/useflags/soapy) | Build with SoapySDR support through net-wireless/soapysdr | 
| [uhd](https://packages.gentoo.org/useflags/uhd) | Build with Ettus Research USRP Hardware Driver support through net-wireless/uhd | 
| [xtrx](https://packages.gentoo.org/useflags/xtrx) | Build with xtrx Hardware Driver support through net-wireless/libxtrx | 

For example, if one has an RTL-SDR, the following can be used:

**`/etc/portage/package.use`**

**Setting USE variable for the RTL-SDR**

### Emerge

`root #``emerge --ask net-wireless/gqrx`
## Configuration

### Files

- \~/.config/gqrx/default.conf - User configuration file for the configured SDR device.

The above file will be created on first launch when selecting the SDR device and associated options. If the supplied config results in an error, gqrx will detect this and let the user re-specify their config on the next launch.

## Usage

### Invocation

To start gqrx:

`user $``gqrx`
Additional options can be seen with -h/--help:

`user $``gqrx --help`
Controlport disabled
Gqrx software defined radio receiver 2.12
Command line options:
  -h \[ --help \]         This help message
  -s \[ --style \] arg    Use the give style (fusion, windows)
  -l \[ --list \]         List existing configurations
  -c \[ --conf \] arg     Start with this config file
  -e \[ --edit \]         Edit the config file before using it
  -r \[ --reset \]        Reset configuration file

### Scanning

The [net-wireless/gqrx-scanner](https://packages.gentoo.org/packages/net-wireless/gqrx-scanner) package can be used while gqrx is running to scan a range of frequencies. Refer to [the project page](https://github.com/neural75/gqrx-scanner) for further information.

## Troubleshooting

**SDR device not listed**

If the SDR device necessary is not listed, then it is possible that [net-wireless/gr-osmosdr](https://packages.gentoo.org/packages/net-wireless/gr-osmosdr) was not compiled with the correct USE flag(s). Launch gqrx from the command line and look for the following line:

`user $``gqrx`
...
built-in source types: ...
...

For reference, a build of gr-osmosdr with support for most SDR devices will look like this:

`user $``gqrx`
... built-in source types: file osmosdr fcd rtl rtl\_tcp plutosdr miri hackrf bladerf rfspace airspy airspyhf soapy redpitaya ...


Check the USE flags that are set for the net-wireless/gr-osmosdr package to ensure the correct one(s) are set for the needed devices.

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose net-wireless/gqrx`
