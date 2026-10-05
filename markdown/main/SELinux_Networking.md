<!-- source: https://wiki.gentoo.org/wiki/SELinux/Networking | group: Gentoo Wiki (Main) | wiki-title: SELinux/Networking -->
---
title: SELinux/Networking
url: https://wiki.gentoo.org/wiki/SELinux/Networking
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-06-14"
fingerprint: e438d9194101b8e7
license: CC BY-SA 4.0
---

# SELinux/Networking

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

SELinux supports multiple networking related access controls. Alongside the TCP and UDP socket support, it also supports packet labeling (through SECMARK) and even peer labeling where the label of a process on one system is reflected in the data communication towards the other system, providing end-to-end policy decisions to be taken.

## Introduction

### TCP and UDP socket support

The default access controls for networking by SELinux are based on the labels assigned to TCP and UDP ports and sockets. For instance, the TCP port 80 is labeled with `http_port_t` (and class `tcp_socket`). Access towards this port is then governed through SELinux access controls, such as *name\_connect* and *name\_bind*.

When an application is connecting to a port, the *name\_connect* permission is checked. However, when an application binds to the port, the *name\_bind* permission is checked.

### SECMARK

With SECMARK, it isn't the port that is labeled, but the network package itself.

When a network package is handled by the Linux kernel, the `netfilter` or `iptables` code can be used to manage how these packages are handled. With SECMARK, one of the rules that can be implemented is to add a label to these packages. Once network packages are labeled, SELinux access controls can be implemented on these packages.

Now, it isn't that the network packages themselves are physically labeled - it is only on the current host that labels are assigned to the packages. That means that SECMARK is a *local* aspect for labeling. Labels are *not* traversing the network interface boundary at all.

Once SECMARK labeling is in place, the `packet` class is enabled with the `send` and `recv` permissions. For instance:

#### nftables Example

In this example, ingress ssh traffic will be tagged with the context *system\_u:object\_r:ssh\_server\_packet\_t:s0*, and egress ssh traffic will be tagged with the context *system\_u:object\_r:ssh\_client\_packet\_t:s0*. Only SELinux labelling occurs; a more complete ruleset will require accepting and dropping various packets per administrator discretion.

To start off with, create a new table.

`root #``nft add 'table inet filter'`
Now, create create SECMARK identifiers to associate our packet contexts with an nftables identifier.

`root #``nft add 'secmark inet filter ssh_server { "system_u:object_r:ssh_server_packet_t:s0" }'``root #``nft add 'secmark inet filter ssh_client { "system_u:object_r:ssh_client_packet_t:s0" }'`
Next create two chains, an input chain for ingress traffic, and an output chain for egress traffic. A relatively high priority is assigned as labelling is important and is one of the first things that should occur. When designing a more complete ruleset, traffic that should not be interacted with by the system, i.e. traffic that should be immediately dropped, may be dropped first; there is no need to apply labelling to traffic that never comes into contact with any subjects.

`root #``nft add 'chain inet filter input { type filter hook input priority 225; }'``root #``nft add 'chain inet filter output { type filter hook output priority 225; }'`
Finally, tag tcp and udp for our input and output chains with their respective SECMARK identifiers.

`root #``nft add 'rule inet filter input tcp dport 22 meta secmark set ssh_server'``root #``nft add 'rule inet filter input udp dport 22 meta secmark set ssh_server'``root #``nft add 'rule inet filter output tcp dport 22 meta secmark set ssh_client'``root #``nft add 'rule inet filter output udp dport 22 meta secmark set ssh_client'`
The completed basic ruleset looks something like this:

`root #``nft list ruleset````
table inet filter {
        secmark ssh_server {
                "system_u:object_r:ssh_server_packet_t:s0"
        }
	secmark ssh_client {
                "system_u:object_r:ssh_client_packet_t:s0"
        }
	chain input {
                type filter hook input priority 225; policy accept;
                tcp dport 22 meta secmark set "ssh_server"
                udp dport 22 meta secmark set "ssh_server"
        }
	chain output {
                type filter hook output priority 225; policy accept;
                tcp dport 22 meta secmark set "ssh_client"
                udp dport 22 meta secmark set "ssh_client"
        }
}
```
### Labeled IPSec

### NetLabel

## Handling TCP and UDP socket support

### Listing labels on TCP and UDP sockets

With `semanage port` and `seinfo`, labels assigned to TCP and UDP sockets can be (re)viewed.

For instance, to check the label on the TCP port 80 with `seinfo`:

`user $``seinfo --portcon=80````
        portcon tcp 80 system_u:object_r:http_port_t
        portcon tcp 1-511 system_u:object_r:reserved_port_t
        portcon udp 1-511 system_u:object_r:reserved_port_t
```
To check the label for port 9001 with `semanage`:

`root #``semanage port -l | grep 9001`
tor\_port\_t                     tcp      6969, 9001, 9030, 9050, 9051

### Changing socket labels

When the port is labeled with one of the following labels, then it can be modified to have a different label:

- `unreserved_port_t` (for 1024+ ports that are not directly assigned yet)
- `hi_reserved_port` (for 512-1023 that are not directly assigned yet)
- `reserved_port_t` (for 1-511 that are not directly assigned yet)

So assign a specific label, use `semanage` like so:

`root #``semanage port -a -t http_port_t -p tcp 9224`
### Removing custom port labeling

When a label was assigned through `semanage`, it can be removed with `semanage` as well:

`root #``semanage port -d -t http_port_t -p tcp 9224`
### Listing domains with access to a particular port

To query which domains have access to a particular port, use the `sesearch` command, like so:

`root #``sesearch -t http_port_t -c tcp_socket -p name_bind --allow````
Found 7 semantic av rules:
   allow corenet_unconfined_type port_type : tcp_socket { recv_msg send_msg name_bind name_connect } ; 
   allow svirt_t port_type : tcp_socket { recv_msg send_msg name_bind name_connect } ; 
   allow svirt_lxc_net_t port_type : tcp_socket { recv_msg send_msg name_bind name_connect } ; 
   allow httpd_t http_port_t : tcp_socket { recv_msg send_msg name_bind } ; 
   allow sysadm_t http_port_t : tcp_socket name_bind ; 
   allow stunnel_t port_type : tcp_socket { recv_msg send_msg name_bind name_connect } ; 
ET allow qemu_t port_type : tcp_socket { name_bind name_connect } ; [ qemu_full_network ]
```
## Managing SECMARK rules

### Listing SECMARK firewall rules

To list the current SECMARK label rules, use `iptables`. SECMARK labels are usually assigned to a "mangle" table.

`root #``iptables -t mangle --list`
### Adding a SECMARK labeling rule

For instance, to label packets that originate from 192.168.1.2 and arrive on port 443 with the `myauth_packet_t` label:

`root #``iptables -t mangle -A INPUT -p tcp --src 192.168.1.2 --dport 443 -j SECMARK --selctx system_u:object_r:myauth_packet_t`
