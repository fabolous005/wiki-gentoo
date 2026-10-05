<!-- source: https://wiki.gentoo.org/wiki/QEMU/Guest/GUI_approach | group: Gentoo Wiki (Main) | wiki-title: QEMU/Guest/GUI approach -->
---
title: QEMU/Guest/GUI approach
url: https://wiki.gentoo.org/wiki/QEMU/Guest/GUI_approach
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-01"
fingerprint: "5c19ba3947b3cbc0"
license: CC BY-SA 4.0
---

# QEMU/Guest/GUI approach

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article details the creation of a guest virtual machine (VM) running inside a QEMU hypervisor using just the virt-manager GUI tool.



## Installation

For installation of desktop user interface for management of virtual machines and containers through the libvirt library, see [virt-manager](https://wiki.gentoo.org/wiki/Virt-manager).



### Additional software

See [libvirt](https://wiki.gentoo.org/wiki/Libvirt) for installation of [virsh](https://wiki.gentoo.org/wiki/Virsh) and virt-xml-validate.

XML editing requires [app-text/xmlstarlet](https://packages.gentoo.org/packages/app-text/xmlstarlet) for extreme ease of editing complex XML files in this page.

For custom UEFI, Optional OVMF firmware is in [app-emulation/virt-firmware](https://packages.gentoo.org/packages/app-emulation/virt-firmware).

User name qemu is required, defined by [acct-user/qemu](https://packages.gentoo.org/packages/acct-user/qemu) and evoked by [sys-emulator/qemu](https://packages.gentoo.org/packages/sys-emulator/qemu) package.

Group name qemu is required, defined by [acct-group/qemu](https://packages.gentoo.org/packages/acct-group/qemu) and evoked by [sys-emulator/qemu](https://packages.gentoo.org/packages/sys-emulator/qemu) package.

To connect to the SPICE server of QEMU, a GUI client like [net-misc/spice-gtk](https://packages.gentoo.org/packages/net-misc/spice-gtk) is required.

Guest Linux OS requires [sys-power/acpid](https://packages.gentoo.org/packages/sys-power/acpid) for proper handling of guest shutdown that are initiated by the host OS using libvirt.

## Configuration

Creation of a **domain** entails the following stages for a new [domain](https://wiki.gentoo.org/wiki/Libvirt/domain):

- Firmware (BIOS/UEFI)
- [Bootloader](https://wiki.gentoo.org/wiki/Bootloader) Manager
- OS
- Network
- Passthru devices (optional)



### Creation by virt-manager

To use the GUI approach to create a virtual machine, start the Virtual Machine Management application, **[virt-manager](https://wiki.gentoo.org/wiki/Virt-manager)**.

`user $``virt-manager`
![virt-manager main window](https://wiki.gentoo.org/images/thumb/c/c1/Virt-manager-4.1-main-window.png/600px-Virt-manager-4.1-main-window.png)




### Create a New VM

Hover the mouse over the button showing a console icon with shiny star tag (or use \`File\`->\`New Virtual Machine\` from menu bar:

![VirtManager - New Virtual Machine](https://wiki.gentoo.org/images/3/35/Virt-manager-4.1-button-icon-new.png)




### How to Install

A new dialog appears that is titled "New VM" and highlighted "Create a new virtual machine" "Step 1 of 5".

![virt-manager new ISO image](https://wiki.gentoo.org/images/7/74/Virt-manager-4.1-new-VM-ISO.png)


Select the radio button to "Local install media".

To advance to the next step, press the "Forward" button.



### Choose Image Media

"Choose ISO or CDROM image media" "Step 2 of 5" appears.

Hit the "Browse" button and find your downloaded image file: Gentoo, we hope, but any image media having this ISO 9660 CD-ROM filesystem data (DOS/MBR boot sector) will do.

![Step 2 - ISO CDROM](https://wiki.gentoo.org/images/thumb/e/e0/Virt-manager-4.1-new-VM-step2-ISO-CDROM.png/600px-Virt-manager-4.1-new-VM-step2-ISO-CDROM.png)


Select the image file.

![Step 2 - ISO CDROM](https://wiki.gentoo.org/images/thumb/2/21/Virt-manager-4.1-new-VM-step2.2-ISO-CDROM.png/600px-Virt-manager-4.1-new-VM-step2.2-ISO-CDROM.png)


Click on the "Choose Volume" button.

Its "Locate ISO media volume" file dialog box disappears and returns you back to the "New VM" dialog box.

Make sure that "Automatically detect from installation media / source" checkbox is DISABLED.

In the textbox titled "Choose the operating system you are installing:", enter in \`gentoo\` and the popup combo box appears. Mouse-click on "Gentoo Linux (gentoo)"

![Step 2 - ISO CDROM](https://wiki.gentoo.org/images/5/51/Virt-manager-4.1-new-VM-step2.3-ISO-CDROM.png)


Press "Forward" button.



### Memory and CPUs

Selecting memory and CPU is basically rocket science. Pick them as you need them. Memory can be adjusted at next run; storage size, not as easily.



#### Memory

In Step 3 of 5, select the amount of memory that the operating system of new virtual machine desires.

Select the number of CPUs to make available to the new virtual machine.

![Step 3 - Create Virtual Machine](https://wiki.gentoo.org/images/b/b5/Virt-manager-4.1-new-VM-step3-create-VM.png)




#### Storage

In the "New VM" dialog box, "Create a new virtual machine" and "Step 4 of 5" appears.

In the "Create a disk image for the virtual machine" textbox, increase it to the desired storage size, in gigabytes.

![Step 4 - Create Storage](https://wiki.gentoo.org/images/6/66/Virt-manager-4.1-new-VM-step4-create-storage.png)


Press "Forward" button to continue to the next step.



### Begin Install

Step 5 of 5 window appears.

Expand the "Network Selection".

![Step 5 - Begin Install](https://wiki.gentoo.org/images/f/fb/Virt-manager-4.1-new-VM-step5-begin-install-customize.png)


Enable the checkbox to "Customize configuration before install".

![Step 5a - Begin Install](https://wiki.gentoo.org/images/f/f3/Virt-manager-4.1-new-VM-step5b-begin-install.png)


Ensure that "Virtual Network 'default': NAT" is already selected as a minimum.

Press the "Finish" button.



### Tweaking VM

With the basic configuration largely done, you can then perform customization of this virtual machine.

![Step 6 - Virtual Machine Configuration](https://wiki.gentoo.org/images/thumb/5/55/Virt-manager-4.1-step6-VM-configuration.png/600px-Virt-manager-4.1-step6-VM-configuration.png)


In the "Description:" textbox, add in your comment about this virtual machine.

![Step 7 - Configuration Begin](https://wiki.gentoo.org/images/thumb/0/00/Virt-manager-4.1-step7-VM-configuration-begin.png/600px-Virt-manager-4.1-step7-VM-configuration-begin.png)


At the top menu bar, press "Begin installation" button.



### Boot Up Result

After BIOS and Linux kernel bootup, you should get a virtual machine up and running.

![Last Step - Boot-up Result - Live Gentoo](https://wiki.gentoo.org/images/thumb/4/47/Virt-manager-4.1-step7-after-bootup.png/600px-Virt-manager-4.1-step7-after-bootup.png)




## Usages

### VM viewing

To view a virtual machine from virt-manager, execute:

`user $``virt-manager`
A main window titled "Virtual Machine Manager" appears.

![virt-manager main window](https://wiki.gentoo.org/images/thumb/c/c1/Virt-manager-4.1-main-window.png/600px-Virt-manager-4.1-main-window.png)


A list of domains appears at the bottom of main window.

The domain to view may be under the **QEMU/KVM** subcategory or under the **LXC** subcategory. Expand the **QEMU/KVM** category, if needed.

Select row of the domain that you wish to view in the main body window.

Start the virtual machine in one of two ways:

- Icons menu bar,  menu option![virt-manager open icon](https://wiki.gentoo.org/images/4/44/Virt-manager-icon-open5.png)
- Mouse-hover over domain row in the list of domain groupbox, select **Open** option.

## Removal

To remove a domain, go to the main menu bar of virt-manager, and select the `Edit` menu option then `Delete` submenu option.

A new dialog titled "Delete Virtual Machine" appears, click on `Delete` button to delete the selected domain (virtual machine).

## See also

- [Virtualization](https://wiki.gentoo.org/wiki/Virtualization) — the concept and technique that permits running software in an environment separate from a computer operating system.
- [QEMU](https://wiki.gentoo.org/wiki/QEMU) — a generic, open-source hardware emulator and virtualization suite.
- [QEMU/Front-ends](https://wiki.gentoo.org/wiki/QEMU/Front-ends) — provide graphical, terminal, web-based, or command-line interfaces for configuring, managing, or accessing QEMU virtual machines.

- [Libvirt](https://wiki.gentoo.org/wiki/Libvirt) — a virtualization management toolkit
- [Libvirt/QEMU\_networking](https://wiki.gentoo.org/wiki/Libvirt/QEMU_networking) — details the setup of Gentoo networking by [Libvirt](https://wiki.gentoo.org/wiki/Libvirt) for use by guest containers and [QEMU](https://wiki.gentoo.org/wiki/QEMU)-based virtual machines.
- [Libvirt/QEMU\_guest](https://wiki.gentoo.org/wiki/Libvirt/QEMU_guest) — creation of a guest domain (virtual machine, VM), running inside a QEMU hypervisor, using tools found in [libvirt](https://packages.gentoo.org/packages/libvirt) package.

- [Virt-manager](https://wiki.gentoo.org/wiki/Virt-manager) — lightweight GUI application designed for managing virtual machines and containers via the [libvirt](https://wiki.gentoo.org/wiki/Libvirt) API.

- [QEMU/Linux guest](https://wiki.gentoo.org/wiki/QEMU/Linux_guest) — describes the setup of a Gentoo Linux guest in [QEMU](https://wiki.gentoo.org/wiki/QEMU) using Gentoo bootable media.
