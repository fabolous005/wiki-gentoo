<!-- source: https://wiki.gentoo.org/wiki/Raspberry_Pi_Stratum_1_Time_Server | group: Gentoo Wiki (Main) | wiki-title: Raspberry Pi Stratum 1 Time Server -->
---
title: Raspberry Pi Stratum 1 Time Server
url: https://wiki.gentoo.org/wiki/Raspberry_Pi_Stratum_1_Time_Server
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-31"
fingerprint: b7b8715950a23186
license: CC BY-SA 4.0
---

# Raspberry Pi Stratum 1 Time Server

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article covers **setting up a [Raspberry Pi 4/5](https://wiki.gentoo.org/wiki/Raspberry_Pi4_64_Bit_Install) as a Stratum 1 Time Server**.

Any Pi will do for a Stratum 1 NTP server. A Pi4 or Pi5, that does not use USB for Ethernet, is recommended for minimal jitter. It also provides two USB3 ports that can be used for gigabit Ethernet so that routing can be added.

There is a lot of documentation on Pi stratum 1 NTP servers around the internet. Most of it applies to earlier models of the Pi, which have completely different I/O arrangements.

## Motivation

The price of electricity in the UK has gone up by a factor of four in the last six months (August, 2022).

I have been running an original Pi B with 256MB RAM (one of the first 10,000 made) as a stratum 1 NTP server, using GPS as a source of atomic time, to get to stratum 1. Just for my internal network. It was just a load and go Debian install, that I always intended to upgrade to Gentoo.

I have also been running a HP Gen 7 Microserver, hosting various KVMs. I'm just left with my router KVM and original Pi NTP server.

By moving the Pi NTP server to a Pi4, there will be enough spare I/O bandwidth for the router too. The net result is that the Microserver can be turned off, saving about 60W on my base load, or about 1.5kW/h per day, or £22.50/month.

## Overview

### Hardware Requirements

![](https://wiki.gentoo.org/images/thumb/5/5a/GPS_Time.jpg/300px-GPS_Time.jpg)

The following hardware is required to follow on with this article:

- A Pi 4 or Pi5 -- this article was based on a repurposed 4GB Pi 4.
- A GPS Time HAT with the PPS on GPIO 18. Do fit the battery to keep the clock alive.

That it's a GPS Time HAT with the PPS on GPIO 18 matters.

The image shows a GPS HAT, designed for a Pi 1, fitted to a Pi 4 - hence the additional GPI0 Pins.

### Software Requirements

The following packages are required:

A Pi should already have [net-misc/ntp](https://packages.gentoo.org/packages/net-misc/ntp), as its missing an RTC but it will need to be configured as a server for the local network and to use localhost as a source of time.

[sci-geosciences/gpsd](https://packages.gentoo.org/packages/sci-geosciences/gpsd) takes care of the gps side of things.

### Installation Site

The GPS antenna needs a clear view of sky towards the equator from it's position. It may not work indoors, even behind windows that are coated to retain/reflect heat as these coatings can reflect/block the GPS radio signal.

### How It Works

GPS works by solving four simultaneous equations, to derive Latitude, Longitude, Altitude and Time from at least four satellites. Modules that only care about time assume that they are at a fixed location and average out Latitude, Longitude and Altitude changes, which produces a better quality of time.

There are actually two solutions to the simultaneous equations, given only four satellites. One close to the earth, one thousands of miles away in space. The latter is discarded.

PPS on GPIO 18 is the default in the Pi pps-gpio overlay. If the GPS HAT uses a different GPIO pin, a parameter must be passed to
`dtoverlay=pps-gpio` in /boot/config.txt.

The PPS produces an interrupt, which is the boundary between seconds. It says nothing about which seconds the boundary is between.

Time is read from the GPS module over the serial port. That will be /dev/ttyS0.

## Setting Up

Carry out a basic install. Follow the [Raspberry Pi Install Guide](https://wiki.gentoo.org/wiki/Raspberry_Pi_Install_Guide). Use the `default/linux/arm64/23.0` profile as the NTP server will be headless.

Using the mainline kernel should work but old habits die hard.

As the mini-UART will be used, Wifi and Bluetooth can work, but are not included here. Bluetooth is connected to the PL011 main UART. Wifi uses the second SDIO interface.

### net-misc/ntp

At the time of writing, [parse-clocks](https://packages.gentoo.org/useflags/parse-clocks) [is masked on](https://wiki.gentoo.org/wiki/USE_flag) [net-misc/ntp](https://packages.gentoo.org/packages/net-misc/ntp) for arm64. The mask is over 10 years old and it appears to work for some. Meanwhile, add an entry to /etc/portage/profile/package.use.mask/ntp that reads `net-misc/ntp -parse-clocks`.

**TODO:
 Todo:**

- After further testing, fix the ::gentoo repo.

Build [net-misc/ntp](https://packages.gentoo.org/packages/net-misc/ntp) with `USE="caps ipv6 parse-clocks readline ssl threads"`.

[parse-clocks](https://packages.gentoo.org/useflags/parse-clocks) [adds PPS support, which is fairly essential.](https://wiki.gentoo.org/wiki/USE_flag)

### sci-geosciences/gpsd

[sci-geosciences/gpsd](https://packages.gentoo.org/packages/sci-geosciences/gpsd) needs `USE="cxx ipv6 ncurses ntp python shm sockets"` and support for the protocol used by the GPS module.

All are built by default. `GPSD_PROTOCOLS="+aivdm +ashtech +earthmate +evermore +fury +fv18 +garmin +garmintxt +geostar +gpsclock +greis +isync +itrax +mtk3301 +navcom +nmea0183 +nmea2000 +ntrip +oceanserver +oncore +passthrough +rtcm104v2 +rtcm104v3 +sirf +skytraq +superstar2 +tnt +tripmate +tsip +ublox"`

Set `GPSD_PROTOCOLS` in make.conf according to taste.

gpsmon reports the protocol is use. Such as:

`root #``gpsmon`
tcp://localhost:2947          NMEA0183>

### Kernel Options

For Pi4 `make bcm2711_defconfig` and for a Pi5 `make bcm2712_defconfig`should just work. It has `PPS_GPIO` and `8250_SERIAL` support available. The former is for our PPS interrupt and the latter is for the mini-UART.

It's full of things not needed, so feel free to slim it down.

A Pi 4 takes about an hour to build a bcm2711\_defconfig kernel for itself, so cross compiling is not the essential that it once was.

## Putting the Pieces Together.

Now all the bits are ready, it's time to set up the configuration files and start services.

### /boot/config.txt

In addition to everything else, /boot/config.txt requires:

**`/boot/config.txt`**

Adding a non default GPIO pin here is left as an exercise for the reader.

This loads the pps-gpio overlay, which sets up GPIO 18 as an input and attaches it to an IRQ. /proc/interrupts will include something like:

`root #``cat /proc/interrupts````
 
           CPU0       CPU1       CPU2       CPU3       
 49:      90274          0          0          0  pinctrl-bcm2835  18 Edge      pps@12.-1
```
### /boot/cmdline.txt

First and foremost, /boot/cmdline.txt must be a single line.

It needs to include `8250.nr_uarts=1` to enable the mini-UART.

The default `8250.nr_uarts=0` disables the mini-UART so no serial data will be shown.

### /etc/conf.d/gpsd

The non comment lines in /etc/conf.d/gpsd need to read:

**`/etc/conf.d/gpsd`**

### /etc/ntp.conf

/etc/ntp.conf needs some changes.

The first change allows all of 192.168.0.0/16 to get time from the NTP server. If the LAN uses a different private network range, change the restrict statement:

**`/etc/ntp.conf`**

Tell ntpd to use the PPS:

**`/etc/ntp.conf`**

And the Shared Memory Clock:

**`/etc/ntp.conf`**

Find a local NTP pool to use, rather then the Gentoo pools. For example, for a UK pool:

**`/etc/ntp.conf`**

### rc-update

Add both `ntpd` and `gpsd` to the default runlevel, then reboot to test.

## Powering Up The First Time

Acquiring the first satellite can take from minutes to hours. However, all satellites transmit the ephemeris, which tells the receiver where all the other satellites are. Once the receiver has the time and ephemeris, the battery makes resuming operation after a power loss fairly quick.

There are GPS receivers that have a PPS LED, which flashes once satellites have been acquired. Interestingly, in one test, the receiver will not acquire the first satellite behind thermal glass but does maintain lock once it has the ephemeris.

## Testing the NTP Server

Look at the output of uname -a and check that the date and time correspond to the date and time of the kernel build.

In the example, the position, accurate to a few meters, has been redacted:

`root #``gpsmon`
tcp://localhost:2947          NMEA0183>
┌──────────────────────────────────────────────────────────────────────────────┐
│Time: 2022-08-28T14:30:40.000Z   Lat: xx xx.xxxxxx' y   Lon: zzz zz.zzzzzz' a │
└───────────────────────────────── Cooked TPV ─────────────────────────────────┘
┌──────────────────────────────────────────────────────────────────────────────┐
│ GLGSV GNGLL GNRMC GNVTG GNGGA GNGSA GPGSV                                    │
└───────────────────────────────── Sentences ──────────────────────────────────┘
┌───────────────────────┌─────────────────────────┌────────────────────────────┐
│ SVID  PRN  Az El SN HU│Time:     143040.00	  │Time:      143040.00        │
│GP  2    2 315 22 25  Y│Latitude:   xxxx.xxxxx y │Latitude:  xxxx.xxxxx       │
│GP  3    3 103 11 18  Y│Longitude: zzzzz.zzzzz a │Longitude: zzzzz.zzzzz      │
│GP  4    4  66 45 21  Y│Speed:    0.428          │Altitude:  55.3             │
│GP  6    6 226 64 18  Y│Course:                  │Quality:   1   Sats: 12     │
│GP  7    7 159 23 30  Y│Status:   A        FAA:A │HDOP:      0.82             │
│GP  9    9 125 78 29  Y│MagVar:                  │Geoid:     bb.b             │
│GP 11   11 292 49 27  Y└───────── RMC ───────────└─────────── GGA ────────────┘
│GP 19   19 219  8 27  Y┌─────────────────────────┌────────────────────────────┐
│GP 20   20 287 26 21  Y│Mode: A3 Sats: 2 3 4 6 + │UTC:           RMS:         │
│GL  3   67  67 23 17  Y│DOP H=0.82 V=1.06 P=1.34 │MAJ:           MIN:         │
│GL  4   68  38 73 24  Y│TOFF:  0.130244530	  │ORI:           LAT:         │
│GL  5   69 263 46 24  Y│PPS: N/A                 │LON:           ALT:         │
└───v──── GSV ──────────└────── GSA + PPS ────────└─────────── GST ────────────┘

As all the satellites are all located above the receiver, altitude is very poor.

Notice the `sync_pps` and `stratum=1` below:

`root #``ntpq -c rv`
associd=0 status=0118 leap\_none, sync\_pps, 1 event, no\_sys\_peer,
version="ntpd 4.2.8p15@1.3728-o Sun Aug 28 16:43:23 UTC 2022 (1)",
processor="aarch64", system="Linux/5.15.61-v8+", leap=00, stratum=1,
precision=-22, rootdelay=0.000, rootdisp=1.105, refid=PPS,
reftime=e6b62c86.af3504fd  Sun, Aug 28 2022 18:03:50.684,
clock=e6b62c8d.b4dd7b5b  Sun, Aug 28 2022 18:03:57.706, peer=26483, tc=4,
mintc=3, offset=-0.016612, frequency=-9.991, sys\_jitter=0.002136,
clk\_jitter=0.006, clk\_wander=0.006

The microsecond values of jitter and wander are encouraging too:

`root #``ntpq -p`
remote           refid      st t when poll reach   delay   offset  jitter
==============================================================================
\*PPS(0)          .PPS.            0 l   14   16  377    0.000   -0.170   0.042
xSHM(0)          .GPS.            0 l   13   16  377    0.000  +22.963   0.801
+217.114.59.3 (d 139.143.5.31     3 u   21   64  377   25.015   +0.157   0.489
+time.shf.uk.as4 81.21.76.27      3 u   11   64  377   19.868   -0.080   0.259
+ntp-cov-1.lewis 202.70.69.81     2 u   29   64  377   24.786   -0.266   0.255

Offset and jitter are in milliseconds. Values of a few microseconds can be expected given a few days operation.

## Client Configuration

Its all very well having a local working stratum 1 NTP server on your network. Tell your clients all about it or it will not be used.

The default gentoo /etc/ntp.conf needs a one line addition

**`/etc/ntp.conf`**

This allows the four gentoo.pool.ntp.org entries to remain as fallbacks.

Restart the ntpd service.

## Client Testing

`user $``$ ntpq -c rv` associd=0 status=0615 leap\_none, sync\_ntp, 1 event, clock\_sync,
version="ntpd 4.2.8p15@1.3728-o Fri Feb 17 18:43:26 UTC 2023 (1)",
processor="x86\_64", system="Linux/6.2.1-gentoo", leap=00, stratum=2,
precision=-24, rootdelay=0.137, rootdisp=71.062, refid=192.168.100.252,
reftime=e7e92c1d.8a69a49c  Tue, Apr 18 2023 15:47:57.540,
clock=e7e92c95.bff11931  Tue, Apr 18 2023 15:49:57.749, peer=12183, tc=7,
mintc=3, offset=+0.156207, frequency=+1.166, sys\_jitter=0.120063,
clk\_jitter=0.187, clk\_wander=0.013

Notice the refid=192.168.100.252, which is the IP address of the server.

## TODO

1. Add IPv6 configuration.

## See also

- [Network Time Protocol](https://wiki.gentoo.org/wiki/Network_Time_Protocol) — used to synchronize the [system time](https://wiki.gentoo.org/wiki/System_time) with other devices over the network.
- [System time](https://wiki.gentoo.org/wiki/System_time) — is used in Unix systems to keep track of time.
