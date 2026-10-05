<!-- source: https://wiki.gentoo.org/wiki/QEMU/Guest/CLI_approach | group: Gentoo Wiki (Main) | wiki-title: QEMU/Guest/CLI approach -->
---
title: QEMU/Guest/CLI approach
url: https://wiki.gentoo.org/wiki/QEMU/Guest/CLI_approach
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-27"
fingerprint: "19b2ed94c92a384"
license: CC BY-SA 4.0
---

# QEMU/Guest/CLI approach

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article details the creation of a guest domain (virtual machine, VM), running inside a QEMU hypervisor, using tools found in [libvirt](https://packages.gentoo.org/packages/libvirt) package.

[CLI](https://wiki.gentoo.org/wiki/CLI) tools like virt-install, virt-xml-validate, xmlstarlet and virsh commands are detailed here, as well as in [Libvirt](https://wiki.gentoo.org/wiki/Libvirt).

## Installation

Creation of a **domain** entails the following stages for a new [domain](https://wiki.gentoo.org/wiki/Libvirt/domain):

- Firmware (BIOS/UEFI)
- [Bootloader](https://wiki.gentoo.org/wiki/Bootloader) Manager
- OS
- Network
- Passthru devices (optional)



### Initialize **domain** file

First step is to get an XML-format file as the new **domain** configuration file.

`host-platform-root#``virt-install --osinfo gentoo --cdrom ~/Downloads/livegui-amd64-20250216T164837Z.iso`
Using default --name gentoo-2
Using gentoo default --memory 512
Using gentoo default --disk size=5
Starting install...
Allocating 'gentoo-2.qcow2'                                                                                                       |    0 B  00:00:00 ... 
Creating domain...                                                                                                                |    0 B  00:00:00     
Running graphical console command: virt-viewer --connect qemu:///system --wait gentoo-2

Domain configuration file are stored in /etc/libvirt/qemu/gentoo-2.xml.

### Additional software

See [libvirt](https://wiki.gentoo.org/wiki/Libvirt) for installation of [virsh](https://wiki.gentoo.org/wiki/Virsh) and virt-xml-validate.

Guest Linux OS requires [sys-power/acpid](https://packages.gentoo.org/packages/sys-power/acpid) for proper handling of guest shutdown that are initiated by the host OS using libvirt.

XML editing requires [app-text/xmlstarlet](https://packages.gentoo.org/packages/app-text/xmlstarlet) for extreme ease of editing complex XML files in this page.

For custom UEFI, Optional OVMF firmware is in [app-emulation/virt-firmware](https://packages.gentoo.org/packages/app-emulation/virt-firmware).

User name qemu is required, defined by [acct-user/qemu](https://packages.gentoo.org/packages/acct-user/qemu) and evoked by [sys-emulator/qemu](https://packages.gentoo.org/packages/sys-emulator/qemu) package.

Group name qemu is required, defined by [acct-group/qemu](https://packages.gentoo.org/packages/acct-group/qemu) and evoked by [sys-emulator/qemu](https://packages.gentoo.org/packages/sys-emulator/qemu) package.

To connect to the SPICE server of QEMU, a GUI client like [net-misc/spice-gtk](https://packages.gentoo.org/packages/net-misc/spice-gtk) is required.

## Configuration

For command-line approach of domain file creation, there are two methods:

- virt-install (in [app-emulation/virt-manager](https://packages.gentoo.org/packages/app-emulation/virt-manager))
- xmlstarlet tool


The only difference between above methods is that ALL of its domain options must be predetermined and pre-planned before doing the one-time execution using virt-install tool.

The modification of options in the **domain** file can be done in any order using xmlstarlet tool.

Choose one of the [One-Step domain creation](https://wiki.gentoo.org/wiki/Libvirt/QEMU_guest#OneStep) section below or the [XML approach](https://wiki.gentoo.org/wiki/Libvirt/QEMU_guest#XMLapproach) section further down.



Perusing through all the options below for a single-line command to create a **domain**, using virt-install.

#### Name of domain

Decide on a name for this domain (**--name gentoo-2**).

#### Install Media

For installation from ISO image, either as image on hard drive or in CD/DVD drive, l
locate the path to your ISO boot image file (**--cdrom \<your\_Gentoo\_livecdrom.iso>**).

#### Memory

Determine how much guest virtual memory to have (**--memory 384** for 384 MiB).

#### Disk

Allocate how much disk space to set aside for this domain (**--disk size=2** for 2 GiB in default location).

#### UEFI

`host-platform$``virt-install ... --boot uefi=on ...`
#### BIOS

`host-platform$``virt-install ... --boot uefi=off ...`
or

`host-platform$``virt-install ... --boot firmware=bios ...`
To use the custom UEFI boot image file, add following option to **virt-install**:

#### VM type

Make it a fully-virtualized machine (**--hvm** or **-v**).

#### Install

Then execute:

`host-platform$````
virt-install  \
```
--name gentoo-2  \
   --memory 384  \
   --disk size=2 \
   --cdrom <your_Gentoo_cdrom.iso>  \
   --hvm  \

--boot uefi=on
The XML approach entails making individual choices in any order into the **domain** configuration file using the xmlstarlet tool

#### Domain name

To define the **domain** name, add the **\<name>** node to **\<domain>**, then add the name of this domain as a value to **name** element inside the **domain** node to get **\<domain>\<name>gentoo-2\</name>\</domain>** XML snippet:

`host-platform-root#``xmlstarlet ed -u '/domain/name' -v gentoo-2 /etc/libvirt/qemu/gentoo.xml` In the rare case that **\<name>\</name>** subnode is missing, execute:

`host-platform-root#``xmlstarlet ed -s '/domain' -type elem -n name -v gentoo-2 /etc/libvirt/qemu/gentoo.xml`
#### Firmware

Boot firmware is the very first piece of software to run after a domain's CPU has been powered on or after a reset.

BIOS and UEFI are examples of boot firmware.

Boot firmware are declared in the **firmware=** attribute of **os** subnode in the **domain** file.

#### UEFI

UEFI boot is the default.

UEFI firmware are in a separate EFI partition within the storage of its guest domain denoted by /efi/EFI directory.

UEFI stubs (modules) are also in /efi/EFI directory inside the guest domain.

To enable UEFI, add the **firmware=efi** attribute or replace the value of **firmware=** attribute with **efi** to the **domain/os** element to get the **\<domain>\<os firmware=efi>\</domain>** XML snippet:

`host-platform-root#``xmlstarlet -L ed -i domain/os -t attr -n firmware -v efi /etc/libvirt/qemu/gentoo.xml`
Alternatively, to revert at default UEFI, delete **firmware=\*** tag from **domain/os** subnode to :

`host-platform-root#``xmlstarlet -L ed -d '/domain/os/@firmware' /etc/libvirt/qemu/gentoo.xml`


##### Custom UEFI

Custom UEFI firmware and stub (module) image files are stored on the host platform in the following **loader** locations, in search order:

- /usr/share/qemu/firmware
- /etc/qemu/firmware
- $XDG\_CONFIG\_HOME/qemu/firmware

To use a custom UEFI loader image file(s), place the copy of the file(s) the **Libvirt** boot image directory.

Inside the guest domain filesystem, execute:

`guest-root#``cp my_ovmf_boot_image.fd /var/share/OVMF/`
Also place UEFI firmare blob goes into the guest domain's /efi/EFI subdirectories.

Inside the guest domain, use [**efibootmgr**](https://wiki.gentoo.org/wiki/Efibootmgr) to manage the loader entries for UEFI.

`guest-root#``efibootmgr -c -L "Gentoo" -l '\EFI\Gentoo\bzImage.efi'`
#### BIOS

To enable BIOS,

By overwriting an **efi** value or inserting with a **bios** value into the **firmware** attribute of **\<os>** element to get **\<domain>\<os firmware=bios>** XML snippet:

`host-platform-root#````
xmlstarlet -L ed  \
```
-i domain/os -t attr -n firmware -v bios  \
   /etc/libvirt/qemu/gentoo.xml



##### Custom BIOS

BIOS image files are located in /var/lib/libvirt/boot directory.  Custom UEFI are also found in /var/share/OVMF provided by [ovfm](https://packages.gentoo.org/packages/ovfm).

To use a custom boot file, place the copy of custom boot image file into the **Libvirt** boot image directory.

`host-platform-root#``cp my_bios_image.fd /var/lib/libvirt/boot/`
#### Bootloader Manager

In desktop/workstation, the [bootloader](https://wiki.gentoo.org/wiki/Bootloader) manager makes upgrade of operating systems easiest.

In embedded system, the [bootloader](https://wiki.gentoo.org/wiki/Bootloader) often goes straight to its kernel image.

Insert the **bootmenu** node into the **\<os>** node.

Insert the **enable='yes'** attribute into the **\<bootmenu>** element to obtain **\<domain>\<os>\<bootmenu enable='yes'/>** XML snippet:

`host-platform-root#````
xmlstarlet ed  \
```
-s //domain/os -t elem -n bootmenu \
   -a //domain/os/bootmenu -t attr -n enable -v yes  \
   /etc/libvirt/qemu/gentoo.xml

`host-platform$``virsh define gentoo-vm.xml`
Domain 'gentoo' defined from gentoo-vm.xml

#### Image file

Ideally, we want the following XML section /domain/drivers/disk/:

```
  <disk type='file' device='cdrom'>
      <driver name='qemu' type='raw'/>
      <source file='~/Downloads/livegui-amd64-20250216T164837Z.iso' index='1'/>
      <backingStore/>
      <target dev='sda' bus='sata'/>
      <readonly/>
      <alias name='sata0-0-0'/>
      <address type='drive' controller='0' bus='0' target='0' unit='0'/>
  </disk>
```


##### Insert CDROM disc

Add **\<source/>** subnode to **//domains/devices/disk** node:

`host-platform-root#``xmlstarlet ed -L -s "//domain/devices/disk[@device='cdrom']" -t elem -n source /etc/libvirt/qemu/gentoo.xml` To add ISO-9660 image file via CD drive:

`host-platform-root#``xmlstarlet ed -a "//domain/devices/disk[@device='cdrom']/source" -t attr -n file -v /dev/sg0 /etc/libvirt/qemu/gentoo.xml` To add ISO-9660 image from a file:

`host-platform-root#``xmlstarlet ed -a "//domain/devices/disk[@device='cdrom']/source" -t attr -n file -v ~/Downloads/some-livecd-iso9660.image /etc/libvirt/qemu/gentoo.xml` ### Validate **domain**

To validate the **domain** file, execute:

`host-platform-root#``virt-xml-validate /etc/libvirt/qemu/gentoo.xml`
/etc/libvirt/qemu/gentoo.xml validates

### List **domains** (virtual machines)

To list all registered **domains** (VMs), execute:

`host-platform$``virsh list --all`
Id   Name       State
---------------------------
 4    gentoo     running
 5    gentoo-2   running
 -    debian11   shut off

**Name** column shows the **domain** names.

To list all (default) active **domains**, execute:

`host-platform$``virsh list`
Id   Name       State
---------------------------
 4    gentoo     running
 5    gentoo-2   running

### Starting a **domain**

To start a **domain**, execute:

`host-platform$``virsh start <domain-name>`
To enabling autostart, execute:

`host-platform$``virsh autostart <domain-name>`
### Viewing the console of a **domain**

There are two different ways to view the console/display of a domain:

- thru virt-viewer over SPICE/TCP network protocol
- thru virt-manager viewport over UNIX socket



#### Console by virt-manager

See [Virt-manager/QEMU\_guest](https://wiki.gentoo.org/wiki/Virt-manager/QEMU_guest) for GUI-based viewing of VM's console.

#### Console by virt-viewer

To use the virt-viewer in starting VM from the shell, execute:

`host-platform$``virt-viewer --connect=qemu:///session --domain-name gentoo-2` To just display the window but not start the VM yet, execute:

`host-platform$``virt-viewer --connect=qemu:///session --wait --domain-name gentoo-2` ### Shutdown an active domain

Shutdown of a domain is done using one of the following:

- From the host platform:
  - In host shell
  - From host GUI
- From within the guest domain:
  - From the shell
  - From the window manager

From the host OS (outside the guest OS) CLI, the usage syntax to perform a **domain** (VM) shutdown is:

`host-platform$``virsh shutdown --help` ```
  NAME
    shutdown - gracefully shutdown a domain
  SYNOPSIS
    shutdown <domain> [--mode <string>]
  DESCRIPTION
    Run shutdown in the target domain.
  OPTIONS
    [--domain] <string>  domain name, id or uuid
    --mode <string>  shutdown mode: acpi|agent|initctl|signal|paravirt
```


#### Shutdown, from host shell

`host-platform$``virsh shutdown <domain>|<vm-id>|<uuid>` Hard shutdown, similar to pulling the power cord on a physical machine. This type of shutdown lets the machine abruptly interrupts any state that the operation system has been maintaining.

`host-platform$``virsh destroy <domain>|<vm-id>|<uuid>` 


#### Shutdown, from host GUI

In [virt-manager](https://wiki.gentoo.org/wiki/Virt-manager#VM_shutdown) main menu bar using **Virtual Machine -> Shutdown**



#### Shutdown, from guest shell

From the shell inside a guest domain

`guest$``shutdown -h -t 0`


#### Shutdown, from guest GUI

From the window manager inside a guest, Start -> Shutdown icon.

### Delete and destroy a domain

For removing a domain from the list of VMs maintained by libvirtd, its usage syntax is:

`host-platform$``virsh undefine --help````
  NAME
    undefine - undefine a domain
  SYNOPSIS
    undefine <domain> [--managed-save] [--storage <string>] [--remove-all-storage] [--delete-storage-volume-snapshots] [--wipe-storage] [--snapshots-metadata] [--checkpoints-metadata] [--nvram] [--keep-nvram] [--tpm] [--keep-tpm]
  DESCRIPTION
    Undefine an inactive domain, or convert persistent to transient.
  OPTIONS
    [--domain] <string>  domain name, id or uuid
    --managed-save   remove domain managed state file
    --storage <string>  remove associated storage volumes (comma separated list of targets or source paths) (see domblklist)
    --remove-all-storage  remove all associated storage volumes (use with caution)
    --delete-storage-volume-snapshots  delete snapshots associated with volume(s), requires --remove-all-storage (must be supported by storage driver)
    --wipe-storage   wipe data on the removed volumes
    --snapshots-metadata  remove all domain snapshot metadata (vm must be inactive)
    --checkpoints-metadata  remove all domain checkpoint metadata (vm must be inactive)
    --nvram          remove nvram file
    --keep-nvram     keep nvram file
    --tpm            remove TPM state
    --keep-tpm       keep TPM state
```
To completely remove a virtual machine from the list of domains, and and delete all images files related to this domain's storage(s), execute:

`host-platform$``virsh undefine --remove-all-storage <domain>` Domain 'gentoo-2' has been undefined
Volume 'vda'(/var/lib/libvirt/images/gentoo-2.qcow2) removed.



## Troubleshooting

### Debug logging

Enable detailed logging for a system instance of libvirtd:

`host-platform-root#``virt-admin -c virtqemud:///system daemon-log-outputs "3:journald 1:file:/var/log/libvirt/libvirtd.log"``host-platform-root#``virt-admin -c virtqemud:///system daemon-log-filters "3:remote 4:event 3:util.json 3:util.object 3:util.dbus 3:util.netlink 3:node_device 3:rpc 3:access 1:*"``host-platform-root#``virt-admin -c virtqemud:///system daemon-timeout --timeout 0`
These settings are temporary and are lost when the libvirt daemon is restarted or the host is rebooted.



## See also

- [Libvirt/Windows\_XP\_guest](https://wiki.gentoo.org/wiki/Libvirt/Windows_XP_guest) — creation and handling of Windows XP guest virtual machine on QEMU host
- [Libvirt/domain](https://wiki.gentoo.org/wiki/Libvirt/domain) — represents a virtualized guest instance, such as a virtual machine, container, or other supported environment.
- [QEMU/Linux guest](https://wiki.gentoo.org/wiki/QEMU/Linux_guest) — describes the setup of a Gentoo Linux guest in [QEMU](https://wiki.gentoo.org/wiki/QEMU) using Gentoo bootable media.
- [QEMU/Guest/Gentoo Linux](https://wiki.gentoo.org/wiki/QEMU/Guest/Gentoo_Linux) — supplemental to Gentoo [Handbook](https://wiki.gentoo.org/wiki/Handbook:Main_Page) reference guide
- [Virt-manager](https://wiki.gentoo.org/wiki/Virt-manager) — lightweight GUI application designed for managing virtual machines and containers via the [libvirt](https://wiki.gentoo.org/wiki/Libvirt) API.
- [Libvirt](https://wiki.gentoo.org/wiki/Libvirt) — a virtualization management toolkit

- [QEMU](https://wiki.gentoo.org/wiki/QEMU) — a generic, open-source hardware emulator and virtualization suite.
- [Libvirt/QEMU\_networking](https://wiki.gentoo.org/wiki/Libvirt/QEMU_networking) — details the setup of Gentoo networking by [Libvirt](https://wiki.gentoo.org/wiki/Libvirt) for use by guest containers and [QEMU](https://wiki.gentoo.org/wiki/QEMU)-based virtual machines.
- [QEMU/Guest/GUI approach](https://wiki.gentoo.org/wiki/QEMU/Guest/GUI_approach) — creation of a guest virtual machine (VM) running inside a QEMU hypervisor using just the virt-manager GUI tool.



## External resources

- [Libvirt Domain XML Format](https://libvirt.org/formatdomain.html) - Detailed description of a Domain XML format file.
- file:///usr/share/doc/libvirt-doc/ - Documentation for libvirt (via [app-emulation/libvirt](https://packages.gentoo.org/packages/app-emulation/libvirt)).
- [andreaskaris.github.io/blog](https://andreaskaris.github.io/blog/linux/libvirt-uefi-without-secureboot/) - Libvirt UEFI without SecureBoot
