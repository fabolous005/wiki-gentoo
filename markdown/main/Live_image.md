<!-- source: https://wiki.gentoo.org/wiki/Live_image | group: Gentoo Wiki (Main) | wiki-title: Live image -->
---
title: Live image
url: https://wiki.gentoo.org/wiki/Live_image
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-17"
fingerprint: "908fff781d638994"
license: CC BY-SA 4.0
---

# Live image

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

A **live image** provides an operating system (OS) environment contained within a file that can be used to [boot](https://en.wikipedia.org/wiki/Booting) a system. Live images are typically written to local media for booting into a *live environment*. Such an environment is often non-persistent, but can sometimes be used to install a *permanent* OS to non-volatile storage.

Live images are used to create a [live environment](https://wiki.gentoo.org/wiki/Live_environment) from which to install Gentoo to persistent storage. Live images may also be used to maintain a Gentoo-based operating system, particularly in the case of issues resulting in a non-bootable system.

Gentoo offers the *LiveGUI USB Image*, the *Minimal Installation CD*, and an *Admin CD*.

A live image can also be called a *LiveUSB* image, or *live operating system* image. Historically, a live image has been referred to as a "LiveCD", "LiveDVD", "ISO file", "LiveUSB GUI", etc.

The [Gentoo release engineering project](https://wiki.gentoo.org/wiki/Project:RelEng) produces live images for installation of or maintenance to Gentoo-based operating systems.

Go to the [bootable media](https://wiki.gentoo.org/wiki/Bootable_media) article to see what live images are offered by Gentoo for download.

Live images are used for a variety of useful purposes such as:

- Installation of a Linux or other operating system
- Demonstrating features prior to installation
- System rescue
  - Failed disk drive hardware
  - Failed or corrupted file system
  - Corrupted or missing kernel binary or modules preventing normal use of the system. In particular when missing a network interface driver or a filesystem required to mount rootfs
- Digital forensics
- Development
  - Kernel driver development
- Portable installation
- Accessing/controlling physical hardware resources with an unlocked bootloader *without* needing special credentials for login :)
- Testing
  - Hardware testing
  - Software testing

## Format

Live images are usually [.iso](https://en.wikipedia.org/wiki/ISO_9660) image files, using either [El Torito](https://en.wikipedia.org/wiki/ISO_9660#El_Torito) or [Joliet](https://en.wikipedia.org/wiki/ISO_9660#Joliet) extensions.

Gentoo live images are hybrid, meaning the image can either be burned to optical media or directly written to a USB drive using a data dump tool such as [dd](https://wiki.gentoo.org/wiki/Dd).

- [Bootable media](https://wiki.gentoo.org/wiki/Bootable_media) — Gentoo offers **bootable media** that can be used to [install](https://wiki.gentoo.org/wiki/Installation), maintain, or try out Gentoo Linux
- [Installation](https://wiki.gentoo.org/wiki/Installation) — an overview of the principles and practices of installing Gentoo on a running system.
- [Live environment](https://wiki.gentoo.org/wiki/Live_environment) — a term that describes an ephemeral or disposable operating system environment, typically created from a [live image].
- [LiveUSB](https://wiki.gentoo.org/wiki/LiveUSB) — explains how to create a *Gentoo LiveUSB* or, in other words, how to emulate a **x86** or **amd64** Gentoo LiveCD using a USB drive.
- [Stage file](https://wiki.gentoo.org/wiki/Stage_file) — an archive that contains all the files to run a minimal Gentoo environment, typically to serve as a seed for a Gentoo installation

## External resources

- [Downloads – Gentoo Linux](https://www.gentoo.org/downloads/) -- the download page for bootable media and stage files.
