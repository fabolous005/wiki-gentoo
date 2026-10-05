<!-- source: https://wiki.gentoo.org/wiki/Fan_speed_control/thinkfan | group: Gentoo Wiki (Main) | wiki-title: Fan speed control/thinkfan -->
---
title: thinkfan
url: https://wiki.gentoo.org/wiki/Fan_speed_control/thinkfan
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "9c4717cdfc4650ee"
license: CC BY-SA 4.0
---

# thinkfan

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Thinkfan** is a fan controlling deamon aimed specifically towards IBM/Lenovo Thinkpad [laptops](https://wiki.gentoo.org/wiki/Category:Laptops). It can read multiple sensors, but control only a single fan.

## Installation

### Kernel

The kernel needs to have the Thinkpad ACPI driver configured:

**Enable Thinkpad ACPI support in the kernel**

This will create a module called thinkpad\_acpi, which needs to be configured to allow for controlling a fan.

Edit or create /etc/modprobe.d/thinkpad.conf as follows:

**`/etc/modprobe.d/thinkpad.conf`**

```
 thinkpad_acpi fan_control=1
```
Most Thinkpads provide /proc/acpi/ibm/fan as a path to the fan device.

### USE flags


### Emerge

`root #``emerge --ask app-laptop/thinkfan`
## Configuration

The configuration file of thinkfan is in /etc/thinkfan.conf. It requires manual configuration and supports as of version 1.0 configuration based on yaml. An example of a simple mode configuration is as follows:

**`/etc/thinkfan.conf`**

```
sensors:
  - tpacpi: /proc/acpi/ibm/thermal
    indices: [0]
fans:
  - tpacpi: /proc/acpi/ibm/fan
levels:
  - [0, 0,  41]
  - [1, 38, 51]
  - [2, 45, 56]
  - [3, 51, 61]
  - [4, 55, 64]
  - [5, 60, 66]
  - [6, 63, 68]
  - [7, 65, 74]
  - [127, 70, 32767]
```
Details about the configuration can be found in /usr/share/doc/thinkfan-\*/thinkfan.yaml.bz2, and in thinkfan.conf man page.

Thinkfan also takes commandline parameters, see thinkfan's man page for details. Take special note of the bias value, which will exaggerate or dampen a sudden increase or decrease between two temperature samples.

### Verification

Test the behaviour of thinkfan as follows:

`root #``thinkfan -n -b -5 -c /etc/thinkfan.conf`
Temperatures(bias): 46(0) -> level 1
Temperatures(bias): 51(0) -> level 2
Temperatures(bias): 59(-3) -> level 3
Temperatures(bias): 64(-3) -> level 4
Temperatures(bias): 68(-3) -> level 5
Temperatures(bias): 69(-2) -> level 6
Temperatures(bias): 62(0) -> level 5
Temperatures(bias): 69(-3) -> level 6
Temperatures(bias): 62(0) -> level 5
Temperatures(bias): 66(0) -> level 6
Temperatures(bias): 62(0) -> level 5

It may take a few cycles of configuration update and testing to find the optimum between all the different settings that are possible.

### Service

#### OpenRC

Update /etc/init.d/thinkfan as per the selected commandline parameters:

**`/etc/init.d/thinkfan`**

```
#command_args="-q -s5 -c /etc/thinkfan.conf"
command_args="-q -s2 -b0 -c /etc/thinkfan.conf"
```
Start the thinkfan as a service and enable it as follows:

`root #``rc-service thinkfan start``root #``rc-config add thinkfan`
#### systemd

The thinkfan commandline parameters are passed via the /etc/systemd/system/thinkfan.service.d/default.conf file.

After setting the desired parameters enable and start the thinkfan service:

`root #``systemctl enable --now thinkfan.service`
## Manual fan control

It is also possible to control the fan speed manually:

`root #``echo level 7 > /proc/acpi/ibm/fan``root #``cat /proc/acpi/ibm/fan`
status:         enabled
speed:          3957
level:          7
commands:       level \<level> (\<level> is 0-7, auto, disengaged, full-speed)
commands:       enable, disable
commands:       watchdog \<timeout> (\<timeout> is 0 (off), 1-120 (seconds))

There are a few special values of `level`:

- `level auto` - the fan RPM is controlled by the [BIOS](https://wiki.gentoo.org/wiki/BIOS)
- `level full-speed` - the maximum fan speed while being monitored
- `level disengaged` - even faster speed, where the controller does not monitor the fan speed.
