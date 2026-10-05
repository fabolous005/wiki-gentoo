<!-- source: https://wiki.gentoo.org/wiki//dev | group: Gentoo Wiki (Main) | wiki-title: /dev -->
---
title: "/dev"
url: https://wiki.gentoo.org/wiki//dev
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-09-28"
fingerprint: "1ea988158f8933e9"
license: CC BY-SA 4.0
---

# /dev

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## The /dev directory

Most Linux users understand that /dev/sda1 is just a fast way of referring to the first partition on the first disk that the kernel found. That's pretty easy, right?

But consider hotpluggable devices like USB, IEEE 1394, hot-swappable PCI, etc. What is the first device for each of these? And for how long? What will the other devices be named when the first one disappears? How will that affect ongoing transactions? Wouldn't it be fun if a printing job were suddenly moved from a high-end laser printer to an almost-dead matrix printer just because someone decided to pull the plug on the laser printer (which just happened to be the first printer)?

Enter the device manager. A modern device manager must:

- Run in userspace.
- Dynamically create and remove [device files](https://wiki.gentoo.org/wiki/Device_file).
- Provide consistent device naming.
- Provide a userspace application program interface (API).

Every time a change happens within the device structure, the kernel emits a *uevent* which gets picked up by the device manager. The device manager then follows the rules declared in the /etc/udev/rules.d, /run/udev/rules.d and /lib/udev/rules.d directories. Based on the information contained within the uevent, it finds the rule or rules it needs to trigger and performs the required actions. These actions may involve the creation or deletion of device files, and may also trigger the loading of particular firmware files into kernel memory.
