<!-- source: https://wiki.gentoo.org/wiki/Docker | group: Gentoo Wiki (Main) | wiki-title: Docker -->
---
title: Docker
url: https://wiki.gentoo.org/wiki/Docker
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-20"
fingerprint: "3b07fb5701a71984"
license: CC BY-SA 4.0
---

# Docker

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Docker** is a [container](<https://en.wikipedia.org/wiki/Container_(virtualization)>)-based [virtualization](https://wiki.gentoo.org/wiki/Virtualization) system which can be used to establish development or runtime environments without modifying the base operating system.

Docker is built on a thin layer of virtualization, using the host [kernel](https://wiki.gentoo.org/wiki/Kernel), and as such is "lighter" than full hardware virtualization, incurring a lesser performance tradeoff. It allows easy deployment of instances of containers to different hosts.

## Installation

### USE flags


| [+container-init](https://packages.gentoo.org/useflags/+container-init) | Makes the a staticly-linked init system tini available inside a container. | 
| [+overlay2](https://packages.gentoo.org/useflags/+overlay2) | Enables dependencies for the "overlay2" graph driver, including necessary kernel flags. | 
| [apparmor](https://packages.gentoo.org/useflags/apparmor) | Enable support for the AppArmor application security system | 
| [btrfs](https://packages.gentoo.org/useflags/btrfs) | Enables dependencies for the "btrfs" graph driver, including necessary kernel flags. | 
| [cuda](https://packages.gentoo.org/useflags/cuda) | Enable NVIDIA CUDA support (computation on GPU) | 
| [seccomp](https://packages.gentoo.org/useflags/seccomp) | Enable seccomp (secure computing mode) to perform system call filtering at runtime to increase security of programs | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 

### Kernel

If the kernel has not been configured properly before merging the [app-containers/docker](https://packages.gentoo.org/packages/app-containers/docker) package, a list of missing kernel options will be printed by emerge. These kernel features must be enabled [manually](https://wiki.gentoo.org/wiki/Kernel/Configuration).

For the most up-to-date values, check the contents of the `CONFIG_CHECK` in /var/db/repos/gentoo/app-containers/docker/docker-9999.ebuild file.

A graphical representation would look something like this:

**Configuring the kernel for Docker**

General setup  --->
  \[\*\] POSIX Message Queues [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_POSIX\_MQUEU\</code> to find this item.
  BPF subsystem  --->
     \[\*\] Enable bpf() system call (Optional) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_BPF\_SYSCALL\</code> to find this item.
  \[\*\] Control Group support  ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CGROUPS\</code> to find this item.
     \[\*\] Memory controller [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_MEMCG\</code> to find this item.
     \[\*\] Swap controller (Optional)
     \[\*\]   Swap controller enabled by default (Optional)
     \[\*\] IO controller (Optional)
     \[\*\] CPU controller  ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CGROUP\_SCHED\</code> to find this item.
        \[\*\] Group scheduling for SCHED\_OTHER (Optional) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_FAIR\_GROUP\_SCHED\</code> to find this item.
        \[\*\]   CPU bandwidth provisioning for FAIR\_GROUP\_SCHED (Optional) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CFS\_BANDWIDTH\</code> to find this item.
        \[\*\] Group scheduling for SCHED\_RR/FIFO (Optional) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_RT\_GROUP\_SCHED\</code> to find this item.
     \[\*\] PIDs controller (Optional) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CGROUP\_PIDS\</code> to find this item.
     \[\*\] Freezer controller [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CGROUP\_FREEZER\</code> to find this item.
     \[\*\] HugeTLB controller (Optional) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CGROUP\_HUGETLB\</code> to find this item.
     \[\*\] Cpuset controller [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CPUSETS\</code> to find this item.
        \[\*\]  Include legacy /proc/\<pid>/cpuset file (Optional) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_PROC\_PID\_CPUSET\</code> to find this item.
     \[\*\] Device controller [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CGROUP\_DEVICE\</code> to find this item.
     \[\*\] Simple CPU accounting controller [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CGROUP\_CPUACCT\</code> to find this item.
     \[\*\] Perf controller (Optional) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CGROUP\_PERF\</code> to find this item.
     \[\*\] Support for eBPF programs attached to cgroups (Optional) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CGROUP\_BPF\</code> to find this item.
  \[\*\] Namespaces support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NAMESPACES\</code> to find this item.
     \[\*\] UTS namespace [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_UTS\_NS\</code> to find this item.
     \[\*\] IPC namespace [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_IPC\_NS\</code> to find this item.
     \[\*\] User namespace (Optional) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_USER\_NS\</code> to find this item.
     \[\*\] PID Namespaces [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_PID\_NS\</code> to find this item.
     \[\*\] Network namespace [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NET\_NS\</code> to find this item.
General architecture-dependent options  --->
  \[\*\] Enable seccomp to safely execute untrusted bytecode (Optional) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SECCOMP\</code> to find this item.
\[\*\] Enable the block layer  ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_BLOCK\</code> to find this item.
  \[\*\] Block layer bio throttling support (Optional) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_BLK\_DEV\_THROTTLING\</code> to find this item.
\[\*\] Networking support  ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NET\</code> to find this item.
   Networking options  --->
      \[\*\] Network packet filtering framework (Netfilter)  ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NETFILTER\</code> to find this item.
           \[\*\] Advanced netfilter configuration [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NETFILTER\_ADVANCED\</code> to find this item.
           \[\*\]   Bridged IP/ARP packets filtering [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_BRIDGE\_NETFILTER\</code> to find this item.
              Core Netfilter Configuration  --->
                 \[\*\] Netfilter connection tracking support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NF\_CONNTRACK\</code> to find this item.
                 \[\*\] Network Address Translation support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NF\_NAT\</code> to find this item.
                 \[\*\] MASQUERADE target support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NETFILTER\_XT\_TARGET\_NFQUEUE\</code> to find this item.
                 \[\*\] Netfilter Xtables support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NETFILTER\_XTABLES\</code> to find this item.
                 \[\*\]    "addrtype" address type match support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NETFILTER\_XT\_MATCH\_ADDRTYPE\</code> to find this item.
                 \[\*\]    "conntrack" connection tracking match support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NETFILTER\_XT\_MATCH\_CONNTRACK\</code> to find this item.
                 \[\*\]    "ipvs" match support (Optional) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NETFILTER\_XT\_MATCH\_IPVS\</code> to find this item.
                 \[\*\]    "mark" match support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NETFILTER\_XT\_MATCH\_MARK\</code> to find this item.
           \[\*\] IP virtual server support  ---> (Optional) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_IP\_VS\</code> to find this item.
              \[\*\] TCP load balancing support (Optional) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_IP\_VS\_PROTO\_TCP\</code> to find this item.
              \[\*\] UDP load balancing support (Optional) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_IP\_VS\_PROTO\_UDP\</code> to find this item.
              \[\*\] round-robin scheduling (Optional) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_IP\_VS\_RR\</code> to find this item.
              \[\*\] Netfilter connection tracking (Optional) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_IP\_VS\_NFCT\</code> to find this item.
           IP: Netfilter Configuration  --->
              \[\*\] IP tables support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_IP\_NF\_IPTABLES\</code> to find this item.
              \[\*\]    raw table support (required for NOTRACK/TRACE) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_IP\_NF\_RAW\</code> to find this item.
              \[\*\]    Packet filtering [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_IP\_NF\_FILTER\</code> to find this item.
              \[\*\]    iptables NAT support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_IP\_NF\_NAT\</code> to find this item.
              \[\*\]      MASQUERADE target support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_IP\_NF\_TARGET\_MASQUERADE\</code> to find this item.
              \[\*\]      REDIRECT target support (Optional) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NF\_TARGET\_REDIRECT\</code> to find this item.
       \[\*\] 802.1d Ethernet Bridging [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_BRIDGE\</code> to find this item.
       \[\*\]   VLAN filtering [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_BRIDGE\_VLAN\_FILTERING\</code> to find this item.
       \[\*\] 802.1Q/802.1ad VLAN Support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_VLAN\_8021Q\</code> to find this item.
       \[\*\] QoS and/or fair queueing  --->  (Optional) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NET\_SCHED\</code> to find this item.
          \[\*\] Control Group Classifier (Optional) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NET\_CLS\_CGROUP\</code> to find this item.
       \[\*\] L3 Master device support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NET\_L3\_MASTER\_DEV\</code> to find this item.
       \[\*\] Network priority cgroup (Optional) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CGROUP\_NET\_PRIO\</code> to find this item.
Device Drivers  --->
  \[\*\] Multiple devices driver support (RAID and LVM)  ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_MD\</code> to find this item.
     \[\*\] Device mapper support (Optional) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_BLK\_DEV\_DM\</code> to find this item.
     \[\*\]  Thin provisioning target (Optional) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_DM\_THIN\_PROVISIONING\</code> to find this item.
   \[\*\] Network device support  ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NETDEVICES\</code> to find this item.
      \[\*\] Network core drive support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NET\_CORE\</code> to find this item.
      \[\*\]   Dummy net driver support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_DUMMY\</code> to find this item.
      \[\*\]   MAC-VLAN net driver support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_MACVLAN\</code> to find this item.
      \[\*\]   IP-VLAN support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_IPVLAN\</code> to find this item.
      \[\*\]   Virtual eXtensible Local Area Network (VXLAN) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_VXLAN\</code> to find this item.
      \[\*\]   Virtual ethernet pair device [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_VETH\</code> to find this item.
   Character devices  --->
       -\*- Enable TTY [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_TTY\</code> to find this item.
       -\*-    Unix98 PTY support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_UNIX98\_PTYS\</code> to find this item.
       \[\*\]       Support multiple instances of devpts (option appears if you are using systemd)
File systems  --->
  \[\*\] Btrfs filesystem support (Optional) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_BTRFS\_FS\</code> to find this item.
  \[\*\]   Btrfs POSIX Access Control Lists (Optional) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_BTRFS\_FS\_POSIX\_ACL\</code> to find this item.
  \[\*\] Overlay filesystem support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_OVERLAY\_FS\</code> to find this item.
  Pseudo filesystems  --->
     \[\*\] HugeTLB file system support (Optional) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_HUGETLBFS\</code> to find this item.
Security options  --->
  \[\*\] Enable access key retention support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_KEYS\</code> to find this item.


After exiting the kernel configuration, [rebuild the kernel](https://wiki.gentoo.org/wiki/Kernel/Rebuild). If the kernel rebuild also performs a kernel upgrade, be sure to rebuild the [bootloader](https://wiki.gentoo.org/wiki/Bootloader)'s menu configuration, then reboot the system to the newly recompiled kernel binary.

#### Snippet

**`/etc/kernel/config.d/docker-linux6-11-10.config`**

#### Compatibility check

To re-run the kernel configuration compatibility check, issue:

`user $``/usr/share/docker/contrib/check-config.sh`
### Emerge

Install [app-containers/docker](https://packages.gentoo.org/packages/app-containers/docker) and [app-containers/docker-cli](https://packages.gentoo.org/packages/app-containers/docker-cli):

`root #``emerge --ask --verbose app-containers/docker app-containers/docker-cli`
#### PaX kernel

Tools in the [sys-apps/paxctl](https://packages.gentoo.org/packages/sys-apps/paxctl) package are necessary for this operation. See [Hardened/PaX Quickstart](https://wiki.gentoo.org/wiki/Hardened/PaX_Quickstart) for an introduction.

`root #``/sbin/paxctl -m /usr/bin/containerd`
For the **hello-world** example, set this flag for **containerd-shim** and **runc**:

`root #````
/sbin/paxctl -m /usr/bin/containerd-shim
```
`root #````
/sbin/paxctl -m /usr/bin/runc
```
If an [issue with denied chmods in chroots](https://github.com/docker/docker/issues/20303) occurs, a more recent version of Docker (>=1.12) is needed. Use the **\~amd64** [Keyword](https://wiki.gentoo.org/wiki//etc/portage/package.accept_keywords) for Docker and its dependencies listed subsequently when running emerge app-containers/docker again.

## Configuration

The docker daemon configuration is located at /etc/docker/daemon.json, more information on configuring this file is available at: [https://docs.docker.com/config/daemon/](https://docs.docker.com/config/daemon/)

The current docker configuration can be viewed with:

`root #``docker info`
### Service

#### OpenRC

OpenRC users can adjust the `DOCKER_OPTS` variable in the service configuration file located in /etc/conf.d. The example below displays a change to the storage driver to [btrfs](https://wiki.gentoo.org/wiki/Btrfs) and the docker engine root to /srv/var/lib/docker:

**`/etc/conf.d/docker`**

```
DOCKER_OPTS="--storage-driver btrfs --data-root /srv/var/lib/docker"
```
After Docker has been successfully installed and configured, it can be added to the system's default runlevel, starting it at boot:

`root #````
rc-update add docker default
```
`root #``rc-service docker start`
If the registry service is required:

`root #````
rc-update add registry default
```
`root #````
rc-service registry start
```
#### systemd

To have Docker start on boot, enable it:

`root #``systemctl enable docker.service`
To start it now:

`root #``systemctl start docker.service`
### Permissions

Add relevant users to the docker group:

`root #``usermod -aG docker <username>`
### Storage driver

Overlay2 storage driver is the preferred storage driver for all currently supported Linux distributions, and requires no extra configuration.

View Docker's settings in detail with the info subcommand:

`user $``docker info`


**`/etc/docker/daemon.json`**

**Set the docker storage driver to use overlay2**

```
{
    "storage-driver": "overlay2"
}
```
#### btrfs

To change the storage driver, first verify the host machines kernel has support for the desired filesystem. The [btrfs](https://wiki.gentoo.org/wiki/Btrfs) filesystem will be used in this example:

**`/etc/docker/daemon.json`**

**Set the docker storage driver to use btrfs**

```
{
    "storage-driver": "btrfs"
}
```
`user $``grep btrfs /proc/filesystems`
When using the btrfs driver, be aware the *data-root* directory of the docker engine (/var/lib/docker/ by default) may need be adjusted to a non-default location if directories backing filesystem is not btrfs. For situations where the btrfs storage pool is located under a different device and/or mountpoint (such as /mnt or /srv), adjust the data-root location accordingly:

**`/etc/docker/daemon.json`**

**Moving the root directory of the docker storage engine**

```
{
    "storage-driver": "btrfs",
    "data-root": "/srv/var/lib/docker"
}
```
Insure the running service is stopped before moving data from the previous data-root location, then copy the data to the new location:

`root #````
systemctl stop docker
```
`root #````
mkdir --parents /srv/var/lib/docker
```
`root #````
rsync -a /var/lib/docker/* /srv/var/lib/docker
```
`root #````
systemctl start docker
```
### Networking

Port forwarding must be enabled for docker container networking to work.

This can be temporarily enabled using [procfs](https://wiki.gentoo.org/wiki/Procfs):

`user $``sudo sysctl net.ipv4.ip_forward=1`
A more permanent change can be made with:

**`/etc/sysctl.d/local.conf`**

**Enable ip forwarding persistently**

## Usage

### Testing

In order to test the installation, run the following command:

`user $``docker run --rm hello-world````
Hello from Docker.
This message shows that your installation appears to be working correctly.
To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.
To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash
Share images, automate workflows, and more with a free Docker Hub account:
 https://hub.docker.com
For more examples and ideas, visit:
 https://docs.docker.com/userguide/
```
That will first download from the [Docker Hub](https://hub.docker.com/) the image named *hello-world* (if it has not been downloaded locally yet), then it will run it inside new [namespaces](https://wiki.gentoo.org/wiki/Namespaces). It purpose is just to display some text through a container.

### Listing images

Current images can be listed with:

`user $``docker images`
### Starting a container from an image

A new container can be started using an image with **run**. The following command starts a docker container which is an Alpine Linux shell:

`user $``docker run -it --rm alpine:3.18 ash`
### Listing containers

Current containers can be listed with:

`user $``docker container list`
### Viewing container config

To view the configuration for a container:

`user $``docker container inspect {container name}` ### Running a command in a running container

To execute a command in an already running container:

`user $``docker exec {container name} {command}` ### Stopping a container

A running container can be stopped with:

`user $``docker stop {container name}` ### Starting a container

If a container has been stopped, it can be started again with:

`user $``docker start {container name}` ### Building from a Dockerfile

Create a new Dockerfile in an empty directory with the following content:

**`Dockerfile`**

Run:

`user $````
docker build -t my-php-app .
```
`user $````
docker run -it --rm --name my-running-app my-php-app
```
## Custom Images

Containers are generally structured with either of the following approaches:

- The minimal approach: According to the [container philosophy](https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/#run-only-one-process-per-container) a container should **only** contain what is needed to serve one process. In this case ideally the container consists of one static binary.
- The VM approach: A container can be treated like a full system virtualization environment. In this case the container includes a whole operating system.

### Building the image environment

The image can be constructed using many methods. The simplest would involve adding a single binary which can be executed. Using emerge to generate the environment is a simple and effective method, but more advanced methods such as using [crossdev](https://wiki.gentoo.org/wiki/Crossdev) or [catalyst](https://wiki.gentoo.org/wiki/Catalyst) are possible.

#### Using emerge to build the environment

Portage can be used to simply construct an application environment.

A Gentoo-based Docker image can be constructed by using emerge with the **--root** flag. Simply **--oneshot** the desired packages to that destination.

The following command creates a container for [net-p2p/transmission](https://packages.gentoo.org/packages/net-p2p/transmission) at /var/lib/chroot/builddir/transmission:

`root #``emerge --ask --verbose --root /var/lib/chroot/builddir/transmission --oneshot transmission`
#### Alternative minimal approach: Dynamically linked binaries using Kubler

[Kubler](https://github.com/edannenberg/kubler) is a generic, extendable build orchestrator, written in Bash. It can be used to take advantage of Portage's features to build lightweight Docker or Podman images without needing to mess with crossdev, or as a tool to assist with ebuild development.

Detailed instructions for using Kubler are available [Here](https://wiki.gentoo.org/wiki/Kubler).

### Packing the environment into a tarball

Once the build environment has been created, the contents can be archived with tar to be imported into Docker.

The following command creates gentoo-transmission.tar.gz based on the contents of /var/lib/chroot/builddir/transmission/:

`root #``tar -czf gentoo-transmission.tar.gz -C /var/lib/chroot/builddir/transmission/ .`
### Importing into Docker

If using a *Dockerfile*, the tarball can be imported with *ADD*. To import gentoo-transmission.tar.gz:

**`Dockerfile`**

```
FROM scratch
ADD gentoo-transmission.tar.gz /
EXPOSE 9091/tcp
EXPOSE 51413/tcp
EXPOSE 51413/udp
USER transmission
CMD ["/usr/bin/transmission-daemon", "-f", "-g", "/config"]
```
The image can be manually imported with:

`root #``docker import gentoo-transmission.tar.gz`
#### Tagging the Image

The imported image should be visible using docker images:

`root #``docker images`
REPOSITORY           TAG       IMAGE ID       CREATED          SIZE
\<none>               \<none>    a5c15b539917   13 minutes ago   622MB

Using the image ID *a5c15b539917*, the tag *gentoo-transmission* can be applied to this image:

`root #``docker tag a5c15b539917 gentoo-transmission`
## Setting up rootless Docker

Docker runs a client/server model. The daemon *dockerd* runs as root by default, the result is that anyone that can execute docker commands can effectively run commands as the root user.

Docker provides the option to run as the the daemon on a per user basis instead with a little additional set up documented below.

There are some caveats when running in rootless mode please see [the upstream known limitations](https://docs.docker.com/engine/security/rootless/troubleshoot/#known-limitations) for the up to date list.

**Note: by default rootless docker stores images etc. in \~/.local/share/docker instead of /var/lib/docker images aren't shared between users and you may wish to exclude this folder from any backups.**

### Disable the existing docker daemon

First disable the existing docker service.

#### OpenRC

`root #````
rc-update del docker default
```
`root #``rc-service docker stop`
#### Systemd

`root #``systemctl disable --now docker.service docker.socket`
### Installation

Remove the docker socket if it exists

`root #` `rm /var/run/docker.sock`
Configure the kernel modules to load at boot. The docker root service loads the required modules using modprobe the user services don't have permission to do this.

**`/etc/modules-load.d/docker.conf`**

**Load ip\_tables and overlay modules at boot**

Install [app-containers/slirp4netns](https://packages.gentoo.org/packages/app-containers/slirp4netns) and [sys-apps/rootlesskit](https://packages.gentoo.org/packages/sys-apps/rootlesskit):

`root #``emerge --ask --verbose app-containers/slirp4netns sys-apps/rootlesskit`
To install in rootless mode run:

`user $``dockerd-rootless-setuptool.sh install`
This will output various information, it's recommend you read these for more information they maybe useful when troubleshooting.

To allow running docker at boot run under systemd run:

`root #``loginctl enable-linger <username>``user $``systemctl --user enable docker.service`
On OpenRC rootless docker needs to be manually started with:

`user $``dockerd-rootless.sh`
### Confirming rootless installation

Check the current configuration with

`user $``docker info`
At the top of the output you should see output similar to:

Client:      
Version:    28.4.0         
Context:    rootless
Debug Mode: false

*Context: rootless* incidates rootless is working.

### Setting up a user service (OpenRC)

To start the Docker daemon automatically with OpenRC, create the following user service:

**`~/.config/rc/init.d/docker-rootless`**

**Rootless docker service**

```
#!/sbin/openrc-run
# Copyright 2026 Gentoo Authors
# Distributed under the terms of the GNU General Public License v2
name="docker-rootless"
description="Rootless Docker daemon"
command="/usr/bin/dockerd-rootless.sh"
pidfile="${XDG_RUNTIME_DIR}/docker.pid"
command_args="-p ${pidfile} ${DOCKER_OPTS}"
DOCKER_STATE_DIR="${XDG_STATE_HOME:-$HOME/.local/state}/docker"
DOCKER_LOGFILE="${DOCKER_LOGFILE:-${DOCKER_STATE_DIR}/docker.log}"
DOCKER_ERRFILE="${DOCKER_ERRFILE:-${DOCKER_LOGFILE}}"
DOCKER_OUTFILE="${DOCKER_OUTFILE:-${DOCKER_LOGFILE}}"
start_stop_daemon_args="--background \
	--stderr \"${DOCKER_ERRFILE}\" \
	--stdout \"${DOCKER_OUTFILE}\""
retry="${DOCKER_RETRY:-TERM/60/KILL/10}"
start_pre() {
	checkpath -d -m 0700 "${XDG_RUNTIME_DIR}"
	checkpath -d -m 0700 "${DOCKER_STATE_DIR}"
	checkpath -f -m 0600 "${DOCKER_LOGFILE}"
}
```
Set DOCKER\_HOST in \~/.profile so that the Docker client connects to the rootless daemon:

**`~/.profile`**

Start the user service:

`user $``rc-service --user docker-rootless start`
Verify that the Docker daemon is accessible:

`user $``DOCKER_HOST=unix:///run/user/$(id -u)/docker.sock docker ps`
Enable the service to start automatically:

`user $``rc-update -U add docker-rootless`
## Troubleshooting

### Docker service crashes/fails to start (OpenRC)

After adding `--storage-driver btrfs` to `DOCKER_OPTS` and restarting the Docker service, Docker may crash. Check this with rc-status.

If this is the case, try adding the `btrfs` USE flag for the Docker package, and updating Docker package.

`root #````
touch /etc/portage/package.use/docker
```
`root #````
nano /etc/portage/package.use/docker
```
**`/etc/portage/package.use/docker`**

```
 btrfs device-mapper
```
Install Docker with the new USE flags

`root #``emerge --update --deep --newuse app-containers/docker`
Docker service restart

`root #``rc-service docker restart`
### Docker service fails because cgroup device not mounted (OpenRC)

On an error like:

```
 to start container process: error during container init: error mounting "cgroup" to rootfs at "/sys/fs/cgroup": mount cgroup:/sys/fs/cgroup/openrc (via /proc/self/fd/6), flags: 0xf, data: openrc: invalid argument
```
The solution is to set following: [https://github.com/abiosoft/colima/issues/764#issuecomment-1701672142](https://github.com/abiosoft/colima/issues/764#issuecomment-1701672142)

**`/etc/rc.conf`**

and restart

`root #``rc-service cgroups restart`
### Docker service fails to start (systemd)

Some users have issues on starting `docker.service` because of device-mapper error. It can be solved by loading a different storage-driver. E.g. Loading “overlay” graph driver instead of “device-mapper” graph driver.

“overlay” graph driver requires "Overlay filesystem support" in kernel configuration:

**Configuring the kernel for Docker**

File systems --->

   `<*> Overlay filesystem support` [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_OVERLAY_FS</code> to find this item.
Add following to `/etc/portage/package.use/docker`, then re-emerge Docker will solve this issue:

**`/etc/portage/package.use/docker`**

```
 overlay -device-mapper
```
In case of an error saying, `Error starting daemon: Error initializing network controller: list bridge addresses failed: no available network`, the docker0 network bridge may be missing. Please see the following Docker issue which provides a bash script solution to create the docker0 network bridge: [https://github.com/docker/docker/issues/31546](https://github.com/docker/docker/issues/31546)

### Docker service runs but fails to start container (systemd)

If using systemd-232 or higher and receive an error related to [cgroups](https://wiki.gentoo.org/wiki/Cgroups):

`user $``docker run hello-world`
container\_linux.go:247: starting container process caused
"process\_linux.go:359:lib/docker/overlay2/523ed887f681de6ea3838aa5b26c57e88547d65bdd883a6d3538729f19a3
docker: Error response from daemon: invalid header field value "oci runtime errotainer
init caused \\\"rootfs\_linux.go:54: mounting \\\\\\\"cgroup\\\\\\\" to
ro38729f19a34501/merged\\\\\\\" at \\\\\\\"/sys/fs/cgroup\\\\\\\" caused \\\\\\\"n

Add the following line to the kernel boot parameters:

### Docker service runs but fails to start container (systemd)

If using systemd-232 or higher, and it throws this error:

`user $``docker run hello-world`
applying cgroup configuration for process caused \"open /sys/fs/cgroup/docker/cpuset.cpus.effective: no such file or directory

Add the following line to the kernel boot parameters:

Since systemd v256-rc3, another kernel boot parameter is required to force systemd to enable cgroup v1 support:

If using systemd and received this error:

`user $``docker run hello-world`
cgroup mountpoint does not exist

Run the following commands as root:

`root #``mkdir /sys/fs/cgroup/systemd``root #``mount -t cgroup -o none,name=systemd cgroup /sys/fs/cgroup/systemd`
This is not ideal as these commands must be run after each reboot, but it works.

### Docker service fails because cgroup device not mounted (systemd)

By default systemd uses hybrid cgroup hierarchy combining cgroup and cgroup2 devices. Docker still needs cgroup(v1) devices.
Activate USE flag `cgroup-hybrid` for systemd.

Activate USE flag for systemd

**`/etc/portage/package.use/systemd`**

```
 cgroup-hybrid
```
Install systemd with the new USE flags

`root #``emerge --ask --oneshot sys-apps/systemd`
### systemd-networkd

If `systemd-networkd` is used for network management, additional options are needed for IP forwarding and/or IP masquerade.

**`/etc/systemd/network/50-static.network`**

```
[Match]
Name=enp6s0
[Network]
DHCP=yes
IPForward=true
IPMasquarade=true
```
These options are used instead of the sysctl settings for ip forwarding and/or masquerade.

In case the Docker containers are shutting down, with errors from `systemd-udevd` that complain of not being able to assign persistent MAC address to virtual interface(s):
See [https://github.com/systemd/systemd/issues/3374#issuecomment-339258483](https://github.com/systemd/systemd/issues/3374#issuecomment-339258483)

**`/etc/systemd/network/99-default.link`**

```
[Link]
NamePolicy=kernel database onboard slot path
MACAddressPolicy=none
```
## See also

- [Docker/Compose](https://wiki.gentoo.org/wiki/Docker/Compose) — a tool for running multi-container applications on [Docker] defined using the Compose file format
- [Kubernetes](https://wiki.gentoo.org/wiki/Kubernetes) — open-source system for automating deployment, scaling, and management of containerized applications
- [LXC](https://wiki.gentoo.org/wiki/LXC) — a [virtualization](https://wiki.gentoo.org/wiki/Virtualization) system that leverages Linux's [namespaces](https://wiki.gentoo.org/wiki/Namespaces) and [cgroups](https://wiki.gentoo.org/wiki/Cgroups) to create containers isolated from the host system
- [Podman](https://wiki.gentoo.org/wiki/Podman) — a daemonless container engine for developing, managing, and running [OCI Containers](https://opencontainers.org/), aiming to be a drop-in replacement for much of [Docker]
- [Project:Docker](https://wiki.gentoo.org/wiki/Project:Docker) — provides minimal docker images so that the Gentoo community can have a consistent experience when building application-specific [Docker] containers.
- [Virtualization](https://wiki.gentoo.org/wiki/Virtualization) — the concept and technique that permits running software in an environment separate from a computer operating system.
