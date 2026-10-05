<!-- source: https://wiki.gentoo.org/wiki/Dmidecode | group: Gentoo Wiki (Main) | wiki-title: Dmidecode -->
---
title: Dmidecode
url: https://wiki.gentoo.org/wiki/Dmidecode
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-10-07"
fingerprint: a4ad5f1104f3ad85
license: CC BY-SA 4.0
---

# Dmidecode

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

dmidecode is a software tool that enables extraction of detailed hardware information from a system by decoding the DMI (Desktop Management Interface) table. It provides valuable insights into the system's hardware configuration as described in SMBIOS (System Management BIOS). With dmidecode, you can retrieve information such as the system's [BIOS](https://wiki.gentoo.org/wiki/BIOS) vendor, version, release date, and revision, as well as details about the system's manufacturer, product name, serial number, UUID, SKU number, family, baseboard information, chassis details, processor family, manufacturer, version, and frequency.

## Installation

### USE flags

To install dmidecode, you can configure the following USE flag:


### Emerge

To install dmidecode using the Portage package manager, run the following command:

`root #``emerge --ask sys-apps/dmidecode`


## Description

Here is a description of the various keywords and their corresponding information:

| Keyword | Description | 
|---|---|
| bios-vendor | The vendor responsible for manufacturing the system's BIOS firmware. | 
| bios-version | The specific release or version number assigned to the system's BIOS firmware. | 
| bios-release-date | The date when the system's BIOS firmware was initially released or made available. | 
| bios-revision | The specific version or revision number assigned to the system's BIOS firmware. | 
| firmware-revision | The specific version or revision number assigned to the system's firmware, including the BIOS. | 
| system-manufacturer | The manufacturer responsible for manufacturing the system. | 
| system-product-name | The specific model or name assigned to the system by the manufacturer. | 
| system-version | The specific release or version number assigned to the system. | 
| system-serial-number | The unique serial number assigned to the system by the manufacturer. | 
| system-uuid | The Universally Unique Identifier (UUID) assigned to the system, providing a globally unique identifier across platforms and environments. | 
| system-sku-number | The Stock Keeping Unit (SKU) number assigned to the system, used for inventory or product tracking. | 
| system-family | The family or category of the system, indicating similarities or belonging to the same product line. | 
| baseboard-manufacturer | The manufacturer responsible for manufacturing the motherboard or baseboard. | 
| baseboard-product-name | The specific model or name assigned to the motherboard or baseboard by the manufacturer. | 
| baseboard-version | The specific release or version number assigned to the motherboard or baseboard. | 
| baseboard-serial-number | The unique serial number assigned to the motherboard or baseboard by the manufacturer. | 
| baseboard-asset-tag | The asset tag assigned to the motherboard or baseboard for tracking or identification purposes. | 
| chassis-manufacturer | The manufacturer responsible for manufacturing the chassis or case of the system. | 
| chassis-type | The type or form factor of the chassis, such as tower, rack-mount, or desktop. | 
| chassis-version | The specific release or version number assigned to the chassis. | 
| chassis-serial-number | The unique serial number assigned to the chassis by the manufacturer. | 
| chassis-asset-tag | The asset tag assigned to the chassis for tracking or identification purposes. | 
| processor-family | The family or category of the processor, indicating similarities or belonging to the same processor family. | 
| processor-manufacturer | The manufacturer responsible for manufacturing the processor. | 
| processor-version | The specific release or version number assigned to the processor. | 
| processor-frequency | The operating frequency or speed of the processor, typically measured in gigahertz (GHz). | 

## Command Usage

Here is the detailed hardware information that can be retrieved using dmidecode:

### BIOS Information

#### BIOS Vendor

To obtain the vendor information of the system's BIOS firmware, use the following command:

`root #``dmidecode -s bios-vendor`
#### BIOS Version

To retrieve the version number of the system's BIOS firmware, use the following command:

`root #``dmidecode -s bios-version`
#### BIOS Release Date

To obtain the release date of the system's BIOS firmware, use the following command:

`root #``dmidecode -s bios-release-date`
#### BIOS Revision

To retrieve the revision number of the system's BIOS firmware, use the following command:

`root #``dmidecode -s bios-revision`
#### Firmware Revision

To obtain the revision number of the system's firmware, including the BIOS, use the following command:

`root #``dmidecode -s firmware-revision`
### System Information

#### System Manufacturer

To retrieve the manufacturer information of the system, use the following command:

`root #``dmidecode -s system-manufacturer`
#### System Product Name

To obtain the product name assigned to the system by the manufacturer, use the following command:

`root #``dmidecode -s system-product-name`
#### System Version

To retrieve the version number assigned to the system, use the following command:

`root #``dmidecode -s system-version`
#### System Serial Number

To obtain the unique serial number assigned to the system by the manufacturer, use the following command:

`root #``dmidecode -s system-serial-number`
#### System UUID

To retrieve the Universally Unique Identifier (UUID) assigned to the system, use the following command:

`root #``dmidecode -s system-uuid`
#### System SKU Number

To obtain the Stock Keeping Unit (SKU) number assigned to the system, use the following command:

`root #``dmidecode -s system-sku-number`
#### System Family

To retrieve the family or category information of the system, use the following command:

`root #``dmidecode -s system-family`
### Baseboard Information

#### Baseboard Manufacturer

To obtain the manufacturer information of the motherboard or baseboard, use the following command:

`root #``dmidecode -s baseboard-manufacturer`
#### Baseboard Product Name

To retrieve the product name assigned to the motherboard or baseboard, use the following command:

`root #``dmidecode -s baseboard-product-name`
#### Baseboard Version

To obtain the version number assigned to the motherboard or baseboard, use the following command:

`root #``dmidecode -s baseboard-version`
#### Baseboard Serial Number

To retrieve the unique serial number assigned to the motherboard or baseboard, use the following command:

`root #``dmidecode -s baseboard-serial-number`
#### Baseboard Asset Tag

To obtain the asset tag assigned to the motherboard or baseboard, use the following command:

`root #``dmidecode -s baseboard-asset-tag`
### Chassis Information

#### Chassis Manufacturer

To retrieve the manufacturer information of the chassis or case of the system, use the following command:

`root #``dmidecode -s chassis-manufacturer`
#### Chassis Type

To obtain the type or form factor of the chassis, use the following command:

`root #``dmidecode -s chassis-type`
#### Chassis Version

To retrieve the version number assigned to the chassis, use the following command:

`root #``dmidecode -s chassis-version`
#### Chassis Serial Number

To obtain the unique serial number assigned to the chassis, use the following command:

`root #``dmidecode -s chassis-serial-number`
#### Chassis Asset Tag

To retrieve the asset tag assigned to the chassis, use the following command:

`root #``dmidecode -s chassis-asset-tag`
### Processor Information

#### Processor Family

To obtain the family or category information of the processor, use the following command:

`root #``dmidecode -s processor-family`
#### Processor Manufacturer

To retrieve the manufacturer information of the processor, use the following command:

`root #``dmidecode -s processor-manufacturer`
#### Processor Version

To obtain the version number assigned to the processor, use the following command:

`root #``dmidecode -s processor-version`
#### Processor Frequency

To retrieve the operating frequency or speed of the processor, use the following command:

`root #``dmidecode -s processor-frequency`
## Ownership

The **ownership** command allows you to manipulate ownership options for memory reading. You can specify different options to customize the behavior of the command.

To read memory from a specific device file (/dev/custom\_mem), you can use the following command:

`root #``ownership -d /dev/custom_mem`
## Vpddecode

The **vpddecode** command is used for decoding VPD (Vital Product Data) records. You can provide various options to customize the behavior of the command.

To read memory from a specific device file (e.g., /dev/custom\_mem), you can use the following command:

`root #``vpddecode -d /dev/custom_mem`
To only display the value of a specific VPD string (e.g., "motherboard-serial-number"), use the following command:

`root #``vpddecode -s motherboard-serial-number`
To prevent the decoding of VPD records and dump the raw data, use the following command:

`root #``vpddecode -u`
## Tips and Tricks

### Systemd

If dmidecode is not available or inaccessible, there are alternative methods to retrieve BIOS information. In addition to the previously mentioned tips, you can also utilize systemd commands to access relevant data. Some of the systemd commands for retrieving BIOS info include:

\- To access kernel output related to the BIOS, you can use the following command:

`root #``journalctl --quiet --system --boot SYSLOG_IDENTIFIER=kernel`
\- Another command that can provide BIOS-related information is:

`root #``systemctl show --property=EFI_VARS`
Please note that the availability and output of these commands may vary depending on the system configuration.

### Reading DMI Data Without dmidecode and as Non-Root

If you are unable to launch dmidecode as usual or do not have root privileges, there are alternative methods to read DMI (Desktop Management Interface) information. Please note that some information, such as serial numbers, may require root privileges to access.

One option is to access the DMI information under the directory /sys/class/dmi/id/ as a regular user. This directory contains various files that provide DMI-related information. However, please be aware that the following options require root access:

- Chassis serial number: /sys/class/dmi/id/chassis\_serial
- Product serial number: /sys/class/dmi/id/product\_serial
- Product UUID: /sys/class/dmi/id/product\_uuid


For example, you can retrieve the platform type (system product name) by reading the file:

`user $``cat /sys/class/dmi/id/product_name`
Similarly, you can obtain the BIOS version by reading:

`user $``cat /sys/class/dmi/id/bios_version`
To retrieve the amount of physical memory, you can make use of the following command:

`user $``cat /sys/class/dmi/id/memory/size`
## See also

- [Lshw](https://wiki.gentoo.org/wiki/Lshw) — a small tool that provides detailed information on the hardware configuration of the machine. It can report exact memory configuration, firmware version, mainboard configuration, CPU version and speed, cache configuration, bus speed, etc. on DMI-capable x86 or EFI (IA-64) systems and on some PowerPC machines (PowerMac G4 is known to work).
- [Lspci](https://wiki.gentoo.org/wiki/Lspci) — contains various utilities dealing with the [PCI bus](https://en.wikipedia.org/wiki/Peripheral_Component_Interconnect) (primarily lspci).
- [Lsusb](https://wiki.gentoo.org/wiki/Lsusb) — a collection various utilities for querying the the Universal Serial Bus (USB).
- [BIOS\_Update#Gather\_firmware\_information](https://wiki.gentoo.org/wiki/BIOS_Update#Gather_firmware_information)
