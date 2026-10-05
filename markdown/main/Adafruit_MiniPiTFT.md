<!-- source: https://wiki.gentoo.org/wiki/Adafruit_MiniPiTFT | group: Gentoo Wiki (Main) | wiki-title: Adafruit MiniPiTFT -->
---
title: Adafruit MiniPiTFT
url: https://wiki.gentoo.org/wiki/Adafruit_MiniPiTFT
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-03-11"
fingerprint: "4a85bd44b8e379d3"
license: CC BY-SA 4.0
---

# Adafruit MiniPiTFT

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**DRAFT / WIP**

Adafruit MiniPiTFT is a small, 240x135 or 240x240 pixels display with two additional buttons, designed for Raspberry Pi. Adafruit only provides a raspbian installer, so these steps are necessary to work on Gentoo.

This wiki entry has not yet been tested and contains some stuff pulled from the original install script and not yet translated for Gentoo.

This is a starting point for using the Adafruit MiniPiTFT display in python OR as a console.

Original source: [https://learn.adafruit.com/adafruit-mini-pitft-135x240-color-tft-add-on-for-raspberry-pi/kernel-module-install](https://learn.adafruit.com/adafruit-mini-pitft-135x240-color-tft-add-on-for-raspberry-pi/kernel-module-install)

# Prerequisites

## Kernel config

correct kernel options should already be set ; in doubt, make sure you have

## Software packages

\# emerge dev-vcs/git
# emerge media-fonts/terminus-font media-fonts/dejavu
# emerge dev-python/wheel dev-python/numpy
## apt-get install python3-pil
# pip install --user adafruit-python-shell
# pip install --user click
# pip install --user adafruit-circuitpython-rgb-display
# pip install --upgrade --force-reinstall spidev
## echo "=media-libs/raspberrypi-userland-9999   \*\*" >> /etc/portage/package.accept\_keywords/rpi
## emerge media-libs/raspberrypi-userland
# git clone [https://github.com/adafruit/rpi-fbcp.git](https://github.com/adafruit/rpi-fbcp.git)
$ cd rpi-fbcp
$ mkdir build
$ cd build
$ cmake ..
$ make 
# install fbcp /usr/local/bin/fbcp

original script also says:

\# apt-get install -y bc fbi git python3-dev python3-pip python3-smbus python3-spidev evtest libts-bin device-tree-compiler

and

```
# dtc --warning no-unit_address_vs_reg -I dts -O dtb -o {pitft_config['overlay_dest']} {pitft_config['overlay_src']}
```
at this point you should be able to get something displayed with [https://learn.adafruit.com/pages/17702/elements/3044300/download](https://learn.adafruit.com/pages/17702/elements/3044300/download)

# Configuration

it makes no sense to clone [https://github.com/adafruit/Raspberry-Pi-Installer-Scripts.git](https://github.com/adafruit/Raspberry-Pi-Installer-Scripts.git) ; instead :

The console font needs to be changed. For OpenRC:

**`/etc/conf.d/consolefont`**

note: I have yet to find the 6x12 terminus font if that's not it.

add the correct options in /boot/cmdline.txt:

**`/boot/cmdline.txt`**

edit /boot/config.txt:

**`/boot/config.txt`**

`root #````
echo "/usr/local/bin/fbcp \&" > /etc/local.d/rpi-tft
```
`root #``chmod +x /etc/local.d/rpi-tft`
\[Unit\]
Description=Framebuffer copy utility for PiTFT
After=network.target
\[Service\]
Type=simple
ExecStartPre=/bin/sleep 10
ExecStart=/usr/local/bin/fbcp
\[Install\]
WantedBy=multi-user.target



## disabling console blanking

TODO unless it was successful above
