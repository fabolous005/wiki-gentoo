<!-- source: https://wiki.gentoo.org/wiki/Sierra_EM7455_(Dell_wireless_modem_DW5560) | group: Gentoo Wiki (Main) | wiki-title: Sierra EM7455 (Dell wireless modem DW5560) -->
---
title: Sierra EM7455 (Dell wireless modem DW5560)
url: https://wiki.gentoo.org/wiki/Sierra_EM7455_(Dell_wireless_modem_DW5560)
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-03-07"
fingerprint: "5ed76f75c923b1bc"
license: CC BY-SA 4.0
---

# Sierra EM7455 (Dell wireless modem DW5560)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Hardware

The Sierra EM7455 is a PCIe M.2 4G LTE modem module, found in several notebooks. It can be used with an external USB adapter as a mobile broadband modem with any system. To the time of writing (Mar 2024) such a module costs only about 15 Euro on ebay and enables high-speed mobile internet, faster than most 4G USB-dongles on the market. This article focusses on the use as external modem, for the built in version also see here: [Dell\_Latitude\_7390#WWAN](https://wiki.gentoo.org/wiki/Dell_Latitude_7390#WWAN).

## Kernel drivers

To make it work, the following kernel drivers (as of linux-4.9) need to be activated:

- **PPP (point-to-point protocol) support** ( `CONFIG_PPP` )
- **PPP support for sync tty ports** ( `CONFIG_PPP__SYNC_TTY` )
- **USB Modem (CDC ACM) support** ( `CONFIG_USB_ACM` )
- **USB Wireless Device Management support** ( `CONFIG_USB_WDM` )
- **USB MBIM support** ( `CONFIG_USB_NET_CDC_MBIM` )

For **rfkill**, the **Dell Laptop Extras** ( `CONFIG_DELL_LAPTOP` ) option might need to be enabled as well.

## Software

Install:

`root #``emerge --ask net-libs/libmbim``root #``emerge --ask net-libs/libqmi`
For serial communication using AT-commands:

`root #``emerge --ask net-dialup/minicom`
## Drivers

The device needs qcserial module to create /dev/ttyUSB0, /dev/ttyUSB1 and /dev/ttyUSB2 for serial connection. It further needs cdc-mbim module to create /dev/cdc-wdm0 for qmi connection.

Generic version:

`user $``lsusb -vt````
...
    |__ Port 012: Dev 012, If 0, Class=Vendor Specific Class, Driver=qcserial, 480M
        ID 1199:9071 Sierra Wireless, Inc. AirPrime MC7455 3G/4G LTE Modem
    |__ Port 012: Dev 012, If 2, Class=Vendor Specific Class, Driver=qcserial, 480M
        ID 1199:9071 Sierra Wireless, Inc. AirPrime MC7455 3G/4G LTE Modem
    |__ Port 012: Dev 012, If 3, Class=Vendor Specific Class, Driver=qcserial, 480M
        ID 1199:9071 Sierra Wireless, Inc. AirPrime MC7455 3G/4G LTE Modem
    |__ Port 012: Dev 012, If 12, Class=Communications, Driver=cdc_mbim, 480M
        ID 1199:9071 Sierra Wireless, Inc. AirPrime MC7455 3G/4G LTE Modem
    |__ Port 012: Dev 012, If 13, Class=CDC Data, Driver=cdc_mbim, 480M
        ID 1199:9071 Sierra Wireless, Inc. AirPrime MC7455 3G/4G LTE Modem
...
```
Branded version:

`user $``lsusb`
Bus 002 Device 003: ID 413c:818e Dell Computer Corp. 
...

## Firmware

Download new firmware from manufacturer: [sierra website](https://source.sierrawireless.com/resources/airprime/minicard/74xx/em_mc74xx-approved-fw-packages/#sthash.Z1RNfqTe.dpbs). To flash the firmware:

`root #``qmi-firmware-update -w /dev/cdc-wdm0 -u SWI9X30C_02.33.03.00.cwe SWI9X30C_02.33.03.00_GENERIC_002.072_000.nvu --device-open-mbim`
Make sure no other program is sending AT commands to the device meanwhile, e.g disable NetworkManager, ModemManager etc. The branding is removed when using a generic firmware.

## Settings via AT commands

The modem can be configured using AT commands: To send AT commands, quit any services communicating with the modem (e.g. ModemManager) and run

`root #``minicom -b 9600 -D /dev/ttyUSB2`
then type the commands as described in the [AT command reference](https://source.sierrawireless.com/resources/airprime/minicard/74xx/4117727-airprime-em74xx-mc74xx-at-command-reference/). These can for example be used to restore factory defaults or change the branding. Just type a command and hit enter.

### Set correct model PRI part number and revision after firmware flashing

To find out part and revision number, in the directory with the firmware files, run:

`user $``qmi-firmware-update -z SWI*.nvu`
Find the version line, looks something like this:

\[cwe 0\] version: 9999999\_**9904609**\_SWI9X30C\_02.33.03.00\_00\_GENERIC\_**002.072**\_001

The module part number to set are the second 7 digits, the revision number are the 6 digits at the end (ignoring the last 3). Set with AT-command:

### Change password

Sensible commands are protected by a password of up to 15 digits. Supported characters: ‘0’–’9’, ‘A’–’Z’, ‘a’–’z’, special characters (e.g. “!#$%&’()\*+,-./:\<>=?@” Double quotes (“) are not allowed. You can change it like this:

## Internet connection

**Important**: (Re-) Emerge NetworkManager with modemmanager USE-flag enabled and ModemManager with mbim and qmi USE-flags enabled. It will not work without these flags.

Add user to the dialout group:

`root #``usermod -aG dialout Larry`
Find the modem number (ModemManager cli):

`user $``mmcli -L`
Check the modem (assume number 0):

`user $``mmcli -m 0`
If that works, use NetworkManager (NetworkManager cli): Find device, look for TYPE 'gsm':

`root #``nmcli device status`
Add connection with the device name you found:

`root #``nmcli c add type gsm ifname` *device* con-name *my-connection* apn internet.your-provider.com user Larry password 123
apn, username and password can be found on the website of you service provider, if they are not correct, it will not work. Set connection up:

`root #``nmcli c up` *my-connection*
That's it. Internet connection should work and autoconnect when the modem is plugged in.



## Troubleshooting

### Firmware update not working

In one terminal run minicom to send AT commands via serial:

`root #``minicom -D /dev/ttyUSB2`
Enable advanced commands and reboot to 'boot and hold' mode:

Immediatly run from a second terminal:

`root #``qmi-firmware-update -t /dev/ttyUSB3 -U SWI9X30C_02.33.03.00.cwe SWI9X30C_02.33.03.00_GENERIC_002.072_001.nvu` ### ModemManager crashes when attempting to connect

Make sure you have built modemmanager with qmi and mbim USE-flags enabled.

### Device not working on USB 3.x port

Force USB 2.0 (doesn't slow down):
