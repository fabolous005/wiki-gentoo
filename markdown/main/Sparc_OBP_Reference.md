<!-- source: https://wiki.gentoo.org/wiki/Sparc/OBP_Reference | group: Gentoo Wiki (Main) | wiki-title: Sparc/OBP Reference -->
---
title: Sparc/OBP Reference
url: https://wiki.gentoo.org/wiki/Sparc/OBP_Reference
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-08"
fingerprint: a4f97c5600e7b3f8
license: CC BY-SA 4.0
---

# Sparc/OBP Reference

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


The OpenBoot PROM (OBP) Reference provides a list of useful OBP commands that can help when booting, configuring and performing diagnostics upon Sun SPARC hardware and clones.

## About this Document

This document provides a list of useful OBP commands that can be helpful when booting, configuring and performing diagnostics upon Sun SPARC hardware and clones.

Note that not all commands are available on all PROM versions. In particular, sun4c systems have a different input mode, and some JavaStation models are missing a lot of OBP functionality.

OBP is somewhat inconsistent when it comes to command names. Sometimes hyphens are used to separate words, sometimes they are not. Some commands which display information start with or contain a dot (this is a FORTH convention).

The terms "OBP", "OpenBoot PROM" and "PROM" are used interchangeably in this document.

## Entering OBP

### Systems with Sun Keyboards

On regular Sun systems (and clones which use Sun keyboards), press `Stop+A` when the system boots to enter OBP. The `Stop` key is the top left key on the extra block of keys on the left hand side of the keyboards (on really old systems it might be marked `L1` ).

The `Stop+N` keyboard combination at boot will reset many systems to the default PROM settings. `Stop+D` will enable diagnostics on some boards.

### Over Serial Console

To enter OBP over serial console, send a break. In minicom, do this by pressing `ctrl+A F` . In xc, use `ctrl+A B` .

### Systems with Non-Sun (PC Style) Keyboards

On systems with PC-style keyboards (such as JavaStations and some Ultra clones), `ctrl+break` or `alt+ctrl+break` is generally used.

Note that on the JavaStation JK, `ctrl+break` will restart the box rather than enter PROM. To get at OBP, you will need to set jumper J1300 pins 7-8 on the mainboard.

## Basic System Commands

### Halting and Restarting

The `power-off` command will halt the box and turn off power. This command is sometimes screwy on Ultra5/10 systems.

The `reset` command will perform a soft reset. If diagnostics are enabled, they will *not* be rerun. In some documents, this command is referred to as `reset-all` .

The `boot` command boots the system. A parameter may be provided to override the default boot device -- this can be a full device name or a device alias such as `disk` , `cdrom` or `net` . Any additional arguments are passed to the bootloader or operating system.

### PROM and System Information

The `.version` command will show the OBP version.

`ok``.version`
Release 3.7 Version 0 created 1997/01/09 13:06
OBP 3.7.0 1997/01/09 13:06
POST 3.2.1 1996/12/20 03:01

The `banner` command will show the system information banner.

`ok``banner`
Sun Ultra 2 UPA/SBus (2 X UltraSPARC-II 296MHz), No Keyboard
OpenBoot 3.7, 704 MB memory installed, Serial #9705521.
Ethernet address 8:0:20:94:18:31, Host ID: 80941831

The `.speed` command will show bus speeds.

`ok``.speed`
CPU  Speed : 296.00 MHz
UPA  Speed : 098.66 MHz
SBus Speed : 025.00 MHz

### Device Information

The `show-devs` command will give a list of devices available to the system.

The `probe-scsi` command will show internal SCSI devices available to the system. The `probe-scsi-all` command will show both internal and external devices on every SCSI interface. After running either of these commands, you *must* issue a `reset-all` before trying to boot an operating system.

`ok``probe-scsi-all````
This command may hang the system if a Stop-A or halt command
has been executed.  Please type reset-all to reset the system
before executing this command.
Do you wish to continue? (y/n) y
/sbus@1f,0/SUNW,fas@e,8800000
Target 1
  Unit 0   Disk     SEAGATE ST34371W SUN4.2G74629710B00491
                    Copyright (c) 1997 Seagate
                    All rights reserved
```
The `probe-ide` command is also available on IDE-based systems.

`ok``probe-ide````
  Device 0  ( Primary Master )
          ATA Model: ST320420A
  
  Device 1  ( Primary Slave )
          ATA Model: Maxtor 6E040L0
  
  Device 2  ( Secondary Master )
         Not Present
  
  Device 3  ( Secondary Slave )
         Not Present
```
## Environment Variables and Device Aliases

### Getting and Setting Environment Variables

Use `printenv` to get a list of environment variables. To see the value of a specific variable, use `printenv fnord` .

To set a variable, use `setenv myvar the new value` .

To restore a variable's default value, use `set-default blah` . To revert *all* variables to default, use `set-defaults` .

Note that boolean variables usually end in a `?` , and that they are separate from variables of the same name without the question mark. The values `true` and `false` are used for booleans.

### Useful Environment Variables

Some useful environment variables:

| Variable | Meaning | 
|---|---|
| `ttya-mode` | Sets the baud rate and related settings used by the serial console. By default `9600,8,n,1,-` is used, meaning 9600 baud, 8 bits, no parity, one stop bit, no flow control. | 
| `auto-boot?` | By default OBP will automatically boot upon startup. Set this to `false` if you'd rather send the boot command manually. | 
| `boot-device` | When auto-booting, and when `boot` is issued with no arguments, this variable is used to determine the boot device. More than one value can be provided (space separated), in which case each is tried in turn. Each value can be a full device path or a device alias. | 
| `diag-device` | As `boot-device` , but used when `diag-switch?` is enabled. | 
| `local-mac-address?` | If set, network interfaces will use their own MAC rather than the system-wide MAC. This is consistent with how PCs behave. | 
| `diag-switch?` | If set, additional diagnostic checks will be run at power on. Note that this can take a *very* long time on SMP and / or HyperSparc systems. In addition, the `diag-device` variable will also be used to determine the boot device rather than `boot-device` . Some systems have a mainboard jumper or a switch on the front of the machine which forces this setting on. | 

### Getting and Setting Device Aliases

Device aliases can be used to simplify the arguments to many commands. Instead of typing `boot /sbus/SUNW,hme@e,8c00000` , for example, one could use `boot net` . A number of device aliases are defined by default on every system.

To view all device aliases, use the `devalias` command. To view a specific alias, use `devalias whatever` . To set a device alias, use `devalias whatever newvalue` .

### Changing Monitor Resolutions

The `output-device` variable can be used to control which framebuffer is used, and at what resolution it is run, for PROM console. For example, to use the Creator card on an Ultra 1C or 2 at a resolution of 1024x768@76Hz:

`ok``devalias screen /SUNW,ffb``ok``setenv output-device screen:r1024x768x76`
Changes will not take effect until after a reset. Not all resolutions and refresh are available on all cards.

## Diagnostics

### Entering Diagnostic Mode

Before running any diagnostics, it is best to enable `diag-switch?` and do a full power off / on cycle (hard power off). Additional diagnostic information will be provided over the serial console when the machine boots.

### Basic Tests

If the power on self test (POST) succeeds, additional tests can be performed using the `test` , `test-all` , `watch-net` and `watch-clock` commands.

The `test somedevice` command will perform checks upon the specified device (this can be a full device path or a device alias).

`ok``test scsi````
        CE DMA fill from address fff8e000 for 80 bytes   succeeded.
         Dma register test -- succeeded.
         Esp register test -- succeeded.
         Dma read test -- succeeded.
         Dma write test -- succeeded.
```
`ok``test /sbus/SUNW,hme`
Internal loopback test -- succeeded.
Transceiver check  -- Using Onboard Transceiver - Link Up.
passed

`ok``test ttya`
!"#$%&'()\*+,-./0123456789:;\<=>?@ABCDEFGHIJKLMNOPQRSTUVWXYZ\[\\\]^\_\`abcdefghijklmn

The `test-all` command will perform checks upon every device capable of self tests.

The `watch-net` command can be used to watch for broadcast packets being sent over the network.

The `watch-clock` command can be used to verify that the internal clock is working. It should count from 0 to 59 in a loop with an interval of one second.

### Extended Diagnostics Using obdiag

To enable extended diagnostics mode, run the following:

`ok``setenv mfg-mode on`
mfg-mode =            on

`ok``setenv diag-switch? true`
diag-switch? =        true

`ok``setenv auto-boot? false`
auto-boot? =          false

`ok``reset-all``ok``obdiag`
This should display a menu. First, select `16` to enable verbose messages.

Individual tests can then be run by selecting the relevant number. Note that some tests (for example, serial ports) assume that an external loopback connector is attached. The Ethernet test assumes that a cable is attached and connected to a switch.

To exit, use `14` .

## References

- OpenBoot 3.x Quick Reference, Sun P/N 802-3240 ( [PDF Version)](http://sunsolve.sun.com/data/802/802-3240/pdf/802-3240-10.pdf)
- OpenBoot 3.x Command Reference, Sun P/N 802-3242 ( [HTML Version)](http://sunsolve.sun.com/data/802/802-3242/html/TOC.html)
- OpenBoot 3.x Supplement for PCI, Sun P/N 802-7679 ( [Collection of PDF files)](http://sunsolve.sun.com/data/802/802-7679/pdf/)
- [Open Firmware Quick Reference](http://firmworks.com/QuickRef.html) (Not Sun OBP specific)
