<!-- source: https://wiki.gentoo.org/wiki/MTPfs | group: Gentoo Wiki (Main) | wiki-title: MTPfs -->
---
title: MTPfs
url: https://wiki.gentoo.org/wiki/MTPfs
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-10-03"
fingerprint: "370405169bcb38c4"
license: CC BY-SA 4.0
---

# MTPfs

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


MTPfs is a FUSE-based filesystem providing access to MTP devices. Typically these devices are Android phones or similar mobile devices.

## Installation

### Kernel

See the [MTP](https://wiki.gentoo.org/wiki/MTP) meta article or the [FUSE](https://wiki.gentoo.org/wiki/FUSE) article for instructions on enabling FUSE support in the Linux kernel.

### USE flags


[sys-fs/mtpfs](https://packages.gentoo.org/packages/sys-fs/mtpfs) depends on [media-libs/libmtp](https://packages.gentoo.org/packages/media-libs/libmtp). It turned out<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> that at least version 1.1.8<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup> is needed to make it work.<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup>.

### Emerge

Install [sys-fs/mtpfs](https://packages.gentoo.org/packages/sys-fs/mtpfs):

`root #``emerge --ask sys-fs/mtpfs`
## Configuration

### Android devices

Android devices will need to be put into MTP mode. The way this is done changes slightly based on the version of Android running. On more modern versions, slide down the top of the screen once to show notifications. There should be a notification from the Android System indicating something to the effect of "USB debugging connected." Below this notification is a second notification. Tap the second notification, then change the setting from "Charge only" to "USB for file transfer."

### Files

Modify /etc/fuse.conf:

**`/etc/fuse.conf`**

### User privileges

Appropriate user(s) need to be in the `plugdev` group:

`root #``gpasswd -a <USER_NAME> plugdev`
## Usage

`user $``mkdir ~/AndroidDevice``user $``mtpfs ~/AndroidDevice`
Unable to open \~/.mtpz-data for reading, MTPZ disabled.
Device 0 (VID=0bb4 and PID=0c93) is a HTC EVO 4G LTE/One V (ID1).
Android device detected, assigning default bug flags

Unmount:

`user $``fusermount -u ~/AndroidDevice`
## See also

- [MTP](https://wiki.gentoo.org/wiki/MTP) — a protocol to allow the transfer of files to external devices.

## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) [http://forums.gentoo.org/viewtopic-p-7633366.html#7633366](http://forums.gentoo.org/viewtopic-p-7633366.html#7633366)
2. [↑](https://wiki.gentoo.org#cite_ref-2) [bug #527086](https://bugs.gentoo.org/show_bug.cgi?id=527086)
3. [↑](https://wiki.gentoo.org#cite_ref-3) libmtp will scan the usb bus and send messages to the log for all attached usb devices Dec 29 11:58:42 localhost kernel: usb 1-1.2: new full-speed USB device number 8 using ehci-pci Dec 29 11:58:42 localhost mtp-probe: checking bus 1, device 8: "/sys/devices/pci0000:00/0000:00:1d.7/usb1/1-1/1-1.2" Dec 29 11:58:42 localhost mtp-probe: bus: 1, device: 8 was not an MTP device Dec 29 11:59:51 localhost kernel: usb 1-5.1: new low-speed USB device number 10 using ehci-pci Dec 29 11:59:51 localhost kernel: input: Logitech Trackball as /devices/pci0000:00/0000:00:1d.7/usb1/1-5/1-5.1/1-5.1:1.0/0003:046D:C404.0007/input/input25 Dec 29 11:59:51 localhost kernel: hid-generic 0003:046D:C404.0007: input: USB HID v1.10 Mouse \[Logitech Trackball\] on usb-0000:00:1d.7-5.1/input0 Dec 29 11:59:51 localhost mtp-probe: checking bus 1, device 10: "/sys/devices/pci0000:00/0000:00:1d.7/usb1/1-5/1-5.1" Dec 29 11:59:51 localhost mtp-probe: bus: 1, device: 10 was not an MTP device Dec 29 11:59:51 localhost kernel: input: Logitech Logitech Illuminated Keyboard as /devices/pci0000:00/0000:00:1d.7/usb1/1-5/1-5.4/1-5.4:1.0/0003:046D:C318.0008/input/input26 Dec 29 11:59:51 localhost kernel: hid-generic 0003:046D:C318.0008: input: USB HID v1.11 Keyboard \[Logitech Logitech Illuminated Keyboard\] on usb-0000:00:1d.7-5.4/input0 Dec 29 11:59:51 localhost kernel: input: Logitech Logitech Illuminated Keyboard as /devices/pci0000:00/0000:00:1d.7/usb1/1-5/1-5.4/1-5.4:1.1/0003:046D:C318.0009/input/input27 Dec 29 11:59:51 localhost kernel: hid-generic 0003:046D:C318.0009: input: USB HID v1.11 Device \[Logitech Logitech Illuminated Keyboard\] on usb-0000:00:1d.7-5.4/input1 Dec 29 11:59:51 localhost mtp-probe: checking bus 1, device 11: "/sys/devices/pci0000:00/0000:00:1d.7/usb1/1-5/1-5.4" Dec 29 11:59:51 localhost mtp-probe: bus: 1, device: 11 was not an MTP device
