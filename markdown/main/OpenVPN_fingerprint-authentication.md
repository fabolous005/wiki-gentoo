<!-- source: https://wiki.gentoo.org/wiki/OpenVPN/fingerprint-authentication | group: Gentoo Wiki (Main) | wiki-title: OpenVPN/fingerprint-authentication -->
---
title: OpenVPN/fingerprint-authentication
url: https://wiki.gentoo.org/wiki/OpenVPN/fingerprint-authentication
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-06-07"
fingerprint: e999fd49f10aad83
license: CC BY-SA 4.0
---

# OpenVPN/fingerprint-authentication

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Article status**

- add IPv6 example(when IPv4 is ready)

[OpenVPN](https://wiki.gentoo.org/wiki/OpenVPN)'s **traditional** setup works with using and managing Certificate Authorities (CA)s. Many TLS-based protocols also allow an alternative and simpler mode in which rather than verify certificates using Certificate Authorities (CA)s, the certificate itself is hashed and compared against a pre-known set of acceptable hashes. This is usually referred to as *fingerprint verification*. It is popular across SMTP servers, IRC servers, XMPP servers, and even in the context of HTTP with pinning.

Since OpenVPN version 2.7 the `--secret` support has been removed. The static key setup mode (non-TLS), is no longer a secure enough for nowadays requirements. The replacement for the removed static key setup method is now using TLS mode instead.

The `--peer-fingerprint` makes the TLS mode setup as easy, as it has been using the `--secret` static key setup using older OpenVPN versions.

This setup is not suggested to be a replacement for the [main and **traditional** OpenVPN setup](https://wiki.gentoo.org/wiki/OpenVPN) using a Certificate Authority (CA). This *alternative* way of configuring OpenVPN is meant for *situational* and *ad-hoc* network setup and is targeted for *small* VPN network setups. This could be used as a *fallback* or *backup* solution for the traditional OpenVPN setup.

Further technical informations regarding its main focused use-case area in openvpn-networking, and the [initial request for adding *support for fingerprint authenticaction* are found in the official mailing](https://patchwork.openvpn.net/project/openvpn2/patch/20180417165000.3872-1-Jason@zx2c4.com/).

## Installation

### Additional software

- [dev-libs/openssl](https://packages.gentoo.org/packages/dev-libs/openssl) - Robust, full-featured Open Source Toolkit for the Transport Layer Security (TLS)

## Configuration

The setup explained below uses *suggestive* configure filename /etc/openvpn/P2P.conf for the openvpn service and has the smallest configurable IP VPN client pool configured, to support and to focus on its targeted **small** VPN network, *Peer to Peer* or Point-to-Point, ad-hoc network primary usage area.

The setup example setup explained below uses a different UDP `11194`port setting for the openvpn server, and the VPN client subnet is set to `192.168.254.248/29`, compared to the main [OpenVPN](https://wiki.gentoo.org/wiki/OpenVPN) setup. Using that settings, both setups of openvpn, [OpenVPN](https://wiki.gentoo.org/wiki/OpenVPN) and explained below, can run on the same openvpn server simultaneously.

### Environment variables

Configuration:

- `dev` - `tun1` or `tap1` device
- `proto` -  Define protocol `udp`, `tcp`, `udp6` or `tcp6` for communicating with peer.
- `port` - TCP/UDP port for for local and remote.
- `topology` -  Set IP topology used for `tun` device: `subnet` or `p2p`.

Table overview:

| Hostname | Interface | IP address | Gateway | Network description | 
|---|---|---|---|---|
| **server** | eth0 | `192.168.100.1/24````   ```  |  | Private | 
|  | eth1 | `203.0.113.1/24````   ```  |  | Public - *internet* | 
|  | tun1 | `192.168.254.249/29` |  | VPN | 
| **client** | eth0 | `192.0.2.11/24````   ```  | `192.0.2.1````   ```  | Private or Public | 
|  | tun1 | `192.168.254.250/29` |  | VPN | 
| **dns1** | eth0 | `192.168.100.100/24` | `192.168.100.1` | Private | 

*Explain shortly, using simple words about the DNS and its* gateway *in this scenario, and why it is working*

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

### Server

#### Files

- /etc/conf.d/openvpn - Daemon (system wide) configuration file.
- /etc/openvpn/P2P.conf - Global (system wide) configuration file.
- /etc/openvpn/server/server.key - OpenVPN server's private key
- /etc/openvpn/server/server.crt - OpenVPN server's **self-signed** certificate.
- /etc/openvpn/server/dh2048.pem - Diffie-Helman (DH) parameter file.

Create necessary directory structure

`root #``mkdir -p /etc/openvpn/server`
Change to the openvpn configuration directory, before generating any files

`root #``cd /etc/openvpn`
Generate server.key, a self-signed certificate server.crt. The certificate created in is valid the next 3650 days:

`root #``openssl req -x509 -newkey ec:<(openssl ecparam -name secp384r1) -keyout server.key -out server.crt -nodes -sha256 -days 3650 -subj '/CN=server'`
Generate a SHA256 fingerprint of the /etc/openvpn/server.crt file:

`root #``openssl x509 -fingerprint -sha256 -in server.crt -noout`
sha256 Fingerprint=18:6D:DD:6A:C8:45:7F:A0:9D:89:B0:30:D4:67:02:18:2D:F4:A8:83:F8:EA:D7:14:6D:0E:0B:47:BF:31:71:CF

This generated fingerprint will be put into openvpn clients configuration:

18:6D:DD:6A:C8:45:7F:A0:9D:89:B0:30:D4:67:02:18:2D:F4:A8:83:F8:EA:D7:14:6D:0E:0B:47:BF:31:71:CF

Generate the DH parameter file using openssl. Generate directly to /etc/openvpn/server/ server directory:

`root #``openssl dhparam -out server/dh2048.pem 2048`
Finally move the generated `server.key` and the `server.crt` files to the /etc/openvpn/server directory:

`root #``mv *.key *.crt /etc/openvpn/server`
Create a server configuration file named /etc/openvpn/P2P.conf:

**`/etc/openvpn/P2P.conf`**

**Server example setup with peer-fingerprint authentication**

```
# openvpn 2.6.x version 
# set interface
dev tun1
# set protocol
proto udp
# set port
port 11194
# certificates and keys paths
cert server/server1.crt
key server/server1.key
dh server/dh2048.pem
# set the cipher to be used
cipher AES-256-GCM:AES-128-GCM:CHACHA20-POLY1305
# set the topology
topology subnet
# vpn client IP subnet assing pool
server 192.168.254.248 255.255.255.248
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
# run the server as system user/group nobody
user nobody
group nobody
                            
# fingerprints of trusted VPN clients. Restart is mandatory after modifying client entries.
<peer-fingerprint>
07:A8:30:0D:F5:8A:9A:F3:70:1C:7C:FD:FC:BD:36:97:45:FE:19:2A:C9:1F:72:B1:C0:A4:EE:81:27:22:BD:A6
</peer-fingerprint>
```
Add a openvpn service to openrc

`root #``ln -s /etc/init.d/openvpn /etc/init.d/openvpn.P2P`
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
rc-service openvpn.P2P start
```
`root #````
rc-update add openvpn.P2P default
```
After the openvpn daemon has been started following system entries have changed.

Use the ip link show up command to list the currently active system interfaces. This output displays now **3** active interfaces on the server. Notice the *new* `tun1` inteface:

`user $``ip link show up````
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
    link/ether 0c:64:f8:f1:00:00 brd ff:ff:ff:ff:ff:ff
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
    link/ether 0c:64:f8:f1:00:01 brd ff:ff:ff:ff:ff:ff
8: tun1: <POINTOPOINT,MULTICAST,NOARP,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UNKNOWN mode DEFAULT group default qlen 500
    link/none
```
Use the ip route command to display the servers IP routing table. This shows the server is now connected to a additional IP network `192.168.254.249/29` using the IP address `192.168.254.249` on the `tun1` interface, this is the VPN client network:

`user $``ip route`
default via 203.0.113.1 dev eth0 metric 202 
192.168.100.0/24 dev eth1 proto kernel scope link src 192.168.100.1
192.168.254.248/29 dev tun1 proto kernel scope link src 192.168.254.249
203.0.113.0/24 dev eth0 proto kernel scope link src 203.0.113.2

Verify the new `tun1` interface by simply using the ping command to its configured IP address:

`user $``ping 192.168.254.249`
64 bytes from 192.168.254.249: seq=0 ttl=42 time=0.067 ms
\[...\]
--- 192.168.254.249 ping statistics ---
1 packets transmitted, 5 packets received, 0% packet loss
round-trip min/avg/max = 0.067/0.067/0.067 ms

The interface should respond with usual ICMP echo reply message to the send ping command.

### Client

The client configuration process shown below compared to the official setup has *one* difference. [The original client setup](https://github.com/openvpn/openvpn/blob/master/doc/man-sections/example-fingerprint.rst#adding-a-client) generates the `fingerprint` for the openvpn server using a *already finished* /etc/openvpn/P2P.conf configuration file. Citation from the original setup document:

*Generate the fingerprint of the client certificate. For that we will let OpenSSL read the client configuration file as the x509 command will ignore anything that is not between the begin and end markers of the certificate.*


The resulting command *would* be here:

`user $``openssl x509 -fingerprint -sha256 -noout -in /etc/openvpn/P2P.conf`
The *begin* and the *end* markers in the client configuration file:

\<cert>
-----BEGIN CERTIFICATE-----
\[...\]
-----END CERTIFICATE-----
\</cert>

#### Files

- /etc/conf.d/openvpn - Daemon (system wide) configuration file.
- /etc/openvpn/P2P.conf - Global (system wide) configuration file.
- /etc/openvpn/client/client.key - OpenVPN client's private key.
- /etc/openvpn/client/client.crt - OpenVPN client's **self-signed** certificate.

Create necessary directory structure

`root #``mkdir -p /etc/openvpn/client`
Change to the openvpn configuration directory, before generating any files

`root #``cd /etc/openvpn`
Generate a self-signed certificate for the client

`root #``openssl req -x509 -newkey ec:<(openssl ecparam -name secp384r1) -keyout client.key -out client.crt -nodes -sha256 -days 3650 -subj '/CN=client'`
Generate SHA256 fingerprint of the  `client.crt` file

`root #``openssl x509 -fingerprint -sha256 -noout -in client.crt`
sha256 Fingerprint=07:A8:30:0D:F5:8A:9A:F3:70:1C:7C:FD:FC:BD:36:97:45:FE:19:2A:C9:1F:72:B1:C0:A4:EE:81:27:22:BD:A6

This generated fingerprint will be put into openvpn servers configuration, into the `<peer-fingerprint>` section, at last entry:

07:A8:30:0D:F5:8A:9A:F3:70:1C:7C:FD:FC:BD:36:97:45:FE:19:2A:C9:1F:72:B1:C0:A4:EE:81:27:22:BD:A6

Include the files  `client.key` and `client.crt`, generated at the beginning in the configuration file.

- Put the content of the `client.key` file to the `<key></key>` section
- Put the content of the `client.crt` file to the `<cert></cert>` section

Finally move the generated `client.key` and the `client.crt` files to the /etc/openvpn/client/ directory:

`root #``mv *.key *.crt /etc/openvpn/client`
Both files are now included in the configuriaton file and are not needed anymore.

Create a client configuration file named /etc/openvpn/P2P.conf:

**`/etc/openvpn/P2P.conf`**

**Clients peer-fingerprint authentication**

```
# openvpn 2.6.x version 
# set interface
dev tun1
# set protocol
proto udp
# set the target IP and port of trusted server
remote 203.0.113.2 11194
# client
client
# add the content of the clients .key and .crt file here:
<key>
-----BEGIN PRIVATE KEY-----
MIG2AgEAMBAGByqGSM49AgEGBSuBBAAiBIGeMIGbAgEBBDBuGSpQzWMSK/vxrmxz
pSrxNfoIKJhlzgB5+Or9e0S8ApTwRfbBSqUQd7kDtlfwJNihZANiAARbNnf1L0di
zO+cgQ517CYk6TBMHHAAB5dDNjhk28xipw8Af47oGQny7KR8wpyyAIuqBXv3+A27
ECwDkaB+qIBziQqyBnuh+MoMp01yOYXBveC0fFdkMslPwyzmNcKjeuA=
-----END PRIVATE KEY-----
</key>
<cert>
-----BEGIN CERTIFICATE-----
MIIBtjCCATygAwIBAgIUQbcJEr4KDd9+0/Hvgq1vYX/DvuEwCgYIKoZIzj0EAwIw
EjEQMA4GA1UEAwwHY2xpZW50MTAeFw0yNDAxMjYxNjAzNDhaFw0zNDAxMjMxNjAz
NDhaMBIxEDAOBgNVBAMMB2NsaWVudDEwdjAQBgcqhkjOPQIBBgUrgQQAIgNiAARb
Nnf1L0dizO+cgQ517CYk6TBMHHAAB5dDNjhk28xipw8Af47oGQny7KR8wpyyAIuq
BXv3+A27ECwDkaB+qIBziQqyBnuh+MoMp01yOYXBveC0fFdkMslPwyzmNcKjeuCj
UzBRMB0GA1UdDgQWBBSp/xvYQTJ9akjLp0X1G3THwQYIjTAfBgNVHSMEGDAWgBSp
/xvYQTJ9akjLp0X1G3THwQYIjTAPBgNVHRMBAf8EBTADAQH/MAoGCCqGSM49BAMC
A2gAMGUCMQCJCSNV3CTGtXukTCrpCQuL+2ZOVNafb2Mg8QyAgw2VnXlG9f6i2jA9
dPaE2VYKQfoCMGIWVCPHz7GuCmXpt5U7p9SRqudZAhb/+olgVB5Z09gdzC9Fv2VO
bYFoTsNsKKbodA==
-----END CERTIFICATE-----
</cert>
# set the trusted openvpn server "peer-fingerprint" here:
peer-fingerprint 18:6D:DD:6A:C8:45:7F:A0:9D:89:B0:30:D4:67:02:18:2D:F4:A8:83:F8:EA:D7:14:6D:0E:0B:47:BF:31:71:Cf
```
Add a openvpn service to openrc

`root #``ln -s /etc/init.d/openvpn /etc/init.d/openvpn.P2P`
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
    link/ether 0c:a2:44:59:00:00 brd ff:ff:ff:ff:ff:ff
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

Now start the openvpn.P2P application:

`root #``service openvpn.P2P start`
### systemd

`root #``systemctl start openvpn-client`
The routine explained below is a set of commands to verify the proper working for this setup.

#### Client

**link** 

Use the ip link show up command to list the currently active system interfaces. The output shows the `tun1` interface has shown up among the active interfaces:

`user $``ip link show up````
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
    link/ether 0c:a2:44:59:00:00 brd ff:ff:ff:ff:ff:ff
4: tun1: <POINTOPOINT,MULTICAST,NOARP,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UNKNOWN mode DEFAULT group default qlen 500
    link/none
```
 **IP routing** 

The clients routing table shows 2 new entries:

- VPN client IP network `192.168.254.248/29` available on the `tun1` interface
- VPN client IP network `192.168.100.0/24` available **via** the `tun1` interface

`user $``ip route`
default via 192.0.2.1 dev eth0 metric 202 
192.0.2.0/24 dev eth0 proto kernel scope link src 192.0.2.10 
192.168.100.0/24 via 192.168.254.249 dev tun1 
192.168.254.248/29 dev tun1 proto kernel scope link src 192.168.254.250

 **DNS** 

Display the current setting for DNS on the opevpn client the system:

`user $``more /etc/resolv.conf`
\# Generated by openvpn for interface tun1
domain example.net
nameserver 192.168.100.100

The output shows the openvpn clients DNS server entry has changed to the DNS server configured for the VPN network in the server configuration part. It is set to a IP address used in  the VPN private IP network, (check the IP table overview) it is the `192.168.100.100`.

The IP address set by the openvpn software, should be reachable and respond to send ping commands. Verify its reachability using ping:

`user $``ping 192.168.100.100`
PING 192.168.100.100 (192.168.100.100): 56 data bytes
\[...\]
64 bytes from 192.168.100.100: seq=4 ttl=42 time=2.688 ms
--- 192.168.100.100 ping statistics ---
5 packets transmitted, 5 packets received, 0% packet loss
round-trip min/avg/max = 2.688/3.231/3.664 ms

Now as last check, try out if the name resolution works. Here pinging the dns1 server:

`user $``ping dns1`
PING dns1 (192.168.100.100): 56 data bytes
\[...\]
64 bytes from 192.168.100.100: seq=4 ttl=42 time=3.146 ms
--- dns1 ping statistics ---
5 packets transmitted, 5 packets received, 0% packet loss
round-trip min/avg/max = 3.146/3.280/3.468 ms

The setup of a secure connection is now completed. Everything should work as expected at this point. If still running into issues read the troubleshooting section below.

#### Server

To find out which IP address, from the configured IP pool, `192.168.254.248/29` has been assigned to the connected client VPN client, use the OpenVPN management interface or use the command:

`root #``grep 192.168.254 /var/log/messages`
...
Feb 12 19:01:42 server daemon.notice P2P\[2139\]: client/192.0.2.10:43026 MULTI\_sva: pool returned IPv4=192.168.254.250, IPv6=(Not enabled)

Verify the client connectivity by using the ping command. Ping the clients `tun1`IP address, after the connection from the client has been established.

`user $``ping 192.168.254.250`
PING 192.168.254.250 (192.168.254.250): 56 data bytes
\[...\]
--- 192.168.254.250 ping statistics ---
5 packets transmitted, 5 packets received, 0% packet loss
round-trip min/avg/max = 2.008/2.252/2.525 ms

The remote target IP of the client, connected to the `tun1` interface, should respond with ICMP echo reply message to the send ping command.

### Gentoo specifics

The init script allows multiple tunnels. Decide on a name for the tunnel - eg EXAMPLE.

#### OpenRC

`root #``ln -s /etc/init.d/openvpn /etc/init.d/openvpn.EXAMPLE`
Now create config as /etc/openvpn/EXAMPLE.conf:

`root #``service openvpn.EXAMPLE start`
#### systemd

Due to dependencies server and client operations are separated into two units.

Create the server config as /etc/openvpn/server/EXAMPLE.conf:

`root #``systemctl start openvpn-server@EXAMPLE`
Create the client config as /etc/openvpn/client/EXAMPLE.conf:

`root #``systemctl start openvpn-client@EXAMPLE`
More tunnels can be created by replacing EXAMPLE with more names. Each one has its own configuration and can be stopped and started individually. The default is simply to use openvpn.conf and not symlink the service. Both methods may of course be used.

## Troubleshooting

### Management Interface

Add following lines to the /etc/openvpn/P2P.conf. Choose any desirable, free TCP port available at localhost. In example `7206`:

**`/etc/openvpn/openvpn.conf`**

**Add right at the top, 1-st configuration entry**

Restart the daemon:

`root #``service openvpn restart`
Connect to the openvpn management interface. The management session output shown on the openvpn server side.

Example of a *successfull* authentication procedure done with openvpn using configuration shown:

`user $``telnet localhost 7206`
Trying ::1...
Connected to localhost.
Escape character is '^\]'.
>INFO:OpenVPN Management Interface Version 5 -- type 'help' for more info
>CLIENT:ESTABLISHED,0
>CLIENT:ENV,n\_clients=1
>CLIENT:ENV,time\_unix=1707763011
>CLIENT:ENV,time\_ascii=2024-02-12 19:36:51
>CLIENT:ENV,ifconfig\_pool\_netmask=255.255.255.248
>CLIENT:ENV,ifconfig\_pool\_remote\_ip=192.168.254.250
>CLIENT:ENV,trusted\_port=60503
>CLIENT:ENV,trusted\_ip=192.0.2.10
>CLIENT:ENV,common\_name=client1
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
>CLIENT:ENV,untrusted\_port=60503
>CLIENT:ENV,untrusted\_ip=192.0.2.10
>CLIENT:ENV,tls\_serial\_hex\_0=41:b7:09:12:be:0a:0d:df:7e:d3:f1:ef:82:ad:6f:61:7f:c3:be:e1
>CLIENT:ENV,tls\_serial\_0=375166226863142187075343339831211958748733226721
>CLIENT:ENV,tls\_digest\_sha256\_0=07:a8:30:0d:f5:8a:9a:f3:70:1c:7c:fd:fc:bd:36:97:45:fe:19:2a:c9:1f:72:b1:c0:a4:ee:81:27:22:bd:a6
>CLIENT:ENV,tls\_digest\_0=96:f4:1b:ae:75:08:27:d9:4d:d2:cf:ee:e4:66:f8:7c:a1:de:2b:ff
>CLIENT:ENV,tls\_id\_0=CN=client1
>CLIENT:ENV,X509\_0\_CN\_1=client1
>CLIENT:ENV,X509\_0\_CN=client1
>CLIENT:ENV,remote\_port\_1=11194
>CLIENT:ENV,local\_port\_1=11194
>CLIENT:ENV,proto\_1=udp
>CLIENT:ENV,daemon\_pid=2345
>CLIENT:ENV,daemon\_start\_time=1707763000
>CLIENT:ENV,daemon\_log\_redirect=0
>CLIENT:ENV,daemon=1
>CLIENT:ENV,verb=1
>CLIENT:ENV,config=/etc/openvpn/P2P.conf
>CLIENT:ENV,PEER\_DNS=yes
>CLIENT:ENV,RC\_SVCNAME=openvpn.P2P
>CLIENT:ENV,ifconfig\_local=192.168.254.249
>CLIENT:ENV,ifconfig\_netmask=255.255.255.248
>CLIENT:ENV,script\_context=init
>CLIENT:ENV,tun\_mtu=1500
>CLIENT:ENV,dev=tun1
>CLIENT:ENV,dev\_type=tun
>CLIENT:ENV,redirect\_gateway=0
>CLIENT:ENV,END

Leave the OpenVPN *management interface* by using the quit command:

`user $``exit`
For further information read [OpenVPN management interface](https://openvpn.net/community-resources/management-interface/) usage manual.
