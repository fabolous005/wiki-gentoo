<!-- source: https://wiki.gentoo.org/wiki/ISCSI/Initiator | group: Gentoo Wiki (Main) | wiki-title: ISCSI/Initiator -->
---
title: ISCSI/Initiator
url: https://wiki.gentoo.org/wiki/ISCSI/Initiator
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-06-16"
fingerprint: fe08b87f79965be9
license: CC BY-SA 4.0
---

# ISCSI/Initiator

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

An iSCSI initiator is a machine that connects to a storage machine on the network. The opposite side of the connection, the storage machine itself, is referred to as the [iSCSI Target](https://wiki.gentoo.org/wiki/ISCSI#iSCSI_Target). Multiple iSCSI initiators can be pointed to one iSCSI Target.

## Installation

### Kernel configuration

**Enabling the iSCSI driver:`CONFIG_INET=y`, `CONFIG_BLK_DEV_SD=y`, `CONFIG_SCSI_LOWLEVEL=y`, `CONFIG_ISCSI_TCP=M` and `CONFIG_SCSI_ISCSI_ATTRS=M`**

In-kernel support can be enabled by building the driver into the kernel (as suggested with the `*`). If building the driver as an external module (as suggested with the `M`) running the make modules\_install command will install all the modules into the kernel, which is essentially the same as building in the modules initially. Anyway sys-block/open-iscsi says builtin doesn't work. Built-in modules for an active kernel are listened in the /lib/modules/$(uname -r)/modules.builtin file. To check to see if if the iscsi\_tcp.ko driver has been built-in, run:

`user $``grep iscsi_tcp.ko "/lib/modules/$(uname -r)/modules.builtin"`
### USE flags


| [+tcp](https://packages.gentoo.org/useflags/+tcp) | Support iSCSI over TCP | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [infiniband](https://packages.gentoo.org/useflags/infiniband) | Enable Infiniband RDMA transport support | 
| [rdma](https://packages.gentoo.org/useflags/rdma) | Support iSCSI over RDMA | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 

### Emerge

After USE flags have been set (or unset) emerge the iSCSI package:

`root #``emerge --ask sys-block/open-iscsi`
## Configuration

### Loading the module

Before iscsid startup the lsmod command can be used to confirm the module has been loaded:

`root #``lsmod | grep iscsi_tcp`
If output is seen from the command above, then the following step is not needed:

`root #``modprobe iscsi_tcp`
### Service

#### OpenRC

To add the iSCSI daemon to the default runlevel:

`root #``rc-update add iscsid default`
To start the iSCSI daemon now:

`root #``service iscsid start`


#### Systemd

To add the iSCSI daemon to the default runlevel:

`root #``systemctl enable iscsid` To start the iSCSI daemon:

`root #``systemctl start iscsid` ## Usage

### Update initator name if needed

**`/etc/iscsi/initiatorname.iscsi`**

**Set InitiatorName**

### Target discovery

Discover iscsi targets:

`root #``iscsiadm -m discovery -t st -p <ip> -P 1`
### Adding targets

Add a target to the configuration:

`root #``iscsiadm -m node -T <target iqn listed in discovery> [-p <ip[:port]>] -l`
### Example

`root #``iscsiadm -m discovery -t st -p 192.168.1.10 -P 1`
Target: iqn.2025-06.com.example:server.target01
	Portal: 192.168.1.10:3260,1
		Iface Name: default

`root #``iscsiadm -m node -T iqn.2025-06.com.example:server.target01 -l`
Login to \[iface: default, target: iqn.2025-06.com.example:server.target01, portal: 192.168.1.10,3260\] successful.

`root #``dmesg` \[ 7672.992912\] thunderbolt 0000:00:0d.2: 0:6 \<-> 1:14 (DP): failed to change bandwidth allocation
\[ 7712.128451\] Loading iSCSI transport class v2.0-870.
\[ 7712.130614\] iscsi: registered transport (tcp)
\[ 7757.121749\] scsi host1: iSCSI Initiator over TCP/IP
\[ 7757.125558\] scsi 1:0:0:0: RAID              IET      Controller       0001 PQ: 0 ANSI: 5
\[ 7757.129678\] scsi 1:0:0:0: Attached scsi generic sg2 type 12
\[ 7757.130419\] scsi 1:0:0:1: Direct-Access     IET      VIRTUAL-DISK     0001 PQ: 0 ANSI: 5
\[ 7757.132855\] sd 1:0:0:1: Attached scsi generic sg3 type 0
\[ 7757.133029\] sd 1:0:0:1: Power-on or device reset occurred
\[ 7757.133306\] sd 1:0:0:1: \[sdc\] 734003200 512-byte logical blocks: (376 GB/350 GiB)
\[ 7757.133310\] sd 1:0:0:1: \[sdc\] 4096-byte physical blocks
\[ 7757.133445\] sd 1:0:0:1: \[sdc\] Write Protect is off
\[ 7757.133447\] sd 1:0:0:1: \[sdc\] Mode Sense: 69 00 10 08
\[ 7757.133737\] sd 1:0:0:1: \[sdc\] Write cache: enabled, read cache: enabled, supports DPO and FUA
\[ 7757.161678\] sd 1:0:0:1: \[sdc\] Attached SCSI disk

### Target persistence

And tell the iscid service to start a target automatically on startup

`root #``iscsiadm -m node -T <target iqn listed in discovery> [-p <ip[:port]>] -o update -n node.startup -v automatic` If you are using systemd, you also need to enable iscsi.service:

`root #``systemctl enable iscsi`
## See also

- [ISCSI/Target](https://wiki.gentoo.org/wiki/ISCSI/Target) — machines that offer storage via iSCSI to a network.
