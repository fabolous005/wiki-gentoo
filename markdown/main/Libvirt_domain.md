<!-- source: https://wiki.gentoo.org/wiki/Libvirt/domain | group: Gentoo Wiki (Main) | wiki-title: Libvirt/domain -->
---
title: libvirt/domain
url: https://wiki.gentoo.org/wiki/Libvirt/domain
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-01"
fingerprint: df911d510c523b85
license: CC BY-SA 4.0
---

# libvirt/domain

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**domain** in [Libvirt](https://wiki.gentoo.org/wiki/Libvirt) represents a virtualized guest instance, such as a virtual machine, container, or other supported environment.

A Libvirt domain is the core abstraction for guests, which are defined and managed through libvirt APIs or tools such as [virsh](https://wiki.gentoo.org/wiki/Virsh) and [Virt-manager](https://wiki.gentoo.org/wiki/Virt-manager).

[Domains] are described using libvirt’s XML configuration format, specifying resources such as CPU, memory, storage devices, and network interfaces. This article also covers common lifecycle operations (start, suspend, shutdown, migration, and deletion) and provides examples of XML definitions and related management commands.

## Terminology

To clarify all these Libvirt-specific terms related to [Virtualization](https://wiki.gentoo.org/wiki/Virtualization), we have:

- **domain** → the central object (abstraction for a guest).
- **guest** → the operating system inside the domain.
- **host** → the real machine managing everything.
- **instance** → a concrete occurrence of a domain (defined and optionally running).


Then we have lifecycle of an **instance** (or guest instance):

- shut off (defined but not running)
- running
- paused
- shutting down
- crashed

## Available software

Tools that exist to interact, maintain, and configure a **domain** file:

- [virt-manager](https://wiki.gentoo.org/wiki/Virt-manager), creates and edits
- [virt-install](https://wiki.gentoo.org/wiki/Virt-install), creates and edits
- [virt-xml](https://wiki.gentoo.org/index.php?title=Virt-xml&action=edit&redlink=1), edits
- [virsh](https://wiki.gentoo.org/wiki/Virsh), virtual machine shell for Libvirt **domain**.
  - [virsh edit](https://wiki.gentoo.org/wiki/Virsh#edit), edits
  - [virsh clone](https://wiki.gentoo.org/wiki/Virsh#clone), creates from another but existing domain file.
  - [virsh create](https://wiki.gentoo.org/wiki/Virsh#create), takes the current domain, and adds to Libvirt domain list.
  - [virsh destroy](https://wiki.gentoo.org/wiki/Virsh#destroy), removes from Libvirt domain list, and optionally deletes its image files.
  - [virsh start](https://wiki.gentoo.org/wiki/Virsh#start), starts the domain (virtual machine).
  - [virsh dumpxml](https://wiki.gentoo.org/index.php?title=Virsh_dumpxml&action=edit&redlink=1), creates from an active VM

## Domain file

The format of a **domain** configuration file is XML.

File name of the **domain** configuration file is user-definable and accepted by UNIX-like filesystem.

File type of the **domain** configuration file is .xml.

### Domain location

The **domain** file is stored in one of the following directories:

1. System mode: /etc/libvirt/qemu/, /var/lib/libvirt/qemu
2. User mode: $HOME/.config/libvirt/qemu/


Also during opening of a **domain**, the order of search is listed above.

### Example

[Libvirt/QEMU guest](https://wiki.gentoo.org/wiki/Libvirt/QEMU_guest), [virt-manager](https://wiki.gentoo.org/wiki/Virt-manager), or virt-install creates a **domain** file.

The virt-install is done by executing:

`host-root#``virt-install --osinfo gentoo --cdrom ~/Downloads/livegui-amd64-20250216T164837Z.iso`
Using default --name gentoo-2
Using gentoo default --memory 512
Using gentoo default --disk size=5
Starting install...
Allocating 'gentoo-2.qcow2'                                                                                                       |    0 B  00:00:00 ... 
Creating domain...                                                                                                                |    0 B  00:00:00     
Running graphical console command: virt-viewer --connect qemu:///system --wait gentoo-2

to create this default **domain** configuration file.

## Lifecycle of **domain**

For overview relation graph of tools that work with the **domain** file, see below:

![Libvirt domain XML-format file](https://wiki.gentoo.org/images/thumb/3/38/Libvirt-domain-xml.png/600px-Libvirt-domain-xml.png)



For more **domain** interactions, see **Libvirt Virtual Machine Lifecycle**<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.

#### File format

The default domain XML file is:

`user $``cat /etc/libvirt/qemu/gentoo-vm.xml````
<!--
WARNING: THIS IS AN AUTO-GENERATED FILE. CHANGES TO IT ARE LIKELY TO BE
OVERWRITTEN AND LOST. Changes to this xml configuration should be made using:
  virsh edit gentoo-2
or other application using the libvirt API.
-->
<domain type='kvm'>
  <name>gentoo-2</name>
  <uuid>e171ac3a-17a6-46ab-94a3-ba5ef25d4427</uuid>
  <metadata>
    <libosinfo:libosinfo xmlns:libosinfo="http://libosinfo.org/xmlns/libvirt/domain/1.0">
      <libosinfo:os id="http://gentoo.org/gentoo/rolling"/>
    </libosinfo:libosinfo>
  </metadata>
  <memory unit='KiB'>524288</memory>
  <currentMemory unit='KiB'>524288</currentMemory>
  <vcpu placement='static'>1</vcpu>
  <os>
    <type arch='x86_64' machine='pc-q35-7.2'>hvm</type>
    <boot dev='hd'/>
  </os>
  <features>
    <acpi/>
    <apic/>
    <vmport state='off'/>
  </features>
  <cpu mode='host-passthrough' check='none' migratable='on'/>
  <clock offset='utc'>
    <timer name='rtc' tickpolicy='catchup'/>
    <timer name='pit' tickpolicy='delay'/>
    <timer name='hpet' present='no'/>
  </clock>
  <on_poweroff>destroy</on_poweroff>
  <on_reboot>restart</on_reboot>
  <on_crash>destroy</on_crash>
  <pm>
    <suspend-to-mem enabled='no'/>
    <suspend-to-disk enabled='no'/>
  </pm>
  <devices>
    <emulator>/usr/bin/qemu-system-x86_64</emulator>
    <disk type='file' device='disk'>
      <driver name='qemu' type='qcow2' discard='unmap'/>
      <source file='/var/lib/libvirt/images/gentoo-2.qcow2'/>
      <target dev='vda' bus='virtio'/>
      <address type='pci' domain='0x0000' bus='0x04' slot='0x00' function='0x0'/>
    </disk>
    <disk type='file' device='cdrom'>
      <driver name='qemu' type='raw'/>
      <target dev='sda' bus='sata'/>
      <readonly/>
      <address type='drive' controller='0' bus='0' target='0' unit='0'/>
    </disk>
    <controller type='usb' index='0' model='qemu-xhci' ports='15'>
      <address type='pci' domain='0x0000' bus='0x02' slot='0x00' function='0x0'/>
    </controller>
    <controller type='pci' index='0' model='pcie-root'/>
    <controller type='pci' index='1' model='pcie-root-port'>
      <model name='pcie-root-port'/>
      <target chassis='1' port='0x10'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x02' function='0x0' multifunction='on'/>
    </controller>
    <controller type='pci' index='2' model='pcie-root-port'>
      <model name='pcie-root-port'/>
      <target chassis='2' port='0x11'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x02' function='0x1'/>
    </controller>
    <controller type='pci' index='3' model='pcie-root-port'>
      <model name='pcie-root-port'/>
      <target chassis='3' port='0x12'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x02' function='0x2'/>
    </controller>
    <controller type='pci' index='4' model='pcie-root-port'>
      <model name='pcie-root-port'/>
      <target chassis='4' port='0x13'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x02' function='0x3'/>
    </controller>
    <controller type='pci' index='5' model='pcie-root-port'>
      <model name='pcie-root-port'/>
      <target chassis='5' port='0x14'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x02' function='0x4'/>
    </controller>
    <controller type='pci' index='6' model='pcie-root-port'>
      <model name='pcie-root-port'/>
      <target chassis='6' port='0x15'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x02' function='0x5'/>
    </controller>
    <controller type='pci' index='7' model='pcie-root-port'>
      <model name='pcie-root-port'/>
      <target chassis='7' port='0x16'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x02' function='0x6'/>
    </controller>
    <controller type='pci' index='8' model='pcie-root-port'>
      <model name='pcie-root-port'/>
      <target chassis='8' port='0x17'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x02' function='0x7'/>
    </controller>
    <controller type='pci' index='9' model='pcie-root-port'>
      <model name='pcie-root-port'/>
      <target chassis='9' port='0x18'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x03' function='0x0' multifunction='on'/>
    </controller>
    <controller type='pci' index='10' model='pcie-root-port'>
      <model name='pcie-root-port'/>
      <target chassis='10' port='0x19'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x03' function='0x1'/>
    </controller>
    <controller type='pci' index='11' model='pcie-root-port'>
      <model name='pcie-root-port'/>
      <target chassis='11' port='0x1a'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x03' function='0x2'/>
    </controller>
    <controller type='pci' index='12' model='pcie-root-port'>
      <model name='pcie-root-port'/>
      <target chassis='12' port='0x1b'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x03' function='0x3'/>
    </controller>
    <controller type='pci' index='13' model='pcie-root-port'>
      <model name='pcie-root-port'/>
      <target chassis='13' port='0x1c'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x03' function='0x4'/>
    </controller>
    <controller type='pci' index='14' model='pcie-root-port'>
      <model name='pcie-root-port'/>
      <target chassis='14' port='0x1d'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x03' function='0x5'/>
    </controller>
    <controller type='sata' index='0'>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x1f' function='0x2'/>
    </controller>
    <controller type='virtio-serial' index='0'>
      <address type='pci' domain='0x0000' bus='0x03' slot='0x00' function='0x0'/>
    </controller>
    <interface type='network'>
      <mac address='52:54:00:87:e4:d0'/>
      <source network='default'/>
      <model type='virtio'/>
      <address type='pci' domain='0x0000' bus='0x01' slot='0x00' function='0x0'/>
    </interface>
    <serial type='pty'>
      <target type='isa-serial' port='0'>
        <model name='isa-serial'/>
      </target>
    </serial>
    <console type='pty'>
      <target type='serial' port='0'/>
    </console>
    <channel type='unix'>
      <target type='virtio' name='org.qemu.guest_agent.0'/>
      <address type='virtio-serial' controller='0' bus='0' port='1'/>
    </channel>
    <channel type='spicevmc'>
      <target type='virtio' name='com.redhat.spice.0'/>
      <address type='virtio-serial' controller='0' bus='0' port='2'/>
    </channel>
    <input type='tablet' bus='usb'>
      <address type='usb' bus='0' port='1'/>
    </input>
    <input type='mouse' bus='ps2'/>
    <input type='keyboard' bus='ps2'/>
    <graphics type='spice' autoport='yes'>
      <listen type='address'/>
      <image compression='off'/>
    </graphics>
    <sound model='ich9'>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x1b' function='0x0'/>
    </sound>
    <audio id='1' type='spice'/>
    <video>
      <model type='qxl' ram='65536' vram='65536' vgamem='16384' heads='1' primary='yes'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x01' function='0x0'/>
    </video>
    <redirdev bus='usb' type='spicevmc'>
      <address type='usb' bus='0' port='2'/>
    </redirdev>
    <redirdev bus='usb' type='spicevmc'>
      <address type='usb' bus='0' port='3'/>
    </redirdev>
    <memballoon model='virtio'>
      <address type='pci' domain='0x0000' bus='0x05' slot='0x00' function='0x0'/>
    </memballoon>
    <rng model='virtio'>
      <backend model='random'>/dev/urandom</backend>
      <address type='pci' domain='0x0000' bus='0x06' slot='0x00' function='0x0'/>
    </rng>
  </devices>
</domain>
```


## Usage

### Validate **domain**

To validate the **domain** file, execute:

`host-root#``virt-xml-validate /etc/libvirt/qemu/gentoo.xml`
/etc/libvirt/qemu/gentoo.xml validates

### List **domains** (virtual machines)

To list all registered **domains** (VMs), execute:

`host-root#``virsh list --all`
Id   Name       State
---------------------------
 4    gentoo     running
 5    gentoo-2   running
 -    debian11   shut off

**Name** column shows the **domain** names.

To list all (default) active **domains**, execute:

`host-root#``virsh list`
Id   Name       State
---------------------------
 4    gentoo     running
 5    gentoo-2   running

### Starting a **domain**

To start a **domain**, execute:

`host-root#``virsh start <domain-name>`
To enabling autostart, execute:

`host-root#``virsh autostart <domain-name>`
### Viewing the console of a **domain**

There are two different ways to view the console/display of a domain:

- thru virt-viewer over SPICE/TCP network protocol
- thru virt-manager viewport over UNIX socket



#### Console by virt-manager

See [virt-manager](https://wiki.gentoo.org/wiki/Virt-manager#Starting_VM) for GUI-based viewing of VM's console.

#### Console by virt-viewer

To use the virt-viewer in starting VM from the shell, execute:

`host$``virt-viewer --connect=qemu:///session --domain-name gentoo-2` To just display the window but not start the VM yet, execute:

`host$``virt-viewer --connect=qemu:///session --wait --domain-name gentoo-2` ### Shutdown an active domain

Shutdown of a domain is done using one of the following:

- From the host:
  - In host shell, execute virsh shutdown
  - In virt-manager main menu bar using **Virtual Machine -> Shutdown**
- From within the guest domain:
  - From the shell, shutdown -h -t 0
  - From the window manager, Start -> Shutdown icon

From the host OS (outside the guest OS) CLI, the usage syntax to perform a **domain** (VM) shutdown is:

`host-root#``virsh shutdown --help` ```
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
`host-root#``virsh shutdown <domain>|<vm-id>|<uuid>` Hard shutdown, similar to pulling the power cord on a physical machine. This type of shutdown lets the machine abruptly interrupts any state that the operation system has been maintaining.

`host-root#``virsh destroy <domain>|<vm-id>|<uuid>` ### Delete and destroy a domain

For removing a domain from the list of VMs maintained by libvirtd, its usage syntax is:

`host-root#``virsh undefine --help````
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

`host-root#``virsh undefine --remove-all-storage <domain>` Domain 'gentoo-2' has been undefined
Volume 'vda'(/var/lib/libvirt/images/gentoo-2.qcow2) removed.

## See also

- [QEMU](https://wiki.gentoo.org/wiki/QEMU) — a generic, open-source hardware emulator and virtualization suite.
- [Libvirt](https://wiki.gentoo.org/wiki/Libvirt) — a virtualization management toolkit
- [virsh](https://wiki.gentoo.org/wiki/Virsh) — a CLI-based [virtualization](https://wiki.gentoo.org/wiki/Virtualization) management toolkit
- [Virt-manager](https://wiki.gentoo.org/wiki/Virt-manager) — lightweight GUI application designed for managing virtual machines and containers via the [libvirt](https://wiki.gentoo.org/wiki/Libvirt) API.
- [Libvirt/QEMU\_guest](https://wiki.gentoo.org/wiki/Libvirt/QEMU_guest) — creation of a guest domain (virtual machine, VM), running inside a QEMU hypervisor, using tools found in [libvirt](https://packages.gentoo.org/packages/libvirt) package.
- [QEMU/Linux guest](https://wiki.gentoo.org/wiki/QEMU/Linux_guest) — describes the setup of a Gentoo Linux guest in [QEMU](https://wiki.gentoo.org/wiki/QEMU) using Gentoo bootable media.

## External resources

- [Libvirt Domain XML Format](https://libvirt.org/formatdomain.html) - Detailed description of a Domain XML format file.
- [https://libvirt.org/kbase/secureboot.html](https://libvirt.org/kbase/secureboot.html)
- [https://wiki.libvirt.org/VM\_lifecycle.html](https://wiki.libvirt.org/VM_lifecycle.html)
