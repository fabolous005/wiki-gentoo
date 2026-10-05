<!-- source: https://wiki.gentoo.org/wiki/Virt-install | group: Gentoo Wiki (Main) | wiki-title: Virt-install -->
---
title: virt-install
url: https://wiki.gentoo.org/wiki/Virt-install
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-01"
fingerprint: f898a3c0fb273e8
license: CC BY-SA 4.0
---

# virt-install

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Resources**

virt-install is a CLI-based [virtualization](https://wiki.gentoo.org/wiki/Virtualization) machine creator utility.

The program is used to create a new virtual machine.



## Installation

See [virt-manager](https://wiki.gentoo.org/wiki/Virt-manager) for the installation of [virt-install]. It is installed by [app-emulation/virt-manager](https://packages.gentoo.org/packages/app-emulation/virt-manager).

#### System user/group

For non-root usage of virt-install, user must have group name libvirt as part of its supplemental group.   Defined by [acct-group/libvirt](https://packages.gentoo.org/packages/acct-group/libvirt); evoked by [app-emulation/libvirt](https://packages.gentoo.org/packages/app-emulation/libvirt) package.

## Configuration

### Environment variables

The following environment variables are used by virt-install:

| Variable | Description | 
|---|---|
| DISPLAY | Specifies the X display used by VirtualBox. | 
| GSETTINGS\_SCHEMA\_DIR | Specifies the directory containing GSettings schema files. | 
| GSETTINGS\_BACKEND | Selects the GSettings backend. | 
| PATH | Specifies the command search path. | 
| VIRTINSTALL\_OSINFO\_DISABLE\_REQUIRE | Disables the requirement for osinfo data and ignores the --osinfo command-line option. | 
| WAYLAND\_DISPLAY | Specifies the Wayland display. | 
| XDG\_CACHE\_HOME | Specifies the base directory for user-specific cached data. | 
| XDG\_DATA\_HOME | Specifies the base directory for user-specific application data. | 

The following environment variables are used by the virt-install test suite:

| Variable | Description | 
|---|---|
| VIRTINST\_TEST\_SUITE | Enables the virt-install test suite. | 
| VIRTINST\_TESTSUITE\_HACK\_DESTROY | Enables test-suite handling for automatic domain destruction. | 
| VIRTINST\_TEST\_SUITE\_FORCE\_LIBOSINFO | Forces the test suite to use libosinfo. | 
| VIRTINST\_TEST\_SUITE\_PRINT\_CLOUDINIT | Prints generated cloud-init configuration during testing. | 
| VIRTINST\_TEST\_SUITE\_INCREMENT\_MACADDR | Increments generated MAC addresses during testing. | 
| VIRTINST\_TEST\_SUITE\_FAKE\_NO\_SPICE | Simulates the absence of SPICE support during testing. | 
| VIRTXML\_TESTSUITE\_UPDATE\_IGNORE\_FAIL | Allows test-suite updates to continue when an update fails. | 

### Files

Files accessed by virt-install or used to locate hypervisor backends include:

| Path | Description | 
|---|---|
| /usr/bin/kvm | KVM executable. | 
| /usr/lib/libvirt/libvirt\_lxc | libvirt LXC driver executable. | 
| /usr/lib64/libvirt/libvirt\_lxc | libvirt LXC driver executable. | 
| /usr/bin/qemu-kvm | QEMU KVM executable. | 
| /usr/libexec/qemu-kvm | QEMU KVM executable. | 
| /usr/bin/qemu-system-\* | QEMU system-emulation executable for the selected target architecture. | 

### User permissions

To use virt-install as a non-root user, ensure each user has been added to the libvirt group:

`root #``gpasswd -a <user> libvirt`
See [libvirt configuration](https://wiki.gentoo.org/wiki/Libvirt#Configuration) for more setup on enabling user to use the virt-install command.

## Usage

### Invocation

In case the mind slips, the group of virt-install-related options can be obtained by doing:

`user $``virt-install --help````
$ virt-install --help
usage: virt-install --name NAME --memory MB STORAGE INSTALL [options]
Create a new virtual machine from specified install media.
options:
  -h, --help            show this help message and exit
  --version             show program's version number and exit
  --connect URI         Connect to hypervisor with libvirt URI
General Options:
  -n NAME, --name NAME  Name of the guest instance
  --memory MEMORY       Configure guest memory allocation. Ex:
                        --memory 1024 (in MiB)
                        --memory memory=1024,currentMemory=512
  --vcpus VCPUS         Number of vCPUs to configure for your guest. Ex:
                        --vcpus 5
                        --vcpus 5,maxvcpus=10,cpuset=1-4,6,8
                        --vcpus sockets=2,cores=4,threads=2
  --cpu CPU             CPU model and features. Ex:
                        --cpu coreduo,+x2apic
                        --cpu host-passthrough
                        --cpu host
  --metadata METADATA   Configure guest metadata. Ex:
                        --metadata name=foo,title="My pretty title",uuid=...
                        --metadata description="My nice long description"
  --xml XML             Perform raw XML XPath options on the final XML. Example:
                        --xml ./cpu/@mode=host-passthrough
                        --xml ./devices/disk[2]/serial=new-serial
                        --xml xpath.delete=./clock
Installation Method Options:
  --cdrom CDROM         CD-ROM installation media
  -l LOCATION, --location LOCATION
                        Distro install URL, eg. https://host/path. See man page for specific distro examples.
  --pxe                 Boot from the network using the PXE protocol
  --import              Build guest around an existing disk image
  -x EXTRA_ARGS, --extra-args EXTRA_ARGS
                        Additional arguments to pass to the install kernel booted from --location
  --initrd-inject INITRD_INJECT
                        Add given file to root of initrd from --location
  --unattended [UNATTENDED]
                        Perform an unattended installation
  --install INSTALL     Specify fine grained install options
  --reinstall DOMAIN    Reinstall existing VM. Only install options are applied, all other VM configuration options are ignored.
  --cloud-init [CLOUD_INIT]
                        Perform a cloud image installation, configuring cloud-init
  --boot BOOT           Configure guest boot settings. Ex:
                        --boot hd,cdrom,menu=on
                        --boot init=/sbin/init (for containers)
  --idmap IDMAP         Enable user namespace for LXC container. Ex:
                        --idmap uid.start=0,uid.target=1000,uid.count=10
OS options:
  --os-variant OS_VARIANT, --osinfo OS_VARIANT
                        The OS being installed in the guest.
                        This is used for deciding optimal defaults like VirtIO.
                        Example values: fedora29, rhel7.0, win10, ...
                        Use '--osinfo list' to see a full list.
Device Options:
  --disk DISK           Specify storage with various options. Ex.
                        --disk size=10 (new 10GiB image in default location)
                        --disk /my/existing/disk,cache=none
                        --disk device=cdrom,bus=scsi
                        --disk=?
  -w NETWORK, --network NETWORK
                        Configure a guest network interface. Ex:
                        --network bridge=mybr0
                        --network network=my_libvirt_virtual_net
                        --network network=mynet,model=virtio,mac=00:11...
                        --network none
                        --network help
  --graphics GRAPHICS   Configure guest display settings. Ex:
                        --graphics spice
                        --graphics vnc,port=5901,listen=0.0.0.0
                        --graphics none
  --controller CONTROLLER
                        Configure a guest controller device. Ex:
                        --controller type=usb,model=qemu-xhci
                        --controller type=scsi,model=virtio-scsi
  --input INPUT         Configure a guest input device. Ex:
                        --input tablet
                        --input keyboard,bus=usb
  --serial SERIAL       Configure a guest serial device
  --parallel PARALLEL   Configure a guest parallel device
  --channel CHANNEL     Configure a guest communication channel
  --console CONSOLE     Configure a text console connection between the guest and host
  --hostdev HOSTDEV     Configure physical USB/PCI/etc host devices to be shared with the guest
  --filesystem FILESYSTEM
                        Pass host directory to the guest. Ex: 
                        --filesystem /my/source/dir,/dir/in/guest
                        --filesystem template_name,/,type=template
  --sound [SOUND]       Configure guest sound device emulation
  --audio AUDIO         Configure host audio backend for sound devices
  --watchdog WATCHDOG   Configure a guest watchdog device
  --video VIDEO         Configure guest video hardware.
  --smartcard SMARTCARD
                        Configure a guest smartcard device. Ex:
                        --smartcard mode=passthrough
  --redirdev REDIRDEV   Configure a guest redirection device. Ex:
                        --redirdev usb,type=tcp,server=192.168.1.1:4000
  --memballoon MEMBALLOON
                        Configure a guest memballoon device. Ex:
                        --memballoon model=virtio
  --tpm TPM             Configure a guest TPM device. Ex:
                        --tpm /dev/tpm
  --rng RNG             Configure a guest RNG device. Ex:
                        --rng /dev/urandom
  --panic PANIC         Configure a guest panic device. Ex:
                        --panic default
  --shmem SHMEM         Configure a guest shared memory device. Ex:
                        --shmem name=shmem0
  --memdev MEMDEV       Configure a guest memory device. Ex:
                        --memdev dimm,target.size=1024
  --vsock VSOCK         Configure guest vsock sockets. Ex:
                        --vsock cid.auto=yes
                        --vsock cid.address=7
  --iommu IOMMU         Configure an IOMMU device. Ex:
                        --iommu model=intel,driver.aw_bits=48
Guest Configuration Options:
  --iothreads IOTHREADS
                        Set domain <iothreads> and <iothreadids> configuration.
  --seclabel SECLABEL, --security SECLABEL
                        Set domain seclabel configuration.
  --keywrap KEYWRAP     Set guest to perform the S390 cryptographic key management operations.
  --cputune CPUTUNE     Tune CPU parameters for the domain process.
  --numatune NUMATUNE   Tune NUMA policy for the domain process.
  --memtune MEMTUNE     Tune memory policy for the domain process.
  --blkiotune BLKIOTUNE
                        Tune blkio policy for the domain process.
  --memorybacking MEMORYBACKING
                        Set memory backing policy for the domain process. Ex:
                        --memorybacking hugepages=on
  --features FEATURES   Set domain <features> XML. Ex:
                        --features acpi=off
                        --features apic=on,apic.eoi=on
  --clock CLOCK         Set domain <clock> XML. Ex:
                        --clock offset=localtime,rtc_tickpolicy=catchup
  --pm PM               Configure VM power management features
  --events EVENTS       Configure VM lifecycle management policy
  --resource RESOURCE   Configure VM resource partitioning (cgroups)
  --sysinfo SYSINFO     Configure SMBIOS System Information. Ex:
                        --sysinfo host
                        --sysinfo bios.vendor=MyVendor,bios.version=1.2.3,...
  --qemu-commandline QEMU_COMMANDLINE
                        Pass arguments directly to the QEMU emulator. Ex:
                        --qemu-commandline='-display gtk,gl=on'
                        --qemu-commandline env=DISPLAY=:0.1
  --launchSecurity LAUNCHSECURITY, --launchsecurity LAUNCHSECURITY
                        Configure VM launch security (e.g. SEV memory encryption). Ex:
                        --launchSecurity sev
Virtualization Platform Options:
  -v, --hvm             This guest should be a fully virtualized guest
  -p, --paravirt        This guest should be a paravirtualized guest
  --container           This guest should be a container guest
  --virt-type VIRT_TYPE
                        Hypervisor name to use (kvm, qemu, xen, ...)
  --arch ARCH           The CPU architecture to simulate
  --machine MACHINE     The machine type to emulate
Miscellaneous Options:
  --autostart           Have domain autostart on host boot up.
  --transient           Create a transient domain.
  --destroy-on-exit     Force power off the domain when the console viewer is closed.
  --wait [WAIT]         Minutes to wait for install to complete.
  --autoconsole AUTOCONSOLE
                        Configure guest console auto connect. Example:
                        --autoconsole text
                        --autoconsole graphical
                        --autoconsole none
  --noautoconsole       Don't automatically try to connect to the guest console
  --noreboot            Don't boot guest after completing install.
  --print-xml [XMLONLY]
                        Print the generated domain XML rather than create the guest.
  --dry-run             Run through install process, but do not create devices or define the guest.
  --check CHECK         Enable or disable validation checks. Example:
                        --check path_in_use=off
                        --check all=off
  -q, --quiet           Suppress non-error output
  -d, --debug           Print debugging information
Use '--option=?' or '--option help' to see available suboptions
See man page for examples and full option syntax.
```
Then you can add the `help` suboption for more detailed options specific to, for example, the virt-install --osinfo help command:

`user $``virt-install --osinfo help`
$ virt-install --osinfo help
--os-variant options:
  detect
  id
  name
  require
  short-id

Or for more help with CPU options:

`user $``virt-install --cpu help`
$ virt-install --cpu help   
--cpu options:
  clearxml
  xpath\[0-9\]\*.create
  xpath\[0-9\]\*.delete
  xpath\[0-9\]\*.set
  xpath\[0-9\]\*.value
  cache.level
  cache.mode
  check
  disable
  forbid
  force
  match
  migratable
  mode
  model
  model.fallback
  model.vendor\_id
  numa.cell\[0-9\]\*.cache\[0-9\]\*.associativity
  numa.cell\[0-9\]\*.cache\[0-9\]\*.level
  numa.cell\[0-9\]\*.cache\[0-9\]\*.line.unit
  numa.cell\[0-9\]\*.cache\[0-9\]\*.line.value
  numa.cell\[0-9\]\*.cache\[0-9\]\*.policy
  numa.cell\[0-9\]\*.cache\[0-9\]\*.size.unit
  numa.cell\[0-9\]\*.cache\[0-9\]\*.size.value
  numa.cell\[0-9\]\*.cpus
  numa.cell\[0-9\]\*.discard
  numa.cell\[0-9\]\*.distances.sibling\[0-9\]\*.id
  numa.cell\[0-9\]\*.distances.sibling\[0-9\]\*.value
  numa.cell\[0-9\]\*.id
  numa.cell\[0-9\]\*.memAccess
  numa.cell\[0-9\]\*.memory
  numa.cell\[0-9\]\*.unit
  numa.interconnects.bandwidth\[0-9\]\*.cache
  numa.interconnects.bandwidth\[0-9\]\*.initiator
  numa.interconnects.bandwidth\[0-9\]\*.target
  numa.interconnects.bandwidth\[0-9\]\*.type
  numa.interconnects.bandwidth\[0-9\]\*.unit
  numa.interconnects.bandwidth\[0-9\]\*.value
  numa.interconnects.latency\[0-9\]\*.cache
  numa.interconnects.latency\[0-9\]\*.initiator
  numa.interconnects.latency\[0-9\]\*.target
  numa.interconnects.latency\[0-9\]\*.type
  numa.interconnects.latency\[0-9\]\*.unit
  numa.interconnects.latency\[0-9\]\*.value
  optional
  require
  secure
  topology.cores
  topology.dies
  topology.sockets
  topology.threads
  vendor



## Removal

Removal of libvirt package (toolkit, library, and utilities) can be done by executing:

`root #``emerge --ask --depclean --verbose app-emulation/libvirt`


## See also

- [QEMU](https://wiki.gentoo.org/wiki/QEMU) — a generic, open-source hardware emulator and virtualization suite.
- [QEMU/Front-ends](https://wiki.gentoo.org/wiki/QEMU/Front-ends) — provide graphical, terminal, web-based, or command-line interfaces for configuring, managing, or accessing QEMU virtual machines.
- [Libvirt](https://wiki.gentoo.org/wiki/Libvirt) — a virtualization management toolkit
- [Libvirt/QEMU\_networking](https://wiki.gentoo.org/wiki/Libvirt/QEMU_networking) — details the setup of Gentoo networking by [Libvirt](https://wiki.gentoo.org/wiki/Libvirt) for use by guest containers and [QEMU](https://wiki.gentoo.org/wiki/QEMU)-based virtual machines.
- [Virt-manager](https://wiki.gentoo.org/wiki/Virt-manager) — lightweight GUI application designed for managing virtual machines and containers via the [libvirt](https://wiki.gentoo.org/wiki/Libvirt) API.
- [Virt-manager/QEMU\_guest](https://wiki.gentoo.org/wiki/Virt-manager/QEMU_guest) — creation of a guest virtual machine (VM) running inside a QEMU hypervisor using just the virt-manager GUI tool.
- [QEMU/Linux guest](https://wiki.gentoo.org/wiki/QEMU/Linux_guest) — describes the setup of a Gentoo Linux guest in [QEMU](https://wiki.gentoo.org/wiki/QEMU) using Gentoo bootable media.
