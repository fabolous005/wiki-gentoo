<!-- source: https://wiki.gentoo.org/wiki/OpenVPN | group: Gentoo Wiki (Main) | wiki-title: OpenVPN -->
---
title: OpenVPN
url: https://wiki.gentoo.org/wiki/OpenVPN
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-06-22"
fingerprint: ec98bd597902b982
license: CC BY-SA 4.0
---

# OpenVPN

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

**Article status**

- files needs permissions
- add IPv6 example

**OpenVPN** (Open Virtual Private Network) is software that enables the creation of secure point-to-point or site-to-site connections. This document describes the most common setup configuring a secured, routed VPN connection, using the linux `tun` interface and the [net-vpn/openvpn](https://packages.gentoo.org/packages/net-vpn/openvpn) version 2.6.x  ebuild.

## Installation

### Kernel

**Enable`CONFIG_TUN` in the kernel**

### USE flags


| [+lz4](https://packages.gentoo.org/useflags/+lz4) | Enable support for lz4 compression (as implemented in app-arch/lz4) | 
| [+lzo](https://packages.gentoo.org/useflags/+lzo) | Enable support for lzo compression | 
| [+openssl](https://packages.gentoo.org/useflags/+openssl) | Use OpenSSL as the backend crypto library | 
| [+plugins](https://packages.gentoo.org/useflags/+plugins) | Enable the OpenVPN plugin system | 
| [dco](https://packages.gentoo.org/useflags/dco) | Enable support for kernel data channel offload | 
| [down-root](https://packages.gentoo.org/useflags/down-root) | Enable the down-root plugin | 
| [examples](https://packages.gentoo.org/useflags/examples) | Install examples, usually source code | 
| [inotify](https://packages.gentoo.org/useflags/inotify) | Enable inotify filesystem monitoring support | 
| [iproute2](https://packages.gentoo.org/useflags/iproute2) | Enabled iproute2 support instead of net-tools | 
| [kernel-ovpn](https://packages.gentoo.org/useflags/kernel-ovpn) | Transitional flag to support OVPN mainline module | 
| [mbedtls](https://packages.gentoo.org/useflags/mbedtls) | Use mbed TLS as the backend crypto library | 
| [pam](https://packages.gentoo.org/useflags/pam) | Add support for PAM (Pluggable Authentication Modules) - DANGEROUS to arbitrarily flip | 
| [pkcs11](https://packages.gentoo.org/useflags/pkcs11) | Enable PKCS#11 smartcard support | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

### Emerge

Install the OpenVPN package:

`root #``emerge --ask net-vpn/openvpn`
### Additional software

3 examples DNS server that could be used:

- [net-dns/dnsmasq](https://packages.gentoo.org/packages/net-dns/dnsmasq) - Small forwarding DNS server
- [net-dns/knot](https://packages.gentoo.org/packages/net-dns/knot) - Framework for managing DNS information
- [net-dns/openresolv](https://packages.gentoo.org/packages/net-dns/openresolv) - Framework for managing DNS information

Choose one DNS server product from all availble in portage:

`user $``eix -Sc | grep DNS`
## Configuration

A secure and full setup of OpenVPN explained here, requires placing a large amount of files on different nodes, correctly. For a overview of all *required* files and their placement, lookup the [openvpn.net community article about creation a PKI infrastructure - Key Files](https://openvpn.net/community-resources/setting-up-your-own-certificate-authority-ca/), verify with the table displayed at the end linked article.

The setup of DNS server is *optional* and not explained below. Please follow [dnsmasq](https://wiki.gentoo.org/wiki/Dnsmasq) or [BIND](https://wiki.gentoo.org/wiki/BIND) articles to setup a working DNS server.

### Environment variables

Configuration:

- `dev` - `tun0` or `tap0` device
- `proto` -  Define protocol `udp`, `tcp`, `udp6` or `tcp6` for communicating with peer.
- `port` - TCP/UDP port for for local and remote.
- `topology` -  Set IP topology used for `tun` device: `subnet` or `p2p`

IP address table:

| Hostname | Interface | IP address | Gateway | Network description | 
|---|---|---|---|---|
| **server1** | eth0 | `192.168.100.1/24````   ```  |  | Private | 
|  | eth1 | `203.0.113.1/24````   ```  |  | Public - *internet* | 
|  | tun1 | `192.168.255.1/24` |  | VPN | 
| **client1** | eth0 | `192.0.2.10/24````   ```  | `192.0.2.1````   ```  | Private or Public | 
|  | tun1 | `192.168.255.2/24` |  | VPN | 
| **dns1** | eth0 | `192.168.100.100/24` | `192.168.100.1` | Private | 

### Files

Set the openvpn deamon running options to use local syslog. Add the `--syslog` running option to enable syslog logging:

**`/etc/conf.d/openvpn`**

**Set logging to local syslog**

```
[...]
# Additional arguments to pass to openvpn.
command_args="--syslog"
```
Apply this setting to all nodes, here in document to the server and to the client.

The file /etc/openvpn/\*/ca.crt is the Certifcate Autority's (CA's) *public* certificate. That file is *identical* on **all** VPN domain participating nodes. It is used in different paths in this document:

- /etc/openvpn/server/ca.crt
- /etc/openvpn/client/ca.crt

### Server

#### Files

- /etc/conf.d/openvpn - Daemon (system wide) configuration file.
- /etc/openvpn/openvpn.conf - Global (system wide) configuration file.
- /etc/openvpn/server/ca.crt - CA (VPN domain wide) public certificate.
- /etc/openvpn/server/server1.key - OpenVPN server private key
- /etc/openvpn/server/server1.crt - OpenVPN server's **CA signed** certificate.
- /etc/openvpn/server/dh2048.pem - Diffie-Helman (DH) parameter file.

If this is the first time setting up an openvpn server, we will need to [create a PKI (Public Key Infrastructure) from scratch](https://wiki.gentoo.org/wiki/Create_a_Public_Key_Infrastructure_Using_the_easy-rsa_Scripts).

In this section place all generated keys and certificates into the /etc/openvpn/server/ directory.

The document explains a simple setup for one OpenVPN server only. For multiple OpenVPN instances see the '[Gentoo specifics](https://wiki.gentoo.org#Gentoo_specifics)' section.

Write a server-side openvpn configuration.

**`/etc/openvpn/openvpn.conf`**

**Setup a example UDP OpenVPN server**

```
# openvpn 2.6.x version 
# set interface
dev tun0
# set protocol
proto udp
# set port
port 1194
# certificates and keys paths
ca server/ca.crt
cert server/server1.crt
key server/server1.key
dh server/dh2048.pem
# set the topology
topology subnet
# vpn client IP subnet assigning pool
server 192.168.255.0 255.255.255.0
# persistent device and key settings
persist-key
persist-tun
# update vpn clients IP routing table
push "route 192.168.100.0 255.255.255.0"
# update vpn clients DNS entry
push "dhcp-option DNS 192.168.100.100"
# connection
keepalive 60 300
# notify clients on deamon restart to reconnect quickly
explicit-exit-notify 1
# run the server as system user nobody
user nobody
group nobody
```
Before starting the configured openvpn daemon on the server, display the current state of:

- Active system interfaces
- IP routing table

Use the ip link show up command to list the currently active system interfaces. The command line output shows 2 active interfaces:

- `eth0`
- `eth1`

when leaving out the **lo** - loopback interface.

`user $``ip link show up````
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
    link/ether 0c:64:f8:f1:00:00 brd ff:ff:ff:ff:ff:ff
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
    link/ether 0c:64:f8:f1:00:01 brd ff:ff:ff:ff:ff:ff
```
Use the ip route command to display the IP routing table. The routing table displays 2 entries:

- private IP network `192.168.100.0/24` conntected to the `eth0` interface
- public IP network `203.0.113.0/24` conntected to the `eth1` interface

`user $``ip route`
default via 203.0.113.1 dev eth0 metric 202 
192.168.100.0/24 dev eth1 proto kernel scope link src 192.168.100.1 
203.0.113.0/24 dev eth0 proto kernel scope link src 203.0.113.2

The openvpn server is the IP gateway for both connected networks at `eth0`  and the `eth1` interfaces.

Start the daemon and add to the default startup:

`root #````
rc-service openvpn start
```
`root #````
rc-update add openvpn default
```
After the openvpn daemon has been started following system entries have changed.

Use the ip link show up command to list the currently active system interfaces. This output displays now **3** active interfaces on the server. Notice the *new* `tun0` inteface:

`user $``ip link show up````
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
    link/ether 0c:64:f8:f1:00:00 brd ff:ff:ff:ff:ff:ff
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
    link/ether 0c:64:f8:f1:00:01 brd ff:ff:ff:ff:ff:ff
8: tun0: <POINTOPOINT,MULTICAST,NOARP,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UNKNOWN mode DEFAULT group default qlen 500
    link/none
```
Use the ip route command to display the servers IP routing table. This shows the server is now connected to a additional IP network `192.168.255.0/24` using the IP address `192.168.255.1` on the `tun0` interface, this is the VPN client network:

`user $``ip route`
default via 203.0.113.1 dev eth0 metric 202 
192.168.100.0/24 dev eth1 proto kernel scope link src 192.168.100.1
192.168.255.0/24 dev tun0 proto kernel scope link src 192.168.255.1
203.0.113.0/24 dev eth0 proto kernel scope link src 203.0.113.2

Verify the new `tun0` interface by simply using the ping command to its configured IP address:

`user $``ping 192.168.255.1`
64 bytes from 192.168.255.1: seq=0 ttl=42 time=0.067 ms
\[...\]
--- 192.168.255.1 ping statistics ---
1 packets transmitted, 5 packets received, 0% packet loss
round-trip min/avg/max = 0.067/0.067/0.067 ms

The interface should respond with usual ICMP echo reply message to the send ping command.

### Client

#### Files

- /etc/conf.d/openvpn - Daemon (system wide) configuration file.
- /etc/openvpn/openvpn.conf - Global (system wide) configuration file.
- /etc/openvpn/client/ca.crt - CA (VPN domain wide) public certificate.
- /etc/openvpn/client/client1.key - OpenVPN client private key.
- /etc/openvpn/client/client1.crt - OpenVPN client **CA signed** certificate.

Write a client-side openvpn configuration file:

**`/etc/openvpn/openvpn.conf`**

**client-side udp openvpn configuration**

```
# openvpn 2.6.x version
# specify client-side
client
# tun/tap device
dev tun0
# protocol, according to server
proto udp
# target public IP server address
remote 203.0.113.2 1194
# persistent device and keys
persist-key
persist-tun
# keys settings
ca client/ca.crt
cert client/client1.crt
key client/client1.key
```
## Usage

Before starting the configured openvpn on the client, display the current state of:

- Active system interfaces
- IP routing table
- DNS setting, *only the VPN client*

### Client

Use the ip link show up command to list the currently active system interfaces. The command line output shows 2 active interfaces:

- `lo`
- `eth1`

`user $``ip link show up````
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
    link/ether 0c:f2:7d:16:00:00 brd ff:ff:ff:ff:ff:ff
```
Use the ip route command to display the IP routing table on the client:

- IP network `192.0.2.0/24`connected to the `eth0` interface

`user $``ip route`
default via 192.0.2.1 dev eth0 metric 202 
192.0.2.0/24 dev eth0 proto kernel scope link src 192.0.2.10

Display the current setting for DNS on the opevpn client the system:

`user $``more /etc/resolv.conf`
nameserver 192.0.2.1

The output shows the openvpn clients DNS server is set to a IP in the local area network `192.0.2.1`. *In most cases the local router IP address, the default gateway IP address is also the DNS server*.

### OpenRC

Now start the openvpn application:

`root #``rc-service openvpn start`
### systemd

`root #``systemctl start openvpn-client`
The routine explained below is a set of commands to verify the proper working for this setup.

#### Client

**link** 

Use the ip link show up command to list the currently active system interfaces. The output shows the `tun0` interface has shown up among the active interfaces:

`user $``ip link show up````
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
    link/ether 0c:f2:7d:16:00:00 brd ff:ff:ff:ff:ff:ff
3: tun0: <POINTOPOINT,MULTICAST,NOARP,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UNKNOWN mode DEFAULT group default qlen 500
    link/none 
```
 **IP routing** 

The clients routing table shows 2 new entries:

- VPN client IP network `192.168.255.0/24` available on the `tun0` interface
- VPN client IP network `192.168.100.0/24` available **via** the `tun0` interface

`user $``ip route`
default via 192.0.2.1 dev eth0 metric 202 
192.0.2.0/24 dev eth0 proto kernel scope link src 192.0.2.10
192.168.100.0/24 via 192.168.255.1 dev tun0 
192.168.255.0/24 dev tun0 proto kernel scope link src 192.168.255.2

 **DNS** 

Display the current setting for DNS on the opevpn client the system:

`user $``more /etc/resolv.conf`
\# Generated by openvpn for interface tun0
domain example.net
nameserver 192.168.100.100

The output shows the openvpn clients DNS server entry has changed to the DNS server configured for the VPN network in the server configuration part. It is set to a IP address used in  the VPN private IP network, (check the IP table overview) it is the `192.168.100.100`.

The IP address set by the openvpn software, should be reachable and respond to send ping commands. Verify its reachability using ping:

`user $``ping 192.168.100.100`
PING 192.168.100.100 (192.168.100.100): 56 data bytes
\[...\]
64 bytes from 192.168.100.100: seq=4 ttl=42 time=3.525 ms
--- 192.168.100.100 ping statistics ---
5 packets transmitted, 5 packets received, 0% packet loss
round-trip min/avg/max = 2.648/3.451/4.731 ms

Now as last check, try out if the name resolution works. Here pinging the dns1 server:

`user $``ping dns1`
PING dns1 (192.168.100.100): 56 data bytes
\[...\]
64 bytes from 192.168.100.100: seq=4 ttl=42 time=3.525 ms
--- dns1 ping statistics ---
5 packets transmitted, 5 packets received, 0% packet loss
round-trip min/avg/max = 2.691/2.857/3.003 ms

The setup of a secure connection is now completed. Everything should work as expected at this point. If still running into issues read the troubleshooting section below.

#### Server

To find out which IP address, from the configured IP pool, `192.168.255.0/24` has been assigned to the connected client VPN client, use the OpenVPN management interface or use the command:

`root #``grep 192.168.255 /var/log/messages`
...
Jan 30 07:44:40 server1 daemon.notice openvpn\[2121\]: client1/192.0.2.10:52523 MULTI\_sva: pool returned IPv4=192.168.255.2, IPv6=(Not enabled)

Verify the client connectivity by using the ping command. Ping the clients `tun0`IP address, after the connection from the client has been established.

`user $``ping 192.168.255.2`
PING 192.168.255.2 (192.168.255.2): 56 data bytes
\[...\]
--- 192.168.255.2 ping statistics ---
5 packets transmitted, 5 packets received, 0% packet loss
round-trip min/avg/max = 1.784/1.939/2.094 ms

The remote target IP of the client, connected to the `tun0` interface, should respond with ICMP echo reply message to the send ping command.

### Gentoo specifics

The init script allows the creation and running of multiple tunnels/connections at the same. Below the connection name is named: EXAMPLE.

#### OpenRC

Create the client configuration as /etc/openvpn/client/EXAMPLE.conf:

`root #``nano -w /etc/openvpn/EXAMPLE.conf`
Create a symbolic link with the name to the configuraiton file:

`root #``ln -s /etc/init.d/openvpn /etc/init.d/openvpn.EXAMPLE`
Add the connetction to the default runlevel, when the connection should start at system boot:

`root #``rc-update add openvpn.EXAMPLE default`
To open the connection right now, use:

`root #``rc-service openvpn.EXAMPLE start`
#### systemd

Due to dependencies server and client operations are separated into two units.

Create the server config as /etc/openvpn/server/EXAMPLE.conf:

`root #``systemctl start openvpn-server@EXAMPLE`
Create the client config as /etc/openvpn/client/EXAMPLE.conf:

`root #``systemctl start openvpn-client@EXAMPLE`
More tunnels can be created by replacing EXAMPLE with more names. Each one has its own configuration and can be stopped and started individually. The default is simply to use openvpn.conf and not symlink the service. Both methods may of course be used.

## Troubleshooting

### Sanitize configuration

Verifying /etc/openvpn/openvpn.conf configuration file with `--verbose`. Change into the /etc/openvpn directory, the configuration has relative configuration paths included:

`root #``cd /etc/openvpn`
The command line output will display the matching configuration line and config option used `18:cipher`, if the syntax does not match. openvpn verbosity levels `--verb` are `0 - 11`. The default verbosity level is `1`. Example `--verb 2` command output displaying a configuration error:

`root #``openvpn --config openvpn.conf --verb 2`
Options error: Unrecognized option or missing or extra parameter(s) in openvpn.conf:18: cipher (2.6.8)
Use --help for more information.

If the config is sane, openvpn will start the in the terminal.

### Management Interface

Add following lines to the /etc/openvpn/openvpn.conf. Choose any desirable, free TCP port available at localhost. In example `7301`:

**`/etc/openvpn/openvpn.conf`**

**Add right at the top, 1-st configuration entry**

Restart the daemon:

`root #``rc-service openvpn restart`
Connect to the openvpn management interface. The management session output running on the openvpn server side.

The output shows a *successfull* authentication procedure done with openvpn software from a client using configuration from this example.

All important settings used in the configure section above in this document in the *Server* and the *Client* part, be easily spotted in the management interface output log:

`user $``telnet localhost 7301`
Trying ::1...
Connected to localhost.
Escape character is '^\]'.
>INFO:OpenVPN Management Interface Version 5 -- type 'help' for more info
>CLIENT:ESTABLISHED,0
>CLIENT:ENV,n\_clients=1
>CLIENT:ENV,time\_unix=1706538709
>CLIENT:ENV,time\_ascii=2024-01-29 15:31:49
>CLIENT:ENV,ifconfig\_pool\_netmask=255.255.255.0
>CLIENT:ENV,ifconfig\_pool\_remote\_ip=192.168.255.2
>CLIENT:ENV,trusted\_port=33675
>CLIENT:ENV,trusted\_ip=192.0.2.10
>CLIENT:ENV,common\_name=VPNcl1
>CLIENT:ENV,IV\_COMP\_STUBv2=1
>CLIENT:ENV,IV\_COMP\_STUB=1
>CLIENT:ENV,IV\_LZO\_STUB=1
>CLIENT:ENV,IV\_PROTO=990
>CLIENT:ENV,IV\_CIPHERS=AES-256-GCM:AES-128-GCM:CHACHA20-POLY1305
>CLIENT:ENV,IV\_NCP=2
>CLIENT:ENV,IV\_MTU=1600
>CLIENT:ENV,IV\_TCPNL=1
>CLIENT:ENV,IV\_PLAT=linux
>CLIENT:ENV,IV\_VER=2.6.8
>CLIENT:ENV,untrusted\_port=33675
>CLIENT:ENV,untrusted\_ip=192.0.2.10
>CLIENT:ENV,tls\_serial\_hex\_0=3b:92:62:75:d9:25:8f:62:f2:c8:63:25:8c:a9:bc:e7
>CLIENT:ENV,tls\_serial\_0=79184524105695947877959046836863483111
>CLIENT:ENV,tls\_digest\_sha256\_0=58:0e:a2:c2:29:b4:51:19:9e:33:d0:c2:18:5a:7f:82:12:12:33:cc:bb:80:b9:58:09:be:73:3e:9e:01:d4:7f
>CLIENT:ENV,tls\_digest\_0=2f:47:20:f4:ff:4b:66:f8:2b:42:45:f2:c0:8d:b0:0b:7d:e2:9d:28
>CLIENT:ENV,tls\_id\_0=CN=VPNcl1
>CLIENT:ENV,X509\_0\_CN=VPNcl1
>CLIENT:ENV,tls\_serial\_hex\_1=07:d1:dd:c5:64:d2:20:52:69:c0:88:45:e9:f3:8a:31:e5:0b:f6:89
>CLIENT:ENV,tls\_serial\_1=44643110127090816830928405865790676542383978121
>CLIENT:ENV,tls\_digest\_sha256\_1=e9:15:25:77:c6:33:58:69:a3:c7:ce:d0:ea:33:95:94:fd:a1:11:d1:fe:ac:78:e3:5c:3c:e6:c6:7e:c0:f0:46
>CLIENT:ENV,tls\_digest\_1=c7:07:fd:09:96:fd:0d:60:38:bd:cb:d1:9f:54:2a:ee:12:e5:4c:e6
>CLIENT:ENV,tls\_id\_1=CN=Easy-RSA CA
>CLIENT:ENV,X509\_1\_CN=Easy-RSA CA
>CLIENT:ENV,remote\_port\_1=1194
>CLIENT:ENV,local\_port\_1=1194
>CLIENT:ENV,proto\_1=udp
>CLIENT:ENV,daemon\_pid=2388
>CLIENT:ENV,daemon\_start\_time=1706538686
>CLIENT:ENV,daemon\_log\_redirect=0
>CLIENT:ENV,daemon=1
>CLIENT:ENV,verb=1
>CLIENT:ENV,config=/etc/openvpn/openvpn.conf
>CLIENT:ENV,PEER\_DNS=yes
>CLIENT:ENV,RC\_SVCNAME=openvpn
>CLIENT:ENV,ifconfig\_local=192.168.255.1
>CLIENT:ENV,ifconfig\_netmask=255.255.255.0
>CLIENT:ENV,script\_context=init
>CLIENT:ENV,tun\_mtu=1500
>CLIENT:ENV,dev=tun0
>CLIENT:ENV,dev\_type=tun
>CLIENT:ENV,redirect\_gateway=0
>CLIENT:ENV,END

The management interface output shows the Certificate Authority (CA) name was using *Easy-RSA*.

Leave the OpenVPN *management interface* by using the exit command:

`user $``quit`
For further information read [OpenVPN management interface](https://openvpn.net/community-resources/management-interface/) usage manual.

## See also

- [OpenVPN/fingerprint-authentication](https://wiki.gentoo.org/wiki/OpenVPN/fingerprint-authentication)
- [Resolv.conf](https://wiki.gentoo.org/wiki/Resolv.conf) — used to configure hostname resolution.
- [vpnc](https://wiki.gentoo.org/wiki/Vpnc) — IPsec (Cisco/Juniper) VPN concentrator client
- [WireGuard](https://wiki.gentoo.org/wiki/WireGuard) — a modern, simple, and secure VPN that utilizes state-of-the-art cryptography.
- [VPN services](https://wiki.gentoo.org/wiki/VPN_services)
