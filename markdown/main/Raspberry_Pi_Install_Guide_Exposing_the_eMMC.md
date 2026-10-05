<!-- source: https://wiki.gentoo.org/wiki/Raspberry_Pi_Install_Guide/Exposing_the_eMMC | group: Gentoo Wiki (Main) | wiki-title: Raspberry Pi Install Guide/Exposing the eMMC -->
---
title: Raspberry Pi Install Guide/Exposing the eMMC
url: https://wiki.gentoo.org/wiki/Raspberry_Pi_Install_Guide/Exposing_the_eMMC
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-01-05"
fingerprint: "2a15a95cddeab0c2"
license: CC BY-SA 4.0
---

# Raspberry Pi Install Guide/Exposing the eMMC

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Exposing Compute Module eMMC

Fit jumper to disable eMMC boot.

The Raspberry Pi utility `usbboot` is required. It's not yet packaged for Gentoo, so do it the hard way

`~/usbboot` `make`
Take care to set the INSTALL\_PREFIX.

`~/usbboot``sudo INSTALL_PREFIX="/usr/local" make install`
install -m 755 rpiboot /usr/local/bin/
install -d /usr/local/share/rpiboot
install -d /usr/local/share/rpiboot/msd
install -d /usr/local/share/rpiboot/mass-storage-gadget64
install -m 644 msd/bootcode.bin  /usr/local/share/rpiboot/msd
install -m 644 msd/bootcode4.bin /usr/local/share/rpiboot/msd
install -m 644 msd/start.elf  /usr/local/share/rpiboot/msd
install -m 644 msd/start4.elf /usr/local/share/rpiboot/msd
install -m 644 mass-storage-gadget64/boot.img /usr/local/share/rpiboot/mass-storage-gadget64
install -m 644 mass-storage-gadget64/config.txt /usr/local/share/rpiboot/mass-storage-gadget64
install -m 644 mass-storage-gadget64/bootfiles.bin /usr/local/share/rpiboot/mass-storage-gadget64

Power up the Pi using a USB data port from a PC or USB hub. On the CM5 tested, the PSU load was under 400mA.

`user $``sudo ./rpiboot -d /usr/local/share/rpiboot/mass-storage-gadget64` RPIBOOT: build-date Jan  2 2025 version 20240422~085300 ef721cda
If the device fails to connect then please see https://rpltd.co/rpiboot for debugging tips.
Loading: /usr/local/share/rpiboot/mass-storage-gadget64/bootfiles.bin
Using /usr/local/share/rpiboot/mass-storage-gadget64/bootfiles.bin
Waiting for BCM2835/6/7/2711/2712...
Sending bootcode.bin
Successful read 4 bytes 
Waiting for BCM2835/6/7/2711/2712...
Second stage boot server
File read: mcb.bin
File read: memsys00.bin
File read: memsys01.bin
File read: memsys02.bin
File read: memsys03.bin
File read: bootmain
Loading: /usr/local/share/rpiboot/mass-storage-gadget64/config.txt
File read: config.txt
Loading: /usr/local/share/rpiboot/mass-storage-gadget64/boot.img
File read: boot.img
Second stage boot server done

Will load the boot code over USB using the mass-storage-gadget64, then expose the eMMC as a USB storage device over the USB2 provided in the power connector.

`user $``sudo ./rpiboot -d /usr/local/share/rpiboot/mass-storage-gadget64`
must be used every time that USB Mass Storage mode is required.

Other payloads can also be booted this way. See `./Readme.md`

## Testing

Inspect the end of the output of dmesg

`user $``dmesg` ```
...
[10096.645486] usb 1-4.2: new high-speed USB device number 11 using xhci_hcd
[10096.740158] usb 1-4.2: New USB device found, idVendor=0a5c, idProduct=0104, bcdDevice= 1.00
[10096.740165] usb 1-4.2: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[10096.740168] usb 1-4.2: Product: Raspberry Pi multi-function USB device
[10096.740170] usb 1-4.2: Manufacturer: Raspberry Pi
[10096.740172] usb 1-4.2: SerialNumber: c8dee776c5ff72a0
[10096.794078] usb-storage 1-4.2:1.0: USB Mass Storage device detected
[10096.794306] scsi host10: usb-storage 1-4.2:1.0
[10097.805288] scsi 10:0:0:0: Direct-Access     mmcblk0                        PQ: 0 ANSI: 2
[10097.805405] scsi 10:0:0:0: Attached scsi generic sg6 type 0
[10097.805680] sd 10:0:0:0: Power-on or device reset occurred
[10097.806636] sd 10:0:0:0: [sde] 61071360 512-byte logical blocks: (31.3 GB/29.1 GiB)
[10097.943867] sd 10:0:0:0: [sde] Write Protect is off
[10097.943874] sd 10:0:0:0: [sde] Mode Sense: 0f 00 00 00
[10098.079880] sd 10:0:0:0: [sde] Write cache: enabled, read cache: enabled, doesn't support DPO or FUA
[10098.373702] sd 10:0:0:0: [sde] Attached SCSI removable disk
```

The partitioning tool of your choice can be used on /dev/sde, from the example dmesg output above.

`root #````
fdisk -l /dev/sde
Disk /dev/sde: 29.12 GiB, 31268536320 bytes, 61071360 sectors
```
`root #````
Disk model:                 
```
`root #````
Units: sectors of 1 * 512 = 512 bytes
```
`root #````
Sector size (logical/physical): 512 bytes / 512 bytes
```
`root #``I/O size (minimum/optimal): 512 bytes / 512 bytes`
## Installing

Follow the rest of this guide, then come back here
