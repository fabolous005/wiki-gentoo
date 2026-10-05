<!-- source: https://wiki.gentoo.org/wiki/I2C | group: Gentoo Wiki (Main) | wiki-title: I2C -->
---
title: I2C
url: https://wiki.gentoo.org/wiki/I2C
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-25"
fingerprint: "88a60e781a96db62"
license: CC BY-SA 4.0
---

# I2C

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes the setup of *I<sup>2</sup>C* (**I**nter-**I**ntegrated **C**ircuit) controllers.

## Hardware detection

To choose the right driver, first detect the used I<sup>2</sup>C controllers. The [lspci tool](https://wiki.gentoo.org/wiki/Hardware_detection) can be used for this task:

`root #``lspci | grep -i smbus`
## Installation

### Kernel

The following kernel options need to be activated:

Once you have rebooted with your new kernel, you can look for the I2C devices present on your host. Install the i2c-tools package, and then detect your I2C buses.

`root #````
emerge sys-apps/i2c-tools
```
`root #````
i2cdetect -l
```
i2c-0     smbus         SMBus PIIX4 adapter port 0 at 0b00    SMBus adapter

i2c-1     smbus         SMBus PIIX4 adapter port 2 at 0b00    SMBus adapter

i2c-2    smbus         SMBus PIIX4 adapter port 1 at 0b20    SMBus adapter

i2c-3     i2c           AMDGPU DM i2c hw bus 0                I2C adapter

i2c-4     i2c           AMDGPU DM i2c hw bus 1                I2C adapter

i2c-5     i2c           AMDGPU DM i2c hw bus 2                I2C adapter

i2c-6     i2c           AMDGPU DM i2c hw bus 3                I2C adapter

i2c-7     i2c           AMDGPU DM i2c hw bus 4                I2C adapter

i2c-8     i2c           AMDGPU DM aux hw bus 0                I2C adapter

i2c-9     i2c           AMDGPU DM aux hw bus 1                I2C adapter

i2c-10    i2c           AMDGPU DM aux hw bus 2                I2C adapter

i2c-11    i2c           Realtek RTL2832U reference design     I2C adapter

i2c-12    i2c           i2c-11-mux (chan_id 0)                I2C adapter
Then look at what devices are on the first 3 buses.

`root #````
i2cdetect 0
```
WARNING! This program can confuse your I2C bus, cause data loss and worse!

I will probe file /dev/i2c-0.

I will probe address range 0x08-0x77.

Continue? [Y/n]

0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f

00:                         -- -- -- -- -- -- -- --

10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --

20: 20 -- -- -- -- -- -- -- -- -- -- -- -- -- -- --

30: 30 31 -- -- 34 35 36 -- -- -- -- -- -- -- -- --

40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --

50: 50 51 -- -- -- -- -- -- -- -- -- -- -- -- -- --

60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --

70: -- -- -- -- -- -- -- --

`root #````
i2cdetect 1
```
WARNING! This program can confuse your I2C bus, cause data loss and worse!

I will probe file /dev/i2c-1.

I will probe address range 0x08-0x77.

Continue? [Y/n]

0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f

00:                         -- -- -- -- -- -- -- --

10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --

20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --

30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --

40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --

50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --

60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --

70: -- -- -- -- -- -- -- --

`root #````
i2cdetect 2
```
WARNING! This program can confuse your I2C bus, cause data loss and worse!

I will probe file /dev/i2c-2.

I will probe address range 0x08-0x77.

Continue? [Y/n]

0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f

00:                         -- -- -- -- -- -- -- --

10: 10 -- -- -- -- -- -- -- -- -- -- -- -- -- -- --

20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --

30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --

40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --

50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --

60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --

70: -- -- -- -- -- -- -- --
