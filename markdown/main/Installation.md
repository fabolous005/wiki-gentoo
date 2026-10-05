<!-- source: https://wiki.gentoo.org/wiki/Installation | group: Gentoo Wiki (Main) | wiki-title: Installation -->
---
title: Installation
url: https://wiki.gentoo.org/wiki/Installation
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-01-06"
fingerprint: d0bbff8a5c8bc33c
license: CC BY-SA 4.0
---

# Installation

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This is an overview of the principles and practices of installing Gentoo on a running system.

The installation of a Gentoo Linux system is very different from how the most well-known, desktop-oriented operating systems usually work. Common OSs, such as Microsoft Windows, MacOS, and the more popular Linux distributions come with graphically guided installers (wizards) with which most users will be familiar, but installing Gentoo is markedly different.

Gentoo is a versatile distribution geared towards advanced users. For those starting out with Linux, or even with computers in general, Gentoo will likely require more effort to learn than so-called "beginner-friendly" distributions. For someone aiming to get the most out of the tools they use (for example, developers, hobbyists, and tinkerers) Gentoo is an excellent choice.

See the [FAQ](https://wiki.gentoo.org/wiki/FAQ) about [what makes Gentoo different](https://wiki.gentoo.org/wiki/FAQ#What_makes_Gentoo_different.3F) for an overview of Gentoo's architecture and capabilities.

## Principles

Gentoo is installed following a manual procedure. In order to prepare a bootable system, the user inputs and executes commands at the system [prompt](https://wiki.gentoo.org/wiki/Shell) following the guidelines laid out in the [Handbook](https://wiki.gentoo.org/wiki/Handbook). Installing Gentoo is thus somewhat more involved than for most other distributions, though for users used to the command line (or to manually installing other distributions) it is a rather simple and fast procedure.

Installing Gentoo for the first time is akin to following a tutorial. Do not be discouraged if it seems difficult in the beginning. Getting a Gentoo system up and running is a learning process that rewards the user with valuable insight into its workings, as well as those of other Linux (and Unix-like) distributions in general.

Gentoo is a powerful operating system. As with other highly capable tools, investing the time in learning how they work is sure to pay dividends down the line.

## Overview

A Gentoo installation starts with booting a complete Linux environment (with [shell](https://wiki.gentoo.org/wiki/Shell) access). There are several means to doing this, such as using the [Minimal Installation CD](https://wiki.gentoo.org/wiki/Bootable_media#Minimal_Installation_CD), Gentoo's [LiveGUI ISO](https://wiki.gentoo.org/wiki/Bootable_media#LiveGUI_ISO), or even another distribution's Live ISO or pre-existing installation.

After booting into this environment, installation files will need to be downloaded or copied to a persistent drive, thus creating a new directory hierarchy with all the tools needed and configured for a complete system. From here the user can reboot into the newly installed Gentoo base, and finish installing and configuring needed programs.

## The Gentoo Handbook

The [Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:Main_Page) provides the official method for installing the system, along with various options for tailoring the system to meet one's needs or wishes. Though it is *possible* to deviate from the Handbook, issues may arise since alternative means have not been tested. (This is especially true for whatever directives are presented as imperative.) If the handbook is not followed properly, the community may not be able to [help](https://wiki.gentoo.org/wiki/Support). Think of the Handbook as an actual installation program. Failing to comply with the directives of one is like failing to comply with the directives of the other.

The Handbook is edited and maintained by the [Handbook Project](https://wiki.gentoo.org/wiki/Project:Handbook) in close cooperation with the release engineering team and the various architecture teams and their maintainers.

## Tips

- Skim through the whole handbook to get an idea of what needs to be done.
- Consider using the binary [distribution kernel](https://wiki.gentoo.org/wiki/Kernel#Distribution_kernels) to get up and running more easily. This may be changed later on.
- Ask for help on [IRC](https://wiki.gentoo.org/wiki/IRC) if any issues are encountered (see also the [support](https://wiki.gentoo.org/wiki/Support) article).

## See also

- [Bootable media](https://wiki.gentoo.org/wiki/Bootable_media) — Gentoo offers **bootable media** that can be used to [install], maintain, or try out Gentoo Linux
- [Category:Installation](https://wiki.gentoo.org/wiki/Category:Installation) — Articles pertaining to the **installation** of Gentoo Linux.
- [Installation alternatives](https://wiki.gentoo.org/wiki/Installation_alternatives) — describes alternative installation methods, i.e. other than booting the LiveDVD in the optical drive.
- [Install Gentoo on a bootable USB stick](https://wiki.gentoo.org/wiki/Install_Gentoo_on_a_bootable_USB_stick) — describes how to install Gentoo onto a USB stick that can be booted on any computer.
- [LiveUSB](https://wiki.gentoo.org/wiki/LiveUSB) — explains how to create a *Gentoo LiveUSB* or, in other words, how to emulate a **x86** or **amd64** Gentoo LiveCD using a USB drive.
- [Quick Installation Checklist](https://wiki.gentoo.org/wiki/Quick_Installation_Checklist) — for *experienced* users who desire a quick, less detailed installation guide. It doubles as a checklist so essential installation steps are not forgotten.
- [Stage file](https://wiki.gentoo.org/wiki/Stage_file) — an archive that contains all the files to run a minimal Gentoo environment, typically to serve as a seed for a Gentoo installation
- [Upgrading Gentoo](https://wiki.gentoo.org/wiki/Upgrading_Gentoo) — explains how to **upgrade (update)** Gentoo, as well as how to proceed for a well maintained system.

### Unofficial instructions

Various community-driven installation instructions have been produced. Here are some:

- [Complete Handbook](https://wiki.gentoo.org/wiki/Complete_Handbook) — A community-driven effort to create a fully documented approach to Gentoo Linux.
- [Installing Gentoo on a ThinkPad X220](https://www.thinkwiki.org/wiki/Installing_Gentoo_on_a_ThinkPad_X220) — Instructions specific to this particular laptop.
