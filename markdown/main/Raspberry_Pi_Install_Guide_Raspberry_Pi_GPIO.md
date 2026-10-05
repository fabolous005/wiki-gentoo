<!-- source: https://wiki.gentoo.org/wiki/Raspberry_Pi_Install_Guide/Raspberry_Pi_GPIO | group: Gentoo Wiki (Main) | wiki-title: Raspberry Pi Install Guide/Raspberry Pi GPIO -->
---
title: Raspberry Pi Install Guide/Raspberry Pi GPIO
url: https://wiki.gentoo.org/wiki/Raspberry_Pi_Install_Guide/Raspberry_Pi_GPIO
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-30"
fingerprint: "1f4b89495cff8384"
license: CC BY-SA 4.0
---

# Raspberry Pi Install Guide/Raspberry Pi GPIO

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Overview

![](https://wiki.gentoo.org/images/thumb/3/3f/RaspberryPi_GPIO_Pinout.png/400px-RaspberryPi_GPIO_Pinout.png)

Raspberry Pi single board computers contain GPIO (General-Purpose Input-Output) header pins by default on most boards. These pins allow control of external electronic components ranging from a simple LED to more complex hardware commonly found on Raspberry Pi HATs (Hardware Attached on Top). Some GPIO pins can be configured to provide alternate functionality such as:

- PWM
- I2C
- SPI
- UART


Following steps the at [Raspberry\_Pi\_Install\_Guide#Installing\_the\_Raspberry\_Pi\_Foundation\_files](https://wiki.gentoo.org/wiki/Raspberry_Pi_Install_Guide#Installing_the_Raspberry_Pi_Foundation_files) will provide a kernel which contains the /dev/gpiomem kernel character device by default. This character device it is used by programming libraries such as python's gpiozero to provide an easy way of programming GPIO control. The idea behind the /dev/gpiomem device is to allow user access to the GPIO memory bank without giving user access to the entire system memory through the /dev/mem file. The only issue is that by default on a Gentoo system, /dev/gpiomem only allows root read and write access. This page will demonstrate how to configure udev rules so that sudo/root isn't needed to execute programs written using a GPIO library or when using command line pin control tools. These rules will also be useful for configuring alternate pin functionality such as I2C and SPI. The Raspberry Pi 5 operates a little differently to the former versions of the hardware.

### Raspberry Pi 5 Changes

All Raspberry Pi's prior to the Raspberry Pi 5 shared the same memory within the VC4 chip to control the GPIO pins, accessed through the /dev/gpiomem character device provided by the Linux Kernel. This required some library writers to manage the GPIO interface using the VC Mailbox API. The Raspberry Pi 5 brought a physically small but practically large change with it's hardware architecture; the RP1 micro-controller.

The RP1 micro-controller is responsible for managing the various south-bridge interfaces. For the purposes of GPIO access, this has meant that the VC4 graphics chip is no longer directly handling the memory for GPIO pins and subsequently, the /dev/gpiomem character device no longer exists on the Pi 5. Instead, you'll find /dev/gpiomem\[0-4\] as well as gpiochip\[0-4\]. This change has meant that some older GPIO libraries aren't compatible with the Raspberry Pi 5. As libraries adapt to the RP1, this is subject to change.

The above information can be verified by grepping through the /dev directory on a Raspberry Pi 5:

`root #``ls -lFa /dev | grep gpio`
crw-rw----   1 root gpio    254,   0 Jul  1 20:39 gpiochip0
crw-rw----   1 root gpio    254,   1 Jul  1 20:39 gpiochip1
crw-rw----   1 root gpio    254,   2 Jul  1 20:39 gpiochip2
crw-rw----   1 root gpio    254,   3 Jul  1 20:39 gpiochip3
crw-rw----   1 root gpio    254,   4 Jul  1 20:39 gpiochip4
crw-rw----   1 root gpio    234,   0 Jul  1 20:39 gpiomem0
crw-rw----   1 root gpio    239,   0 Jul  1 20:39 gpiomem1
crw-rw----   1 root gpio    238,   0 Jul  1 20:39 gpiomem2
crw-rw----   1 root gpio    236,   0 Jul  1 20:39 gpiomem3
crw-rw----   1 root gpio    235,   0 Jul  1 20:39 gpiomem4

For comparison sake, the following is the output from a Raspberry Pi 1 B+:

`root #``ls -lFa /dev | grep gpio`
crw-rw----   1 root gpio    254,   0 May 27 23:40 gpiochip0
crw-rw----   1 root gpio    242,   0 Jul  1 20:07 gpiomem

This change practically means that instead of addressing /dev/gpiomem, some libraries such as [dev-libs/libgpiod](https://packages.gentoo.org/packages/dev-libs/libgpiod) will require you to address /dev/gpiochip4. Chip 4 is the \`pinctrl\` ABI which maps to the RP1's GPIO controller chip. It's always advised to read through your chosen library's documentation to understand if there are any extra requirements when writing software for a Raspberry Pi 5.

The specifics regarding how the RP1 handles pin functionality are found within the [RP1 Peripherals datasheet](https://datasheets.raspberrypi.com/rp1/rp1-peripherals.pdf). As per the datasheet, the functional blocks and their locations on the GPIO pins have been chosen to match user-facing functions on the 40-pin header of a Raspberry Pi 4 Model B.

## Pin Functions

GPIO Pins have two different numbering schemes which they may be referred to:

- Physical Pin Numbering
  - Begins from the top left with pin 1 through to the bottom right with pin 40
  - Example: Pin 2 which refers to the 5V pin
- GPIO Pin Numbering (also referred to as BCM pin numbering)
  - Pins are connected to the Broadcom chip (RP1 on the Pi 5)
  - Example: GPIO 18 refers to physical pin 12


Different programming libraries may refer to either physical or GPIO pin numbering.

### Power & Ground

Not all pins on the GPIO header operate as Input/Outputs. Some of them provide specialized functionality such as power output and ground pins. Other pins can operate as standard GPIO but can be configured to provide interfaces for different protocols.

Raspberry Pi's provide the following power and ground pins:

- Two 5V power output pins
  - Pins 2 & 4
  - Will provide as much current as your power adapter allows
- Two 3V3 power output pins
  - Pins 1 & 17
  - 500mA - Available since the Raspberry Pi 1 B+ and newer
- Eight ground pins (all connected to the same rail)
  - Pins 6, 9, 14, 20, 25, 30, 34, and 39


The above power and ground pins can not be reconfigured for regular GPIO functionality. Devices that need higher current should be powered externally and the Pi itself should only be used to control those devices.

### GPIO

GPIO pins on Raspberry Pi's can be configured to either read input or provide output. Because the GPIO pins operate using 3.3v digital logic, the inputs and the outputs represent binary bits by setting the voltage between 0v or 3.3v. When a pin is 0v, it can be described as, "low" which represents a 0. The opposite also holds true where 3.3v is described as, "high" which represents a 1.

Setting a GPIO to either an input or output is library specific and the relevant documentation should be consulted.

The [dev-embedded/raspberrypi-utils](https://packages.gentoo.org/packages/dev-embedded/raspberrypi-utils) package provides the "pinctrl" program which allows for pull-up and pull-down reads/write of pins. This is useful for one-off tests where the user may wish to, for example, flash an LED on and off. Users may find more documentation in the [Raspberry Pi Utils pinctrl page](https://github.com/raspberrypi/utils/tree/master/pinctrl) (not to be confused with the Linux kernel driver of the same name).

GPIO pins can provide a maximum current draw of 16mA.<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup> High current demanding components such as motors should be powered externally while the GPIO pin should only be used to control the component. Motors in particular may benefit from more fine grained controlled through the use of PWM.

Less power-hungry components such LED's should use a resistor to limit the current and voltage pulled by the component. Failure to do so may damage the GPIO pin (and potentially the Raspberry Pi board itself).

### PWM

All GPIO pins can be configured to provide software PWM (Pulse-width modulation). Hardware controlled PWM is available on GPIO12, GPIO13, GPIO18, and GPIO19. PWM is useful for controlling motors and similar hardware where variable speed is desired. The methods for configuring software PWM on a pin are dependent on the GPIO library being used.

### I2C

I2C (inter-integrated circuit) is a two wire/signal serial communication protocol. The protocol is a relative of serial protocols in that it provides a master/slave relationship between devices/components. Unlike SPI, I2C allows for multiple devices or components to be configured as masters.<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup> Raspberry Pi's can be configured as either a master or slave but operating as both simultaneously is not supported.[\[4\]](https://wiki.gentoo.org#cite_note-4)

The I2C protocol needs the following two signals to function:

- SDA (Data Signal)
  - Sends/Receives data
- SCL (Data Clock)
  - Keeps the devices/components in sync


On the Raspberry Pi, the following pin pairs can provide an I2C interface<sup>[\[5\]](https://wiki.gentoo.org#cite_note-5)</sup>:

| I2C0 / EEPROM0 |  |  |  | 
|---|---|---|---|
| I2C Function | Physical Pin | Broadcom/GPIO Pin | Pin Function | 
|---|---|---|---|
| Data Signal | 27 | GPIO0 | SDA0/ID\_SD | 
| Data Clock | 28 | GPIO1 | SCL0/ID\_SC | 

The EEPROM0 I2C interface is primarily used by HATs (Hardware Attached on Top) to communicate it's identification, capabilities and firmware (device tree overlay) necessary for the HAT to function. More information about the HAT standard can be found in [HAT+ specification datasheet](https://datasheets.raspberrypi.com/hat/hat-plus-specification.pdf).

| I2C1 |  |  |  | 
|---|---|---|---|
| I2C Function | Physical Pin | Broadcom/GPIO Pin | Pin Function | 
|---|---|---|---|
| Data Signal | 3 | GPIO2 | SDA1 | 
| Data Clock | 5 | GPIO3 | SCL1 | 

I2C1 provides the interface for user projects.

Since the Raspberry Pi 4, more GPIO pins can be configured to provide I2C pins. More information can be found regarding the specifics within each Raspberry Pi's CPU (RP1 for Pi 5) peripherals datasheets. For example, for the  Pi 4 this can be found in the [BCM2711 Peripherals data sheet; chapter 3: BSC](https://datasheets.raspberrypi.com/bcm2711/bcm2711-peripherals.pdf). For the Pi 5, this can be found in the [RP1 Peripherals data sheet; chapter 3.5: I2C](https://datasheets.raspberrypi.com/rp1/rp1-peripherals.pdf).

### SPI

SPI (serial peripheral interface) is a short-distance protocol used primarily by embedded devices to communicate with components. Because SPI is a serial protocol, it operates using a master/slave communication design. On a Raspberry Pi, the SPI interface is used by a number of peripherals which can include: displays, network controllers (Ethernet, CAN bus), UARTs, etc.[\[6\]](https://wiki.gentoo.org#cite_note-6)

The SPI protocol requires 3 serial wires and one chip enable to operate in **Standard Mode**:

- CE (Chip Enable; also referred to as Chip Select)
  - The CE line is pulled low to tell the SPI peripheral that communication is ready
- SCLK (Serial Clock)
  - The master device sends a clock cycle which keeps the main device and peripheral in sync
- MOSI (Master Out, Slave In)
  - Data line from main device -> peripheral device
- MISO (Master In, Slave Out)
  - Data line from peripheral device -> main device


The SPI protocol isn't strictly defined and different hardware vendors implement the functionality of SPI in different ways. For example, some hardware such as LCD screens may only need a CE, MOSI and SCLK and use the MOSI pin as a regular GPIO (pull up and pull down) to signal when data is ready to transfer to the screen. Other hardware may use as little as 2 pin as high as 6 pins to correctly function using SPI. It's always advisable to read the datasheet of the component being using to understand how to correctly wire it up to the Pi.

The Raspberry Pi Zero, 1, 2, and 3 have two/three SPI hardware controllers.<sup>[\[7\]](https://wiki.gentoo.org#cite_note-7)</sup> The specifics of how SPI operates on these versions of the Raspberry Pi may be found within [BCM2835 Peripherals Datasheet](https://datasheets.raspberrypi.com/bcm2835/bcm2835-peripherals.pdf). Specifically Chapter 2.3 - Universal SPI Master (2x) explains how SPI operates and functions, Chapter 10 - SPI explains the specific implementations of the protocol.

Similar to I2C, each SPI interface is named as SPI0, SPI1, etc. The following tables show the pins used for each SPI interface. These tables are *very slightly* adapted from [Raspberry Pi Hardware documentation: Serial Peripheral Interface (SPI)](https://www.raspberrypi.com/documentation/computers/raspberry-pi.html#serial-peripheral-interface-spi), which you should read.

| SPI0 - Two hardware chip enables |  |  |  | 
|---|---|---|---|
| SPI function | Physical Pin | Broadcom/GPIO Pin | Pin Function | 
|---|---|---|---|
| MOSI | 19 | GPIO10 | SPI0\_MOSI | 
| MISO | 21 | GPIO9 | SPI0\_MISO | 
| SCLK | 23 | GPIO11 | SPI0\_SCLK | 
| CE0 | 24 | GPIO8 | SPI0\_CE0\_N | 
| CE1 | 26 | GPIO7 | SPI0\_CE1\_N | 

| SPI1 - Three hardware chip enables. Not available on 26 GPIO models. |  |  |  | 
|---|---|---|---|
| SPI function | Physical Pin | Broadcom/GPIO Pin | Pin Function | 
|---|---|---|---|
| MOSI | 38 | GPIO20 | SPI1\_MOSI | 
| MISO | 35 | GPIO19 | SPI1\_MISO | 
| SCLK | 40 | GPIO21 | SPI1\_SCLK | 
| CE0 | 12 | GPIO18 | SPI1\_CE0\_N | 
| CE1 | 11 | GPIO17 | SPI1\_CE1\_N | 
| CE2 | 36 | GPIO16 | SPI1\_CE2\_N | 

| SPI2 - Only available on Compute Modules only, except CM4. |  |  | 
|---|---|---|
| SPI function | Broadcom/GPIO Pin | Pin Function | 
|---|---|---|
| MOSI | GPIO41 | SPI2\_MOSI | 
| MISO | GPIO40 | SPI2\_MISO | 
| SCLK | GPIO42 | SPI2\_SCLK | 
| CE0 | GPIO43 | SPI2\_CE0\_N | 
| CE1 | GPIO44 | SPI2\_CE1\_N | 
| CE2 | GPIO45 | SPI2\_CE2\_N | 


The Raspberry Pi 4, 400, Compute Module 4, and 5 contain six usable SPI buses. SPI0 and SPI1 are still available on these newer Pi models as well as the following:

| SPI3 |  |  |  | 
|---|---|---|---|
| SPI function | Physical Pin | Broadcom/GPIO Pin | Pin Function | 
|---|---|---|---|
| MOSI | 3 | GPIO2 | SPI3\_MOSI | 
| MISO | 28 | GPIO1 | SPI3\_MISO | 
| SCLK | 5 | GPIO3 | SPI3\_SCLK | 
| CE0 | 27 | GPIO0 | SPI3\_CE0\_N | 
| CE1 | 18 | GPIO24 | SPI3\_CE1\_N | 

| SPI4 |  |  |  | 
|---|---|---|---|
| SPI function | Physical Pin | Broadcom/GPIO Pin | Pin Function | 
|---|---|---|---|
| MOSI | 31 | GPIO6 | SPI4\_MOSI | 
| MISO | 29 | GPIO5 | SPI4\_MISO | 
| SCLK | 26 | GPIO7 | SPI4\_SCLK | 
| CE0 | 7 | GPIO4 | SPI4\_CE0\_N | 
| CE1 | 22 | GPIO25 | SPI4\_CE1\_N | 

| SPI5 |  |  |  | 
|---|---|---|---|
| SPI function | Physical Pin | Broadcom/GPIO Pin | Pin Function | 
|---|---|---|---|
| MOSI | 8 | GPIO14 | SPI5\_MOSI | 
| MISO | 33 | GPIO13 | SPI5\_MISO | 
| SCLK | 10 | GPIO15 | SPI5\_SCLK | 
| CE0 | 32 | GPIO12 | SPI5\_CE0\_N | 
| CE1 | 37 | GPIO26 | SPI5\_CE1\_N | 

| SPI6 |  |  |  | 
|---|---|---|---|
| SPI function | Physical Pin | Broadcom/GPIO Pin | Pin Function | 
|---|---|---|---|
| MOSI | 38 | GPIO20 | SPI6\_MOSI | 
| MISO | 35 | GPIO19 | SPI6\_MISO | 
| SCLK | 40 | GPIO21 | SPI6\_SCLK | 
| CE0 | 12 | GPIO18 | SPI6\_CE0\_N | 
| CE1 | 13 | GPIO27 | SPI6\_CE1\_N | 

### UART

UART (Universal Asynchronous Receiver / Transmitter), or more commonly just called, "Serial," is a serial protocol which doesn't rely upon a shared clock. Synchronization between devices is achieved by setting the same baud/bit rate on both ends of the connection.

More information regarding this protocol as well as set up on a Gentoo system running on a Raspberry Pi can be found on the  [Raspberry\_Pi\_Serial\_Ports](https://wiki.gentoo.org/wiki/Raspberry_Pi_Serial_Ports) page.

## Configuration

Configuration of GPIO pins and their alternate functions rely upon two things. The first is the ability to access the memory bank for the GPIO pins which consists of adding udev rules as well as creating groups then adding users to groups. The second is adding the appropriate lines to the /boot/config.txt or /boot/firmware/config.txt. With some protocols such as I2C, the appropriate kernel module needs to be loaded as well. Configuring udev rules and fixing permission issues is a good place to begin.

### User GPIO Access

When using using a programming library, such as gpiozero for python or rust\_gpiozero for rust, the user running the program won't have the necessary permissions to access the /dev/gpiomem or /dev/gpiochip4 character device. Simply changing the permissions on /dev/gpiomem and similar won't persist between boots. Udev rules provide a way for persistently changing the file ownership and access management of kernel devices.

The Raspberry Pi OS developers have conveniently already written udev rules, which can be found on their [RPi-OS github page](https://github.com/RPi-Distro/raspberrypi-sys-mods/blob/bookworm/etc.armhf/udev/rules.d/99-com.rules). Copy the contents of those rules to /etc/udev/rules.d/99-com.rules. You may use your favorite text editor, nano is being used in this demonstration.

`root #``nano /etc/udev/rules.d/99-com.rules`
**`/etc/udev/rules.d/99-com.rules`**

**Raspberry Pi GPIO Udev Rules Example**

The rules above add more than just GPIO access; it also handles what are called, "Peripherals," or more accurately, "Pads" (semiconductor design term meaning, "chip connection to the outside world") within the Broadcom chip. For broad GPIO access, the rules above allow users in the, "gpio" group to access gpio-related kernel devices such as /dev/gpiomem, /dev/gpiochip0, etc.

Next create the "gpio" group.

`root #``groupadd gpio`
Now add the user to the "gpio" group.

`root #``gpasswd -a youruser gpio`
Finally, reboot the Raspberry Pi and ensure that /dev/gpiomem has group permissions set to "gpio".

`root #``ls -l /dev/gpiomem`
crw-rw---- 1 root gpio 241, 0 Jun 16 15:12 /dev/gpiomem

#### Raspberry Pi Utils

The [dev-embedded/raspberrypi-utils](https://packages.gentoo.org/packages/dev-embedded/raspberrypi-utils) package brings with it programs that may be useful when writing software for the Raspberry Pi. One of these utilities is the `vcgencmd` utility which can be used to gather information about the configuration and operation of the running system. Like the above section, when attempting to run the `vcgencmd` program, it will fail unless run as root; meaning that there is a permission issue.

The Raspberry Pi OS developers have already come across this issue and created some udev rules, which can be found on their [RPi-OS github page](https://github.com/RPi-Distro/raspberrypi-sys-mods/blob/bookworm/lib/udev/rules.d/10-vc.rules). To make use of these udev rules, they just need to be copied to /etc/udev/rules.d/10-vc.rules.

`root #``nano /etc/udev/rules.d/99-com.rules`
**`/etc/udev/rules.d/10-vc.rules`**

**Raspberry Pi VC Udev Rules Example**

If the "video" group doesn't exist, install [acct-group/video](https://packages.gentoo.org/packages/acct-group/video).

`root #``emerge --ask --verbose acct-group/video`
Then add the user to the "video" group.

`root #``gpasswd -a youruser video`
Reboot the Raspberry Pi and attempt to run a command such as `vcgencmd measure_temp` as a non-root user.

`user $``vcgencmd measure_temp`
temp=54.1'C

### I2C

Having followed the above udev steps, enabling I2C simply requires creating the i2c group, adding the user to that group then enabling i2c within /boot/config.txt as well as loading the kernel module.

First, create the "i2c" group.

`root #``groupadd i2c`
Now add the user to the "i2c" group.

`root #``gpasswd -a youruser i2c`
Now append the following to /boot/config.txt.

**`/boot/config.txt`**

**Enable I2C**

Finally ensure that the "i2c-dev" module loads at boot.

**`/etc/modules-load.d/i2c-dev.conf`**

**Load I2C Module**

Now reboot the Raspberry Pi.

When working with I2C, it may be helpful to install [sys-apps/i2c-tools](https://packages.gentoo.org/packages/sys-apps/i2c-tools). This package is useful in detecting the I2C bus and the connected components.

`root #``emerge --ask sys-apps/i2c-tools`
[sys-apps/i2c-tools](https://packages.gentoo.org/packages/sys-apps/i2c-tools) installs a command line program "i2cdetect". Running that command with the "-l" option will show available i2c buses. The following is example output on a Raspberry Pi 5.

`user $``i2cdetect -l`
i2c-1	i2c       	Synopsys DesignWare I2C adapter 	I2C adapter
i2c-11	i2c       	107d508200.i2c                  	I2C adapter
i2c-12	i2c       	107d508280.i2c                  	I2C adapter

"i2c-1" maps directly to I2C1 as outlined in the Pin Functionality section of this page.

### SPI

Similar to I2C, configuring GPIO pins for SPI requires creating the "spi" group, adding a user to it and then appending a line to /boot/config.txt. Enabling an overlay or using dtparam in /boot/config.txt should load the relevant kernel module so there should be no need for creating a new file in the /lib/modprobe.d.

First create the "spi" group.

`root #``groupadd spi`
Now add the user to the "spi" group.

`root #``gpasswd -a youruser spi`
Now append the following to /boot/config.txt.

**`/boot/config.txt`**

**Enable SPI**

Now reboot the Raspberry Pi.

Upon reboot, the following kernel character devices should be present.

`user $``ls -l /dev | grep spi`
crw-rw----  1 root spi     153,   1 Jul 13 16:18 spidev0.0
crw-rw----  1 root spi     153,   2 Jul 13 16:18 spidev0.1
crw-rw----  1 root spi     153,   0 Jul 13 16:18 spidev10.0

"spidev" is the kernel driver in use. "spidev0" translates to SPI0 as outlined in the Pin Functionality section of this page. "spidev0.0" and "spidev0.1" represent CE0 and CE1.

#### Alternate Busses

To enable different SPI busses (provided that the model of Raspberry Pi being used supports it) can be enabled by using the relevant dtoverlay.

Finding what overlays are available should be present within the /boot/overlays directory.

`user $``ls /boot/overlays | grep spi[0-9]-`
spi0-0cs.dtbo
spi0-1cs.dtbo
spi0-2cs.dtbo
spi1-1cs.dtbo
spi1-2cs.dtbo
spi1-3cs.dtbo
spi2-1cs.dtbo
spi2-1cs-pi5.dtbo
spi2-2cs.dtbo
spi2-2cs-pi5.dtbo
spi2-3cs.dtbo
spi3-1cs.dtbo
spi3-1cs-pi5.dtbo
spi3-2cs.dtbo
spi3-2cs-pi5.dtbo
spi4-1cs.dtbo
spi4-2cs.dtbo
spi5-1cs.dtbo
spi5-1cs-pi5.dtbo
spi5-2cs.dtbo
spi5-2cs-pi5.dtbo
spi6-1cs.dtbo
spi6-2cs.dtbo

Take for instance the dtoverlay, "spi1-2cs," will enable SPI1 with only two chip selects CE0 and CE1.

To use this overlay, append the following to /boot/config.txt.

**`/boot/config.txt`**

**Enable alternate SPI**

Reboot the Raspberry Pi and then confirm that the correct spidev character devices exist.

`user $``ls -l /dev | grep spi`
crw-rw----  1 root spi     153,   2 Jul 13 16:50 spidev1.0
crw-rw----  1 root spi     153,   0 Jul 13 16:50 spidev10.0
crw-rw----  1 root spi     153,   1 Jul 13 16:50 spidev1.1

### UART

Configuring UART can be found on the [Raspberry\_Pi\_Serial\_Ports](https://wiki.gentoo.org/wiki/Raspberry_Pi_Serial_Ports) page.

## Common GPIO Libraries

There are many Raspberry Pi GPIO libraries available. The table below includes the names of some libraries and links to the relevant locations for documentation.

Many of these libraries have working ebuilds found within the [Home Assistant Repository](https://git.edevau.net/onkelbeh/HomeAssistantRepository).

| Raspberry Pi GPIO Libraries |  |  |  |  | 
|---|---|---|---|---|
| Library | Language(s) | Download | Documentation | Notes | 
|---|---|---|---|---|
| libgpiod | C Python Rust | [dev-libs/libgpiod](https://packages.gentoo.org/packages/dev-libs/libgpiod) | [readthedocs.io](https://libgpiod.readthedocs.io/en/latest/core_api.html) |  | 
| bcm2835 | C | [airspayce website](http://www.airspayce.com/mikem/bcm2835/index.html) | [airspayce website](http://www.airspayce.com/mikem/bcm2835/index.html) |  | 
| wiringpi | C Bindings for other languages exist - Severely out of date | [Github](https://github.com/WiringPi/WiringPi) | [Github](https://github.com/WiringPi/WiringPi/blob/master/documentation/english/functions.md) |  | 
| gpiozero | Python | [Github](https://github.com/gpiozero/gpiozero) | [readthedocs.io](https://gpiozero.readthedocs.io/en/latest/) |  | 
| RPi.GPIO | Python | [pypi](https://pypi.org/project/RPi.GPIO/) [sourceforge](https://sourceforge.net/projects/raspberry-gpio-python/) | [sourceforge](https://sourceforge.net/p/raspberry-gpio-python/wiki/Home/) |  | 
| rust\_gpiozero | Rust | [Github](https://github.com/rahul-thakoor/rust_gpiozero) | [docs.rs](https://docs.rs/rust_gpiozero/latest/rust_gpiozero/) [Github](https://github.com/rahul-thakoor/rust_gpiozero) README provides beginner examples |  | 
| rppal | Rust | [Github](https://github.com/golemparts/rppal) | [docs.rs](https://docs.rs/rppal/latest/rppal/) [Github](https://github.com/golemparts/rppal) README provides important information |  | 

## External Resources

- [GPIO and the 40 pin header](https://www.raspberrypi.com/documentation/computers/raspberry-pi.html#gpio-and-the-40-pin-header) section from the Raspberry Pi's hardware page.
- [Taking It To Another Level: Making 3.3V Speak With 5V](https://hackaday.com/2016/12/05/taking-it-to-another-level-making-3-3v-and-5v-logic-communicate-with-level-shifters/) article which explains what level shifters are and explains how to make one.
- [RPi Low-Level peripherals](https://elinux.org/RPi_Low-level_peripherals) page at eLinux wiki.
- [pinout.xyz](https://pinout.xyz/) is an interactive GPIO tool which provides brief details regarding pin functions and can be useful if using multiple hats.
- [I2C Primer: What is I2C? (Part 1)](https://www.analog.com/en/resources/technical-articles/i2c-primer-what-is-i2c-part-1.html) is an explainer regarding the I2C protocol.
- [UART Theory and Programming on the Raspberry Pi](https://lloydrochester.com/post/hardware/uart-raspberry-pi/) talks about programming asynchronous UART and provides details regarding the UART protocol.

### Datasheets

- [BCM2835 Peripherals](https://datasheets.raspberrypi.com/bcm2835/bcm2835-peripherals.pdf) - CPU package used on the Pi 1 and Zero. May prove useful for Pi's up to the 3b+.
- [BCM2836 Peripherals](https://datasheets.raspberrypi.com/bcm2836/bcm2836-peripherals.pdf) - CPU package used on the Pi 2. The following use use the same peripherals data sheet:
  - [BCM2837](https://www.raspberrypi.com/documentation/computers/processors.html#bcm2837) - CPU package of the Pi 2 version 1.2 and 3B. The package is the same as the BCM2836 except the Cortex-A7 cluster was switched out for a quad core Cortex-A52 cluster.
  - [BCM2837B0](https://www.raspberrypi.com/documentation/computers/processors.html#bcm2837b0) - CPU package of the Pi 3A+ and 3B+. The package is the same as the BCM2837 except the Cortex-A52 cluser was switched out for a quad core Cortex-A53 MPCore cluster.
  - [RP3A0](https://www.raspberrypi.com/documentation/computers/processors.html#rp3a0) - CPU package used in the Pi Zero 2 W. The package is similar to the BCM2837.
- [BCM2711 Peripherals](https://datasheets.raspberrypi.com/bcm2711/bcm2711-peripherals.pdf) - CPU package used in the Pi 4.
- [RP1 Peripherals](https://datasheets.raspberrypi.com/rp1/rp1-peripherals.pdf) - The Raspberry Pi 5 southbridge microcontroller. This chip controls most of the IO hardware on the Pi 5.

## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) Raspberry Pi Ltd, [GPIO-Pinout-Diagram](https://www.raspberrypi.com/documentation/computers/images/GPIO-Pinout-Diagram-2.png), [GPIO and the 40-pin header](https://www.raspberrypi.com/documentation/computers/raspberry-pi.html=#=gpio-and-the-40-pin-header), Retrieved on June 25th, 2024
2. [↑](https://wiki.gentoo.org#cite_ref-2) Les Pounder, [Raspberry Pi GPIO Pinout: What Each Pin Does on Pi 4, Earlier Models](https://www.tomshardware.com/reviews/raspberry-pi-gpio-pinout,6122.html), [Tom's Hardware](https://www.tomshardware.com/), Marth 28th, 2023. Retrieved July 6th, 2024
3. [↑](https://wiki.gentoo.org#cite_ref-3) Sam, [I2C with Raspberry Pi](https://core-electronics.com.au/guides/i2c-with-raspberry-pi/), [Core Electronics](https://core-electronics.com.au), October 28th, 2022. Retrieved July 12th, 2024
4. [↑](https://wiki.gentoo.org#cite_ref-4) Raspberry Pi Ltd, [RP1 Peripherals: 3.5. I2C](https://datasheets.raspberrypi.com/rp1/rp1-peripherals.pdf), [Raspberry Pi Website](https://www.raspberrypi.com), 2023. Retrieved July 6th, 2024
5. [↑](https://wiki.gentoo.org#cite_ref-5) Raspberry Pi Ltd, [GPIO and the 40-pin header: Alternative Functions](https://www.raspberrypi.com/documentation/computers/raspberry-pi.html#gpio), [Raspberry Pi Website](https://www.raspberrypi.com), Retrieved July 6th, 2024
6. [↑](https://wiki.gentoo.org#cite_ref-6) Raspberry Pi Ltd, [Serial Peripheral Interface (SPI)](https://www.raspberrypi.com/documentation/computers/raspberry-pi.html#serial-peripheral-interface-spi), [Raspberry Pi Website](https://raspberrypi.com), Retrieved July 10th, 2024
7. [↑](https://wiki.gentoo.org#cite_ref-7) Raspberry Pi Ltd, [Serial Peripheral Interface (SPI)](https://www.raspberrypi.com/documentation/computers/raspberry-pi.html#serial-peripheral-interface-spi), [Raspberry Pi Website](https://raspberrypi.com), Retrieved July 10th, 2024
8. [↑](https://wiki.gentoo.org#cite_ref-8) hhk7734, [python3-gpiod README](https://github.com/hhk7734/python3-gpiod/blob/main/README.md), [python3-gpiod Github repository](https://github.com/hhk7734/python3-gpiod), May 20th, 2024. Retrieved on July 2nd, 2024
9. [↑](https://wiki.gentoo.org#cite_ref-9) Daniella Kicsak, [Gentoo Bugzilla - Bug 949054](https://bugs.gentoo.org/949054), [Gentoo Bugzilla](https://bugs.gentoo.org/), January 30th, 2025. Retrieved on April 5th, 2025
10. [↑](https://wiki.gentoo.org#cite_ref-10) cadre, [Working on Pi 5?](https://groups.google.com/g/bcm2835/c/e44ttskxDv8), [bcm2835 Google Group](https://groups.google.com/g/bcm2835), November 23rd, 2023. Retrieved April 5th, 2025
11. [↑](https://wiki.gentoo.org#cite_ref-11) Gordon Henderson, [wiringPi - deprecated (web archive)](https://web.archive.org/web/20220405225008/http://wiringpi.com/wiringpi-deprecated/), [wiringpi.com (web archive)](https://web.archive.org/web/20220403001626/http://wiringpi.com/), August 6th, 2019. Retrieved July 2nd 2024
12. [↑](https://wiki.gentoo.org#cite_ref-12) GrazerComputerClub, [wiringPi README](https://github.com/WiringPi/WiringPi/blob/master/README.md), [GrazerComputerClub Github Page](https://github.com/GrazerComputerClub), March 13th, 2024. Retrieved on July 2nd, 2024
13. [↑](https://wiki.gentoo.org#cite_ref-13) Raspberry Pi Ltd, [Use Python on a Raspberry Pi](https://www.raspberrypi.com/documentation/computers/os.html#use-python-on-a-raspberry-pi), [Raspberry Pi](https://www.raspberrypi.com/), Retrieved July 2 2024
14. [↑](https://wiki.gentoo.org#cite_ref-14) Ben Nuttall, [gpiozero Documentation - Development](https://gpiozero.readthedocs.io/en/stable/development.html), [gpiozero readthedocs](https://gpiozero.readthedocs.io/en/stable/index.html), Retrieved July 2, 2024
15. [↑](https://wiki.gentoo.org#cite_ref-15) Ben Nuttall & Dave Jones, [gpiozero setup.cfg](https://github.com/gpiozero/gpiozero/blob/master/setup.cfg#L32), [gpiozero Github](https://github.com/gpiozero/gpiozero/), Retrieved July 2nd, 2024
16. [↑](https://wiki.gentoo.org#cite_ref-16) Melissa LeBlanc-Williams, [Plans to support Raspberry Pi 5](https://sourceforge.net/p/raspberry-gpio-python/tickets/214/), [RPi.GPIO sourceforge](https://sourceforge.net/p/raspberry-gpio-python/), October 19th, 2023. Retrieved July 2nd, 2024
