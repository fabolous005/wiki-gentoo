<!-- source: https://wiki.gentoo.org/wiki/Lenovo_Thinkpad_L540 | group: Gentoo Wiki (Main) | wiki-title: Lenovo Thinkpad L540 -->
---
title: Lenovo Thinkpad L540
url: https://wiki.gentoo.org/wiki/Lenovo_Thinkpad_L540
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "771c0d2fd38a1fea"
license: CC BY-SA 4.0
---

# Lenovo Thinkpad L540

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

This article contains information to assist when installing Gentoo on a Lenovo Thinkpad L540.

## Hardware

### lspci

`root #``lspci`
00:00.0 Host bridge: Intel Corporation Xeon E3-1200 v3/4th Gen Core Processor DRAM Controller (rev 06)
00:02.0 VGA compatible controller: Intel Corporation 4th Gen Core Processor Integrated Graphics Controller (rev 06)
00:03.0 Audio device: Intel Corporation Xeon E3-1200 v3/4th Gen Core Processor HD Audio Controller (rev 06)
00:14.0 USB controller: Intel Corporation 8 Series/C220 Series Chipset Family USB xHCI (rev 05)
00:16.0 Communication controller: Intel Corporation 8 Series/C220 Series Chipset Family MEI Controller #1 (rev 04)
00:19.0 Ethernet controller: Intel Corporation Ethernet Connection I217-V (rev 05)
00:1b.0 Audio device: Intel Corporation 8 Series/C220 Series Chipset High Definition Audio Controller (rev 05)
00:1c.0 PCI bridge: Intel Corporation 8 Series/C220 Series Chipset Family PCI Express Root Port #1 (rev d5)
00:1c.1 PCI bridge: Intel Corporation 8 Series/C220 Series Chipset Family PCI Express Root Port #2 (rev d5)
00:1c.4 PCI bridge: Intel Corporation 8 Series/C220 Series Chipset Family PCI Express Root Port #5 (rev d5)
00:1f.0 ISA bridge: Intel Corporation HM86 Express LPC Controller (rev 05)
00:1f.2 SATA controller: Intel Corporation 8 Series/C220 Series Chipset Family 6-port SATA Controller 1 \[AHCI mode\] (rev 05)
00:1f.3 SMBus: Intel Corporation 8 Series/C220 Series Chipset Family SMBus Controller (rev 05)
02:00.0 Network controller: Intel Corporation Wireless 7260 (rev 6b)
03:00.0 Unassigned class \[ff00\]: Realtek Semiconductor Co., Ltd. RTS5227 PCI Express Card Reader (rev 01)

## Troubleshooting

### Unable to shutdown/reboot, irq N:nobody cared (try booting with the "irqpoll" option), system turns dead after resume from hibernation.

It seems that with kernel **\<4.0** you may have issues halting the system, which will result in hang after message about powering off. With kernel **=4.0** you may have no longer such issue but after you resume from hibernation, USB ports may not work at all as well as you may start seeing the message about irq that nobody cared. With kernel **=4.1** systems after resume from hibernation turns dead. The apps are semi-responsible but no new process can be spawned, like storage was offline. Booting with \`**irqpoll**\` does make the system works as it should.

As of kernel 4.2.0-rc1 the issues with IRQ are no more.

### Brightness control does not work

Depends on kernel version, you may need to boot with either **acpi\_osi=Linux** or with empty string **acpi\_osi=**

### After resuming from hibernation system either ends up with black screen or crash shortly after resuming

Randomly, but often, after resuming from on-disk hibernation the system may hang with black screen or resume, but crash shortly after (multiple segfaults, later resulting in whole system freeze after several seconds)

Multiple sources reports that there's a bug in a whole range of Lenovo laptops, including but not limiting to X1 Carbon, E540, L540 and others, the bug affect USB 3.0 port and after disabling USB 3.0 in bios (system uses all ports as usb 2.0 after than) there's no longer issue with resume. In case of this very L540 that wasn't enough. Solution was to disable XHCI (usb 3.0) driver in kernel as well as disabling PREEMPT in the kernel and reducing timer frequency to 100 Hz, by changing the symbols:

`root #``./scripts/diffconfig .config-broken .config`
-USB\_XHCI\_PCI y
 HZ 250 -> 100
 HZ\_100 n -> y
 HZ\_250 y -> n
 PREEMPT\_NONE n -> y
 PREEMPT\_VOLUNTARY y -> n
 USB\_XHCI\_HCD y -> n

This is hardly considered as fix, but rather a workaround that does not trigger the bug.

### Wrong order of ALSA cards (First HDMI, then PCH

For some reason the card0 is not the PCH card. This can be changed with index option of snd-hda-intel module.

One can create /etc/modprobe.d/alsa-reorder.conf with

options snd\_hda\_intel index=1,0

To reorder the PCH as first card.

### Random system hangs with possible screen artifacts

Since update to BIOS version J4ET76WW (1.76) random hard-freezes have been experienced. The screen usually is torn apart. Update to BIOS version 1.78 solve the issue. All version from 1.74 to 1.76 are no longer available on the Lenovo page. 1.77 was never public and 1.78 changelog lists only changes from 1.77 to 1.78.
