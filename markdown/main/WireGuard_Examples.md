<!-- source: https://wiki.gentoo.org/wiki/WireGuard/Examples | group: Gentoo Wiki (Main) | wiki-title: WireGuard/Examples -->
---
title: WireGuard/Examples
url: https://wiki.gentoo.org/wiki/WireGuard/Examples
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-21"
fingerprint: "3df2085ae8007bc1"
license: CC BY-SA 4.0
---

# WireGuard/Examples

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

On this page several example wireguard configurations can be found. Wireguard is often described as simple, yet howto's can be quite daunting. If howto's seem complicated, fear not: the complexity comes from automatically managing the network interfaces and routing. The examples provided here intend to show that configuration can indeed be simple. Three examples are provided:

1. How to setup a wireguard 'split-tunnel' to access a server -- possibly in order to access some application you don't want exposed over public internet.
2. How to access a complete network segment through a wireguard 'split-tunnel' -- this is similar, but also requires relevant routes to be set on the client.
3. How to funnel all network traffic over a wireguard tunnel -- the archetypical VPN-tunnel set-up, which also requires default routing to be set-up.


Although the examples reference clients and servers, wireguard actually has no such distinction and only knows about peers. This makes wireguard suited for other topologies as well.

## Conceptual overview

The wireguard kernel components enabled by CONFIG\_WIREGUARD do more or less three things:

1. They provide a network interface which you can give an IP address etc as typical for network interfaces (usually the wireguard interface is wg0).
2. They provide the mechanism for setting up ciphered connections with peers.
3. They perform routing traffic to/from multiple peers, in case multiple tunnels are set up from the one wireguard interface (e.g. wg0).


Conversely, the wireguard kernel components don't do more or less three things:

1. They don't setup the wireguard interface (e.g. wg0) itself, so unless we do it by hand, we'll need something to do that for us, such as netifrc, systemd-networkd or networkmanager.
2. Also, the wireguard kernel components don't influence Linux' routing table, so we might need something to tell the kernel which traffic should go out over the wireguard interface. This is heavily use case dependent, so it can quickly grow complex -- although it need not.
3. And finally, the wireguard kernel components don't distribute the crypto keys and configuration between client(s) and server, that are needed to encode and decode the data flowing over the connection.


Last but not least, we need to briefly touch on [net-vpn/wireguard-tools](https://packages.gentoo.org/packages/net-vpn/wireguard-tools). They are not kernel components. They do however include the utilities to easily generate the crypto keys needed to succesfully setup wireguard tunnels:

`user $``$(umask 077; wg genkey | tee wg-priv.key | wg pubkey > wg-pub.key)`
## Generic setup

There are two files that need to be edited on both the client and the server:

1. A file that configures how to bring up the wireguard interfaces (such as assigning IP addresses and routes). For netifrc, it will usually be /etc/conf.d/net. For systemd-networkd, it will usually be /etc/systemd/network/50-wg.network.
2. A file with configuration that wireguard needs to setup the tunnel. (Or multiple tunnels in case of use case #1 'server access'.) Usually this will be /etc/wireguard/wg0.conf.


Please bear in mind that the examples provided below use both public-private keypairs for client and server, and additionally a pre-shared key. Only the two public-private key pairs are necessary. The tunnel will function correctly with the pre-shared key directives left out of the client and server /etc/wireguard/wg0.conf files.

## Netifrc examples

### Split tunnel (server access)

This examples sets up a wireguard 'split-tunnel' to access a server -- possibly in order to access some application you don't want exposed over public internet. It will only allow connections to the IP address assigned to the wireguard interface, restricting access to other resources behind the server.

The ip-range chosen for the wireguard network interfaces should not collide with ip-ranges in use by 'regular' network interfaces on client or server. Feel free to change the port number to some other high port number. As an example, we presume the server to have IP address 4.4.4.4. We presume the wireguard interfaces to be: server = 10.0.0.1, client = 10.0.0.2.

If more clients would need to access the server, additional \[Peer\] sections should be added, with the same PreSharedKey, but different AllowedIPs /32 addresses and PublicKeys, that correspond to those clients. Wireguard uses the PublicKey for authentication, and uses AllowedIPs to route traffic to the correct peer based on destination IPs, and to accept traffic from a given peer based on source IPs.

#### Server

**`/etc/conf.d/net`**

**`/etc/wireguard/wg0.conf`**

#### Client

**`/etc/conf.d/net`**

**`/etc/wireguard/wg0.conf`**

### Split tunnel (network segment access)

This example will provide access to a complete network segment through a wireguard 'split-tunnel' -- this is similar to the 'simple' split tunnel, but also requires relevant routes to be set on the client.

In elaboration of the 'Split tunnel (simple)', besides the ip-range for the wireguard interfaces, we also need an ip-range that's 'behind' the wireguard server. As an example, we presume this to be the IP address range 172.24.137.0/24.

#### Server

**`/etc/conf.d/net`**

**`/etc/wireguard/wg0.conf`**

#### Client

**`/etc/conf.d/net`**

**`/etc/wireguard/wg0.conf`**

### VPN tunnel

If you want to use the VPN tunnel for privacy or security, this adds complexity in order to prevent leaking data outside of the tunnel.

#### Server

**`/etc/conf.d/net`**

**`/etc/wireguard/wg0.conf`**



#### Client

**`/etc/conf.d/net`**

**`/etc/wireguard/wg0.conf`**

## Testing the tunnel

Feel free to test the tunnel by sending ICMP ECHO (ping), i.e. on client:

`user $``ping -c 4 10.0.0.1`
You can observe bytes having traveled over the tunnel by noting the byte counters increase in 'wg' on both client and server:

`user $``wg`
interface: wg0
  public key: \<public key>
  private key: (hidden)
  listening port: 24242
peer: \<key>
  preshared key: (hidden)
  endpoint: 4.4.4.4:24242
  allowed ips: 10.0.0.0/24
  latest handshake: 10 minutes, 51 seconds ago
  transfer: 1.04 KiB received, 760 B sent



Alternatively, you can use [net-analyzer/netcat](https://packages.gentoo.org/packages/net-analyzer/netcat). On server:

`user $``nc -l -p 5555 -s 10.0.0.1`
On client:

`user $``nc 10.0.0.1 5555`
ohi

(This 'ohi' message should now also be shown on the server terminal.)
