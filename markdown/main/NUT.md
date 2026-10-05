<!-- source: https://wiki.gentoo.org/wiki/NUT | group: Gentoo Wiki (Main) | wiki-title: NUT -->
---
title: NUT
url: https://wiki.gentoo.org/wiki/NUT
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-03"
fingerprint: dc949e723f2699d4
license: CC BY-SA 4.0
---

# NUT

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**NUT** provides control and monitoring features for uninterruptible power supplies, power distribution units, automatic transfer switches, and solar controllers.

Do you have a UPS? Do you want to have you system gracefully shutdown in case of a power outage? This guide will show how you can make the most out of your UPS by using NUT from the [Network UPS Tools](https://www.networkupstools.org/) project

## Installation

### USE flag

To connect a UPS via USB, make sure to set the `usb` USE flag:


| [+man](https://packages.gentoo.org/useflags/+man) | Build and install man pages | 
| [+usb](https://packages.gentoo.org/useflags/+usb) | Includes all UPS drivers that use USB. | 
| [cgi](https://packages.gentoo.org/useflags/cgi) | Add CGI script support | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [gpio](https://packages.gentoo.org/useflags/gpio) | Includes all UPS drivers that use GPIO. | 
| [i2c](https://packages.gentoo.org/useflags/i2c) | Includes all UPS drivers that use I2C. | 
| [ipmi](https://packages.gentoo.org/useflags/ipmi) | Includes all UPS drivers that use ipmi. | 
| [modbus](https://packages.gentoo.org/useflags/modbus) | Includes all UPS drivers that use MODBUS. | 
| [monitor](https://packages.gentoo.org/useflags/monitor) | Add a Qt6 gui monitor. | 
| [python](https://packages.gentoo.org/useflags/python) | Add optional support/bindings for the Python language | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [serial](https://packages.gentoo.org/useflags/serial) | Includes all UPS drivers that use SERIAL. | 
| [snmp](https://packages.gentoo.org/useflags/snmp) | Includes all UPS drivers that use SNMP. | 
| [ssl](https://packages.gentoo.org/useflags/ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [tcpd](https://packages.gentoo.org/useflags/tcpd) | Add support for TCP wrappers | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [xml](https://packages.gentoo.org/useflags/xml) | Includes all UPS drivers that use XML. | 
| [zeroconf](https://packages.gentoo.org/useflags/zeroconf) | Support for DNS Service Discovery (DNS-SD) | 

### Emerge

`root #``emerge --ask sys-power/nut`
## Configuration standalone

Search a UPS with nut-scanner:

`root #``/usr/bin/nut-scanner````
Scanning USB bus.
Scanning XML/HTTP bus.
No start IP, skipping NUT bus (old connect method)
[nutdev1]
        driver = "usbhid-ups"
        port = "auto"
        vendorid = "051D"
        productid = "0002"
        product = "Back-UPS SMC 1500 FW:879.L4 .I USB FW:L4"
        serial = "***"
        vendor = "American Power Conversion"
        bus = "003"
```
Take note of driver name and port type for configuration file.

If UPS is connected via USB port, add user nut to group usb.

`root #``usermod -a -G usb nut`
### Files

#### /etc/nut/nut.conf

Set mode to standalone if the machine is connected to the UPS directly and to run NUT on this machine.

`root #``nano -w /etc/nut/nut.conf`
**`/etc/nut/nut.conf`**

**Desired settings**

#### /etc/nut/ups.conf

The main UPS configuration file. Make sure that the UPS name (the text in the brackets) doesn't have any spaces. For configuration specific to the UPS you need to look it up here: [NUT Hardware Compatibility Lookup](https://www.networkupstools.org/stable-hcl.html)

`root #``nano -w /etc/nut/ups.conf`
**`/etc/nut/ups.conf`**

**Example ups.conf configuration**

```
[APCSMC1500]
    driver = usbhid-ups
    port = auto
    desc = "APC SMC1500 UPS"
# Change some variables in UPS hardware. Not all devices or drivers
# implement this, so this may not have effect on your system.
    override.battery.charge.low = 30
    override.battery.runtime.low = 180
```
#### /etc/nut/upsd.users

Configure at least one user so that upsmon can be launched later. upsd will create a TCP connection that upsmon will use to check on the status of the UPS.

`root #``nano -w /etc/nut/upsd.users`
**`/etc/nut/upsd.users`**

**Example upsd.users configuration**

```
[monmaster]
    password = masterpassword
    upsmon master
```
#### /etc/nut/upsmon.conf

Create a MONITOR configuration in /etc/nut/upsmon.conf:

`root #``nano -w /etc/nut/upsmon.conf`
**`/etc/nut/upsmon.conf`**

**Example upsmon configuration**

#### /etc/nut/upssched.conf

Configure operations on that upsmon will check on the status of the UPS:

`root #``nano -w /etc/nut/upssched.conf`
**`/etc/nut/upssched.conf`**

**Example upssched.conf configuration**

```
CMDSCRIPT /usr/bin/upssched-cmd
PIPEFN /var/lib/nut/upssched/upssched.pipe
LOCKFN /var/lib/nut/upssched/upssched.lock
AT ONBATT * START-TIMER onbatt 300
AT ONLINE * CANCEL-TIMER onbatt online
AT LOWBATT * EXECUTE onbatt
AT COMMBAD * START-TIMER commbad 30
AT COMMOK * CANCEL-TIMER commbad commok
AT NOCOMM * EXECUTE commbad
AT SHUTDOWN * EXECUTE powerdown
AT REPLBATT * EXECUTE replacebatt
```
`root #``mkdir -p /var/lib/nut/upssched``root #``chown nut:nut /var/lib/nut/upssched`
#### /usr/bin/upssched-cmd

`root #``nano -w /usr/bin/upssched-cmd`
**`/usr/bin/upssched-cmd`**

**Example upssched.conf configuration**

### Services

#### OpenRC

To add the services to start on system boot:

`root #````
rc-update add upsdrv default
```
`root #````
rc-update add upsd default
```
`root #````
rc-update add upsmon default
```
To start upsd now run:

`root #````
rc-service upsdrv start
```
`root #````
rc-service upsd start
```
`root #````
rc-service upsmon start
```

To check the status of the UPS manually (adjust the UPS name as needed):

`root #``upsc APCSMC1500@127.0.0.1 ups.status`
OL

OL means that the UPS is "online" and not drawing from the battery and is configured correctly.

In the event of a shutdown due to a power failure, nut can additionally turn off the UPS by adding nut.powerfail to the shutdown runlevel:

`root #``rc-update add nut.powerfail shutdown`
#### Systemd

To add the services to start on system boot:

`root #````
systemctl enable nut-driver-enumerator
```
`root #````
systemctl enable nut-server
```
`root #````
systemctl enable nut-monitor
```
`root #````
systemctl enable nut.target
```
To start upsd now run:

`root #````
systemctl start nut-driver-enumerator
```
`root #````
systemctl start nut-server
```
`root #````
systemctl start nut-monitor
```
`root #````
systemctl start nut.target
```

To check the status of the UPS manually (adjust the UPS name as needed):

`root #``upsc APCSMC1500@127.0.0.1 ups.status`
OL

OL means that the UPS is "online" and not drawing from the battery and is configured correctly.

## LAN client/server configuration

### Server configuration

Starting from standalone configuration, change:

#### Server files

##### /etc/nut/nut.conf

`root #``nano -w /etc/nut/nut.conf`
**`/etc/nut/nut.conf`**

**Desired settings**

##### /etc/nut/upsd.conf

`root #``nano -w /etc/nut/upsd.conf`
**`/etc/nut/upsd.conf`**

**Desired settings**

##### /etc/nut/upsd.users

`root #``nano -w /etc/nut/upsd.users`
**`/etc/nut/upsd.users`**

**Example upsd.users configuration**

```
[monmaster]
    password = masterpassword
    upsmon master
[monslave]
    password = slavepassword
    upsmon slave
```
### Client configuration

#### Client files

##### /etc/nut/nut.conf

`root #``nano -w /etc/nut/nut.conf`
**`/etc/nut/nut.conf`**

**Desired settings**

##### /etc/nut/ups.conf

This file is not needed for slave monitor.

##### /etc/nut/upsd.conf

This file is not needed for slave monitor.

##### /etc/nut/upsd.users

`root #``nano -w /etc/nut/upsd.users`
**`/etc/nut/upsd.users`**

**Example upsd.users configuration**

```
[monslave]
    password = slavepassword
    upsmon slave
```
##### /etc/nut/upsmon.conf

Copy the standalone configuration, but create a different MONITOR configuration in /etc/nut/upsmon.conf:

`root #``nano -w /etc/nut/upsmon.conf`
**`/etc/nut/upsmon.conf`**

**Example upsmon configuration**

##### /etc/nut/upssched.conf

This file can be the same of standalone configuration.

##### /usr/bin/upssched-cmd

This file can be the same of standalone configuration.

#### Client services

On client machine, only the upsmon service is needed. To add the services to start on system boot:

`root #````
rc-update add upsmon default
```
To start upsd now run:

`root #````
rc-service upsmon start
```
To check the status of the UPS manually (adjust the UPS name as needed):

`root #``upsc APCSMC1500@192.168.1.1 ups.status`
OL
