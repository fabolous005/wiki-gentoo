<!-- source: https://wiki.gentoo.org/wiki/Virt-manager | group: Gentoo Wiki (Main) | wiki-title: Virt-manager -->
---
title: virt-manager
url: https://wiki.gentoo.org/wiki/Virt-manager
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-12"
fingerprint: "860b9cf94eb38bc0"
license: CC BY-SA 4.0
---

# virt-manager

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



virt-manager is a lightweight GUI application designed for managing virtual machines and containers via the [libvirt](https://wiki.gentoo.org/wiki/Libvirt) API.

It provides a comprehensive view of all domains (VMs and containers), displaying their current state (running or inactive), along with real-time performance data and resource utilization stats. Users can easily create and configure new domains, storage, and network interfaces, while also reallocating host resources between guest domains as needed.

Primarily tailored for managing KVM-based VMs, [virt-manager] also supports [Xen](https://wiki.gentoo.org/wiki/Xen) and [LXC](https://wiki.gentoo.org/wiki/LXC) containers. The interface presents an overview of live domain performance, as well as resource allocation metrics.

Domain creation and configuration are streamlined with intuitive wizards, allowing for fine-grained control over resource allocation and virtual hardware settings.

For hands-on management, [virt-manager] includes an embedded [VNC](https://wiki.gentoo.org/wiki/VNC) and [SPICE](https://wiki.gentoo.org/index.php?title=SPICE&action=edit&redlink=1) client, providing full graphical console access to guest domains.



## Overview

- First, **virt-manager** is a front-end to [QEMU](https://wiki.gentoo.org/wiki/QEMU).
- virt-manager can create, delete, and manage multiple virtual machines (VMs).
- **virt-manager** can start/stop a VM or a container.
- **virt-manager** can mount a CD-ROM ISO image.
- **virt-manager** can create different networking connections for the guest OS in VM to use.
- **virt-manager** can create bridges, MACVLAN, static netdev, and NAT'd IP interface.
- **virt-manager** can create/delete/maintain storage pools using many different filesystems such as directory, direct hard drive, gluster, [iSCSI](https://wiki.gentoo.org/wiki/ISCSI), [LVM](https://wiki.gentoo.org/wiki/LVM), multi-path devices, NetFS, SCSI, RADOS/[Ceph](https://wiki.gentoo.org/wiki/Ceph), and Sheepdog.


virt-manager is a Python3 front-end script to both [libvirt](https://wiki.gentoo.org/wiki/Libvirt) and [QEMU](https://wiki.gentoo.org/wiki/QEMU) main binary. Works on [KDE](https://wiki.gentoo.org/wiki/KDE) and [Gnome](https://wiki.gentoo.org/wiki/Gnome) equally; also leverages gnome-keyring and kwalletcli for proper password storages.

The virt-manager main window looks like:

![](https://wiki.gentoo.org/images/thumb/2/20/Virt-manager--4.0-main-window.png/500px-Virt-manager--4.0-main-window.png)

The domain window in detail mode looks like:

![](https://wiki.gentoo.org/images/thumb/0/00/Virt-manager-4.1-step7-VM-configuration-begin.png/500px-Virt-manager-4.1-step7-VM-configuration-begin.png)

In console mode, the domain window looks like:

![](https://wiki.gentoo.org/images/thumb/4/47/Virt-manager-4.1-step7-after-bootup.png/500px-Virt-manager-4.1-step7-after-bootup.png)



### VM scopes

Each VM is a [domain](https://wiki.gentoo.org/wiki/Libvirt/domain), when working with [libvirt](https://wiki.gentoo.org/wiki/Libvirt) library and libvirt-client applications (ie., [virsh](https://wiki.gentoo.org/wiki/Virsh), [virt-manager]).

Each [domain](https://wiki.gentoo.org/wiki/Libvirt/domain) can be in a different process ownership and a different machine placement:

The process ownership available for a running [domain](https://wiki.gentoo.org/wiki/Libvirt/domain) are:

- Session connection, User - Non-zero user ID
- System connection, System daemon - root, UID=0


The machine placements are:

- on a **local host** machine, on-premise
- in a **remote host** machine, separated by network





#### virt-manager stack

virt-manager is a Python GTK frontend. It does not virtualize itself.  It only interface with [Libvirt](https://wiki.gentoo.org/wiki/Libvirt).

For the primary hypervisor part of the virt-manager stack, it communicates with  [libvirtd](https://wiki.gentoo.org/wiki/Libvirt/libvirtd) daemon, and evokes [QEMU](https://wiki.gentoo.org/wiki/QEMU) (ie., qemu-system-x86\_64).

For the front-end part, virt-manager GUI, using [Libvirt](https://wiki.gentoo.org/wiki/Libvirt) API, evokes [virsh](https://wiki.gentoo.org/wiki/Virsh), [virt-install](https://wiki.gentoo.org/wiki/Virt-install), and [virt-viewer](https://wiki.gentoo.org/wiki/Virt-viewer).



#### Privileges

[Domain](https://wiki.gentoo.org/wiki/Libvirt/domain) can run as the **root** or as the **user**'s group ID matching the **libvirt** group name (or the **wheel** if no [policykit](https://packages.gentoo.org/packages/policykit) package installed).

For a **system** domain, the window title bar does NOT show this "`User session`" phrase following its domain name and virtualization engine (**QEMU/KVM**) type, in either detailed or console view mode.

![Virt-manager-titlebar-system.png](https://wiki.gentoo.org/images/1/1c/Virt-manager-titlebar-system.png)


For a **session** domain, its window title bar does show a "`User session`" after the domain name and virtualization engine (**QEMU/KVM**) type, in either detailed or console window view mode.

![Virt-manager-titlebar-session.png](https://wiki.gentoo.org/images/6/62/Virt-manager-titlebar-session.png)




##### System connection

[Domain](https://wiki.gentoo.org/wiki/Libvirt/domain) can run in privileged (root) mode as a system daemon.

On the host, system [domain](https://wiki.gentoo.org/wiki/Libvirt/domain) is started after the [init system](https://wiki.gentoo.org/wiki/Init_system) initialization of host startup **single-user/storage/network** stages.

To run through a system connection:

`host$``virt-manager --connect=qemu:///system` 


##### Session connection

A domain that runs in per-user mode is a "User session" or session connection, or simply, a session.

To start a session domain, a user must be given the **libvirt** group privilege (see also [permissions in Libvirt](https://wiki.gentoo.org/wiki/Libvirt#permissions)).

Session domain are started from the user CLI or from the display manager **AutoStart** script upon user login and after starting X11 or Wayland window manager.

Session [domain](https://wiki.gentoo.org/wiki/Libvirt/domain) runs in an unprivileged mode (not as root). If a user does not have the **libvirt** group in its supplementary group list, then use sudo start in **root**:

`host$``sudo virt-manager --connect qemu:///session`
#### Machine placements

Each [domain](https://wiki.gentoo.org/wiki/Libvirt/domain) can run on an **on-premise** (**local host**, or sometimes a **bare-metal**) or can run in a **cloud** (**remote host**).

To run a [domain](https://wiki.gentoo.org/wiki/Libvirt/domain), a hypervisor controller (libvirtd) must be up and running on a host machine, local or remote.

libvirtd must have their control socket opened before the [virt-manager]/[virsh](https://wiki.gentoo.org/wiki/Virsh) can manage the [domain(s)](https://wiki.gentoo.org/wiki/Libvirt/domain) on a host.

Control socket of a libvirtd are accessible by an UNIX domain socket (/var/run/libvirt/libvirt-sock) or by a **inet** socket (only for remote host) over TCP/IPv4 or TCP/IPv6 protocol.



##### Local domain placement

A hypervisor controller running on a **local host** controls the local [domain](https://wiki.gentoo.org/wiki/Libvirt/domain).

To control the [domain](https://wiki.gentoo.org/wiki/Libvirt/domain) on a **local host**:

`host$``virt-manager --connect qemu:///session`


##### Remote domain placement

The remote host system (aka cloud-based) are accessible by **libvirt**-based client (virt-manager, virsh).

To access the remote hypervisor controller by a TCP port number (default is **16509/tcp**):

`host$``virt-manager --connect qemu+tcp://remote-host/system`


###### Remote domain placement, by port #

To access the remote hypervisor controller by a non-default port number:

`host$``virt-manager --connect qemu+tcp://remote-host:<port>/system`
To access securely the remote hypervisor controller using SSH protocol over **tcp/22** port:

`host$``virt-manager --connect qemu+ssh://remote-host:<port>/system`
## Installation

### BIOS/UEFI

Verify that the host hardware has the required virtualization support by running the following command:

`host-root#``grep --color -E "vmx|svm" /proc/cpuinfo`
The **vmx** (Intel VT-x) or **svm** (AMD-V) CPU flag should be highlighted in red if present.

Also ensure that the /dev/kvm device node exists:

`host-root#``ls /dev/kvm`
/dev/kvm

If /dev/kvm does not exist, go back to [QEMU](https://wiki.gentoo.org/wiki/QEMU) for compliance.



### USE flags


| [gui](https://packages.gentoo.org/useflags/gui) | Enable support for a graphical user interface | 
| [policykit](https://packages.gentoo.org/useflags/policykit) | Enable PolicyKit (polkit) authentication support | 
| [sasl](https://packages.gentoo.org/useflags/sasl) | Enable connecting to SASL-enabled (e.g. Kerberos-protected) instances | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 



### Emerge

`root #``emerge --ask app-emulation/virt-manager`
### Additional software

The virt-manager requires the [app-emulation/libvirt](https://packages.gentoo.org/packages/app-emulation/libvirt) package.  See [Libvirt](https://wiki.gentoo.org/wiki/Libvirt) — a virtualization management toolkit for installation.

## Configuration

### Environment variables

A list of optional environment variables that are read and checked by the virt-manager command:

| Environment variable name | Description | Type | 
|---|---|---|
| `HOME` | path of home directory. | Filepath | 
| `DISPLAY` | the display X server and screen to which graphical applications should be sent. Needed for GTK/SPICE windows or graphical virt-manager sessions. | X11 address string | 
| `WAYLAND_DISPLAY` | Wayland display socket; used when running under Wayland instead of X11. | String | 
| `LANG` | the default system locale and language settings. | Locale string | 
| `LANGUAGE` | the user's preferred language(s) for software localization, overriding the default language defined by **$LANG** | Colon-separated list | 
| `GSETTINGS_BACKEND` | which backend is used for the GSettings system, which is part of the GNOME configuration system. Valid values are **dconf** and **memory**. | Enum | 
| `GSETTINGS_SCHEMA_DIR` | the directory where GSettings schemas are located. | Filepath | 
| `QEMU_AUDIO_DRV` | Controls QEMU audio subsystem; e.g., `none` disables sound devices entirely. Options are `alsa`, `pa`, `oss`, or `none`. | enum | 
| `XDG_DATA_HOME` | Custom user data path (e.g., app files, preferences). Overridden by libvirt for isolation. Default is \~/.local/share | Filepath | 
| `XDG_CACHE_HOME` | Custom user cache directory containing session-local runtime data for user. Default is \~/.cache | Filepath | 
| `XDG_CONFIG_HOME` | Directory for user configuration files, commonly UI themes or custom keymaps. Default is \~/.config | Filepath | 
| `SSH_ASKPASS_REQUIRE` | Controls whether ssh invokes GUI-based password prompt via `SSH_ASKPASS`; set to `yes` to force GUI prompt even if no TTY | enum | 
| `SSH_ASKPASS` | File path to a graphical SSH password prompt binary, typically ssh-askpass, ksshaskpass. | executable filepath | 
| `LD_LIBRARY_PATH` | Overrides library search paths; useful for debugging alternative GTK or QEMU builds. | colon-separated directory paths | 
| `MESA_LOADER_DRIVER_OVERRIDE` | Forces specific Mesa driver (e.g., `iris`, `i965`) for QEMU's GTK UI when debugging GPU passthrough. | driver name string | 
| `LIBGL_ALWAYS_SOFTWARE` | Forces Mesa to use software OpenGL rendering; useful in headless or virtualized desktop setups. | Boolean string | 
| `QEMU_NB_PROCESSORS` | limits internal QEMU thread usage, commonly for benchmarking or constrained hosts. | integer | 

For additional environment variables used by commands evoked by virt-manager, see [virsh](https://wiki.gentoo.org/wiki/Virsh#Environment_variables), [libvirt](https://wiki.gentoo.org/wiki/Libvirt#Environment_variables), [QEMU](https://wiki.gentoo.org/wiki/QEMU#Environment_variables) or consult its man-page.

#### Testing / Internal Use Environment Variables

These are primarily used during unit testing, automated CI runs, or manual debugging of [virt-manager], [virt-install](https://wiki.gentoo.org/wiki/Virt-install), or related [libvirt](https://wiki.gentoo.org/wiki/Libvirt) bindings.

| Environment variable name | Description | 
|---|---|
| `VIRTINST_TEST_SUITE` | Enables virt-install test suite mode; loads dummy data and test scaffolding. | 
| `VIRTINST_TEST_SUITE_FORCE_LIBOSINFO` | Forces the test suite to use `libosinfo` (OS detection database) even if not needed—helps validate OS variant logic. | 
| `VIRT_MANAGER_TEST_SUITE` | Enables virt-manager's GUI test framework—injects mocked UI states. | 
| `VIRT_MANAGER_DEFAULT_FORK` | If set, runs virt-manager in forked subprocess mode even when not launched as `root`. | 
| `VIRTINST_TEST_DATA_DIR` | overrides the path used to load test data (OS XMLs, install trees, etc.). | 
| `VIRTINST_TEST_OS_LIST` | Replaces the default OS list with a test-specific one (used in combination with mock libosinfo). | 
| `PYTHONWARNINGS=default` | Ensures warnings are printed during test runs—important.  Typical value: `default`. | 

### Files

Whenever a domain starts, virt-manager checks for that [domain](https://wiki.gentoo.org/wiki/Libvirt/domain) XML file in each of the following paths:

- System connection: /etc/libvirt/qemu/ via qemu:///system.
- Session (per-user) connection: $HOME/.config/libvirt/qemu/ via qemu:///session.



#### Libvirt

For virt-manager, files are organized as:

- /etc/libvirt/ -> definitions (XML)
- /var/lib/libvirt/ -> persistent VM state (disks, nvram)
- /run/libvirt/ -> runtime execution state
- /var/log/libvirt/ -> logs

| Path or File | Description | 
|---|---|
| /etc/libvirt/qemu/networks/ | Virtual network configs | 
| /etc/libvirt/storage/ | Storage pool metadata | 
| /etc/apparmor.d/ or /etc/selinux/ | Security profiles (optional) | 
| /proc/sys/kernel/cap\_last\_cap | Exposes the last available Linux capability — used in security context for sandboxing. | 
| /run/libvirt/ or /var/run/libvirt/ | Runtime data (sockets, PIDs, state) | 
| /sys/devices/system/cpu/possible | Lists possible CPUs usable by QEMU for CPU pinning or NUMA-aware configs. | 
| /sys/devices/system/node | NUMA node memory and CPU layout. | 
| /sys/devices/system/node/node0/meminfo |  | 
| /usr/share/OVMF/, /usr/share/qemu/ | Firmware for booting VMs | 
| /var/lib/libvirt/dnsmasq/ | DHCP leases, runtime network info | 
| /var/run/libvirt/libvirt-sock | Control channel to hypervisor controller on local host | 
| $HOME | path of home directory. | 
| $DISPLAY | the display X server and screen to which graphical applications should be sent. | 
| $HOME/.cache/gstreamer-1.0/registry.x86\_64.bin |  | 
| $HOME/.config/dconf/user |  | 
| $HOME/.config/gtk-3.0/colors.css |  | 
| $HOME/.config/gtk-3.0/gtk.css |  | 
| $HOME/.config/gtk-3.0/settings.ini |  | 
| $HOME/.config/gtk-3.0/window\_decorations.css |  | 
| $HOME/.config/user-dirs.dirs |  | 
| $HOME/.local/share/mime//mime.cache |  | 

#### System connection (qemu:///system)

| Path or File | Description | 
|---|---|
| /etc/libvirt/qemu/ | Base directory for system connections; contains domain config files (system) in XML format | 
| /etc/libvirt/qemu/autostart/ | auto-start domain(s) in system context; contains only symlinks to VM XMLs in /etc/libvirt/qemu/ | 
| /etc/libvirt/qemu/networks/ | Network definitions; contains | 
| /var/lib/libvirt/images/ | Default VM disk image storage for system connections; contains very large files having .qcow2, .raw format.  Sometimes used as ISO storage (but not required).  Found in **\<source file='/var/lib/libvirt/images/disk.qcow2'/>** statements in domain XML files. | 
| /var/lib/libvirt/qemu/nvram/ | NVRAM (UEFI state variables); contains OVMF\_VARS\*.fd files (UEFI variable stores per-VM) | 
| /var/log/libvirt/ | Logs by domain. Each log file follows the /var/log/libvirt/\<domain-name>.log filename template | 
| /var/lib/libvirt/dnsmasq/ | DHCP leases, runtime network info | 
| /var/run/libvirt/libvirt-sock | Control channel to hypervisor controller on local host | 

#### Session connection (qemu:///session)

| Path or File | Description | 
|---|---|
| $HOME/.config/libvirt/qemu/ | Primary directory for session connectionss; contains domain config files in XML format | 
| $HOME/.config/libvirt/qemu/autostart/ | Symlinks to VM XMLs in /etc/libvirt/qemu/ that auto-start in context of a session connection | 
| $HOME/.config/libvirt/qemu/images/ | Default VM disk image storage, typically not used in session, but may appear; usually found in /var/lib/libvirt/images. | 
| $HOME/.config/libvirt/qemu/networks/ | Network definitions (session-specific, uncommon); session (per-user) networks are usually in-memory only, but if persisted, goes here. May include | 
| $HOME/.config/libvirt/qemu/nvram/ | NVRAM (UEFI state variables); typically not used in session, but may appear; contains OVMF\_VARS\*.fd files (UEFI variable stores per-VM) | 
| $HOME/.config/libvirt/qemu/secrets/ | Secrets (if used in context of a session connection; contains: XML secret definitions, key material references | 
| $HOME/.cache/libvirt/qemu/log/ | Logs (indirectly referenced, not usually stored here), not typically inside this directory, but related paths exist. Usually goes into /var/log/libvirt/\<domain-name>.log | 
| $HOME/.cache/virt-manager/virt-manager.log | Log file for session connections | 

#### Storage Pool

User-defined storage pools are an alternative to images/ directory.

Libvirt can define arbitrary storage pools, for example:

/srv/vm-storage/
   \~/vm-disks/

Defined via:

virsh pool-define
   virsh pool-start

These are independent of /var/lib/libvirt/images/.



### Netfilter

Virt-manager also reserves the following names of base chains.

| Family | Chain type | Base chain name used | 
|---|---|---|
| ip | filter | LIBVIRT\_INP | 
| ip | filter | INPUT | 
| ip | filter | LIBVIRT\_OUT | 
| ip | filter | OUTPUT | 
| ip | filter | LIBVIRT\_FWD | 
| ip | filter | FORWARD | 
| ip | filter | LIBVIRT\_FWI | 
| ip | filter | LIBVIRT\_FWX | 
| ip | nat | nat | 
| ip | nat | LIBVIRT\_PRT | 
| ip | nat | POSTROUTING | 
| ip | mangle | LIBVIRT\_PRT | 
| ip | mangle | POSTROUTING | 

### Networking

virt-manager supports the following network types:

| GUI name | \`libvirt\` XML name | Description | Use Case | Features | 
|---|---|---|---|---|
| **Virtual network 'default': NAT** | `<interface type='network'>` -> `<nowiki><source network='default'/></nowiki>` | libvirt-managed NAT network (usually 192.168.122.0/24) | General purpose, internet access out of the box, | NAT, DHCP, DNS (dnsmasq), IPv4 & IPv6, port forwarding, isolated from host | 
| **Virtual network ... : NAT** | same as above, but any custom libvirt virtual network with `<forward mode='nat'/>` | User-created NAT networks (you can change subnet, DHCP range, etc.) | Multiple isolated NAT segments | Same as default + custom ranges | 
| **Virtual network ... : Route** | `<forward mode='route'/>` | libvirt virtual network that routes (no NAT) to a physical network | Guests need direct access to corporate LAN but still isolated from host | Routing, DHCP, DNS, no address translation | 
| **Virtual network ... : None (Isolated)** | `<forward mode='none'/>` | Completely isolated virtual switch, no external connectivity | Lab, testing, security | Only inter-guest communication on the same network | 
| **Virtual network ... : Open vSwitch** | `<virtualport type='openvswitch'/>` | libvirt virtual network backed by Open vSwitch instead of linux bridge | Advanced SDN, VLAN tagging, flow rules | Everything bridges do + OpenFlow, VLANs inside the virtual network | 
| **Bridge device** (e.g., `br0`) | `<interface type='bridge'> -> <source bridge='br0'/>` | Direct attachment to a host bridge (usually linux bridge or OVS bridge you created manually) | Maximum performance, same L2 segment as host | Full host networking, SR-IOV, macvtap passthrough, VLAN tagging | 
| **MacVTap** (VEPA/Bridge/Private/Passthrough) | `<interface type='direct'> (libvirt)` -> shown in virt-manager as "**Host device ethX (Bridge)**" or "**MacVTap device**" | Direct macvtap connection to a physical NIC | Near-native performance, SR-IOV-like without VF | VEPA, Bridge, Private, Passthrough modes; no guest-to-host traffic in some modes | 
| **Host device passthrough** (PCI) | `<interface type='hostdev'>` | Full PCI device (NIC) assigned to guest (VFIO) | SR-IOV VF or entire physical NIC dedicated to one guest | Complete isolation, maximum performance | 
| **Generic Ethernet** (userspace) | `<interface type='ethernet'>` | Userspace networking via `tun`/`tap` created by QEMU (rarely used) | Legacy or special scripting scenarios | Slow, deprecated | 
| **vHost-user** (DPDK / OVS-DPDK) | `<interface type='vhostuser'>` | High-speed userspace networking with DPDK/OVS-DPDK | 100 Gbps+ guest networking | Requires special QEMU + OVS-DPDK setup | 
| **Network from NetworkManager** | libvirt integrates with [NetworkManager](https://wiki.gentoo.org/wiki/NetworkManager) bridges | Same as “Bridge device” but created automatically by NetworkManager | When you use nmcli to create a bridge and libvirt detects it | Desktop users who manage bridges via NetworkManager | 

### User permissions

To gain privilege in using virt-manager for a session-based domain, check for the **libvirt** group in user's supplementary group list:

`host$``id`
uid=1000(jdoe) gid=1000(jdoe) groups=1000(jdoe),24(cdrom),25(floppy),27(sudo),29(audio),30(dip),44(video),46(plugdev),100(users),102(input),104(kvm),106(netdev),111(bluetooth),113(lpadmin),117(scanner),130(libvirt),131(wireshark),132(pipewire),993(ssh)

If **libvirt** is in the supplementary group, then virt-manager is ready for session (non-root) usage. Skip this subsection.

If **libvirt** is missing, complete the [Group permission setup for Libvirt](https://wiki.gentoo.org/wiki/Libvirt#permissions).

### Autostart

AutoStart feature enables a domain to power up automatically after the host or X session gets ready.

See [AutoStart in Libvirt](https://wiki.gentoo.org/wiki/Libvirt#Autostart) for autostart features.

### Service

To check if the hypervisor controller (libvirtd) is up, run:

`host$``virsh list --all`
If it works and lists the domains (or "no domains defined"), the daemon is running.

If it errors like:

error: failed to connect to the hypervisor

then the daemon is likely down or you're not using the right [URI](https://wiki.gentoo.org/wiki/Libvirt#URI).

## Usage

A hypervisor (local or remote) must be up and running before virt-manager can start controlling a domain.  See [Libvirt service](https://wiki.gentoo.org/wiki/Libvirt#service) on starting an hypervisor.

The virt-manager can start domains in either system (root) or session (user) connection mode. For persistent domain operation, use system. For desktop session only (it goes away at logout), use session.



### Connect types

virt-manager can connect to multiple **local hosts** and **remote hosts** using different protocols.

The syntax of the URI is:

hypervisor+transport://\[user@\]hostname\[:port\]\[/path\]
   hypervisor+transport:///path

See [URI in Libvirt](https://wiki.gentoo.org/wiki/Libvirt#URI) for more breakdown details of Connect Type URI.



### Starting

To start virt-manager:

`host$``virt-manager`
To start virt-manager with the configuration window to gentoo **guest host**, using default QEMU UNIX socket connection:

`host$``virt-manager --connect qemu:///system --show-domain-editor gentoo`


### Starting domain

Domain can be started in:

- virt-manager domain window, by clicking on `Power On` icon button with tip "Power on the virtual machine"
- virt-manager domain window, by **Virtual Machine->Run** menu options
- virt-manager main window, by selecting a domain in the main panel under "QEMU/KVM User Session" group, then right-mouse context menu, then **Run**
- by the **AutoStart** init script executed by display manager upon user login.



### Shutting down

- From the host platform:
  - In virt-manager main menu bar using **Virtual Machine -> Shutdown**
- From within the guest host:
  - From the window manager, Start -> Shutdown icon



### Invocation

`user $``virt-manager --help````
usage: virt-manager [options]
optional arguments:
  -h, --help            show this help message and exit
  --version             show program's version number and exit
  -c URI, --connect URI
                        Connect to hypervisor at URI
  --debug               Print debug output to stdout (implies --no-fork)
  --no-fork             Don't fork into background on startup
  --show-domain-creator
                        Show 'New VM' wizard
  --show-domain-editor NAME|ID|UUID
                        Show domain details window
  --show-domain-performance NAME|ID|UUID
                        Show domain performance window
  --show-domain-console NAME|ID|UUID
                        Show domain graphical console window
  --show-domain-delete NAME|ID|UUID
                        Show domain delete window
  --show-host-summary   Show connection details window
Also accepts standard GTK arguments like --g-fatal-warnings
```
## Removal

Removal of [app-emulation/libvirt](https://packages.gentoo.org/packages/app-emulation/libvirt) package (toolkit, library, and utilities) can be done by executing:

`root #``emerge --ask --depclean --verbose app-emulation/libvirt`
## Troubleshooting

#### virt-manager gui doesn't start or "virt-manager: command not found"

Version 4.1.0 changed USE flags, switching from `gtk` flag to `gui`. In order to (re)enable gui, the user must enable the `gui` flag before (re)build the package.

`host-root#``echo "app-emulation/virt-manager gui" >> /etc/portage/package.use/app-emulation`


#### no polkit agent available to authenticate action 'org.libvirt.unix.manage'

This usually results from the user not being in the `libvirt` group. Add the user to the group with:

`host-root#``usermod -aG libvirt larry`


#### QEMU/KVM not connected

Virt-manager uses libvirt as its backend to manage virtual machines. Therefore, the libvirt daemon needs to be started.

`host-root#``libvirtd`
Or to start libvirtd at startup, add the daemon to the OpenRC runlevel / systemd target:

In OpenRC boot environment:

`host-root#``rc-update add libvirtd default`
In SystemD boot environment:

`host-root#``systemctl enable libvirtd`


#### 'NoneType' object has no attribute 'conn'

This is typically a result in the cursor settings being misconfigured. The simplest fix is installing [x11-themes/adwaita-icon-theme](https://packages.gentoo.org/packages/x11-themes/adwaita-icon-theme) and updating gsettings to use the Adwaita cursor theme.

`root #``emerge --ask x11-themes/adwaita-icon-theme``host$``gsettings set org.gnome.desktop.interface cursor-theme "Adwaita"`


### Missing **default** network

If you accidentally deleted the 'default' network in virt-manager, to restore **default** network using:

On some system, the default.xml can be found in /usr/share/libvirt/networks/default.xml. If not, create a new default.xml XML file for the default network:

**`/etc/libvirt/qemu/networks/default.xml`**



#### Within virt-manager

Go to main dialog of virt-manager:
![virt-manager main window](https://wiki.gentoo.org/images/2/20/Virt-manager--4.0-main-window.png)


Select the **QEMU/KVM** group item in main window.

At the menubar, select **Edit**, then **Connection details**.

A new **Connection details** dialog box appears.

Select **Virtual connections** tab.

In left navigation window, at the bottom, press the add ('+') button.  A 3rd dialog window titled **Create a new virtual network** appears.

Select the **XML** group tab.

Paste the content of default.xml given above. UUID will automatically be filled in.

#### Using virsh

Or you can use virsh directly:

You can generate a UUID using the command:

`root #``uuidgen`
Insert the UUID into the default.xml file:

`root #``virsh net-define /usr/share/libvirt/networks/default.xml`
Set auto-start to enable:

`root #``virsh net-autostart default`
Start the **default** network:

`root #``virsh net-start default`


#### Confirm **default** network exist

To confirm the network is active

`root #``virsh net-list --all`
Name      State    Autostart   Persistent
--------------------------------------------
 default   active   yes         yes

## See also

- [Libvirt/domain](https://wiki.gentoo.org/wiki/Libvirt/domain) — represents a virtualized guest instance, such as a virtual machine, container, or other supported environment.
- [Virtualization](https://wiki.gentoo.org/wiki/Virtualization) — the concept and technique that permits running software in an environment separate from a computer operating system.
- [QEMU](https://wiki.gentoo.org/wiki/QEMU) — a generic, open-source hardware emulator and virtualization suite.
- [QEMU/Front-ends](https://wiki.gentoo.org/wiki/QEMU/Front-ends) — provide graphical, terminal, web-based, or command-line interfaces for configuring, managing, or accessing QEMU virtual machines.

- [Libvirt](https://wiki.gentoo.org/wiki/Libvirt) — a virtualization management toolkit
- [Libvirt/QEMU\_networking](https://wiki.gentoo.org/wiki/Libvirt/QEMU_networking) — details the setup of Gentoo networking by [Libvirt](https://wiki.gentoo.org/wiki/Libvirt) for use by guest containers and [QEMU](https://wiki.gentoo.org/wiki/QEMU)-based virtual machines.
- [Libvirt/QEMU\_guest](https://wiki.gentoo.org/wiki/Libvirt/QEMU_guest) — creation of a guest domain (virtual machine, VM), running inside a QEMU hypervisor, using tools found in [libvirt](https://packages.gentoo.org/packages/libvirt) package.

- [Virt-manager/QEMU\_guest](https://wiki.gentoo.org/wiki/Virt-manager/QEMU_guest) — creation of a guest virtual machine (VM) running inside a QEMU hypervisor using just the virt-manager GUI tool.

- [QEMU/Linux guest](https://wiki.gentoo.org/wiki/QEMU/Linux_guest) — describes the setup of a Gentoo Linux guest in [QEMU](https://wiki.gentoo.org/wiki/QEMU) using Gentoo bootable media.
- [GPU passthrough with virt-manager, QEMU, and KVM](https://wiki.gentoo.org/wiki/GPU_passthrough_with_virt-manager,_QEMU,_and_KVM) — directly present an internal PCI GPU as-is for direct use by a virtual machine
- [Virsh](https://wiki.gentoo.org/wiki/Virsh) — a CLI-based [virtualization](https://wiki.gentoo.org/wiki/Virtualization) management toolkit
