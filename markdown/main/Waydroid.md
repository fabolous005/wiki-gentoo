<!-- source: https://wiki.gentoo.org/wiki/Waydroid | group: Gentoo Wiki (Main) | wiki-title: Waydroid -->
---
title: Waydroid
url: https://wiki.gentoo.org/wiki/Waydroid
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-03-18"
fingerprint: fe075bd4c3b40d64
license: CC BY-SA 4.0
---

# Waydroid

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Waydroid is a container-based approach to boot a full Android system on regular GNU/Linux systems running Wayland based desktop environments.

## Installation

### Kernel

Since Waydroid utilizes an LXC container, enabling the corresponding kernel options is a prerequisite. See [LXC kernel section.](https://wiki.gentoo.org/wiki/LXC#Kernel)

Set the following kernel options:

```
General setup  --->
    CPU/Task time and stats accounting  --->
         [*] Pressure stall information tracking 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_PSI</code> to find this item.
Memory Management options  --->
    [*] Enable memfd_create() system call [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_MEMFD_CREATE</code> to find this item.
[*] Networking support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_NET</code> to find this item.  --->
    Networking options  --->
         [*] Network packet filtering framework (Netfilter) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_NETFILTER</code> to find this item.  --->
              Core Netfilter Configuration  --->
                   <M/*> Netfilter nf_tables support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_NF_TABLES</code> to find this item.
                   [*]           Netfilter nf_tables mixed IPv4/IPv6 tables support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_NF_TABLES_INET</code> to find this item.
                   [*]           Netfilter nf_tables netdev tables support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_NF_TABLES_NETDEV</code> to find this item.
                   <M/*>    Netfilter nf_tables number generator module [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_NFT_NUMGEN</code> to find this item.
                   <M/*>    Netfilter nf_tables masquerade support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_NFT_MASQ</code> to find this item.
                   <M/*>    Netfilter nf_tables nat module [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_NFT_NAT</code> to find this item.
                   <M/*>    Netfilter nf_tables tunnel module [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_NFT_TUNNEL</code> to find this item.
                   <M/*>    Netfilter nf_tables quota module [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_NFT_QUOTA</code> to find this item.
                   <M/*>    Netfilter nf_tables socket match support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_NFT_SOCKET</code> to find this item.
                   <M/*> Netfilter flow table module [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_NF_FLOW_TABLE</code> to find this item.
                   <M/*> Netfilter Xtables support (required for ip_tables) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_NETFILTER_XTABLES</code> to find this item.
                   <M/*>    "SNAT and DNAT" targets support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_NETFILTER_XT_NAT</code> to find this item.
                   <M/*>    MASQUERADE target support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_NETFILTER_XT_TARGET_MASQUERADE</code> to find this item.
              <M/*> Ethernet Bridge nf_tables support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_NF_TABLES_BRIDGE</code> to find this item.  --->
                   <M/*> Netfilter nf_table bridge meta support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_NFT_BRIDGE_META</code> to find this item.
                   <M/*> Netfilter nf_tables bridge reject support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_NFT_BRIDGE_REJECT</code> to find this item.
Device Drivers  --->
    Android  --->
         [*] Android Binder IPC Driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_ANDROID_BINDER_IPC</code> to find this item.
         [*] Android Binderfs filesystem [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_ANDROID_BINDERFS</code> to find this item.
For android 13 onwards, the filesystem on the \~/.local/share/waydroid directory needs to support Access Control Lists:

```
File systems  --->
    < /*> The Extended 4 (ext4) filesystem 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_EXT4_FS</code> to find this item.
    [ /*]        Ext4 POSIX Access Control Lists [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_EXT4_FS_POSIX_ACL</code> to find this item.
    < /*> JFS filesystem support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_JFS_FS</code> to find this item.
    [ /*]        JFS POSIX Access Control Lists [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_JFS_POSIX_ACL</code> to find this item.
    < /*> XFS filesystem support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_XFS_FS</code> to find this item.
    [ /*]        XFS POSIX ACL support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_XFS_POSIX_ACL</code> to find this item.
    < /*> Btrfs filesystem support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_BTRFS_FS</code> to find this item.
    [ /*]        Btrfs POSIX Access Control Lists [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_BTRFS_FS_POSIX_ACL</code> to find this item.
    < /*> F2FS filesystem support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_F2FS_FS</code> to find this item.
    [ /*]        F2FS extended attributes [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_F2FS_FS_XATTR</code> to find this item.
    [ /*]            F2FS Access Control Lists [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_F2FS_FS_POSIX_ACL</code> to find this item.
Waydroid images use ext4 filesystem, and they will be mounted by waydroid on a loop device:

```
Device Drivers  --->
    [*] Block devices 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_BLK_DEV</code> to find this item. --->
         <M/*> Loopback device support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_BLK_DEV_LOOP</code> to find this item.
File systems  --->
    <M/*> The Extended 4 (ext4) filesystem [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_EXT4_FS</code> to find this item.
bpf syscall is also needed or waydroid will crash when uninstalling an app:

```
General setup  --->
    BPF subsystem  ---> 
         [*] Enable bpf() system call 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_BPF_SYSCALL</code> to find this item.
         [*] Enable BPF Just In Time compiler [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_BPF_JIT</code> to find this item.
    Control Group support  ---> 
         [*] Support for eBPF programs attached to cgroups [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_CGROUP_BPF</code> to find this item.
### Unmask

Waydroid is masked with the \~amd64 keyword, to first unmask it:

**`/etc/portage/package.accept_keywords`**

### Emerge

Then to emerge Waydroid:

`root #``emerge --ask app-containers/waydroid`
## Usage

### Initialization

After installing Waydroid, initialize it:

`root #``waydroid init`
Alternatively, to initialize Waydroid with GAPPS support:

`root #``waydroid init -s GAPPS` ### Starting the container

#### systemd

To start the Waydroid service on [systemd](https://wiki.gentoo.org/wiki/Systemd) systems, start the service first:

`root #``systemctl enable --now waydroid-container.service`
#### OpenRC

On OpenRC, run:

`root #``rc-service waydroid start`
to start the container, and

`root #``rc-update add waydroid`
to have it start on the default runlevel

#### Other

On init systems other than systemd and OpenRC, run:

`root #``waydroid container start`
### Starting the session

To start the session, in a non-root terminal:

`user $``waydroid session start`
## External resources

- [https://docs.waydro.id](https://docs.waydro.id) - The official Waydroid documentation
