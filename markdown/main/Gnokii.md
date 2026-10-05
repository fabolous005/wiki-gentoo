<!-- source: https://wiki.gentoo.org/wiki/Gnokii | group: Gentoo Wiki (Main) | wiki-title: Gnokii -->
---
title: gnokii
url: https://wiki.gentoo.org/wiki/Gnokii
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-11-20"
fingerprint: fe03a10c99b278ec
license: CC BY-SA 4.0
---

# gnokii

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**gnokii** is a modem and fax driver for mobile phones.

## Installation

### USE flags


| [+pcsc-lite](https://packages.gentoo.org/useflags/+pcsc-lite) | Enable smartcard support with sys-apps/pcsc-lite | 
| [X](https://packages.gentoo.org/useflags/X) | Add support for X11 | 
| [bluetooth](https://packages.gentoo.org/useflags/bluetooth) | Enable Bluetooth Support | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [ical](https://packages.gentoo.org/useflags/ical) | Enable support for dev-libs/libical | 
| [irda](https://packages.gentoo.org/useflags/irda) | Enable infrared support | 
| [mysql](https://packages.gentoo.org/useflags/mysql) | Add mySQL Database support | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [postgres](https://packages.gentoo.org/useflags/postgres) | Add support for the postgresql database | 
| [sms](https://packages.gentoo.org/useflags/sms) | Enable SMS support (build smsd) | 
| [usb](https://packages.gentoo.org/useflags/usb) | Add USB support to applications that have optional USB support (e.g. cups) | 

### Emerge

`root #``emerge --ask app-mobilephone/gnokii`
## Configuration

Edit the /etc/gnokiirc file:

FILE **`/etc/gnokiirc`**

```
[global]
port = /dev/ttyUSB0
model = AT
initlength = default
connection = serial
use_locking = yes
serial_baudrate = 19200
smsc_timeout = 10
[gnokiid]
bindir = /usr/sbin/
[connect_script]
TELEPHONE = 12345678
[disconnect_script]
[logging]
debug = on
rlpdebug = off
xdebug = off
```
Setup save the password for PIN login

FILE **`/etc/sms-pin`**

## Usage

### Set PIN to work with gnokii

/usr/sbin/chat is part of [net-dialup/ppp](https://packages.gentoo.org/packages/net-dialup/ppp).

`root #``/usr/sbin/chat -V -f /etc/sms-pin >> /dev/ttyUSB0 < /dev/ttyUSB0`
An other way is just using gnokii:

`user $``gnokii  --entersecuritycode PIN`
Show the PIN status:

`user $``gnokii --getsecuritycodestatus | grep PIN`
### Sending SMS

`user $``echo "test" | gnokii --sendsms +land_and_telephonenumber`
### Write calendar notes

FILE **`gnokii-import.sh`**

```
#!/bin/bash
#########################################################
# gnokii ical import script to fix buggy write mode     #
# don't forget to use "irattach irda0 -s" if using ira  #
#########################################################
read -p "Please give path and file name: " file
declare -i a
a=$(grep -ir "SUMMARY:" $file | wc -l) # count the entries
for i in `seq 1 $a`;do
echo $i
gnokii --writecalendarnote $file $i
sleep 1 # wait a second before the next entry is written
done
```
FILE **`cal.ical`**

`user $``gnokii-import.sh``user $``cal.ica`
