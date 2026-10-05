<!-- source: https://wiki.gentoo.org/wiki/Allow_only_known_usb_devices | group: Gentoo Wiki (Main) | wiki-title: Allow only known usb devices -->
---
title: Allow only known usb devices
url: https://wiki.gentoo.org/wiki/Allow_only_known_usb_devices
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2020-10-17"
fingerprint: be0c6a36f159b360
license: CC BY-SA 4.0
---

# Allow only known usb devices

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes how to protect a GNU/Linux system against rogue USB devices via a white listing policy. Only known USB devices will be accepted by the system.

## Installation

### Kernel

As of **Jan 31, 2019**, this section is **deprecated (obsolete)**. Contents are <u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this section!**

In kernel "sys-kernel/hardened-sources" we have two options for Physical Protections:

**make menuconfig options**

It is a very good choice for servers in datacenter and server's rooms. But at reboot it is still vulnerable! Also it does not give the flexibility needed on workstations when we want to connect USB devices during run time. So let's write *eudev* rules to allow only known USB devices in the system.

## eudev

***eudev*** allows us to create rules to configure devices as they are added or removed from a system, in our case we will allow and disallow USB devices based on their identifying strings.

### Retrieve information about USB devices

The first thing that needs to be done is to collect identifying strings that can be used to white list our USB devices.

#### lsusb

How to obtain more information about connected USB devices? Install usbutils:

`root #``emerge --ask sys-apps/usbutils`
Run:

`root #``lsusb`
Bus 001 Device 005: ID 4444:4444 Example, USB Keyboard
Bus 001 Device 004: ID 3333:3333 Example, USB Optical Mouse
Bus 001 Device 003: ID 5555:5555 USB Hub
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 005 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
Bus 004 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
Bus 003 Device 002: ID 2222:2222 Example, USB Optical Mouse
Bus 003 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
Bus 002 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub

We can see *USB Optical Mouse* connected on bus 3 and listed as device 2. We need first to find this device’s port number. To figure out port numbers run *lsusb* with option *-t*:

`root #``lsusb –t````
/:  Bus 05.Port 1: Dev 1, Class=root_hub, Driver=uhci_hcd/2p, 12M
/:  Bus 04.Port 1: Dev 1, Class=root_hub, Driver=uhci_hcd/2p, 12M
/:  Bus 03.Port 1: Dev 1, Class=root_hub, Driver=uhci_hcd/2p, 12M
    |__ Port 1: Dev 2, If 0, Class=Human Interface Device, Driver=usbhid, 1.5M
/:  Bus 02.Port 1: Dev 1, Class=root_hub, Driver=uhci_hcd/2p, 12M
/:  Bus 01.Port 1: Dev 1, Class=root_hub, Driver=ehci-pci/8p, 480M
    |__ Port 4: Dev 3, If 0, Class=Hub, Driver=hub/8p, 480M
        |__ Port 1: Dev 4, If 0, Class=Human Interface Device, Driver=usbhid, 1.5M
        |__ Port 5: Dev 5, If 0, Class=Human Interface Device, Driver=usbhid, 1.5M
        |__ Port 5: Dev 5, If 1, Class=Human Interface Device, Driver=usbhid, 1.5M
```
Now we can see that *USB Optical Mouse* connected on bus 3 and listed as device 2 really connected at **port 1 of USB bus 3**! Also lsusb with option `-v` expand verbose output, but the format is not convenient for copying and pasting into eudev rules.

#### Sysfs

Information about attached USB devices can be obtained through sysfs:

`root #``ls /sys/bus/usb/devices/`
1-0:1.0  1-4  1-4.1  1-4:1.0  1-4.1:1.0  1-4.5  1-4.5:1.0  1-4.5:1.1  2-0:1.0  3-0:1.0  3-1  3-1:1.0  4-0:1.0  5-0:1.0  usb1  usb2  usb3  usb4  usb5

The subdirectories we are interested in are in the format ***bus\_number-port*** or ***bus\_number-port.port***. The second one is in case hubs are used, then port section of the directory name may be a series of port numbers separated by periods. So pay attention only to follow dirs:

`root #``ls /sys/bus/usb/devices/ |grep -v ':'`
1-4  1-4.1  1-4.5  3-1  usb1  usb2  usb3  usb4  usb5

***usb1  usb2  usb3  usb4  usb5*** is a Linux root hub, don't forgot add rules for them!!!

Sysfs is a virtual file system that gives information and allows the control of devices which the kernel exports. There is one specific setting: ***authorized***. All USB devices should have an ***authorized*** option which controls whether or not the device can communicate with the system.

For example, if we wished to see if the device in port 1 of the 3 USB bus is active we could use the following command:

`root #``cat /sys/bus/usb/devices/3-1/authorized`
1

The 1 in the output tells us the device is authorized, 0 would mean not authorized. So we can allow USB devices by running:

`root #``echo "1" /sys/bus/usb/devices/3-1/authorized`
And disallow:

`root #``echo "0" > /sys/bus/usb/devices/3-1/authorized`
But it is not convenient too.

#### udevadm

To retrieve information like the serial number, vendor and product ID and manufacturer’s name of our device connected to *port 1 on 3 USB bus*, you can run the following command:

`root #``udevadm info -a -p /sys/bus/usb/devices/3-1````
Udevadm info starts with the device specified by the devpath and then walks up the chain of parent devices. For every device found it prints all possible attributes in the udev rules key format. A rule to match, can be composed of the attributes of the device and the attributes from one single parent device.
  looking at device '/devices/pci0000:00/0000:00:1d.1/usb3/3-1':
    KERNEL=="3-1"
    SUBSYSTEM=="usb"
.................................."
    ATTR{idVendor}=="2222"
..................................
    ATTR{configuration}=="HID-compliant MOUSE"
..................................
    ATTR{authorized}=="1"
..................................
    ATTR{manufacturer}=="Example"
..................................
    ATTR{idProduct}=="2222"
    ATTR{bDeviceClass}=="00"
    ATTR{product}=="USB Optical Mouse"
..................................
```
All of these "ATTR" attributes can be used in our eudev rules. You need to add rules to /lib/udev/rules.d/ that will white list only the given devices, and blacklist all the rest.

### Writing eudev rules

Now we can start writing *eudev* rules to enable/disable USB devices. Create a file with rules in */lib/udev/rules.d/01-usb.rules* to ensure that it is running in the beginning.

Pay attention to add all USB hubs at the start of your rules. All USB hubs have *ATTR{bDeviceClass}=="09"*. Also we add more information: *ATTR{manufacturer}* and *ATTR{product}* for human readable descriptions.

To authorize the device set *ATTR{authorized}="1"*, to block it, set *ATTR{authorized}="0"*

**`/lib/udev/rules.d/01-usb.rules`**

**eudev rules for enable only known USB devices**

#### Smartphones, E-books, and so on

Some multi-function modern devices like smartphones, e-books, and so on have many operation modes. It can operate as **USB storage device, USB camera, USB modem, USB network device**, ... So when we write rule for such devices we must add one rule for each operation modes!

Reattach device, switch different operation modes and see deference in output:

`root #``udevadm info -a -p /sys/bus/usb/devices/3-1````
Udevadm info starts with the device specified by the devpath and then walks up the chain of parent devices. For every device found it prints all possible attributes in the udev rules key format. A rule to match, can be composed of the attributes of the device and the attributes from one single parent device.
  looking at device '/devices/pci0000:00/0000:00:1d.1/usb3/3-1':
    KERNEL=="3-1"
    SUBSYSTEM=="usb"
..................................
    ATTR{serial}=="00000000001"
..................................
    ATTR{idProduct}=="000a"
..................................
```
It can change his serial number or ATTR{idProduct}=="000a" attribute! So **eudev** rules for such device look like:

**`/lib/udev/rules.d/01-usb.rules`**

**eudev rules for enable only known Smartphone**

Or

**`/lib/udev/rules.d/01-usb.rules`**

**eudev rules for enable only known E-Book**

### Test eudev rules

To test your script run the following command:

`root #``udevadm test /sys/bus/usb/devices/3-1`
It will not actually apply the changes, it only shows which conditional statements are applied when a certain device is inserted.

## See also

- [Silk Guardian](https://wiki.gentoo.org/wiki/Silk_Guardian) — a Linux kernel module kill switch that upon detecting changes to USB ports, wipes the RAM, securely deletes user specified files, and then shuts down the system.
- [USB/Guide](https://wiki.gentoo.org/wiki/USB/Guide) — helps users setup and configure various USB devices on Gentoo systems.
- [Eudev](https://wiki.gentoo.org/wiki/Eudev) — a fork of [udev](https://wiki.gentoo.org/wiki/Udev), [systemd](https://wiki.gentoo.org/wiki/Systemd)'s [device file](https://wiki.gentoo.org/wiki/Device_file) manager for the Linux kernel.
