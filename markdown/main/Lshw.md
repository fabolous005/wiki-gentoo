<!-- source: https://wiki.gentoo.org/wiki/Lshw | group: Gentoo Wiki (Main) | wiki-title: Lshw -->
---
title: Lshw
url: https://wiki.gentoo.org/wiki/Lshw
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-06-13"
fingerprint: e70e2e155863a705
license: CC BY-SA 4.0
---

# Lshw

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

lshw (Hardware Lister) is a small tool that provides detailed information on the hardware configuration of the machine. It can report exact memory configuration, firmware version, mainboard configuration, CPU version and speed, cache configuration, bus speed, etc. on DMI-capable x86 or EFI (IA-64) systems and on some PowerPC machines (PowerMac G4 is known to work).

## Installation

### USE flags


### Emerge

`root #``emerge --ask sys-apps/lshw`
## Usage

### Invocation

`root #``lshw --help````
Hardware Lister (lshw) - B.02.17
usage: lshw [-format] [-options ...]
       lshw -version
        -version        print program version (B.02.17)
format can be
        -html           output hardware tree as HTML
        -xml            output hardware tree as XML
        -short          output hardware paths
        -businfo        output bus information
options can be
        -class CLASS    only show a certain class of hardware
        -C CLASS        same as '-class CLASS'
        -c CLASS        same as '-class CLASS'
        -disable TEST   disable a test (like pci, isapnp, cpuid, etc. )
        -enable TEST    enable a test (like pci, isapnp, cpuid, etc. )
        -quiet          don't display status
        -sanitize       sanitize output (remove sensitive information like serial numbers, etc.)
        -numeric        output numeric IDs (for PCI, USB, etc.)
  
```
### Launch the X11 GUI

Enables interaction with lshw's graphical user interface, presenting a comprehensive and interactive view of hardware components.

`root #``lshw -X`
### Generate full information report about all detected hardware

Produces a comprehensive report detailing every aspect of the detected hardware components within the system.

`user $``lshw`
### Display brief hardware information

Gives a succinct summary of the hardware configuration, providing a general overview rather than exhaustive details.

`user $``lshw -short`
### Display bus information

Details bus information for all detected hardware components, helpful for understanding the communication routes between hardware elements.

`user $``lshw -businfo`
## Enable/disable specific features

### Disable USB

Prevents the listing of USB devices in the report, useful when USB device details are not necessary for the investigation.

`user $``lshw -disable usb`
### Enable DMI

Turns on the reporting of DMI/SMBIOS information, which provides additional hardware component details stored in the system BIOS.

`user $``lshw -enable dmi`
### Use numeric IDs for devices

Outputs numeric IDs instead of descriptive names for devices, aiding in identifying specific hardware elements based on their standard device IDs.

`user $``lshw -numeric`
### Display less verbose output

Limits the verbosity of the output, leading to a more compact and less detailed report.

`user $``lshw -quiet`
### Remove sensitive information from the output

Ensures the output excludes any sensitive information such as serial numbers, promoting a safer sharing of the report.

`user $``lshw -sanitize`
## Generate reports

### Generate report in HTML format

Creates an HTML report for easy viewing and navigation within a web browser.

`user $``lshw -html > hardware.html`
### Generate report in XML format

Forms an XML report, ideal for parsing and processing by other software or for storage purposes.

`user $``lshw -xml > hardware.xml`
### Generate report in SQLite format

Generates an SQLite database report, allowing efficient querying and extraction of hardware information in structured form.

`user $``lshw -dump > hardware.db`
## Display specific hardware classes

### Processor

Provides details of the CPU including its type, speed, cores, and cache information.

`user $``lshw -class processor`
### Memory

Displays comprehensive details about the system memory such as RAM, ROM, and any associated caches.

`user $``lshw -class memory`
### Disk

Reports on disk drives, including their type, size, and partitioning information.

`user $``lshw -class disk`
### Storage controllers

Presents information on storage controllers, like SATA or NVMe controllers, critical in understanding the storage infrastructure.

`user $``lshw -class storage`
### Network adapters

Details on network interfaces including ethernet and WiFi, providing insights into networking capabilities.

`user $``lshw -class network`
### Display

Describes graphics cards and monitors, essential for understanding the system's display capabilities.

`user $``lshw -class display`
### Multimedia

Details multimedia devices like sound cards, providing information on audio-visual capabilities.

`user $``lshw -class multimedia`
### Printer

Provides information about any attached printers, useful in troubleshooting and setting up printing capabilities.

`user $``lshw -class printer`
### System

Offers extensive details about the system/motherboard, providing a broad understanding of the system's foundation.

`user $``lshw -class system`
### Bus

Reports on busses, like USB or PCI, integral to understanding how components communicate within the system.

`user $``lshw -class bus`
### Bridge

Describes bridge devices, crucial for understanding connections between different parts of the computer system.

`user $``lshw -class bridge`
### Communication

Presents data on communication devices, useful in examining and troubleshooting communication capabilities.

`user $``lshw -class communication`
### Generic

Furnishes information about generic/unclassified devices that may not fit into other specific hardware classes.

`user $``lshw -class generic`
### Power

Reveals details about power-related hardware, like UPS, important in understanding the power management infrastructure.

`user $``lshw -class power`
### Volume

Provides information about volumes/partitions, assisting in disk and storage management tasks.

`user $``lshw -class volume`
