<!-- source: https://wiki.gentoo.org/wiki/Mac_Mini_(PowerPC_G4) | group: Gentoo Wiki (Main) | wiki-title: Mac Mini (PowerPC G4) -->
---
title: Mac Mini (PowerPC G4)
url: https://wiki.gentoo.org/wiki/Mac_Mini_(PowerPC_G4)
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-03-26"
fingerprint: f0953e2c04ef9fcc
license: CC BY-SA 4.0
---

# Mac Mini (PowerPC G4)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

The **Mac Mini (PowerPC G4)** A1103 is a low-spec small form factor Apple desktop computer with a single core 32-bit [PowerPC](https://wiki.gentoo.org/wiki/PPC) G4 CPU. The system itself is a small box 165mm on a side, considerably smaller than other small form factor desktops of the era. Apple achieved this feat by designing the system around laptop components. The Mac Mini G4 lacks external screws as Apple did not intend the device to be user serviceable but the rear of the system features a relatively large number of expansion ports, mostly in the form of USB and Firewire.

This system should work with the unofficial Mac OS X build [Mac OS X 10.5.9 - Sorbet Leopard](https://apple.fandom.com/wiki/Sorbet_Leopard), provided 512MB or more of RAM is available on the system.

## Hardware

The Mac Mini PowerPC G4 A1103 came in a few minor variants, the most significant was the difference in CPU clock speed. The CPU cannot be changed but the system can be overclocked. The Macs of the era opted for [Open Firmware](https://wiki.gentoo.org/wiki/Open_Firmware) for system initialization. Basic low-level configuration changes were handled by the firmware's built-in [Forth](https://wiki.gentoo.org/wiki/Forth) interpreter. No Mac Mini of this line can accept more than 1GB of RAM and the G4 CPU cannot address more than 4GB, including virtual memory. The system's two PATA/100 drives, one a hard disk and the other an optical drive, share the same bus. Hard disk logical block addressing is limited to 28-bit LBA addressing, limiting the maximum supported capacity of a single drive to approximately 137GB.

Some variants include support for Bluetooth 1.1 as component of the 802.11b/g WiFi module.

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | PowerPC G4 (1.25 GHz – 1.5 GHz) |  |  |  |  | Not upgradable | 
| RAM | DDR-333 |  |  |  |  | 1 Slot, Max 1GB | 
| Graphics | ATI Radeon 9200, 32 MB VRAM |  |  |  |  | 1.5 GHz model has 64 MB VRAM | 
| Hard Drive | 40 GB – 80 GB 2.5" PATA/100 |  |  |  |  | 137 GB max drive capacity. | 
| Optical Drive | CD-RW/DVD-ROM |  |  |  |  | Combo drive or Super Drive | 
| WiFi | 802.11b/g |  |  |  |  |  | 
| Ethernet | 10/100 Mbps |  |  |  |  |  | 
| USB 2.0 | 2× USB-A |  |  |  |  |  | 
| Firewire 400 | 1× 6-pin IEEE 1394 |  |  |  |  |  | 

## Installation

It is possible to install Gentoo on a Mac Mini G4, so long as the limitations of the system are kept firmly in mind. Follow the [PPC Handbook](https://wiki.gentoo.org/wiki/Handbook:PPC).

Booting a Gentoo PPC USB stick is a little tricky if you're not used to it. Power on the system and hit `⌘ Command` + `⌥ Option` + `O` + `F`. The system will enter Open Firmware mode and the user will be presented with a Forth prompt. Assuming the USB stick is on the right side of the system, the following command will boot from it:

`0 >``boot usb1/disk@1:,\boot\grub\powerpc.elf`
If the USB stick is on the left side of the system, then the following is correct:

`0 >``boot usb0/disk@1:,\boot\grub\powerpc.elf`
From here the PPC Handbook can be followed very closely.

The biggest problem that needs to be addressed is the system's lack of RAM. A swap partition is an absolute necessity in this case. Further, a [binhost](https://wiki.gentoo.org/wiki/Binary_package_guide) to offload package creation is a very good idea.

## Configuration

### Portage make.conf

The portage makefile needs to be optimized for the PowerPC G4. The following is known-good:

**`/etc/portage/make.conf`**

```
COMMON_FLAGS="-mcpu=7450 -O2 -maltivec -mabi=altivec -pipe"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
```
## Troubleshooting

### Why can't I have an internal hard disk > 137GB?

This is a limitation of 28-bit LBA addressing. This is a limit imposed by the system's disk controller. There is no way around it. Modern SATA devices attached to the USB or Firewire ports will not have this limitation.

### Can I replace the optical drive with another hard disk?

Maybe. The CD-ROM connector is a relatively common (for the era) slim CD-ROM connector, it's not proprietary. It might be possible to insert a Slim CD/DVD-ROM drive to IDE adapter and go from there. Keep in mind the two devices are on the same IDE bus and compete for the same limited (133MB/s) bandwidth.

### How do I increase system disk or swap device performance?

The PATA 100 disk bus is limited to 100MB/s, a spinning disk will not get this performance outside of the drive cache. Opting for a SSD will likely result in disk operations at or very near to the PATA 100 bus maximums. That said, there is more than one factor to consider here.

USB 2.0 has a theoretical maximum bus speed of 480Mbps (60MB/s) and Firewire 400 is 400Mps (50MB/s). It needs to be kept in mind that In the real world, chipsets have a huge impact on this number; these values may not reflect real world performance. Also, if the swap device is on the same bus as the system disk it's easy to saturate the bus and drag down overall system performance.

Depending upon system workload it may help to migrate the swap partition to a USB or Firewire device, even though their bus speeds are less than that of the main IDE bus. Such a configuration may eliminate resource contention on the IDE bus and improve overall performance.

## See Also

- [Forth](https://wiki.gentoo.org/wiki/Forth) — a heavily stack-oriented self-compiling procedural programming language that is only slightly more abstract than [assembly](https://wiki.gentoo.org/wiki/Assembly_language).
- [Open Firmware](https://wiki.gentoo.org/wiki/Open_Firmware) — an [IEEE 1275-1994](https://ieeexplore.ieee.org/document/763383) standard for hardware independent firmware built on top of a [Forth](https://wiki.gentoo.org/wiki/Forth) machine.

## External resources

- [PPC Overclocking Station](https://thehouseofmoth.com/ppc-overclocking-station/) — detailed instructions on how to overclock a Mac Mini G4.
- [Mac OS X 10.5.9 - Sorbet Leopard](https://apple.fandom.com/wiki/Sorbet_Leopard) — an unofficial build of OS X for G4 and G5 PPC Macs.
