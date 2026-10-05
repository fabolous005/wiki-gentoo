<!-- source: https://wiki.gentoo.org/wiki/QEMU/Front-ends | group: Gentoo Wiki (Main) | wiki-title: QEMU/Front-ends -->
---
title: QEMU/Front-ends
url: https://wiki.gentoo.org/wiki/QEMU/Front-ends
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-30"
fingerprint: "4e9999e44b91eb60"
license: CC BY-SA 4.0
---

# QEMU/Front-ends

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

[QEMU](https://wiki.gentoo.org/wiki/QEMU) front-ends provide graphical, terminal, web-based, or command-line interfaces for configuring, managing, or accessing QEMU virtual machines.

Many VM management tools use [libvirt](https://wiki.gentoo.org/wiki/Libvirt) as an abstraction layer between the front-end and QEMU.

Various CLI-based tools to work with QEMU.

| Name | Package | Homepage | Description | 
|---|---|---|---|
| guestfish | - | [https://libguestfs.org/](https://libguestfs.org/) | Tools for accessing and modifying virtual machine disk images. | 
| qemu-init | - | [https://github.com/mm1ke/qemu-init](https://github.com/mm1ke/qemu-init) | A pretty comprehensive qemu-system-\<arch> front-end providing start/stop/snapshot functionality. | 
| virt-clone | [app-emulation/virt-manager](https://packages.gentoo.org/packages/app-emulation/virt-manager) | [https://virt-manager.org/](https://virt-manager.org/) | Duplicate a virtual machine, changing all the unique host side configuration like MAC address, name, etc. | 
| virt-lightning | - | [https://github.com/virt-lightning/virt-lightning](https://github.com/virt-lightning/virt-lightning) | Spawn cloud instances using libvirt. | 
| [virsh](https://wiki.gentoo.org/wiki/Virsh) | [app-emulation/libvirt](https://packages.gentoo.org/packages/app-emulation/libvirt) | [https://www.libvirt.org/](https://www.libvirt.org/) | CLI to manipulate virtual machines. | 

Terminal user interfaces (TUIs) for QEMU include:

| Name | Package | Homepage | Description | 
|---|---|---|---|
| nEMU | [app-emulation/nemu](https://packages.gentoo.org/packages/app-emulation/nemu) | [https://github.com/nemuTUI/nemu](https://github.com/nemuTUI/nemu) | ncurses UI for QEMU. | 

Graphical user interfaces (GUIs) for QEMU include:

### GUI VM Management

| Name | Package | Homepage | Description | 
|---|---|---|---|
| [GNOME Boxes](https://wiki.gentoo.org/wiki/Boxes) | [gnome-extra/gnome-boxes](https://packages.gentoo.org/packages/gnome-extra/gnome-boxes) | [https://wiki.gnome.org/Apps/Boxes](https://wiki.gnome.org/Apps/Boxes) | Simple GNOME graphical interface for creating and managing local and remote virtual machines, using `libvirt/QEMU` library. | 
| [virt-manager](https://wiki.gentoo.org/wiki/Virt-manager) | [app-emulation/virt-manager](https://packages.gentoo.org/packages/app-emulation/virt-manager) | [https://virt-manager.org](https://virt-manager.org) | A graphical tool for administering virtual machines. | 
| qt-virt-manager | [qt-virt-manager::mva](https://gpo.zugaina.org/Overlays/mva/qt-virt-manager) | [https://f1ash.github.io/qt-virt-manager/](https://f1ash.github.io/qt-virt-manager/) | A graphical user interface for libvirt written in Qt5. | 
| Karton | - | [Github/keoi1](https://github.com/kenoi1/Karton) | A Libvirt-based Virtual Machine Manager for KDE. (In-development, Google Summer 2025). | 
| Cockpit | [app-admin/cockpit-machines overlay](https://gpo.zugaina.org/app-admin/cockpit-machines/USE#ptabs) | [https://cockpit-project.org/](https://cockpit-project.org/) | Cockpit is a web-based graphical interface for servers, intended for everyone | 

### Web

| qemu-web-desktop | - | [qemu-web-desktop](https://gitlab.com/soleil-data-treatment/soleil-software-projects/qemu-web-desktop) | A web service that launches remote desktop virtual machines and displays them in your browser. | 

[virt-manager](https://wiki.gentoo.org/wiki/Virt-manager) is a commonly used GUI for administering QEMU/KVM virtual machines through [libvirt](https://wiki.gentoo.org/wiki/Libvirt).

- [QEMU](https://wiki.gentoo.org/wiki/QEMU) — a generic, open-source hardware emulator and virtualization suite.
- [QEMU/Linux guest](https://wiki.gentoo.org/wiki/QEMU/Linux_guest) — describes the setup of a Gentoo Linux guest in [QEMU](https://wiki.gentoo.org/wiki/QEMU) using Gentoo bootable media.

- [libvirt](https://wiki.gentoo.org/wiki/Libvirt) — a virtualization management toolkit
- [libvirt/QEMU guest](https://wiki.gentoo.org/wiki/Libvirt/QEMU_guest) — creation of a guest domain (virtual machine, VM), running inside a QEMU hypervisor, using tools found in [libvirt](https://packages.gentoo.org/packages/libvirt) package.
- [libvirt/QEMU networking](https://wiki.gentoo.org/wiki/Libvirt/QEMU_networking) — details the setup of Gentoo networking by [Libvirt](https://wiki.gentoo.org/wiki/Libvirt) for use by guest containers and [QEMU](https://wiki.gentoo.org/wiki/QEMU)-based virtual machines.

- [virt-manager](https://wiki.gentoo.org/wiki/Virt-manager) — lightweight GUI application designed for managing virtual machines and containers via the [libvirt](https://wiki.gentoo.org/wiki/Libvirt) API.
- [virt-manager/QEMU guest](https://wiki.gentoo.org/wiki/Virt-manager/QEMU_guest) — creation of a guest virtual machine (VM) running inside a QEMU hypervisor using just the virt-manager GUI tool.

- [Virtualization](https://wiki.gentoo.org/wiki/Virtualization) — the concept and technique that permits running software in an environment separate from a computer operating system.
