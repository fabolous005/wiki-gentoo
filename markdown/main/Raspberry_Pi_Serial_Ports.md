<!-- source: https://wiki.gentoo.org/wiki/Raspberry_Pi_Serial_Ports | group: Gentoo Wiki (Main) | wiki-title: Raspberry Pi Serial Ports -->
---
title: Raspberry Pi Serial Ports
url: https://wiki.gentoo.org/wiki/Raspberry_Pi_Serial_Ports
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-19"
fingerprint: "5d1a2ed7d997318e"
license: CC BY-SA 4.0
---

# Raspberry Pi Serial Ports

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Overview

The Gentoo arm64 stage3 at the time of this writing is missing a couple of things to make the serial ports behave as they do in Raspbian Jessie. There is no Udev rule to create the serial0 and serial1 symlinks that point to the serial ports. In Raspbian, the serial0 symlink always points to GPIO14:15 while serial1 always points to the other serial port. Raspbian has a custom Udev rule that is responsible for this, but missing in the arm64 Gentoo stage3.

While Gentoo itself does not depend on the symlinks, they are a nice convenience feature as serial0 will always point to GPIO14:15 regardless of the serial port assigned to GPIO14:15. Code that has been written using these symlinks will not have to be modified run under Gentoo.

The other issue at hand is the console serial port assignment in /etc/inittab. By default, /etc/inittab assigns ttyAMA0 as the console serial port. For non-wireless/Bluetooth Pi units, it works perfectly fine as ttyAMA0 is the console serial port. However, on wireless/Bluetooth equipped units such as the Pi 3 and the Pi Zero W, ttyAMA0 defaults to the Bluetooth module. This cannot work as the console takes over ttyAMA0, creating a conflict.

## UART Setup

In order to set up Gentoo such that it follows the Raspbian configuration, we need to do a few things -

- Unassign ttyAMA0 as the console UART in /etc/inittab
- Assign ttyS0 as the console UART in /etc/inittab (optional)
- Create the dialout group
- Create udev rules file which handles the serial symlinks and assigns the serial ports to the dialout group
- Edit the config.txt and cmdline.txt files for your desired operation

### Changing Console Assignment

#### inittab

Raspbian Jesse does the serial console assignment at the kernel level by passing console=ttyAMA0,115200 to the kernel command line in config.txt. Gentoo, however, uses OpenRC and thus uses /etc/inittab to assign the serial console.

Open up /etc/inittab -

`root #` `nano -w /etc/inittab`
Find the line below and comment it out by appending a # at the beginning of the line -

**`/etc/inittab`**

Save the file and exit.

### Create The Dialout Group

Create the dialout group with a GID of 20 -

`root #` `groupadd -g 20 dialout`
Any users that need serial port permissions will need to be added to the dialout group in order to use the serial ports just as they would under Raspbian Jesse.

### Create Udev Rules

#### 99-com.rules

In Raspbian Jessie, there is a custom Udev rule in /etc/udev/rules.d called 99-com.rules. This rule is responsible for ensuring that the serial0 symlink always points to the GPIO14:15 UART while serial1 always points to the other UART. I've made a slight modification to it to assign the serial hardware to the dialout group.

Create the file 99-com.rules -

`root #` `nano -w /etc/udev/rules.d/99-com.rules`
Copy this text and paste it into the file.

**`/etc/udev/rules.d/99-com.rules`**

Save and exit the file.

### Modify The Boot Files

#### config.txt

Mount the boot partition -

`root #` `mount /boot`
Open up config.txt -

`root #` `nano -w /boot/config.txt`
Add the following at the end of the file -

NOTE: For the RPI5 see RPI5 info as there is an addtional step you need to take for the serial output to be used on GPIO. You will need to add dtparam=uart0 and dtparam=uart0\_console.

**`/boot/config.txt`**

This locks the VPU core frequency at 250MHz under normal mode and 400MHz in turbo mode. Locking the VPU core frequency allows the mini UART to maintain a consistent baud rate.

Save and exit the file.

#### cmdline.txt

The console= parameter was used in Raspbian Jessie to assign ttyS0 to the console output. Gentoo uses OpenRC and /etc/inittab to assign a serial port to the console output. The caveat to this is that you will not have serial console access until after /root is mounted. If boot time serial console access is necessary, pass the console= parameter at the kernel command line in cmdline.txt.

Open up cmdline.txt -

`root #` `nano -w /boot/cmdline.txt`
Add console=ttyS0,115200 to the kernel command line in the file. An example kernel command line is shown below -

**`/boot/cmdline.txt`**

Save and exit the file, then reboot the Pi -

`root #` `reboot`
### Checking The Configuration

Once the Pi is back up, type the following commands -

`root #````
 ls -l /dev | grep ttyS0 && ls -l /dev | grep ttyAMA0
```
lrwxrwxrwx 1 root root           5 Aug 14 22:28 serial0 -> ttyS0
crw-rw---- 1 root dialout   4,  64 Aug 14 22:28 ttyS0
lrwxrwxrwx 1 root root           7 Aug 14 22:28 serial1 -> ttyAMA0
crw-rw---- 1 root dialout 204,  64 Aug 14 22:28 ttyAMA0

### Moving The Serial Ports

By default, the Raspberry Pi 3 Model B assigns ttyS0 to GPIO14:15 while ttyAMA0 serves the Bluetooth module. As the mini UART is not a full featured UART, you may want to use ttyAMA0 on GPIO14:15 instead as it is a full featured UART. Fortunately, there are a couple of device tree overlays that will accomplish this.

#### pi3-miniuart-bt

This overlay flip flops the UARTs by assigning ttyAMA0 to GPIO14:15 while assigning ttyS0 to the Bluetooth module.

#### pi3-disable-bt

This overlay assigns ttyAMA0 to GPIO14:15 while disabling Bluetooth altogether.

In order to implement these overlays, we must add them to our config.txt file. It is recommended that you place both of these overlays in config.txt, then uncomment the one that suits your purpose.

**`/boot/config.txt`**

Once you've made the change, save and reboot the Pi.

### Checking The Configuration

Once the Pi is back up, run ls -l again to check the configuration -

`root #````
 ls -l /dev | grep ttyAMA0 && ls -l /dev | grep ttyS0
```
lrwxrwxrwx 1 root root           7 Dec 31  1969 serial0 -> ttyAMA0
crw-rw---- 1 root dialout 204,  64 Aug 14 22:28 ttyAMA0
lrwxrwxrwx 1 root root           5 Dec 31  1969 serial1 -> ttyS0
crw-rw---- 1 root dialout   4,  64 Dec 31  1969 ttyS0

Once the device tree was implemented and the UARTs were switched around, you'll notice that serial0 now points to ttyAMA0 since it is now on GPIO14:15.

You can think of the serial0 symlink as being GPIO14:15 while ttyAMA0 is the PL011 UART and ttyS0 is the mini UART. The serial0 symlink being GPIO14:15, it is always going to point to the UART which is assigned to those pins.

## Where To Get Help

Raspberry Pi Foundation UART Page - [https://www.raspberrypi.com/documentation/computers/configuration.html#configure-uarts](https://www.raspberrypi.com/documentation/computers/configuration.html#configure-uarts) 

Gentoo On Alternative Architectures - [https://forums.gentoo.org/viewforum-f-32.html](https://forums.gentoo.org/viewforum-f-32.html)

## Acknowledgements

NeddySeagoon - his articles on Gentoo Raspberry Pi 64-bit got me a working Gentoo install on my Pi 3 Model B, and I've been toying with it since.
