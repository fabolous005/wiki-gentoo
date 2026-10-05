<!-- source: https://wiki.gentoo.org/wiki/Xilinx_USB_JTAG_Programmers | group: Gentoo Wiki (Main) | wiki-title: Xilinx USB JTAG Programmers -->
---
title: Xilinx USB JTAG Programmers
url: https://wiki.gentoo.org/wiki/Xilinx_USB_JTAG_Programmers
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-22"
fingerprint: "19cd0a1a109e0488"
license: CC BY-SA 4.0
---

# Xilinx USB JTAG Programmers

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

 **Special Note: This wiki addresses 2 types of JTAG cables: 1. Digilent Xilinx USB JTAG cables 2. Xilinx XUP-USB-JTAG cable as well**

Assuming you have installed the Xilinx installation, this article will guide you on installing Cable Drivers for Xilinx USB JTAB Programmers. If you have problems, please check that you have not done any misstake on the primary installation.

## Digilent Xilinx USB JTAG cable

### Getting what's needed

First of all, this guide assumes you have installed Xilinx ISE (version 13.4 is used here) into the default path of /opt/Xilinx Next, you will need to have GIT installed to get the required libraries. This approach does not use the official Xilinx libraries but a replica of them. You will also need libusb which is required in the compiling of the drivers. On a 64-bit host, you will need to get bin86 and dev86 packages.

### Digilent Adept Runtime x86/x64

Digilent Adept Runtime package is available at [Digilent](http://www.digilentinc.com/Products/Detail.cfm?NavPath=2,66,828&Prod=ADEPT2) website.

Chose the package according to your linux OS. If it’s a 32-bit OS, dowload Adept Runtime x86 Linux. If it’s 64-bit kernel, download Adept Runtime x64 Linux. The downloaded software package is wrapped in format .tar.gz.

### Digilent Adept Utilities x86/x64

Digilent Adept Runtime package is available at [Digilent](http://www.digilentinc.com/Products/Detail.cfm?NavPath=2,66,828&Prod=ADEPT2) website.

Chose the package according to your linux OS. If it’s a 32-bit OS, dowload Adept Utilities x86 Linux. If it’s 64-bit kernel, download Adept Utilities x64 Linux. The downloaded software package is wrapped in format .tar.gz.

### Digilent Plugins x86/x64

Download Digilent Plugin for Xilinx Design Suite if you want to download your bitstream from XPS, ISE or iMPACT directly and debug with SDK or Chipscope. Digilent Plugin is alse available at [Digilent](http://www.digilentinc.com/Products/Detail.cfm?NavPath=2,66,768&Prod=DIGILENT-PLUGIN) website.

Chose the package according to your linux OS. If it’s a 32-bit OS, dowload Digilent Plug-in x86 Linux. If it’s 64-bit kernel, download Digilent Plug-in x64 Linux. The downloaded software package is wrapped in format .tar.gz.

## Installation

First of all, decompress all packages.

`root #````
tar xzf digilent.adept.runtime_<version>.tar.gz
```
`root #````
 tar xzf digilent.adept.utilities_<version>.tar.gz
```
`root #` `tar xzf libCseDigilent_<version>.tar.gz`
#### Install Digilent Adept Runtime

Enter directory digilent.adept.runtime\_\<version>-\<platform> then run the install script

`root #``./install.sh`
The lines should look like this:

#### Install Digilent Adept Utilities

Enter directory digilent.adept.utilities\_\<version>-\<platform>, and run command the install script. This time we will keep all default locations unchanged.

`root #``./install.sh`
#### Install Digilent Plugin for Xilinx Design Suites

Enter directory libCseDigilent\_2.0.5-\<platform> Enter directory of the Xilinx DS version installed on your computer. There is a pdf under the folder named: Digilent\_Plug-in\_Xilinx\_\<versionNo>.pdf. The document tells exactly how to install the Digilent Plugin and how to use it. To install plugin is quite easy, all you need to do is copy the libCseDigilent.so and libCseDigilent.xml Digilent plugins folder

##### 64 bits

`root #``cp libCseDigilent.so <Xilinx_DS_Path>/ISE/lib/lin64/plugins/Digilent/libCseDigilent/ && cp libCseDigilent.xml <Xilinx_DS_Path>/ISE/lib/lin64/plugins/Digilent/libCseDigilent/`
##### 32 bits

`root #``cp libCseDigilent.so <Xilinx_DS_Path>/ISE/lib/lin/plugins/Digilent/libCseDigilent/ && cp libCseDigilent.xml <Xilinx_DS_Path>/ISE/lib/lin/plugins/Digilent/libCseDigilent/`
If there is no dir *Digilent* under \<Xilinx\_DS\_Path>/ISE/lib/lin64/plugins/, just make a new dir with command mkdir.

`root #``>mkdir <Xilinx_DS_Path>/ISE/lib/lin64/plugins/Digilent`
### Testing

Connect your digilent board to your PC, and check whether Adept utilities can recognize your board. All you need is to run the command **djtgcfg** and see whether your boards pop up.

`user $``djtgcfg enum`
Now open Impact

`user $``/Xilinx_DS_Path>/ISE/bin/lin/impact&`
See if any FPGA can be found on your JTAG chain.

For now on I belive that you can walk by yourself, and begin to program your device.

## Xilinx XUP USB JTAG cable

These drivers and method of using them is verified with ISE 14.5 on Gentoo 64bit. Drivers provided with ISE installation don't work with Linux system. Even installing as super user gives an error.

### Prerequisites

To follow these instructions git and fxload are required

`root #``emerge --ask git fxload`
### Getting drivers

Clone the git repository for proper drivers:

change to the directory where repository is cloned and compile proper drivers

`user $``make`
#### Setting up Environment

Add compiled driver library to environment path:

`user $``export LD_PRELOAD=path/to/USB/driver/folder/usb-driver/libusb-driver.so`
### Tweaking udev

Copy proper rule file under udev: For impact to work with compiled drivers 'TEMPNODE' needs to be changed to 'tempnode', BUS=="usb" needs to be removed and SYSFS needs to be changed to ATTR.

`root #``sed /path/to/ISE/installation/folder/14.x/ISE_DS/ISE/bin/lin(64)/xusbdfwu.rules -e 's:TEMPNODE:tempnode:g' | sed 's:BUS=="usb", ::g' | sed 's:SYSFS:ATTR:g' | sudo tee /etc/udev/rules.d/50-xusbdfwu.rules 1>/dev/null`
Copy Xilinx hex files to /usr/share

`root #``cp /path/to/installation/folder/Xilinx/ISE/bin/lin/xusb*.hex /usr/share`
Restart udev

`root #``/etc/init.d/udev restart`
or, if using systemd

`root #``sudo udevadm control --reload`
Now starting impact should detect cable properly.
