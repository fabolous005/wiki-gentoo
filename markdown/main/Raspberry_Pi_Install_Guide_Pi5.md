<!-- source: https://wiki.gentoo.org/wiki/Raspberry_Pi_Install_Guide/Pi5 | group: Gentoo Wiki (Main) | wiki-title: Raspberry Pi Install Guide/Pi5 -->
---
title: Raspberry Pi Install Guide/Pi5
url: https://wiki.gentoo.org/wiki/Raspberry_Pi_Install_Guide/Pi5
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-06-27"
fingerprint: bfd1201061ef7ee1
license: CC BY-SA 4.0
---

# Raspberry Pi Install Guide/Pi5

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

### Bluetooth

The file /lib/firmware/brcm/BCM4345C0.hcd is not in [linux-firmware](https://packages.gentoo.org/packages/linux-firmware) at the time of writing.

### Booting from NVMe

Add NVMe to the boot order. Not first though. We all build a dud kernel from time to time and need a way to recover.

My collection of Pis runs 24/7, so boot time is of not importance.

`root #``rpi-eeprom-config --edit`
[all]
BOOT_UART=1
POWER_OFF_ON_HALT=0
BOOT_ORDER=0xf641
PCIE_PROBE=1

That's SD card first, then USB mass storage and lastly NVMe.

The 'f' means repeat, if nothing was found.

This makes it easy to recover from installing a dud kernel on NVMe.

#### Root on NVMe (not /boot)

There is a selection of NVMe modules that cannot be booted from in the Pi5. The ones I have can be used for the VFAT filesystem or everything else but not for both. The problem seems to be that they go into suspend and need the SUSCLK signal to recover. This is not present on the Pi5 or the CM5 nor common NVMe HATs.

It can be added if you add the oscillator. Its a small surface mount part that needs four wires. The author can barely see it, never mind solder to it. This is not tested.

Far easier to put boot (the VFAT filesystem) on micro SD or a USB stick.

If you use one of these NVMe devices, omit 6 (NVMe) from the boot order. You already know it cannot work.

### Real Time Clock

The Pi5 has a battery backed real time clock. The battery is an optional extra. When the battery is fitted, the hwclock service can be used in place of swclock.

The RTC can be used for alarms, wakeups without a backup battery, provided the Pi is always powered, to keep the clock alive.

Batteries are available in two types. Rechargeable and non-recharagable, like most PC motherboard CMOS batteries.

Battery charging is disabled by default, which is safe as attempting to recharge a non-recharagable lithium battery is both dangerous and bad for the battery lifetime.

Users who are sure that they have a rechargeable battery need to follow [enabling trickle charging](https://www.raspberrypi.com/documentation/computers/raspberry-pi-5.html#real-time-clock-rtc) to turn on the battery charger. Heed the warning there too.

### Serial Output on GPIO 14 and 15

By default the serial output for the RPI5 goes to the dedicated serial header on the board instead of the GPIO pins. You can re-enable the output on GPIO 14/15 by adding in the following code into the config.txt file.

**`/boot/config.txt`**

**config.txt**

### Wifi

The file brcmfmac43455-sdio.txt is required in /lib/firmware/brcm/ before the wifi interface will appear in

`user $``ip link`
~~At the time of writing it was not included in [linux-firmware](https://packages.gentoo.org/packages/linux-firmware).~~

### Xorg on Pi5

The automatic setup fails on a Pi 5. The following xorg.conf fragment is required.

**`/etc/X11/xorg.conf.d/99-vc4.conf`**

**99-vc4.conf**

### Power over Ethernet (PoE) HAT

At startup, the Pi5 determines the output power capabilities of the USB-C connected PSU. With a PoE HAT in use, this check returns that the PSU is not capable of 25W and the Pi5 restricts the USB current available to 600mA. (There is no USB-C connected PSU)

600mA is fine for mice. keyboards and so on but not for storage devices. When the full 1.6A is required for USB peripherals, add the following

**`/boot/config.txt`**

**config.txt**
