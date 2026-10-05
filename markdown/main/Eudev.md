<!-- source: https://wiki.gentoo.org/wiki/Eudev | group: Gentoo Wiki (Main) | wiki-title: Eudev -->
---
title: eudev
url: https://wiki.gentoo.org/wiki/Eudev
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-07-01"
fingerprint: "16a9d018c599a1e8"
license: CC BY-SA 4.0
---

# eudev

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


This article is

**archived (obsolete)**. Contents are surely incorrect for current usage, and are intended for historical reference only.

*Page archived as of**2023-10-12**.*

[udev](https://wiki.gentoo.org/wiki/Udev)from

[sys-apps/systemd-utils](https://packages.gentoo.org/packages/sys-apps/systemd-utils)instead. The transition should be uneventful, however

*the corresponding[News Item](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Reading_news_items) must be read and followed*to avoid any issues.

TLDR:

**Do not use this article!**



eudev is a fork of [udev](https://wiki.gentoo.org/wiki/Udev), [systemd](https://wiki.gentoo.org/wiki/Systemd)'s [device file](https://wiki.gentoo.org/wiki/Device_file) manager for the Linux kernel.<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> It manages device nodes in [/dev](https://wiki.gentoo.org/wiki//dev) and handles all user space actions when adding or removing devices.

The goal of eudev is to obtain better compatibility with existing software such as the [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) init system, [Upstart](<https://en.wikipedia.org/wiki/Upstart_(software)>) init system, older Linux kernels, various toolchains, and anything else required<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup> by (but not well supported through) udev.

Similar to ebuild, eclass, emerge, etc. eudev's name follows the 'E' prefix convention started by Gentoo's founder Daniel Robbins.

System configurations running systemd have no use for eudev.

## The /dev directory

Most Linux users understand that /dev/sda1 is just a fast way of referring to the first partition on the first disk that the kernel found. That's pretty easy, right?

But consider hotpluggable devices like USB, IEEE 1394, hot-swappable PCI, etc. What is the first device for each of these? And for how long? What will the other devices be named when the first one disappears? How will that affect ongoing transactions? Wouldn't it be fun if a printing job were suddenly moved from a high-end laser printer to an almost-dead matrix printer just because someone decided to pull the plug on the laser printer (which just happened to be the first printer)?

Enter the device manager. A modern device manager must:

- Run in userspace.
- Dynamically create and remove [device files](https://wiki.gentoo.org/wiki/Device_file).
- Provide consistent device naming.
- Provide a userspace application program interface (API).

Every time a change happens within the device structure, the kernel emits a *uevent* which gets picked up by the device manager. The device manager then follows the rules declared in the /etc/udev/rules.d, /run/udev/rules.d and /lib/udev/rules.d directories. Based on the information contained within the uevent, it finds the rule or rules it needs to trigger and performs the required actions. These actions may involve the creation or deletion of device files, and may also trigger the loading of particular firmware files into kernel memory.



## Installation

### USE flags

Some packages know the [udev](https://packages.gentoo.org/useflags/udev)[USE flag](https://wiki.gentoo.org/wiki/USE_flag) for enabling [virtual/udev](https://packages.gentoo.org/packages/virtual/udev) integration.

*sys-fs/eudev*correct?

### Emerge

To avoid registration in the [world set](<https://wiki.gentoo.org/wiki/World_set_(Portage)>), the oneshot option should be used.

`root #``emerge --ask --oneshot sys-fs/eudev`
## Configuration

### RC service

The RC name is udev, not eudev. It needs to be registered in the sysinit runlevel.

`root #``rc-update add udev sysinit`
\* rc-update: udev already installed in runlevel \`sysinit'; skipping

From [sys-fs/udev-init-scripts](https://packages.gentoo.org/packages/sys-fs/udev-init-scripts) version 29 onward, `udev-trigger` should also be [added to the sysinit runlevel](https://www.gentoo.org/support/news-items/2015-06-08-udev-init-scripts-changes.html).

### Optional: Keep classic network interface naming

Network device names such as eth0 or wlan0 as provided by the kernel are normally changed on system boot (see dmesg) by the /lib/udev/rules.d/80-net-name-slot.rules udev rule.

To keep the classic naming this rule can be overwritten with an equally named empty file in the /etc/udev/rules.d directory:

`root #````
touch /etc/udev/rules.d/80-net-name-slot.rules
```
Alternatively add `net.ifnames=0` to the kernel command line, change the default policy or create a custom one.

An interesting resource related to the network naming is [I don't like this, how do I disable this](https://wiki.freedesktop.org/www/Software/systemd/PredictableNetworkInterfaceNames/#idontlikethishowdoidisablethis) on the Freedesktop wiki.

## Troubleshooting

### Using udevadm

Start udevadm monitor and see what happens:

`user $``udevadm monitor`
Get device info using udevadm info followed by the device path:

`user $``udevadm info -p /devices/pci0000:00/0000:00:1d.7`
Get the device path using its name:

`user $``udevadm info -q path -n input/mouse1`
/devices/pci0000:00/0000:00:1d.7/usb1/1-3/1-3.4/1-3.4:1.0/0003:046D:C404.0006/input/input24/mouse1

Test an event run using udevadm test followed by the device path for which an event is to be tested:

`user $``udevadm test /devices/pci0000:00/0000:00:1d.7`
To get a list of all udevadm commands:

`user $``udevadm -h`
### Migrating older releases

#### udev 171-r10 to eudev 1.2-r1

See [this post](https://forums.gentoo.org/viewtopic-t-966786-start-0.html) on the Gentoo forums.

### Broken system switching from udev to eudev

See [this post](https://forums.gentoo.org/viewtopic-t-1011456.html) on the Gentoo forums.

## See also

- [Udev](https://wiki.gentoo.org/wiki/Udev) — [systemd's](https://wiki.gentoo.org/wiki/Systemd) device manager for the Linux kernel.
- [Gentoo eudev Project](https://wiki.gentoo.org/index.php?title=Project:Eudev&action=edit&redlink=1) — The official Gentoo project fork of udev.
- [Allow only known usb devices](https://wiki.gentoo.org/wiki/Allow_only_known_usb_devices) — describes how to protect a GNU/Linux system against rogue USB devices via a white listing policy.
- [Google Summer of Code/2016/Ideas/Eudev](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2016/Ideas/Eudev)

## External resources

- [Moving from udev (171-r10) to eudev (1.2-r1)](https://forums.gentoo.org/viewtopic-p-7712064.html) on the Gentoo Forums

## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) [Bug 575718 - Request for council decision RE virtual/udev default provider](https://bugs.gentoo.org/show_bug.cgi?id=575718), [Gentoo's Bugzilla Main Page](https://bugs.gentoo.org/), (Last modified) February 27th, 2016. Retrieved on March 1st, 2016.
2. [↑](https://wiki.gentoo.org#cite_ref-2) [Project:Eudev](https://wiki.gentoo.org/index.php?title=Project:Eudev&action=edit&redlink=1), [Gentoo Wiki](https://wiki.gentoo.org/wiki/Main_Page), (Last modified) November 12th, 2015. Retrieved on March 11th, 2016.
