<!-- source: https://wiki.gentoo.org/wiki/LXC | group: Gentoo Wiki (Main) | wiki-title: LXC -->
---
title: LXC
url: https://wiki.gentoo.org/wiki/LXC
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-14"
fingerprint: be98f9790ba713a2
license: CC BY-SA 4.0
---

# LXC

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**LXC** (Linux Containers) is a [virtualization](https://wiki.gentoo.org/wiki/Virtualization) system that leverages Linux's [namespaces](https://wiki.gentoo.org/wiki/Namespaces) and [cgroups](https://wiki.gentoo.org/wiki/Cgroups) to create containers isolated from the host system.

LXC is conceptually similar to [Solaris's Zones](https://en.wikipedia.org/wiki/Solaris_Zones) and [FreeBSD's Jails](https://en.wikipedia.org/wiki/FreeBSD_Jail), providing more segregation than a simple [chroot](https://wiki.gentoo.org/wiki/Chroot), without introducing the overhead of hardware virtualization. Unlike [systemd-nspawn](https://wiki.gentoo.org/wiki/Systemd/systemd-nspawn) but similarly to other OS-level virtualization technologies on Linux such as [Docker](https://wiki.gentoo.org/wiki/Docker), LXC optionally permits containers to be spawned by an unprivileged user.

[LXD](https://wiki.gentoo.org/wiki/LXD) uses LXC through *liblxc* and its Go binding to help create and manage containers. [Incus](https://wiki.gentoo.org/wiki/Incus), a fork of LXD, is also available.

## Introduction

### Virtualization

Roughly speaking there are two main types of virtualization, container-based virtualization and full virtualization.

#### Container-based virtualization

[Container based virtualization](https://en.wikipedia.org/wiki/OS-level_virtualization), such as LXC or [Docker](https://wiki.gentoo.org/wiki/Docker), is very fast and efficient. It's based on the premise that an OS kernel provides different views of the system to different running processes. This compartmentalization, sometimes called *thick sandboxing*, can be useful for ensuring access to hardware resources such as CPU and IO bandwidth, whilst maintaining security and efficiency.

Conceptually, containers are very similar to using a *chroot*, but offer far more isolation and control, mostly through the use of *cgroups* and *namespace*s. A *chroot* only provides filesystem isolation, while namespaces can provide network and process isolation. *cgroups* can be used to manage container resource allocation.

#### Full virtualization

Full virtualization and paravirtualization solutions, such as [KVM](https://wiki.gentoo.org/wiki/KVM), [Xen](https://wiki.gentoo.org/wiki/Xen), [VirtualBox](https://wiki.gentoo.org/wiki/VirtualBox), and [VMware](https://wiki.gentoo.org/wiki/VMware), aim to simulate the underlying hardware. This method of virtualization results in an environment which provides more isolation, and is generally more compatible, but requires far more overhead.

### LXC limitations

#### Isolation

LXC provides better isolation than a chroot, but many resources can be, or are still shared. Root users in privileged containers have the same level of system access as the root user outside of the container, unless ID mapping is in place.

If the kernel is built allowing a [Magic SysRQ](https://wiki.gentoo.org/wiki/Magic_SysRQ) with `CONFIG_MAGIC_SYSRQ`, a container could abuse this to perform a Denial of Service.

#### Unprivileged Containers

By design, unprivileged containers cannot do several things which require privileged system access, such as:

- Mounting filesystems
- Creating device nodes
- Operations against a UID/GID outside of the mapped set
- Creation of network interfaces

In order to create network interfaces, LXC uses lxc-user-nic which uses *setuid-root* to create the network interfaces.

#### Different Operating Systems and Architectures

Containers cannot be used to run operating systems or software designed for other architectures or operating systems.

### LXC components

#### Control groups

Control Groups are a multi-hierarchy, multi-subsystem resource management / control framework for the Linux kernel. They can be used for advanced resource control and accounting using groups which could apply to one or more processes.

Some of the resources *cgroups* can manage are<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>:

- Memory usage.
- CPU bandwidth.
- Filesystem usage.
- Process limits.
- Block device IO bandwidth.
- Miscellaneous Devices.

The user-space access to these new kernel features is a kernel-provided filesystem, known as 'cgroup'. It is typically mounted at /sys/fs/cgroup and provides files similar to /proc and /sys, representing the running environment and various kernel configuration options.

#### File locations

LXC information is typically stored under:

- /etc/lxc/default.conf - The default guest configuration file. When new containers are created, these values are added.
- /etc/lxc/lxc.conf - LXC system configuration. See man lxc.conf.
- /etc/lxc/lxc-usernet - LXC configuration for user namespace network allocations.
- /var/lib/lxc/{guest\_name}/config - Main guest configuration file.
- /var/lib/lxc/{guest\_name}/rootfs/ - The root of the guest filesystem.
- /var/lib/lxc/{guest\_name}/snaps/ - Guest snapshots.
- /var/log/lxc/{guest\_name}.log - Guest startup logs.



##### Unprivileged locations

When running unprivileged containers, configuration files are loaded from the following locations, based on the user home:

- /etc/lxc/default.conf -> \~/.config/lxc/default.conf
- /etc/lxc/lxc.conf -> \~/.config/lxc/lxc.conf
- /var/lib/lxc/ -> \~/.local/share/lxc/ - This includes a directory for each guest.

## Installation

### USE flags


| [+caps](https://packages.gentoo.org/useflags/+caps) | Use Linux capabilities library to control privilege | 
| [+tools](https://packages.gentoo.org/useflags/+tools) | Build and install additional command line tools | 
| [apparmor](https://packages.gentoo.org/useflags/apparmor) | Enable support for the AppArmor application security system | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [examples](https://packages.gentoo.org/useflags/examples) | Install examples, usually source code | 
| [io-uring](https://packages.gentoo.org/useflags/io-uring) | Enable the use of io\_uring for efficient asynchronous IO and system requests | 
| [landlock](https://packages.gentoo.org/useflags/landlock) | Use Landlock to restrict what the monitor API handlers can do on the system | 
| [man](https://packages.gentoo.org/useflags/man) | Build and install man pages | 
| [pam](https://packages.gentoo.org/useflags/pam) | Add support for PAM (Pluggable Authentication Modules) - DANGEROUS to arbitrarily flip | 
| [seccomp](https://packages.gentoo.org/useflags/seccomp) | Enable seccomp (secure computing mode) to perform system call filtering at runtime to increase security of programs | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [ssl](https://packages.gentoo.org/useflags/ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [test-install](https://packages.gentoo.org/useflags/test-install) | Install testsuite for manual execution by the user | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

### Kernel

The host's kernel must be configured with Control Group, namespace, and virtual networking support:

**Essential configuration options**

### Emerge

`root #``emerge --ask app-containers/lxc`
## Network Configuration

LXC provides a variety of network options for containers. Container interfaces are defined with `lxc.net.{i}.type`, where *i* is the interface index number.

### General Parameters

The following configuration options apply to all interface types, and are prefixed with `lxc.net.{0}.`:

- **flags** - Specifies an action to perform on the interface, *up* starts the interface on container creation, otherwise it must manually be started with ip.
- **link** - Specifies the host interface the container interface should be associated with.
- **mtu** - Specifies the container interface MTU.
- **name** - Defines the name which will be given to the interface within the container.
- **hwaddrd** - Defines the MAC address which will be allocated to the virtual interface, `X`s in the supplied value will be randomized.
- **ipv4.address** - Specifies the interface's assigned IP address, in ip/cidr format, optionally, a broadcast address can be specified after the cidr to use one other than the last value on that network. Ex. `lxc.net.0.ipv4.address = 192.168.123.1 192.168.123.32`.
- **ipv4.gateway** - Specifies the interface's default gateway.

### Interface Types

LXC offers a variety of interface types, which can be configured using `lxc.net.{i}.type`.

#### none

The **none** network type provides *no network isolation or virtualization*, and is comparable to Docker's *host* networking mode.

#### empty

The **empty** interface type starts the container with only a loopback interface, if no other interface is defined, the container should be isolated from the host's network.

#### phys

The **phys** interface type can be used to give a container access to a single interface on the host system. It must be used along with `lxc.net.{i}.link` which specifies the host interface.

**`/var/lib/lxc/{container name}/config`**

**Passing eth0 to the container**

#### veth

The **veth** or *Virtual Ethernet Pair Device* can be used to *bridge* or *route*, specified with `lxc.net.{i}.mode`, traffic between the host and container using virtual ethernet devices. The default mode of operation is [Network bridge](https://wiki.gentoo.org/wiki/Network_bridge) mode.

##### Bridge Mode

When the `lxc.net.{i}.veth.mode` is not specified, *bridge* mode will be used, which generally expects a *link* to be set to a *bridge* interface on the host system with `lxc.net.{i}.link`. If this link is not set, *veth* interface will be created in the container but not configured on the host.

**`/var/lib/lxc/{container name}/config`**

**Defining a*veth* device connected to the default *lxcbr0* interface**

##### Router Mode

If `lxt.net.{i}.veth.mode = router`, static routes are created between the addresses associated with the specified host link device and the container's veth interface. For *privileged* containers, *ARP* and *NDP* entries are proxied between the host interface and the container's specified *gateway* interface.

**`/var/lib/lxc/{container name}/config`**

**Defining a*routed veth* device associated with *eth0*.**

#### vlan

The **vlan** interface type can be used with a host interface specified with `lxc.net.{i}.link` and *VLAN* id specified with `lxc.net.{i}.vlan.id`. This type is comparable to the **phys** type, but only the specified *VLAN* is shared.

**`/var/lib/lxc/{container name}/config`**

**Defining a**vlan** device on VLAN 100, interface eth0**

#### macvlan

The **macvlan** interface type can be used to share a physical interface with the host. A link must be specified with `lxc.net.{i}.link`. The mode can be configured with `lxc.net.{i}.macvlan.mode` where the default mode is *private* mode.

##### private mode

Setting `lxc.net.{i}.macvlan.mode = private` results in a *macvlan* interface which cannot communicate to the devices *upper\_dev*, or the interface which the *macvlan* interface is based off of.

**`/var/lib/lxc/{container name}/config`**

**Defining a*private* **macvlan** interface on eth0.100**

##### vepa (Virtual Ethernet Port Aggregator)

Setting `lxc.net.{i}.macvlan.mode = vepa` results in a *macvlan* interface which operates similarly to *private* mode, but packets are actually sent to the switch, not virtually bridged. This allows different subinterfaces to communicate with each other using another switch.

**`/var/lib/lxc/{container name}/config`**

**Defining a*vepa* **macvlan** interface on eth0.100**

##### bridge

Setting `lxc.net.{i}.macvlan.mode = bridge` results in a *macvlan* interface which only delivers traffic across the bridge, not allowing outbound traffic.

**`/var/lib/lxc/{container name}/config`**

**Defining two*bridge* **macvlan** interfaces on eth0.100**

##### passthru

Only one subinterface can use `lxc.net.{i}.macvlan.mode = bridge`, this operates similarly to using *phys* or *vlan* mode, but offers more isolation, since the container doesn't actually get access to the physical interface, but the *macvlan* interface which is associated with it.

**`/var/lib/lxc/{container name}/config`**

**Using eth0.100 in*passthru* **macvlan** mode.**

### DNS Configuration

DNS settings can be configured by editing /etc/resolv.conf within the container like it would be done on a normal system.

### Host Configuration

LXC will not automatically create interfaces for containers, even if the container is configured to use something like a bridge on the host.

#### OpenRC Netifrc Bridge creation

[Netifrc](https://wiki.gentoo.org/wiki/Netifrc) can be used to create bridges:

**`/etc/conf.d/net`**

A symbolic link for this interface's init unit can be created with:

`root #``ln -s net.lo net.lxcbr0`
The bridge can be created with:

`root #``rc-service net.lxcbr0 start`
This bridge can be created at boot with:

`root #``rc-update add net.lxcbr0`
#### Nftables NAT rules

This file will be automatically loaded if using [Nftables#Modular\_Ruleset\_Management](https://wiki.gentoo.org/wiki/Nftables#Modular_Ruleset_Management), but can be loaded by making the file executable and running it.

**`/etc/nftables.rules.d/12-lxc_base.rules`**

## Service Configuration

### OpenRC

To create an init script for a specific container, simply create a symbolic link from /etc/init.d/lxc to /etc/init.d/lxc.guestname:

`root #``ln -s lxc /etc/init.d/lxc.`**guestname**
This can be started, then set to start at boot with:

`root #``rc-service lxc.`**guestname**
`root #``rc-update add lxc.`**guestname**
#### Unified Cgroups for Unprivileged Containers

If running unprivileged containers on OpenRC based systems, the `rc_cgroup_mode` must be changed from *hybrid* to *unified* and `rc_cgroup_controllers` must be populated:

**`/etc/rc.conf`**

```
rc_cgroup_mode="unified"
rc_cgroup_controllers="yes"
...
```
### Systemd

To start a container by name:

`root #``systemctl start lxc@`**guestname**.service
To stop ii, issue:

`root #``systemctl stop lxc@`**guestname**.service
To start it automatically at (host) system boot up, use:

`root #``systemctl enable lxc@`**guestname**.service
#### Creating User Namespaces for Unprivileged Containers

Unprivileged containers require cgroups to be delegated in advance. These are not managed by LXC.

`lxc@localhost $````
systemd-run --unit=my-unit --user --scope -p "Delegate=yes" -- lxc-start my-container
```
It is possible to delegate unprivileged cgroups by creating a systemd unit:

**`/etc/systemd/system/user@.service.d/delegate.conf`**

```
[Service]
Delegate=cpu cpuset io memory pids
```
## Unprivileged User configuration

While it is possible to create a single LXC user to run all unprivileged containers with, it's a much better practice to create a LXC user for each container, or groups of related containers.

### User Creation

To create a new user, which will be used to run *dnscrypt* containers:

`root #``useradd -m -G lxc lxc-dnscrypt`
### Network Quota Allocation

Allocations for network device access must be made by editing /etc/lxc/lxc-usernet:

**`/etc/lxc/lxc-usernet`**

**Add allocations for the lxc-dnscrypt user to add 1 veth device on the lxbr0 bridge.**

### Sub UID and GID mapping

Once the user has been created, subUIDs and subGIDs must be mapped.

Current mappings can be checked with:

`user $``cat /etc/subuid`
larry:100000:65536
lxc-dnscrypt:165536:65536

`user $``cat /etc/subgid`
larry:100000:65536
lxc-dnscrypt:165536:65536

### Default container configuration creation

LXC does not create user configuration by default, \~/.config/lxc may need to be created:

`user $``mkdir --parents ~/.config/lxc`
Adding the user ID map information is a good base:

**`~/.config/lxc/default.conf`**

**Add user ID maps to the default container configuration file**

## Guest configuration

### Guest Filesystem Creation

To create the base container fileystem, to be imported with the *local* template, emerge can be used:

`root #``emerge --verbose --ask --root /path/to/build/dir category/packagename`
Once the build has completed, it can be archived with:

`root #``tar -cJf rootfs.tar.xz -C /path/to/build/dir .`
### Template scripts

A number of *template scripts* are distributed with the LXC package. These scripts assist with generating various guest environments.

Template scripts exist at /usr/share/lxc/templates as bash scripts, but should be executed via the lxc-create tool as follows:

`root #``lxc-create -n` *guestname* -t *template-name* -f *configuration-file-path*
#### Download template

The **download** template is the easiest and most commonly used template, downloading a container from a repository.

Using **download** as the *template-name* displays a list of available guest environments to download and saves the guest image in /var/lib/lxc.

Available containers can be viewed with:

`root #``sudo lxc-create -t download -n alpha -- --list`
--- DIST RELEASE ARCH VARIANT BUILD --- almalinux 8 amd64 default 20230815\_23:08 almalinux 8 arm64 default 20230815\_23:08 almalinux 9 amd64 default 20230815\_23:08 almalinux 9 arm64 default 20230815\_23:08 alpine 3.15 amd64 default 20230816\_13:00 alpine 3.15 arm64 default 20230816\_13:00 alpine 3.16 amd64 default 20230816\_13:00 alpine 3.16 arm64 default 20230816\_13:00 alpine 3.17 amd64 default 20230816\_13:00 alpine 3.17 arm64 default 20230816\_13:00 alpine 3.18 amd64 default 20230816\_13:00 alpine 3.18 arm64 default 20230816\_13:00 alpine edge amd64 default 20230816\_13:00 alpine edge arm64 default 20230816\_13:00 alt Sisyphus amd64 default 20230816\_01:17 alt Sisyphus arm64 default 20230816\_01:17 alt p10 amd64 default 20230816\_01:17 alt p10 arm64 default 20230816\_01:17 alt p9 amd64 default 20230816\_01:17 alt p9 arm64 default 20230816\_01:17 amazonlinux current amd64 default 20230816\_05:09 amazonlinux current arm64 default 20230816\_05:09 apertis v2020 amd64 default 20230816\_10:53 apertis v2020 arm64 default 20230816\_10:53 apertis v2021 amd64 default 20230816\_10:53 apertis v2021 arm64 default 20230816\_10:53 archlinux current amd64 default 20230816\_04:18 archlinux current arm64 default 20230816\_04:18 busybox 1.34.1 amd64 default 20230816\_06:00 busybox 1.34.1 arm64 default 20230816\_06:00 centos 7 amd64 default 20230816\_07:08 centos 7 arm64 default 20230816\_07:08 centos 8-Stream amd64 default 20230816\_07:49 centos 8-Stream arm64 default 20230816\_07:08 centos 9-Stream amd64 default 20230815\_07:08 centos 9-Stream arm64 default 20230816\_07:50 debian bookworm amd64 default 20230816\_05:24 debian bookworm arm64 default 20230816\_05:24 debian bullseye amd64 default 20230816\_05:24 debian bullseye arm64 default 20230816\_05:24 debian buster amd64 default 20230816\_05:24 debian buster arm64 default 20230816\_05:24 debian sid amd64 default 20230816\_05:24 debian sid arm64 default 20230816\_05:24 devuan beowulf amd64 default 20230816\_11:50 devuan beowulf arm64 default 20230816\_11:50 devuan chimaera amd64 default 20230816\_11:50 devuan chimaera arm64 default 20230816\_11:50 fedora 37 amd64 default 20230816\_20:33 fedora 37 arm64 default 20230816\_20:33 fedora 38 amd64 default 20230816\_20:33 fedora 38 arm64 default 20230816\_20:33 funtoo 1.4 amd64 default 20230816\_16:52 funtoo 1.4 arm64 default 20230816\_16:52 funtoo next amd64 default 20230816\_16:52 funtoo next arm64 default 20230816\_16:52 kali current amd64 default 20230816\_17:14 kali current arm64 default 20230816\_17:14 mint ulyana amd64 default 20230816\_08:52 mint ulyssa amd64 default 20230816\_08:52 mint uma amd64 default 20230816\_08:51 mint una amd64 default 20230816\_08:51 mint vanessa amd64 default 20230816\_08:51 mint vera amd64 default 20230816\_08:51 mint victoria amd64 default 20230816\_08:51 openeuler 20.03 amd64 default 20230816\_15:48 openeuler 20.03 arm64 default 20230816\_15:48 openeuler 22.03 amd64 default 20230816\_15:48 openeuler 22.03 arm64 default 20230816\_15:48 openeuler 23.03 amd64 default 20230816\_15:48 openeuler 23.03 arm64 default 20230816\_15:48 opensuse 15.4 amd64 default 20230816\_04:20 opensuse 15.4 arm64 default 20230816\_04:20 opensuse 15.5 amd64 default 20230816\_04:20 opensuse 15.5 arm64 default 20230816\_04:20 opensuse tumbleweed amd64 default 20230816\_04:20 opensuse tumbleweed arm64 default 20230816\_04:20 openwrt 21.02 amd64 default 20230816\_11:57 openwrt 21.02 arm64 default 20230816\_11:57 openwrt 22.03 amd64 default 20230816\_11:57 openwrt 22.03 arm64 default 20230816\_11:57 openwrt snapshot amd64 default 20230816\_11:57 openwrt snapshot arm64 default 20230816\_11:57 oracle 7 amd64 default 20230816\_08:52 oracle 7 arm64 default 20230816\_08:53 oracle 8 amd64 default 20230816\_07:47 oracle 8 arm64 default 20230815\_11:39 oracle 9 amd64 default 20230816\_07:46 oracle 9 arm64 default 20230816\_07:47 plamo 7.x amd64 default 20230816\_01:33 plamo 8.x amd64 default 20230816\_01:33 rockylinux 8 amd64 default 20230816\_02:06 rockylinux 8 arm64 default 20230816\_02:06 rockylinux 9 amd64 default 20230816\_02:06 rockylinux 9 arm64 default 20230816\_02:06 springdalelinux 7 amd64 default 20230816\_06:38 springdalelinux 8 amd64 default 20230816\_06:38 springdalelinux 9 amd64 default 20230816\_06:38 ubuntu bionic amd64 default 20230816\_07:43 ubuntu bionic arm64 default 20230816\_07:42 ubuntu focal amd64 default 20230816\_07:42 ubuntu focal arm64 default 20230816\_07:42 ubuntu jammy amd64 default 20230816\_07:42 ubuntu jammy arm64 default 20230816\_07:42 ubuntu lunar amd64 default 20230816\_07:42 ubuntu lunar arm64 default 20230816\_07:42 ubuntu mantic amd64 default 20230816\_07:42 ubuntu mantic arm64 default 20230816\_07:42 ubuntu xenial amd64 default 20230816\_07:42 ubuntu xenial arm64 default 20230816\_07:42 voidlinux current amd64 default 20230816\_17:10

voidlinux current arm64 default 20230816\_17:10
To download an Ubuntu container, selecting the *lunar* release, and *amd64* architecture:

`root #``lxc-create -t download -n ubuntu-guest -- -d ubuntu -r lunar -a amd64`
#### Local template

The **local** template allows a guest root filesystem to be imported from a xzipped tarball.

##### Create an empty metadata tarball

If using a LXC version lower than 6, start by creating a metadata tarball:

`user $````
touch config
```
`user $``tar -cJf metadata.tar.xz config`
##### Import a stage3 tarball

LXC's *local* template can be used to natively import stage 3 tarballs:

`user $``lxc-create -t local -n gentoo-guest -- --fstree stage3.tar.xz --no-dev`
### Backing Store

By default, containers will use the **dir** backing, copying the files into a directory.

Using a backing such as **btrfs** will allow lxc-copy to create a new container using another container as a snapshot.

The backing type is specified with **--bdev** or **-B**:

`user $``lxc-create -t local -n gentoo-guest -B btrfs -- --fstree stage3.tar.xz --no-dev`
### Mount configuration

By default, LXC will not make any mounts - including ones like /proc and /sys, to create these mounts:

**`/var/lib/lxc/{container name}/config`**

### TTY configuration

The following configuration is taken from /usr/share/lxc/config/common.conf, it creates 1024 *pty*s and 4 *tty*s:

**`/var/lib/lxc/{container name}/config`**

### Init configuration

The way LXC starts a container can be adjusted with:

**`/var/lib/lxc/{container name}/config`**

### Logging configuration

LXC's log level can be adjusted with `lxc.log.level`:

**`/var/lib/lxc/{container name}/config`**

#### File

To send log information to a file, set the file with `lxc.log.file`:

**`/var/lib/lxc/{container name}/config`**

#### Syslog

LXC containers can be configured to send logs to syslog using `lxc.log.syslog`:

**`/var/lib/lxc/{container name}/config`**

### Console Logging

To log the container console, `lxc.console.logfile` can be used:

**`/var/lib/lxc/{container name}/config`**

## Usage

### Starting a container

To start a container, simply use lxc-start with the container name:

`root #``lxc-start gentoo-guest`
### Stopping a container

A container can be stopped with lxc-stop:

`root #``lxc-stop guest-name`
### lxc-console

Using **lxc-console** provides console access to a guest with `lxc.tty = 1` defined:

`root #``lxc-console -n` **guestname**
### lxc-attach

**lxc-attach** starts a process inside a running container. If no command is specified, it looks for the default shell. Therefore, you can get into the container by:

`root #``lxc-attach -n` **guestname**
### Accessing the container with sshd

A common technique to allow users direct access into a system container is to run a separate sshd inside the container. Users then connect to that sshd directly. In this way, you can treat the container just like you treat a full virtual machine where you grant external access. If you give the container a routable address, then users can reach it without using ssh tunneling.

If you set up the container with a virtual ethernet interface connected to a bridge on the host, then it can have its own Ethernet address on the LAN, and you should be able to connect directly to it without logically involving the host (the host will transparently relay all traffic destined for the container, without the need for any special considerations). You should be able to simply 'ssh \<container\_ip>'.

## Troubleshooting

### Systemd containers on an OpenRC host

To use systemd in the container, a recent enough (>=4.6) kernel version with support for cgroup namespaces is needed. Additionally the host needs to have a `name=systemd` cgroup hierarchy mounted. Doing so does not require running systemd on the host:

`root #````
mkdir -p /sys/fs/cgroup/systemd
```
`root #````
mount -t cgroup -o none,name=systemd systemd /sys/fs/cgroup/systemd
```
`root #``chmod 777 /sys/fs/cgroup/systemd`
### Containters freeze on 'lxc stop' with OpenRC

Please see [LXD#Containters\_freeze\_on\_.27lxc\_stop.27\_with\_OpenRC](https://wiki.gentoo.org/wiki/LXD#Containters_freeze_on_.27lxc_stop.27_with_OpenRC). Just replace 'lxc exec' with 'lxc-console' or 'lxc-attach' to get console access.

### newuidmap error

Packages `sys-auth/pambase-20150213` and `sys-apps/shadow-4.4-r2` provides newuidmap and newgidmap commands without 
required permissions for lxc. As result, all lxc-\* commands return error like:

`newuidmap: write to uid_map failed: Operation not permitted`

For example:

`lxc@localhost $````
lxc-create -t download -n alpha -f ~/.config/lxc/guest.conf -- -d ubuntu -r trusty -a amd64
```
newuidmap: write to uid\_map failed: Operation not permitted
error mapping child
setgid: Invalid argument

To fix this issue, set setuid and setgid flags with command:

`root #````
chmod 4755 /usr/bin/newuidmap
```
`root #``chmod 4755 /usr/bin/newgidmap`
For more details regarding bug, see this issue: [\[1\]](https://github.com/lxc/lxc/issues/1454)

### Could not set clone\_children to 1 for cpuset hierarchy in parent cgroup

As of december 2017, unified cgroups recently introduced in systemd and openrc are messing things up in the realm of unprivileged containers. It's tricky to see in other distros how they solve the problem because Arch doesn't support unprivileged containers and Ubuntu Xenial is on systemd 229 (unified cgroups became the default with 233), Debian stretch is on 232.

I began looking at non-LTS ubuntu releases but I haven't figured out what they're doing yet. It involves using the new `cgfsng` driver which, to my knowledge, has never been made to work on Gentoo. Unprivileged containers always worked with `cgmanager` which is now deprecated.

To work around that on systemd, you'll have to manually set user namespaces by following "Create user namespace manually (no systemd)" instructions above, ignoring the "no systemd" part. After that, you should be good to go.

On the OpenRC side, you also have to disable unified cgroups. You do that by editing `/etc/rc.conf` and setting `rc_cgroup_mode="legacy"`

That will bring you pretty far because your unprivileged container will boot, but if you boot a systemd system, it won't be happy about not being in its cosy systemd world. You will have to manually create a `systemd` cgroup for it. [this write up about LXC under alpine](https://j2h2.com/entry/alpine-linux-and-systemd-containers-round-2) helps a lot.

### tee: /sys/fs/cgroup/memory/memory.use\_hierarchy: Device or resource busy

Current kernel upstream changed behavior for cgroup v1 and it is unable to change memory\_hierarchy after lxc fs mount. 
To use this functionality, set `rc_cggroup_memory_use_hierarchy="yes"` in `/etc/rc.conf`

### lxc-console: no promt / console (may be a temporary as of 2019-10)

Make sure the container spawns the right agetty / ttys, so lxc-console can connect to it. Alternatively, connect to the console.

Gentoo LXC images from jenkins server currently might not work out of the box as expected before - they don't spawn ttys because of this [reported bug](https://discuss.linuxcontainers.org/t/gentoo-image-ttys-die/5139) / [fix](https://github.com/lxc/lxc-ci/commit/39b5e11afdd8d94a26fe30692c4f9e405a27bd19). Try connecting with lxc-console using the "-t 0" option, which explicitly connects to the container console instead of a tty:

`root #``lxc-console -n mycontainer -t 0`
Alternatively, spawn more standard ttys inside the container by editing /etc/inittab:

**`/etc/inittab`**

```
# TERMINALS
x1:12345:respawn:/sbin/agetty 38400 console linux
c1:12345:respawn:/sbin/agetty 38400 tty1 linux
c2:2345:respawn:/sbin/agetty 38400 tty2 linux
#c3:2345:respawn:/sbin/agetty 38400 tty3 linux
...
```
## See also

- [Docker](https://wiki.gentoo.org/wiki/Docker) — a [container](<https://en.wikipedia.org/wiki/Container_(virtualization)>)-based [virtualization](https://wiki.gentoo.org/wiki/Virtualization) system
- [Incus](https://wiki.gentoo.org/wiki/Incus) — next generation system container and virtual machine manager
- [Magic SysRq](https://wiki.gentoo.org/wiki/Magic_SysRq) — a kernel hack that enables the kernel to listen to specific key presses and respond by calling a specific kernel function.
