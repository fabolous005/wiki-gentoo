<!-- source: https://wiki.gentoo.org/wiki/Libvirt | group: Gentoo Wiki (Main) | wiki-title: Libvirt -->
---
title: libvirt
url: https://wiki.gentoo.org/wiki/Libvirt
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-10"
fingerprint: "6e19c95c4c87bbac"
license: CC BY-SA 4.0
---

# libvirt

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


libvirt is a virtualization management toolkit.

The libvirt package is comprised of two components: a toolkit, and a static object library. It primarily provides virtualization support for UNIX.

## Overview

[app-emulation/libvirt](https://packages.gentoo.org/packages/app-emulation/libvirt) package provides a CLI toolkit that can be used to assist in the creation and configuration of new domains.  It is also used to adjust a domain’s resource allocation/virtual hardware.



### Features

The overview of Libvirt features are:

- Guest configuration is stored in the XML format at /etc/libvirt. For example, QEMU config goes under /etc/libvirt/qemu
- Snapshots for virtual machines can be crated and rolled back.
- Network interface creation and management, including bridge and MACVLAN creation.
- Network configuration automation and management for NAT and DHCP.
- Storage pool management for easier mounting on guests, filesystems including:

#### Supported guest types

libvirt can manage the following types of virtual machines and containers, [among others](https://www.libvirt.org/drivers.html#hypervisor-drivers):

Verify host as QEMU-capable:

To verify that the host hardware has the needed virtualization support, issue the following command:

`host$``grep --color -E "vmx|svm" /proc/cpuinfo`
The vmx or svm CPU flag should be red highlighted and available.

File /dev/kvm must exist.



### Kernel

The following kernel config is recommended by the libvirtd daemon.

**libvirt (`CONFIG_BRIDGE_EBT_MARK`, `CONFIG_NETFILTER_ADVANCED`, `CONFIG_NETFILTER_XT_CONNMARK`, `CONFIG_NETFILTER_XT_TARGET_CHECKSUM`, `CONFIG_IP6_NF_NAT`)**

The following kernel options are required to pass some checks by the virt-host-validate tool. That also means that are requirements for some functionality.

**Enabling`blkio` (`CONFIG_BLK_CGROUP`)**

**Enabling`memory` (`CONFIG_MEMORY`)**

**Enabling`tun` (`CONFIG_TUN`) (used in the default libvirt/virt-manager networking setup)**

### USE flags

Some packages are aware of the [`libvirt`](https://packages.gentoo.org/useflags/libvirt) USE flag.

Review the possible USE flags for libvirt:


| [+caps](https://packages.gentoo.org/useflags/+caps) | Use Linux capabilities library to control privilege | 
| [+libvirtd](https://packages.gentoo.org/useflags/+libvirtd) | Builds the libvirtd daemon as well as the client utilities instead of just the client utilities | 
| [+qemu](https://packages.gentoo.org/useflags/+qemu) | Support management of QEMU virtualisation (app-emulation/qemu) | 
| [+udev](https://packages.gentoo.org/useflags/+udev) | Enable virtual/udev integration (device discovery, power and storage device support, etc) | 
| [+virt-network](https://packages.gentoo.org/useflags/+virt-network) | Enable virtual networking (NAT) support for guests. Includes all the dependencies for NATed network mode. Effectively any network setup that relies on libvirt to setup and configure network interfaces on your host. This can include bridged and routed networks ONLY if you are allowing libvirt to create and manage the underlying devices for you. In some cases this requires enabling the 'netcf' USE flag (currently unavailable). | 
| [apparmor](https://packages.gentoo.org/useflags/apparmor) | Enable support for the AppArmor application security system | 
| [audit](https://packages.gentoo.org/useflags/audit) | Enable support for Linux audit subsystem using sys-process/audit | 
| [bash-completion](https://packages.gentoo.org/useflags/bash-completion) | Enable bash-completion support | 
| [dtrace](https://packages.gentoo.org/useflags/dtrace) | Enable dtrace support via dev-debug/systemtap | 
| [firewalld](https://packages.gentoo.org/useflags/firewalld) | DBus interface to iptables/ebtables allowing for better runtime management of your firewall. | 
| [fuse](https://packages.gentoo.org/useflags/fuse) | Allow LXC to use sys-fs/fuse for mountpoints | 
| [glusterfs](https://packages.gentoo.org/useflags/glusterfs) | Enable GlusterFS support via sys-cluster/glusterfs | 
| [iscsi](https://packages.gentoo.org/useflags/iscsi) | Allow using an iSCSI remote storage server as pool for disk image storage | 
| [iscsi-direct](https://packages.gentoo.org/useflags/iscsi-direct) | Allow using libiscsi for iSCSI storage pool backend | 
| [libssh](https://packages.gentoo.org/useflags/libssh) | Use net-libs/libssh to communicate with remote libvirtd hosts, for example: qemu+libssh://server/system | 
| [libssh2](https://packages.gentoo.org/useflags/libssh2) | Use net-libs/libssh2 to communicate with remote libvirtd hosts, for example: qemu+libssh2://server/system | 
| [lvm](https://packages.gentoo.org/useflags/lvm) | Allow using the Logical Volume Manager (sys-fs/lvm2) as pool for disk image storage | 
| [lxc](https://packages.gentoo.org/useflags/lxc) | Support management of Linux Containers virtualisation (app-containers/lxc) | 
| [nbd](https://packages.gentoo.org/useflags/nbd) | Allow using sys-block/nbdkit to access network disks | 
| [nfs](https://packages.gentoo.org/useflags/nfs) | Allow using Network File System mounts as pool for disk image storage | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [numa](https://packages.gentoo.org/useflags/numa) | Use NUMA for memory segmenting via sys-process/numactl and sys-process/numad | 
| [parted](https://packages.gentoo.org/useflags/parted) | Allow using real disk partitions as pool for disk image storage, using sys-block/parted to create, resize and delete them. | 
| [pcap](https://packages.gentoo.org/useflags/pcap) | Support auto learning IP addreses for routing | 
| [policykit](https://packages.gentoo.org/useflags/policykit) | Enable PolicyKit (polkit) authentication support | 
| [rbd](https://packages.gentoo.org/useflags/rbd) | Enable rados block device support via sys-cluster/ceph | 
| [sasl](https://packages.gentoo.org/useflags/sasl) | Add support for the Simple Authentication and Security Layer | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 
| [virtiofsd](https://packages.gentoo.org/useflags/virtiofsd) | Drag in virtiofsd dependency app-emulation/virtiofsd | 
| [virtualbox](https://packages.gentoo.org/useflags/virtualbox) | Support management of VirtualBox virtualisation (app-emulation/virtualbox) | 
| [wireshark-plugins](https://packages.gentoo.org/useflags/wireshark-plugins) | Build the net-analyzer/wireshark plugin for the Libvirt RPC protocol | 
| [xen](https://packages.gentoo.org/useflags/xen) | Support management of Xen virtualisation (app-emulation/xen) | 
| [zfs](https://packages.gentoo.org/useflags/zfs) | Enable ZFS backend storage sys-fs/zfs | 

libvirt comes with a number of USE flags. Please check those flags and set them according to your setup. These are recommended USE flags for libvirt:

**`/etc/portage/package.use/libvirt`**

```
 pcap virt-network numa fuse macvtap vepa qemu
```
##### USE\_EXPAND

See [/etc/portage/make.conf#USE\_EXPAND](https://wiki.gentoo.org/wiki//etc/portage/make.conf#USE_EXPAND) for more detail on `USE_EXPAND`.

### Emerge

After reviewing and adding any desired USE flags, emerge [app-emulation/libvirt](https://packages.gentoo.org/packages/app-emulation/libvirt) and [app-emulation/qemu](https://packages.gentoo.org/packages/app-emulation/qemu) :

`root #``emerge --ask app-emulation/libvirt app-emulation/qemu`
### Additional software

#### Custom UEFI

Custom UEFI are provided by [app-emulation/virt-firmware](https://packages.gentoo.org/packages/app-emulation/virt-firmware) package.

## Configuration

### Environment variables

See specific CLI commands related to Libvirt for its available environment variable settings: [virsh](https://wiki.gentoo.org/wiki/Virsh#Environment_variables), [libvirtd](https://wiki.gentoo.org/wiki/Libvirt/libvirtd#Environment_variables), [virt-manager](https://wiki.gentoo.org/wiki/Virt-manager#Environment_variables).

### Files

When a domain starts, client using [Libvirt] API library (ie., [virt-manager](https://wiki.gentoo.org/wiki/Virt-manager), [virsh](https://wiki.gentoo.org/wiki/Virsh)) checks for that [domain](https://wiki.gentoo.org/wiki/Libvirt/domain) XML file in the following paths:

- System : /etc/libvirt/qemu/ via qemu:///system.
- User session: $HOME/.config/libvirt/qemu/ via qemu:///session.


Other directory paths used by [Libvirt] library are:

- /etc/libvirt/hooks/
- /etc/libvirt/nwfilter/
- /etc/libvirt/secrets/
- /etc/libvirt/storage/
- /proc/
- /proc/sys/ipv4/
- /proc/sys/ipv6/conf/all/
- /proc/sys/ipv6/conf/%s/%s
- /run/libvirt/
- /sys/class/fc\_host/
- /sys/devices/system/%s/cpu/
- /sys/devices/system/node/node0/
- /sys/fs/resctrl/info/%s/
- /sys/kernel/mm/transparent\_hugepage/
- /sys/fs/resctrl/info/%s/
- /sys/fs/resctrl/info/MB/
- /var/lib/libvirt/
- /var/lib/libvirt/boot/
- /var/lib/libvirt/dnsmasq/
- /var/lib/libvirt/images
- /var/lib/libvirt/isos
- /var/lib/libvirt/secrets/
- /var/lib/libvirt/qemu


For specific file accesses, see Libvirt-related CLI commands [virsh](https://wiki.gentoo.org/wiki/Virsh#Files), [libvirtd](https://wiki.gentoo.org/wiki/Libvirt/libvirtd#Files), [virt-manager](https://wiki.gentoo.org/wiki/Virt-manager#Files)..

### User permissions

To have a user join the UNIX **libvirt** group, check that the group name is already defined:

`host$``getent group libvirt`
libvirt:x:1001:

If the output line exist above stating that the **libvirt** group exists, then add the user to that group:

`host$``sudo usermod -aG libvirt username`

If **libvirt** group is missing, then `policykit` may have not been installed.

If not using `policykit` and still wanting to use libvirt session (per-user/user-mode), then add manually the **wheel** group, replace **username** with its actual username, run:

`host$``sudo usermod -aG wheel username`
Uncomment the following lines from the libvirtd configuration file:

**`/etc/libvirt/libvirtd.conf`**

```
auth_unix_ro = "none"
auth_unix_rw = "none"
unix_sock_group = "libvirt"
unix_sock_ro_perms = "0777"
unix_sock_rw_perms = "0770"
```
Be sure to have the user log out then log in again for the new group settings to be applied.

virt-admin should then be launchable as a regular user, after the services have been started.

### Service

#### OpenRC

To start libvirtd daemon using OpenRC and add it to default runlevel:

`host-root#``rc-service libvirtd start && rc-update add libvirtd default`
#### systemd

Historically, all libvirt functionality was provided by the monolithic libvirtd daemon. [Upstream](https://libvirt.org/daemons.html#modular-driver-daemons) has developed a new modular architecture for libvirt where each driver is run in its own daemon. Therefore, recent versions of libvirt (at least >=app-emulation/libvirt-9.3.0) need the service units for the hypervisor drivers enabled. For QEMU this is virtqemud.service, for Xen it is virtxend.service and for LXC virtlxcd.service and their corresponding sockets.

Enable the service units and their sockets, depending on the functionality (qemu, xen, lxc) you need:

`host-root#``systemctl enable --now virtnetworkd.service`
Created symlink '/etc/systemd/system/multi-user.target.wants/virtnetworkd.service' → '/usr/lib/systemd/system/virtnetworkd.service'.
Created symlink '/etc/systemd/system/sockets.target.wants/virtnetworkd.socket' → '/usr/lib/systemd/system/virtnetworkd.socket'.
Created symlink '/etc/systemd/system/sockets.target.wants/virtnetworkd-ro.socket' → '/usr/lib/systemd/system/virtnetworkd-ro.socket'.
Created symlink '/etc/systemd/system/sockets.target.wants/virtnetworkd-admin.socket' → '/usr/lib/systemd/system/virtnetworkd-admin.socket'.

`host-root#``systemctl enable --now virtqemud.service`
Created symlink /etc/systemd/system/multi-user.target.wants/virtqemud.service → /usr/lib/systemd/system/virtqemud.service.
Created symlink /etc/systemd/system/sockets.target.wants/virtqemud.socket → /usr/lib/systemd/system/virtqemud.socket.
Created symlink /etc/systemd/system/sockets.target.wants/virtqemud-ro.socket → /usr/lib/systemd/system/virtqemud-ro.socket.
Created symlink /etc/systemd/system/sockets.target.wants/virtqemud-admin.socket → /usr/lib/systemd/system/virtqemud-admin.socket.
Created symlink /etc/systemd/system/sockets.target.wants/virtlogd.socket → /usr/lib/systemd/system/virtlogd.socket.
Created symlink /etc/systemd/system/sockets.target.wants/virtlockd.socket → /usr/lib/systemd/system/virtlockd.socket.
Created symlink /etc/systemd/system/sockets.target.wants/virtlogd-admin.socket → /usr/lib/systemd/system/virtlogd-admin.socket.
Created symlink /etc/systemd/system/sockets.target.wants/virtlockd-admin.socket → /usr/lib/systemd/system/virtlockd-admin.socket.

`host-root#``systemctl enable --now virtstoraged.socket`
Created symlink /etc/systemd/system/sockets.target.wants/virtstoraged.socket → /usr/lib/systemd/system/virtstoraged.socket.
Created symlink /etc/systemd/system/sockets.target.wants/virtstoraged-ro.socket → /usr/lib/systemd/system/virtstoraged-ro.socket.
Created symlink /etc/systemd/system/sockets.target.wants/virtstoraged-admin.socket → /usr/lib/systemd/system/virtstoraged-admin.socket.

`host-root#``systemctl enable --now virtlogd.service`
Created symlink '/etc/systemd/system/sockets.target.wants/virtlogd.socket' → '/usr/lib/systemd/system/virtlogd.socket'. Created symlink '/etc/systemd/system/sockets.target.wants/virtlogd-admin.socket' → '/usr/lib/systemd/system/virtlogd-admin.socket'.

All the service units use a timeout that causes them to shutdown after 2 minutes if no VM is running. They get automatically reactivated when a socket is accessed, e. g. when virt-manager is started or a virsh command is run.

### Firewall

The following firewall chain names have been reserved by the libvirt library and libvirtd daemon.

| Reserved Firewall Chain Names by libvirt (viriptable.c source) |  | 
|---|---|
| Reserved chain name | Description | 
|---|---|
| nat | NAT | 
| LIBVIRT\_INP | Firewall input | 
| LIBVIRT\_FWI | Firewall input | 
| LIBVIRT\_FWO | Firewall output | 
| LIBVIRT\_FWX | Firewall forward | 
| LIBVIRT\_OUT | Firewall output | 
| LIBVIRT\_PRT | Firewall postrouting | 

### Networking

For configuration of networking under libvirt, continue reading at [QEMU networking in Libvirt](https://wiki.gentoo.org/wiki/Libvirt/QEMU_networking).

### Autostart

Autostarting a domain can be done in session (X) or in system (host).



#### System autostart

AutoStart for system-specific domain is natively supported by libvirtd.

For **AutoStart** option of a domain on power-up/reset:

- run `virsh --connect qemu:///system autostart <vm-name>`; creates a symbolic link to /etc/libvirt/qemu/autostart/\<vm-name>.xml.
- from the virt-manager "`Virtual Machine Manager`" window, select the domain in the main panel, go to **Edit->Virtual Machine Details** menu suboptions,
- or from the virt-manager "`<domain> on QEMU/KVM`" window, go to **View->Detail** menu options then select **Boot Options** line item in left navigation panel: under **Autostart** in main panel


Toggle the checkbox for "Start virtual machine at boot up".

#### Session autostart

The hypervisor controller (libvirtd) does not directly support the autostart of session-specific domains (--connect=qemu:///session).

Because session runs under an unprivileged user, its [libvirt] instance becomes available only after that user logs in. As a result, libvirtd does not manage session autostart.

To automatically start a session-specific domain, one of the following methods can be used:

- XDG Autostart mechanism
- Login script
- X session script

- XDG Autostart mechanism
- Create a .desktop file in \~/.config/autostart/ that calls virsh --connect=qemu:///session start mydomain.
- This will start the domain automatically whenever a desktop session that supports the XDG Autostart specification is launched.
- Example:

`host$``vi ~/.config/autostart/libvirt-mydomain.desktop`
**`$HOME/.config/autostart/libvirt-mydomain.desktop`**

**"Typical`.desktop` file"**

For more fine-grained control under KDE Plasma, one of the following lines may be added to the \~/.config/autostart/libvirt-mydomain.desktop file:

**`$HOME/.config/autostart/libvirt-mydomain.desktop`**

**"Optional KDE-specific autostart phase"**

- Login script
- Add the virsh command to the user’s shell login file so the domain starts whenever the user logs in on a TTY or via SSH.
- Example for bash:

**`$HOME/.bash_profile`**

**"Add to`~/.bash_profile`"**

Redirecting output ensures that the command does not interfere with the login prompt.

- X session script
- Insert the virsh command into the user’s X session startup file. This ensures the domain starts together with the graphical environment.
- Example using .xinitrc:

**`$HOME/.xinitrc`**

**"Add to`~/.xinitrc`"**

For display managers (e.g. GDM, SDDM, LightDM), use \~/.xsession instead. The trailing & runs the domain start command in the background so it does not block the session launch.

## Usage

### Managing domains

List all active domains with:

`host$``virsh list`
Id   Name     State
------------------------
 1    gentoo   running
 2    default  running

### Host information

Show host CPU and memory details with:

`host$``virsh nodeinfo`
CPU model:           x86\_64
CPU(s):              4
CPU frequency:       1600 MHz
CPU socket(s):       1
Core(s) per socket:  4
Thread(s) per core:  1
NUMA cell(s):        1
Memory size:         16360964 KiB

Query host DMI/SMBIOS data with:

`host$``virsh sysinfo````
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
### Host verification

Verify the full host setup of libvirtd with:

`host-root#``virt-host-validate`
QEMU: Checking for hardware virtualization                                 : PASS
  QEMU: Checking if device /dev/kvm exists                                   : PASS
  QEMU: Checking if device /dev/kvm is accessible                            : PASS
  QEMU: Checking if device /dev/vhost-net exists                             : PASS
  QEMU: Checking if device /dev/net/tun exists                               : PASS
  QEMU: Checking for cgroup 'cpu' controller support                         : PASS
  QEMU: Checking for cgroup 'cpuacct' controller support                     : PASS
  QEMU: Checking for cgroup 'cpuset' controller support                      : PASS
  QEMU: Checking for cgroup 'memory' controller support                      : PASS
  QEMU: Checking for cgroup 'devices' controller support                     : PASS
  QEMU: Checking for cgroup 'blkio' controller support                       : PASS
  QEMU: Checking for device assignment IOMMU support                         : PASS
  QEMU: Checking if IOMMU is enabled by kernel                               : PASS
  QEMU: Checking for secure guest support                                    : WARN (Unknown if this platform has Secure Guest support)
   LXC: Checking for Linux >= 2.6.26                                         : PASS
   LXC: Checking for namespace ipc                                           : PASS
   LXC: Checking for namespace mnt                                           : PASS
   LXC: Checking for namespace pid                                           : PASS
   LXC: Checking for namespace uts                                           : PASS
   LXC: Checking for namespace net                                           : PASS
   LXC: Checking for namespace user                                          : PASS
   LXC: Checking for cgroup 'cpu' controller support                         : PASS
   LXC: Checking for cgroup 'cpuacct' controller support                     : PASS
   LXC: Checking for cgroup 'cpuset' controller support                      : PASS
   LXC: Checking for cgroup 'memory' controller support                      : PASS
   LXC: Checking for cgroup 'devices' controller support                     : PASS
   LXC: Checking for cgroup 'freezer' controller support                     : FAIL (Enable 'freezer' in kernel Kconfig file or mount/enable cgroup controller in your system)
   LXC: Checking for cgroup 'blkio' controller support                       : PASS
   LXC: Checking if device /sys/fs/fuse/connections exists                   : PASS

### Connect types - Default

The connect type **--connect \<URI>** CLI option tells the Libvirt clients how to connect (transport), and optionally where.

The connect type option lets the Libvirt client (virt-manager, virsh) connect to and manage the hypervisor (eg. libvirtd, libqemud) daemon.

If the connect type option not used, the default URI is supplied by its Libvirt client application and is always a local hypervisor URI:

| Libvirt client | Default URI | Socket Path | Requires Root? | 
|---|---|---|---|
| [virsh](https://wiki.gentoo.org/wiki/Virsh) as root | qemu:///system | /run/libvirt/libvirt-sock | ✅ | 
| [virsh](https://wiki.gentoo.org/wiki/Virsh) as regular user | qemu:///session | $XDG\_RUNTIME\_DIR/libvirt/libvirt-sock | ❌ | 
| [virt-manager](https://wiki.gentoo.org/wiki/Virt-manager) | Both system + session |  |  | 

### Connect types - Required

URI is used to denote the specific connect type for a libvirt client to connect to a hypervisor aka libvirt daemon (e.g., libqemud).

The generalized syntax of the Libvirt connect URI is:

transport\[+protocol\]:///target-or-path
   transport\[+protocol\]://\[user@\]hostname\[:port\]/target-or-path\[?extra\_parameters\]


Required URI components are **transport** and **target-or-path**, but **hostname** is required only for remote hypervisor daemon.



#### Transport

**qemu** hypervisor type is the most common choice for the mandatory **transport** syntax component.

Set **transport** to one of the following hypervisor type: (`qemu`, `xen`, `lxc`, `vz`, `uml`, `bhyve`, `exs`, `vbox`, `test`, `hv`).

#### **target-or-path**

There are two types of pathways for the **target-or-path** syntax component of connect type URI:

- predefined target name
- absolute path specification


Depending on the **transport**/**protocol** combo, one of above syntax is used and each example given below:

| URI Example | Path / Target | Meaning | 
|---|---|---|
| qemu:///session | session | Per-user session daemon | 
| qemu:///system | system | System-wide daemon | 
| qemu+ssh://user@host/system | system | Remote system daemon via SSH protocol | 
| qemu+unix:///path/to/socket | /path/to/socket | UNIX domain socket path, in absolute path specification. | 
| test:///default | default | Named mock environment, for testing only | 
| esx://user@host/?transport=https | ?transport=https | ESXi uses HTTP query string for configuration, in absolute path specification. | 
| lxc:/// | (empty) | Default system daemon for Linux [LXC](https://wiki.gentoo.org/wiki/LXC) container. | 



##### Predefined target name

The **target** name is the component to communicate within that libvirt daemon.

The connect type depends on $USER environment variable for the **target** part of **target-or-path** keyword used in CLI connect type syntax:

- session, if regular UNIX user. Commonly used in user-logged-in X sessions.
- system, if root user. Useful for persistence at bootup.

##### Path

Absolute paths are the standard and expected form when specifying Unix socket locations in Libvirt URIs.

### Connect types - Optional

Optional components to the connect type URI are detailed below.



#### Hostname

For the **hostname** syntax component, the host name is optional.

Hypervisor (libqemud, formerly libvirtd) can be reach using a UNIX domain socket or an **inet** network socket.



##### No Hostname

No hostname means only **unix** domain socket is used.

UNIX Domain Socket is the most common usage to connect with a local host hypervisor (libqemud).

The most common method of connecting to a local host hypervisor is through UNIX domain socket:

/var/lib/libvirt/libvirt.sock, for non-root user
   /var/lib/libvirt/libvirt-admin.sock, for root

##### Hostname given

If hostname is specified, then **inet** network socket is used instead of UNIX domain socket.

DNS domain name lookup is used to find the IP address of the host name, which can be local or remote.

#### Protocol

- **qemu+ssh://** - a secured connection to QEMU over SSH/TCP protocol
- **xen+ssh://** - a secured connection to Xen Dom0 over SSH/TCP protocol


virt-manager can connect to multiple **local hosts** and **remote hosts** using different protocols.



#### Connect types syntax

The breakdown of Connect Type URI format is:



- `+protocol` - (Optional) connection over transport (`+tcp`, `+ssh`, `+tls`, `+libssh2`, `+unix`).  Default is `unix`.
- `user@` - (Optional) SSH username.  Default is `$USER` environment value.

- `:port` - (Optional) The port number to use other than the default `16509/tcp`.
- `/path` - Path to the libvirt UNIX socket on the remote machine (`/session` or `/system` (default)).

##### Connect Type URI Usage

For command line, pass the connect type for a local host:

`host$``virsh --connect=qemu:///session`
or

`host-root#``virt-manager -c qemu:///system`
Using an environment variable, pass the connect type for a local host:

`host$``export LIBVIRT_DEFAULT_URI=qemu:///session; virsh`
To connect to the hypervisor, choose one of the valid hypervisor code:

| Hypervisor code | Description | 
|---|---|
| qemu | **local host** QEMU/KVM. | 
| xen | **local host** Xen hypervisor. | 
| lxc | Linux Containers. | 
| vz | Virtuozzo. | 
| uml | User-mode Linux. | 
| bhyve | FreeBSD bhyve hypervisor. | 
| exs | VMware ESX hypervisor. | 
| vbox | Oracle VirtualBox. | 
| test | Test hypervisor used for testing. | 
| hv | Microsoft Hyper-V. | 

When using system connection (**--connect=qemu:///system**), the files accessed are:

- /run/libvirt/libvirt-sock
- /run/libvirt/libvirt-admin-sock
- /run/libvirt/libvirt-sock-ro

When using session connection (**--connect=qemu:///session**), the files accessed are:

- /run/user/1000/libvirt/libvirt-sock
- /run/user/1000/libvirt/libvirt-admin-sock


More URI details at \[[libvirt.org URI](https://www.libvirt.org/uri.html)\].

### Invocation

For invocation of the command line interface (CLI) of libvirt, see [virsh invocation](https://wiki.gentoo.org/wiki/Virsh#Invocation).

For invocation of the libvirtd daemon:

`user $``libvirtd --help````
Usage:
  libvirtd [options]
Options:
  -h | --help            Display program help
  -v | --verbose         Verbose messages
  -d | --daemon          Run as a daemon & write PID file
  -l | --listen          Listen for TCP/IP connections
  -t | --timeout <secs>  Exit after timeout period
  -f | --config <file>   Configuration file
  -V | --version         Display version information
  -p | --pid-file <file> Change name of PID file
libvirt management daemon:
  Default paths:
    Configuration file (unless overridden by -f):
      /etc/libvirt/libvirtd.conf
    Sockets:
      /run/libvirt/libvirt-sock
      /run/libvirt/libvirt-sock-ro
    TLS:
      CA certificate: /etc/pki/CA/cacert.pem
      Server certificate: /etc/pki/libvirt/servercert.pem
      Server private key: /etc/pki/libvirt/private/serverkey.pem
    PID file (unless overridden by -p):
      /run/libvirtd.pid
```
## Removal

Removal of [app-emulation/libvirt](https://packages.gentoo.org/packages/app-emulation/libvirt) package (toolkit, library, and utilities) can be done by executing:

`root #``emerge --ask --depclean --verbose app-emulation/libvirt`
## Troubleshooting

### Messages mentioning ...or mount/enable cgroup controller in your system

Some of those messages are addressed on the  [previous section about the kernel configuration](https://wiki.gentoo.org#Some_extra_configurations).

If the above doesn't fix the problem, follow the section [Control groups](https://wiki.gentoo.org/wiki/LXC#Control_groups) on the LXC page to activate the correct kernel options.

### WARN (Unknown if this platform has Secure Guest support)

This message appears on non [IBM s390](https://en.wikipedia.org/wiki/IBM_System/390) or [AMD systems](https://en.wikipedia.org/wiki/Advanced_Micro_Devices) and seems to be of little relevance
[\[1\]](https://wiki.gentoo.org#cite_note-1)[\[2\]](https://wiki.gentoo.org#cite_note-2)[\[3\]](https://wiki.gentoo.org#cite_note-3)<sup>[\[4\]](https://wiki.gentoo.org#cite_note-4)</sup>.

Can Libvirt Work with [Docker](https://wiki.gentoo.org/wiki/Docker)?

While Libvirt itself doesn’t manage Docker containers, there are workarounds to make them work

- Running Docker inside a VM managed by Libvirt:
  - You can create a VM using Libvirt/KVM and install Docker inside it.
  - Useful for isolating Docker workloads in a dedicated VM.
- Using [Libvirt-lxc](https://libvirt.org/drvlxc.html) (Limited Support):
  - Libvirt has an [LXC](https://wiki.gentoo.org/wiki/LXC) (Linux Containers) driver, which is somewhat similar to Docker.
  - However, libvirt-lxc is not as feature-rich as Docker.
- Using [Podman](https://wiki.gentoo.org/wiki/Podman) (A Docker Alternative) with Libvirt:
  - Podman is a rootless container tool compatible with Docker.
  - Unlike Docker, Podman does not require a daemon, making it easier to run inside Libvirt-managed VMs.



Workarounds to Use Libvirt with Android.

While Libvirt cannot directly manage Google AVF and its **pKVM**s, you can:

- Use Libvirt to manage Android x86/x64 VMs on Linux (via QEMU/KVM).
- Run Android inside a Libvirt-managed VM (e.g., using android-x86 ISO on QEMU/KVM).
- Use Libvirt on Android devices running full Linux distributions (e.g., via Termux or a rooted environment).

## See also

- [Virtualization](https://wiki.gentoo.org/wiki/Virtualization) — the concept and technique that permits running software in an environment separate from a computer operating system.
- [QEMU](https://wiki.gentoo.org/wiki/QEMU) — a generic, open-source hardware emulator and virtualization suite.
- [QEMU/Front-ends](https://wiki.gentoo.org/wiki/QEMU/Front-ends) — provide graphical, terminal, web-based, or command-line interfaces for configuring, managing, or accessing QEMU virtual machines.

- [Libvirt/QEMU\_networking](https://wiki.gentoo.org/wiki/Libvirt/QEMU_networking) — details the setup of Gentoo networking by [Libvirt] for use by guest containers and [QEMU](https://wiki.gentoo.org/wiki/QEMU)-based virtual machines.
- [Libvirt/QEMU\_guest](https://wiki.gentoo.org/wiki/Libvirt/QEMU_guest) — creation of a guest domain (virtual machine, VM), running inside a QEMU hypervisor, using tools found in [libvirt](https://packages.gentoo.org/packages/libvirt) package.

- [Virt-manager](https://wiki.gentoo.org/wiki/Virt-manager) — lightweight GUI application designed for managing virtual machines and containers via the [libvirt] API.
- [Virt-manager/QEMU\_guest](https://wiki.gentoo.org/wiki/Virt-manager/QEMU_guest) — creation of a guest virtual machine (VM) running inside a QEMU hypervisor using just the virt-manager GUI tool.

- [QEMU/Linux guest](https://wiki.gentoo.org/wiki/QEMU/Linux_guest) — describes the setup of a Gentoo Linux guest in [QEMU](https://wiki.gentoo.org/wiki/QEMU) using Gentoo bootable media.
- [Virsh](https://wiki.gentoo.org/wiki/Virsh) — a CLI-based [virtualization](https://wiki.gentoo.org/wiki/Virtualization) management toolkit
- [Libvirt/libvirtd](https://wiki.gentoo.org/wiki/Libvirt/libvirtd) — a daemon for [Libvirt] management of virtual machines.
- [Virt-install](https://wiki.gentoo.org/wiki/Virt-install) — a CLI-based [virtualization](https://wiki.gentoo.org/wiki/Virtualization) machine creator utility.

## External resources

- [Daniel P. Berrangé libvirt announcements](https://www.berrange.com/topics/libvirt/)
- [Red Hat Virtualization Network Configuration](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/5/html/virtualization/chap-virtualization-network_configuration)
- [Create libvirt XML file for a virtual machine (VM) of Gentoo Install CD](http://www.unixversal.com/linux/gentoo/install10-1.html)

## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) Libvirt [Protected Virtualization on s390](https://libvirt.org/kbase/s390_protected_virt.html)
2. [↑](https://wiki.gentoo.org#cite_ref-2) libvir-list mailing list [PATCH 3/6 qemu: check if AMD secure guest support is enabled](https://listman.redhat.com/archives/libvir-list/2020-May/202495.html)
3. [↑](https://wiki.gentoo.org#cite_ref-3) libvir-list mailing list [PATCH 4/6 tools: secure guest check on s390 in virt-host-validate](https://listman.redhat.com/archives/libvir-list/2020-May/202496.html)
4. [↑](https://wiki.gentoo.org#cite_ref-4) libvir-list mailing list [PATCH 5/6 tools: secure guest check for AMD in virt-host-validate](https://listman.redhat.com/archives/libvir-list/2020-May/202492.html)
