<!-- source: https://wiki.gentoo.org/wiki/Libvirt/QEMU_networking | group: Gentoo Wiki (Main) | wiki-title: Libvirt/QEMU networking -->
---
title: Libvirt/QEMU networking
url: https://wiki.gentoo.org/wiki/Libvirt/QEMU_networking
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-09-17"
fingerprint: fe9bf39dca227384
license: CC BY-SA 4.0
---

# Libvirt/QEMU networking

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article details the setup of Gentoo networking by [Libvirt](https://wiki.gentoo.org/wiki/Libvirt) for use by guest containers and [QEMU](https://wiki.gentoo.org/wiki/QEMU)-based virtual machines.

## Documentation legend

If the host OS is not a Gentoo OS, consult the corresponding OS guide on network installation and replace the following Gentoo Ethernet device names throughout the document with that OS's device name nomenclature.

- **virbr0** netdev - virtual bridge with NAT
- **enp3s0** netdev - slave to **virbr0** - WAN-side



And optionally have the following netdevs/IP-links:

- **enp4s0** netdev - DMZ-side  (optional)
- **enp5s0** netdev - Internal LAN-side  (optional)
- **virbr1** netdev - closed network



## Packages required

- [app-emulation/libvirt](https://packages.gentoo.org/packages/app-emulation/libvirt), HOWTO in [libvirt](https://wiki.gentoo.org/wiki/Libvirt).
- [sys-apps/iproute2](https://packages.gentoo.org/packages/sys-apps/iproute2)

Check that the libvirt package is installed, the libvirtd service is started and get a positive response from the libvirtd daemon:

`root #``virsh sysinfo````
<sysinfo type='smbios'>
  <bios>
    <entry name='vendor'>Dell Inc.</entry>
    <entry name='version'>A22</entry>
    <entry name='date'>11/29/2018</entry>
    <entry name='release'>4.6</entry>
  </bios>
  <system>
    <entry name='manufacturer'>Dell Inc.</entry>
    <entry name='product'>OptiPlex 3010</entry>
    <entry name='version'>01</entry>
    <entry name='serial'>JRJ0SW1</entry>
    <entry name='uuid'>4c4c4544-0052-4a10-8030-cac04f535731</entry>
    <entry name='sku'>OptiPlex 3010</entry>
    <entry name='family'>Not Specified</entry>
  </system>
  <baseBoard>
    <entry name='manufacturer'>Dell Inc.</entry>
    <entry name='product'>042P49</entry>
    <entry name='version'>A00</entry>
    <entry name='serial'>/JRJ0SW1/CN701632BD05R5/</entry>
    <entry name='asset'>Not Specified</entry>
    <entry name='location'>Not Specified</entry>
  </baseBoard>
  <chassis>
    <entry name='manufacturer'>Dell Inc.</entry>
    <entry name='version'>Not Specified</entry>
    <entry name='serial'>JRJ0SW1</entry>
    <entry name='asset'>Not Specified</entry>
    <entry name='sku'>To be filled by O.E.M.</entry>
  </chassis>
  <processor>
    <entry name='socket_destination'>CPU 1</entry>
    <entry name='type'>Central Processor</entry>
    <entry name='family'>Core i5</entry>
    <entry name='manufacturer'>Intel(R) Corporation</entry>
    <entry name='signature'>Type 0, Family 6, Model 58, Stepping 9</entry>
    <entry name='version'>Intel(R) Core(TM) i5-3470 CPU @ 3.20GHz</entry>
    <entry name='external_clock'>100 MHz</entry>
    <entry name='max_speed'>3200 MHz</entry>
    <entry name='status'>Populated, Enabled</entry>
    <entry name='serial_number'>Not Specified</entry>
    <entry name='part_number'>Fill By OEM</entry>
  </processor>
  <memory_device>
    <entry name='size'>8 GB</entry>
    <entry name='form_factor'>DIMM</entry>
    <entry name='locator'>DIMM1</entry>
    <entry name='bank_locator'>Not Specified</entry>
    <entry name='type'>DDR3</entry>
    <entry name='type_detail'>Synchronous</entry>
    <entry name='speed'>1600 MT/s</entry>
    <entry name='manufacturer'>8C26</entry>
    <entry name='serial_number'>00000000</entry>
    <entry name='part_number'>TIMETEC-UD3-1600</entry>
  </memory_device>
  <memory_device>
    <entry name='size'>8 GB</entry>
    <entry name='form_factor'>DIMM</entry>
    <entry name='locator'>DIMM2</entry>
    <entry name='bank_locator'>Not Specified</entry>
    <entry name='type'>DDR3</entry>
    <entry name='type_detail'>Synchronous</entry>
    <entry name='speed'>1600 MT/s</entry>
    <entry name='manufacturer'>8C26</entry>
    <entry name='serial_number'>00000000</entry>
    <entry name='part_number'>TIMETEC-UD3-1600</entry>
  </memory_device>
  <oemStrings>
    <entry>Dell System</entry>
    <entry>1[0585]</entry>
    <entry>3[1.0]
</entry>
    <entry>12[www.dell.com]
</entry>
    <entry>14[1]</entry>
    <entry>15[11]</entry>
  </oemStrings>
</sysinfo>
```
If it hang or has no output, then start the libvirtd daemon:

For **OpenRC** initd

`root #``rc-service libvirtd start`
For **systemd**:

- Before version 243-rc1, use:

`root #``systemctl start libvirtd`
- After version 243-rc1, the new modular [architecture](https://wiki.gentoo.org/wiki/Libvirt#systemd) for **systemd** has been introduced; we use the **virtnetworkd** unit service instead:

`root #``systemctl start virtnetworkd`
## Network

### Network management controller

Most importantly, one has to decide which network controller will be responsible for the online state of IP interfaces, including as a host OS for the guest containers and QEMU-based virtual machines.

The choices of host-side network management (along with its configuration file path) are:

- libvirtd, /etc/libvirt/qemu/networks/default.xml

or

- OpenRC, /etc/conf.d/net
- systemd-networking, /etc/systemd/network


The libvirtd is the recommended network controller for VMs/containers. libvirtd comes with DHCP server as a default (and that is optional too).

### Current networking

There are three camps of current network setups:

1. default network connection is provided by libvirt at install time.
2. No IP interface nor bridge defined; fresh OS install
3. multiple IP netdevs/links already configured.



#### 1. Default network connection

Check the libvirtd for any existing network connections; a fresh install of [app-emulation/libvirt](https://packages.gentoo.org/packages/app-emulation/libvirt) should leave at least a default network:

`root #``virsh net-list`
Name        State    Autostart   Persistent
----------------------------------------------
 default     active   yes         yes

default network is only used by libvirt-managed virtual machines and containers.



#### 2. Existing network - simple

If a **default** network connection exist, skip this section and go on to "Multiple IP Links Configured" section.

If this is an existing setup is Gentoo Handbook-guided, but its default network became missing sometime after a [sys-emulation/libvirt](https://packages.gentoo.org/packages/sys-emulation/libvirt) installation, that means the [app-emulation/libvirt](https://packages.gentoo.org/packages/app-emulation/libvirt) may not have been installed correctly or that default network may have been deleted.

To recreate and restore the default network using libvirt default settings, execute:

`root #``cp -i /usr/share/libvirt/networks/default.xml /etc/libvirt/qemu/networks/default.xml`
Inform the libvirtd of the new default network settings:

`root #``virsh net-define /usr/share/libvirt/networks/default.xml`
Network test defined from /usr/share/libvirt/networks/default.xml

To change the IP address, IP subnet, gateway, and/or DHCP IP range on this host for the VMs/containers, execute:

`root #``virsh net-edit default`
then save the network XML file.


Test the default network by asking the libvirtd daemon to see this new default network, that will only be used by virtual machines and containers:

`root #``virsh net-list --all`
Name        State    Autostart   Persistent
----------------------------------------------
 default     inactive   no          yes


Enable the default network so that it starts during boot-up time:

`root #``virsh net-autostart default`
Network test marked as autostarted

Start the default network:

`root #``virsh net-start default`
Network default started



#### 3. Open vSwitch Network

Assuming an ovs network named vbrlan0 has already been setup.

`root #``ovs-vsctl list````
    Bridge vbrlan0
        Port vbrlan0
            Interface vbrlan0
                type: internal
        Port bond0
            Interface enp142s0f2
            Interface enp142s0f3
            Interface enp142s0f1
```
Create a network configuration.

**`/etc/libvirt/qemu/networks/ovs-network.xml`**

```
<network>
	<name>ovs</name>
	<uuid></uuid>
	<forward mode='bridge'/>
	<bridge name='vbrlan0'/>
	<virtualport type='openvswitch'/>
</network>
```
Define/activate the network configuration.

`root #``virsh net-define ovs-network.xml`
Network ovs defined from ovs-network.xml

Confirm ovs-network was created.

`root #``virsh net-list --all`
Name      State      Autostart   Persistent
----------------------------------------------
 default   active     yes         yes
 ovs       inactive   no          yes

Enable the ovs network so that it starts during boot-up time:

`root #``virsh net-autostart ovs`
Network test marked as autostarted

Start the ovs network:

`root #``virsh net-start ovs`
Network ovs started

Disable/stop the default network:

`root #``virsh net-destroy default`
Network default destroyed

Disable default network autostart:

`root #``virsh net-autostart --disable default`
Network default unmarked as autostarted

#### 4. Hardware Passthrough (Single Port)

This example utilizes an [Intel I350-T4](https://www.intel.com/content/www/us/en/products/sku/84805/intel-ethernet-server-adapter-i350t4v2/specifications.html) Network Interface Card. Only the first port will be used, while the other 3x ports have been configured into an Open vSwitch bridge vbrlan0 seen above.

Identify the device.

`root #``virsh nodedev-list --tree | grep pci````
  +- pci_0000_8d_01_0
  {{|}}   +- pci_0000_8e_00_0
  {{|}}   +- pci_0000_8e_00_1
  {{|}}   +- pci_0000_8e_00_2
  {{|}}   +- pci_0000_8e_00_3
```
Gather required information such as the domain, bus, and function.

`root #``virsh nodedev-dumpxml pci_0000_8e_00_0````
<device>
  <name>pci_0000_8e_00_0</name>
  <path>/sys/devices/pci0000:8d/0000:8d:01.0/0000:8e:00.0</path>
  <parent>pci_0000_8d_01_0</parent>
  <driver>
    <name>igb</name>
  </driver>
  <capability type='pci'>
    <class>0x020000</class>
    <domain>0</domain>
    <bus>142</bus>
    <slot>0</slot>
    <function>0</function>
    <product id='0x1521'>I350 Gigabit Network Connection</product>
    <vendor id='0x8086'>Intel Corporation</vendor>
    <capability type='virt_functions' maxCount='7'/>
    <iommuGroup number='12'>
      <address domain='0x0000' bus='0x8e' slot='0x00' function='0x0'/>
    </iommuGroup>
    <numa node='0'/>
    <pci-express>
      <link validity='cap' port='4' speed='5' width='4'/>
      <link validity='sta' speed='5' width='4'/>
    </pci-express>
  </capability>
</device>
```
The important information is included in the **\<address domain='0x0000' bus='0x8e' slot='0x00' function='0x0'/>**

Detach the device from the system.

`root #``virsh nodedev-dettach pci_0000_8e_00_0`
Device pci\_0000\_8e\_00\_0 detached

Add device to VM xml.

**`/etc/libvirt/qemu/opnsense.qemu.kvm-x86_64.xml`**

```
<hostdev mode='subsystem' type='pci' managed='yes'>
  <source>
      <address domain='0x0000' bus='0x8e' slot='0x00' function='0x0'/>
  </source>
</hostdev>
```
`root #``setsebool -P virt_use_sysfs 1`
#### 5. Existing multiple IP netdevs/links/interfaces

Assuming that you do not have prior virtualization support being installed on your host, the bare minimum criteria to hosting VMs/containers is to provide a virtual bridge (or MACVLAN). Such virtual bridge may or may not have NAT, and may or may not have a physical Ethernet port-slaved to its bridge.

If an existing bridge exists, is properly configured, and is a suitable candidate for hosting all the guest VM/containers, then replace throughout this documentation page the virbr0 notation with that bridge device name.

If no bridge is available for hosting the guest, then one Ethernet netdev (802.3/USB/Wireless/tunnel) must be available and not a slave to any other bridges; then go back to step 1 above.

Else if non-slave Ethernet interface is not available, then add another Ethernet NIC card or replan your host network configuration.

## See also

- [Virtualization](https://wiki.gentoo.org/wiki/Virtualization) — the concept and technique that permits running software in an environment separate from a computer operating system.
- [QEMU](https://wiki.gentoo.org/wiki/QEMU) — a generic, open-source hardware emulator and virtualization suite.
- [QEMU/Front-ends](https://wiki.gentoo.org/wiki/QEMU/Front-ends) — provide graphical, terminal, web-based, or command-line interfaces for configuring, managing, or accessing QEMU virtual machines.

- [Libvirt](https://wiki.gentoo.org/wiki/Libvirt) — a virtualization management toolkit
- [Libvirt/QEMU\_guest](https://wiki.gentoo.org/wiki/Libvirt/QEMU_guest) — creation of a guest domain (virtual machine, VM), running inside a QEMU hypervisor, using tools found in [libvirt](https://packages.gentoo.org/packages/libvirt) package.

- [Virt-manager](https://wiki.gentoo.org/wiki/Virt-manager) — lightweight GUI application designed for managing virtual machines and containers via the [libvirt](https://wiki.gentoo.org/wiki/Libvirt) API.
- [Virt-manager/QEMU\_guest](https://wiki.gentoo.org/wiki/Virt-manager/QEMU_guest) — creation of a guest virtual machine (VM) running inside a QEMU hypervisor using just the virt-manager GUI tool.

- [QEMU/Linux guest](https://wiki.gentoo.org/wiki/QEMU/Linux_guest) — describes the setup of a Gentoo Linux guest in [QEMU](https://wiki.gentoo.org/wiki/QEMU) using Gentoo bootable media.
