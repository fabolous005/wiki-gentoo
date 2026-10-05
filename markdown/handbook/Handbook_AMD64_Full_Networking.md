<!-- source: https://wiki.gentoo.org/wiki/Handbook:AMD64/Full/Networking | group: Gentoo Handbook | wiki-title: Handbook:AMD64/Full/Networking -->
---
title: "Gentoo Linux amd64 Handbook: Network configuration"
url: https://wiki.gentoo.org/wiki/Handbook:AMD64/Full/Networking
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2014-12-13"
fingerprint: a291f558d8010bc1
license: CC BY-SA 4.0
---

# Gentoo Linux amd64 Handbook: Network configuration

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)




The following portion of the Handbook describes 'simple' network configuration for systems running the [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) init system, utilizing [netifrc](https://wiki.gentoo.org/wiki/Netifrc) as the network management system.

Netifrc is a simple framework for configuring and managing network interfaces on OpenRC based systems. [sys-apps/openrc](https://packages.gentoo.org/packages/sys-apps/openrc) pulls [net-misc/netifrc](https://packages.gentoo.org/packages/net-misc/netifrc) automatically, as the `netifrc` [USE flag](https://wiki.gentoo.org/wiki/USE_flag) is enabled by default.

In order to manage an interface with netifrc, an *init script* for that interface must be created. By default, netifrc installs /etc/init.d/net.lo, which can be symlinked to create *init scripts* for new interfaces.

To create a new init script for interface **eth0**, simply symlink the default **net.lo** script:

`root /etc/init.d #``ln -s net.lo net.eth0`
Ethernet interfaces will often work without any additional config, as netifrc will automatically use DHCP for interfaces with no specified config.

If additional configuration is needed, for options such as static IPs, /etc/conf.d/net can be edited:

**`/etc/conf.d/net`**

**Setting a static IP for**eth0**.**

```
# For static IP using CIDR notation
config_eth0="192.168.0.7/24"
routes_eth0="default via 192.168.0.1"
dns_servers_eth0="192.168.0.1 8.8.8.8"
# For static IP using netmask notation
config_eth0="192.168.0.7 netmask 255.255.255.0"
routes_eth0="default via 192.168.0.1"
dns_servers_eth0="192.168.0.1 8.8.8.8"
```
With the interface init scripts created and configured, netifrc services can be managed using rc-service. To start **eth0**:

`root #``rc-service net.eth0 start`
To start **eth0** automatically at startup, it can be added to the *default* runlevel:

`root #``rc-update add net.eth0 default`








The `config_{interface}` variable - where `{interface}` is an interface name, e.g. `eth0` - is the heart of an interface configuration. The variable's value is a high-level instruction list for configuring the specified interface.

Here's a list of built-in instructions:

| Value | Description | 
|---|---|
| `null` | Do nothing. | 
| `noop` | If the interface is up and there is an address then abort configuration successfully. | 
| An IPv4 or IPv6 address | Add the address to the interface. | 
| `dhcp`, `adsl`, or `apipa` (or a custom value from a 3rd party module) | Run the module which provides the command. For example, `dhcp` will run a module that provides DHCP (e.g. via dhcpcd, dhclient, etc.). | 

To handle command failure, a corresponding fallback value can be specified via the `fallback_{interface}` variable. The value has to exactly match the structure of the corresponding `config_{interface}` variable.

It is possible to chain these values together. Here are some real world examples:

**`/etc/conf.d/net`**

**Configuration examples**

```
# Adding three IPv4 addresses
config_eth0="192.168.0.2/24
192.168.0.3/24
192.168.0.4/24"
  
# Adding an IPv4 address and two IPv6 addresses
config_eth0="192.168.0.2/24
4321:0:1:2:3:4:567:89ab
4321:0:1:2:3:4:567:89ac"
  
# Keep our kernel assigned address, unless the interface goes
# down so assign another via DHCP. If DHCP fails then add a
# static address determined by APIPA
config_eth0="noop
dhcp"
fallback_eth0="null
apipa"
```
Init scripts in /etc/init.d/ can depend on a specific network interface or just "net". All network interfaces in Gentoo's init system provide what is called "net".

If, in /etc/rc.conf, the `rc_depend_strict` variable is set to `YES`, then all network interfaces that provide "net" *must* be active before a dependency on "net" is assumed to be met. In other words, if a system has a net.eth0 and net.eth1 and an init script depends on "net", then both must be enabled.

On the other hand, if `rc_depend_strict="NO"` is set, then the "net" dependency is marked as resolved the moment at least one network interface is brought up.

But what about net.br0 depending on net.eth0 and net.eth1? net.eth1 may be a wireless or PPP device that needs configuration before it can be added to the bridge. This cannot be done in /etc/init.d/net.br0 as that's a symbolic link to net.lo.

The answer is to define a `rc_net_{interface}_need` setting in /etc/conf.d/net:

**`/etc/conf.d/net`**

**Adding a net.br0 dependency**

```
rc_net_br0_need="net.eth0 net.eth1"
```
That alone, however, is not sufficient. Gentoo's networking init scripts use a virtual dependency called "net" to inform the system when networking is available. Clearly, in the above case, networking should only be marked as available when net.br0 is up, not when the others are. So we need to tell that in /etc/conf.d/net as well:

**`/etc/conf.d/net`**

**Updating virtual dependencies and provisions for networking**

```
rc_net_eth0_provide="!net"
rc_net_eth1_provide="!net"
```
For a more detailed discussion about dependencies, consult [the section on writing initscripts in the Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/Initscripts).

Variable names are dynamic. They normally follow the structure: `{variable}_{interface|mac|essid|apmac}`. For example, the variable dhcpcd\_eth0 holds the value for dhcpcd options for eth0 and dhcpcd\_essid holds the value for dhcpcd options when any interface connects to the ESSID "essid".

However, there is no rule that states interface names must be of the form `eth`. For example, by default the kernel will give wireless interfaces names like *x*`wlan`. Also, some user defined interfaces such as bridges can be given any name. To make life more interesting, wireless Access Points can have names with non alpha-numeric characters in them - this is important because users can configure networking parameters per ESSID.
*x*

However, Gentoo uses Bash variables for networking, and variable names in Bash cannot contain anything other than English alpha-numerics. To get around this limitation we change every character that is not an English alpha-numeric into an `_` (underscore) character.

Additionally, some values for Bash variables require certain characters to be escaped: `"` (double quote), `'` (single quote) and `\` (backslash). This can be achieved by placing the '\' (backslash) character in front of the character that needs to be escaped.

For example, to specify the ESSID `My "\ NET`:

**`/etc/conf.d/net`**

**Variable names**

```
# This does work, but the domain is invalid
dns_domain_My____NET="My \"\\ NET"
```
The above sets the DNS domain to *My "\ NET* when a wireless card connects to an AP whose ESSID is *My "\ NET*.

Network interface names are not chosen arbitrarily: the Linux kernel and the device manager (most systems have udev as their device manager although others are available as well) choose the interface name through a fixed set of rules.

When an interface card is detected on a system, the Linux kernel gathers the necessary data about this card. This includes:

- The onboard (on the interface itself) registered name of the network card, which is later seen through the `ID_NET_NAME_ONBOARD` value.
- The slot in which the network card is plugged in, which is later seen through the `ID_NET_NAME_SLOT` value.
- The path through which the network card device can be accessed, which is later seen through the `ID_NET_NAME_PATH` value.
- The (vendor-provided) MAC address of the card, which is later seen through the `ID_NET_NAME_MAC` value.

Based on this information, the device manager decides how to name the interface on the system. By default, it uses the first hit of the first three variables above (`ID_NET_NAME_ONBOARD`, `_SLOT` or `_PATH`). For instance, if `ID_NET_NAME_ONBOARD` is found and set to `eno1`, then the interface will be called eno1.

Given an active interface name, the values of the provided variables can be shown using udevadm:

`root #``udevadm test-builtin net_id /sys/class/net/enp3s0 2>/dev/null`
ID\_NET\_NAME\_MAC=enxc80aa9429d76
ID\_OUI\_FROM\_DATABASE=Quanta Computer Inc.
ID\_NET\_NAME\_PATH=enp3s0

As the first (and actually only) hit of the top three variables is `ID_NET_NAME_PATH`, its value is used as the interface name. If none of the variables contain values, then the system reverts back to the kernel-provided naming (eth0, eth1, etc.)

Before this change, network interface cards were named by the Linux kernel itself, depending on the order that drivers are loaded (amongst other, possibly more obscure reasons). This behavior can still be enabled by setting the `net.ifnames=0` boot parameter in the boot loader.

The entire idea behind the change in naming is not to confuse people, but to make changing the names easier. Suppose a system has two interfaces that are otherwise called eth0 and eth1. One is meant to access the network through a wire, the other one is for wireless access. With the support for interface naming, users can have these called lan0 (wired) and wifi0 (wireless - it is best to avoid using the previously well-known names like eth\* and wlan\* as those can still collide with the suggested names).

Find out what the parameters are for the cards and then use this information to set up a custom own naming rule:

`root #``udevadm test-builtin net_id /sys/class/net/eth0 2>/dev/null`
ID\_NET\_NAME\_MAC=enxc80aa9429d76
ID\_OUI\_FROM\_DATABASE=Quanta Computer Inc.

`root #``vim /etc/udev/rules.d/70-net-name-use-custom.rules````
# First one uses MAC information, and 70- number to be before other net rules
SUBSYSTEM=="net", ACTION=="add", ATTR{address}=="c8:0a:a9:42:9d:76", NAME="lan0"
```
`root #``vim /etc/udev/rules.d/76-net-name-use-custom.rules````
# Second one uses ID_NET_NAME_PATH information, and 76- number to be between
# 75-net-*.rules and 80-net-*.rules
SUBSYSTEM=="net", ACTION=="add", ENV{ID_NET_NAME_PATH}=="enp3s0", NAME="wifi0"
```
Because the rules are triggered before the default one (rules are triggered in alphanumerical order, so 70 comes before 80) the names provided in the rule file will be used instead of the default ones. The number granted to the file should be between 76 and 79 (the environment variables are defined by a rule start starts with 75 and the fallback naming is done in a rule numbered 80).









[Netifrc](https://wiki.gentoo.org/wiki/Netifrc) supports modular networking scripts, which means support for new interface types and configuration modules can easily be added while keeping compatibility with existing ones.

Modules load by default if the package they need is installed. If users specify a module here that doesn't have its package installed then they get an error stating which package they need to install. Ideally, the modules setting is only used when two or more packages are installed that supply the same service and one needs to be preferred over the other.

**`/etc/conf.d/net`**

**Module definitions**

```
# Prefer ifconfig over iproute2
# modules="ifconfig"
  
# You can also specify other modules for an interface
# In this case we prefer dhclient over dhcpcd
modules_eth0="dhclient"
  
# You can also specify which modules not to use - for example you may be
# using a supplicant or linux-wlan-ng to control wireless configuration but
# you still want to configure network settings per ESSID associated with.
modules="!iwconfig"
```
We provide two interface handlers: ifconfig and iproute2. You need one of these to do any kind of network configuration.

Both are installed by default as part of the system profile. iproute2 is the more powerful and flexible package. ifconfig and net-tools should not be used anymore for networking configuration setups.

As iproute2 and ifconfig do very similar things, we allow their basic configuration to work with each other. For example, the below configuration works regardless of the module used.

**`/etc/conf.d/net`**

**Example configuration**

```
config_eth0="192.168.0.2/24"
config_eth0="192.168.0.2 netmask 255.255.255.0"
```
DHCP is a means of obtaining an IP address, together with other network information (DNS server(s), gateway, etc.) from a server. With a DHCP server running on a network, the user just has to tell each device to obtain configuration information via DHCP, which is then used to configure the device automatically. Of course, this requires a network connection (e.g. via a wireless access point, PPPoE, etc.).

DHCP can be provided by dhclient or dhcpcd. Each DHCP module has its pros and cons - here is a quick run down:

| DHCP module | Package | Pros | Cons | 
|---|---|---|---|
| dhclient | [net-misc/dhcp](https://packages.gentoo.org/packages/net-misc/dhcp) | Made by ISC, the same people who make the BIND DNS software. Very configurable. Can be used to provide DHCPv4 or DHCPv6. | Configuration is overly complex, software is quite bloated, cannot get NTP servers from DHCP, does not send hostname by default. No longer maintained upstream. | 
| dhcpcd | [net-misc/dhcpcd](https://packages.gentoo.org/packages/net-misc/dhcpcd) | Long time Gentoo default, no reliance on outside tools, actively developed by Gentoo. Provides DHCPv4 and DHCPv6 at the same time. | Can be slow at times, does not yet daemonize when lease is infinite. | 

If more than one DHCP client is installed, it is possible to specify which client to use by setting the "modules" variable, e.g. `modules="dhcpcd"`. Otherwise, if dhcpcd is installed, it's used by default.

To send specific options to the DHCP module, use *module*\_eth0="...", where *module* is the name of the DHCP module being used - for example, "dhcpcd\_eth0".

We try to make DHCP relatively agnostic - as such we support the following commands using the `dhcp_eth0` variable. The default is not to set any of them:

- `release`
- Releases the IP address for re-use.
- `nodns`
- Don't overwrite /etc/resolv.conf
- `nontp`
- Don't overwrite /etc/ntp.conf
- `nonis`
- Don't overwrite /etc/yp.conf

**`/etc/conf.d/net`**

**Sample DHCP (v4) configuration**

```
# Only needed if more than one DHCP module is installed
modules="dhcpcd"
  
config_eth0="dhcp"
dhcpcd_eth0="-t 10" # Timeout after 10 seconds
dhcp_eth0="release nodns nontp nonis" # Only get an address
```
**`/etc/conf.d/net`**

**Sample DHCPv6 configuration**

```
# Only needed if more than one DHCP module is installed
modules="dhclient"
  
config_eth0="dhcpv6"
# To use both DHCPv4 and DHCPv6 on a dual-stack network, remove the above line and uncomment the following lines
#config_eth0="dhcp
#dhcpv6"
# To pass runtime arguments to dhclient for DHCPv6
dhclientv6_eth0="-t 10" # Timeout after 10 seconds
# Set generic DHCPv6 options
dhcpv6_eth0="release nodns nontp nonis nogateway nosendhost"
```
First install the ADSL software:

`root #``emerge --ask net-dialup/ppp`
Second, create the PPP net script and the net script for the Ethernet interface to be used by PPP:

`root #````
ln -s /etc/init.d/net.lo /etc/init.d/net.ppp0
```
`root #``ln -s /etc/init.d/net.lo /etc/init.d/net.eth0`
Be sure to set `rc_depend_strict` to `YES` in /etc/rc.conf.

Now we need to configure /etc/conf.d/net.

**`/etc/conf.d/net`**

**A basic PPPoE setup**

```
config_eth0=null (Specify the ethernet interface)
config_ppp0="ppp"
link_ppp0="eth0" (Specify the ethernet interface)
plugins_ppp0="pppoe"
username_ppp0='user'
password_ppp0='password'
pppd_ppp0="
noauth
defaultroute
usepeerdns
holdoff 3
child-timeout 60
lcp-echo-interval 15
lcp-echo-failure 3
noaccomp noccp nobsdcomp nodeflate nopcomp novj novjccomp"
  
rc_net_ppp0_need="net.eth0"
```
It is also possible to set the password in /etc/ppp/pap-secrets.

**`/etc/ppp/pap-secrets`**

**Sample pap-secrets**

If PPPoE is used with a USB modem then make sure to emerge br2684ctl. Please read /var/db/repos/gentoo/net-dialup/speedtouch-usb/files/README for information on how to properly configure it.

## APIPA (Automatic Private IP Addressing)

APIPA tries to find a free address in the range 169.254.0.0-169.254.255.255 by using arping to ping a random address in that range on the interface. If no reply is received, that address is assigned to the interface.

This is only useful for LANs where:

- there is no DHCP server;
- the system doesn't connect directly to the Internet; and
- all other computers use APIPA.

For APIPA support, emerge [net-misc/iputils](https://packages.gentoo.org/packages/net-misc/iputils) with the `arping` USE flag or [net-analyzer/arping](https://packages.gentoo.org/packages/net-analyzer/arping).

**`/etc/conf.d/net`**

**APIPA configuration**

```
# Try DHCP first - if that fails then fallback to APIPA
config_eth0="dhcp"
fallback_eth0="apipa"
  
# Just use APIPA
config_eth0="apipa"
```
Bonding is used to increase network bandwidth or to improve resiliency in the face of hardware failures. If a system has two network cards going to the same network, they can be bonded: applications will see just one interface, but both network cards will be used.

There are many ways to configure bonding. Some of them, such as the 802.3ad LACP mode, require support and additional configuration of the network switch. For a reference of the individual options, please refer to the local copy of /usr/src/linux/Documentation/networking/bonding.txt.

First, clear the configuration of the participating interfaces:

**`/etc/conf.d/net`**

**Clearing interface configuration**

```
config_eth0="null"
config_eth1="null"
config_eth2="null"
```
Next, define the bonding between the interfaces:

**`/etc/conf.d/net`**

**Define the bonding**

```
slaves_bond0="eth0 eth1 eth2"
config_bond0="192.168.100.4/24"
# Pick a correct mode and additional configuration options which suit your needs
mode_bond0="balance-alb"
```
Remove the net.eth\* services from the relevant runlevel, create a net.bond0 service, and add that service to the appropriate runlevel.

Bridging is used to join networks together. For example, a network may have a server that connects to the Internet via an ADSL modem, and also has a wireless adapter to allow other devices on the network to access the Internet via that modem. It is possible to create a bridge to join the two interfaces together.

**`/etc/conf.d/net`**

**Bridge configuration**

```
# Configure the bridge - "man brctl" for more details
bridge_forward_delay_br0=0
bridge_hello_time_br0=200
bridge_stp_state_br0=1
  
# To add ports to bridge br0
bridge_br0="eth0 eth1"
  
# You need to configure the ports to null values so dhcp does not get started
config_eth0="null"
config_eth1="null"
  
# Finally give the bridge an address - you could use DHCP as well
config_br0="192.168.0.1/24"
  
# Depend on eth0 and eth1 as they may require extra configuration
rc_net_br0_need="net.eth0 net.eth1"
```
It is possible to change the MAC address of the interfaces through the network configuration file too.

**`/etc/conf.d/net`**

**MAC Address change example**

```
# To set the MAC address of the interface
mac_eth0="00:11:22:33:44:55"
  
# To randomize the last 3 bytes only
mac_eth0="random-ending"
  
# To randomize between the same physical type of connection (e.g. fibre,
# copper, wireless) , all vendors
mac_eth0="random-samekind"
  
# To randomize between any physical type of connection (e.g. fibre, copper,
# wireless) , all vendors
mac_eth0="random-anykind"
  
# Full randomization - WARNING: some MAC addresses generated by this may
# NOT act as expected
mac_eth0="random-full"
```
Tunneling does not require any additional software to be installed as the interface handler can do it.

**`/etc/conf.d/net`**

**Tunneling configuration**

```
# For GRE tunnels
iptunnel_vpn0="mode gre remote 207.170.82.1 key 0xffffffff ttl 255"
  
# For IPIP tunnels
iptunnel_vpn0="mode ipip remote 207.170.82.2 ttl 255"
  
# To configure the interface
config_vpn0="192.168.0.2 peer 192.168.1.1"
```
For VLAN support, make sure that [sys-apps/iproute2](https://packages.gentoo.org/packages/sys-apps/iproute2) is installed, and ensure that iproute2 is used as the configuration module (rather than ifconfig).

A VLAN, Virtual LAN, is a group of network devices that behave as if they were connected to a single network segment, even though they may not be. Members of a VLAN can only see other members of the same VLAN, even when they share the same physical network.

To configure VLANs, first specify the VLAN numbers in /etc/conf.d/net like so:

**`/etc/conf.d/net`**

**Specifying VLAN numbers**

```
vlans_eth0="1 2"
```
Next, configure the interface for each VLAN:

**`/etc/conf.d/net`**

**Interface configuration for each VLAN**

```
config_eth0_1="172.16.3.1 netmask 255.255.254.0"
routes_eth0_1="default via 172.16.3.254"
  
config_eth0_2="172.16.2.1 netmask 255.255.254.0"
routes_eth0_2="default via 172.16.2.254"
```
VLAN-specific configurations are handled by vconfig like so:

**`/etc/conf.d/net`**

**Configuring the VLANs**

```
vlan1_name="vlan1"
vlan1_ingress="2:6 3:5"
eth0_vlan1_egress="1:2"
```








Wireless networking on Linux is usually pretty straightforward. There are three ways of configuring wifi: graphical clients, text-mode interfaces, and command-line interfaces.

The easiest way is to use a graphical client once a desktop environment is installed. Most graphical clients, such as [NetworkManager](https://wiki.gentoo.org/wiki/NetworkManager), are pretty self-explanatory. They offer a handy point-and-click interface that gets users on a network in just a few seconds.

Wireless can also be configured from the command line by editing a few configuration files. This takes a bit more time to setup, but it also requires the fewest packages to download and install. Since the graphical clients are mostly self-explanatory (with helpful screen shots at their home pages), we'll focus on the command line alternatives.

There are three tools that support command-line driven wireless configurations: [net-wireless/iw](https://packages.gentoo.org/packages/net-wireless/iw), [net-wireless/wireless-tools](https://packages.gentoo.org/packages/net-wireless/wireless-tools), and [net-wireless/wpa\_supplicant](https://packages.gentoo.org/packages/net-wireless/wpa_supplicant). Of these three, [net-wireless/wpa\_supplicant](https://packages.gentoo.org/packages/net-wireless/wpa_supplicant) is the preferred one. The important thing to remember is that wireless networks are configured on a global basis and not an interface basis.

The [net-wireless/iw](https://packages.gentoo.org/packages/net-wireless/iw) software, the successor of [net-wireless/wireless-tools](https://packages.gentoo.org/packages/net-wireless/wireless-tools), supports nearly all cards and drivers, but it cannot connect to WPA-only Access Points. If the networks only offer WEP encryption or are completely open, then [net-wireless/iw](https://packages.gentoo.org/packages/net-wireless/iw) beats the other package over simplicity.

Some wireless cards are deactivated by default. To activate them, please consult the hardware documentation. Some of these cards can be unblocked using the rfkill application. If that is the case, use rfkill list to see the available cards and rfkill unblock INDEX to activate the wireless functionality. If not, then the wireless card might need to be unlocked through a button, switch or special key combination on the laptop.

The [WPA supplicant project](http://hostap.epitest.fi/wpa_supplicant) provides a package that allows users to connect to WPA enabled access points.

`root #``emerge --ask net-wireless/wpa_supplicant`
Next, configure /etc/conf.d/net so that the wpa\_supplicant module is preferred over wireless-tools (if both are installed, wireless-tools is the default).

**`/etc/conf.d/net`**

**Force the use of wpa\_supplicant**

```
# Prefer wpa_supplicant over wireless-tools
modules="wpa_supplicant"
```
Next configure wpa\_supplicant itself (which is a bit more tricky depending on how secure the Access Points are). The below example is taken and simplified from /usr/share/doc/wpa\_supplicant-\<version>/wpa\_supplicant.conf.gz which ships with wpa\_supplicant.

**`/etc/wpa_supplicant/wpa_supplicant.conf`**

**Somewhat simplified example**

```
# The below line not be changed otherwise wpa_supplicant refuses to work
ctrl_interface=/var/run/wpa_supplicant
  
# Ensure that only root can read the WPA configuration
ctrl_interface_group=0
  
# Let wpa_supplicant take care of scanning and AP selection
ap_scan=1
  
# Simple case: WPA-PSK, PSK as an ASCII passphrase, allow all valid ciphers
network={
  ssid="simple"
  psk="very secret passphrase"
  # The higher the priority the sooner we are matched
  priority=5
}
  
# Same as previous, but request SSID-specific scanning (for APs that reject
# broadcast SSID)
network={
  ssid="second ssid"
  scan_ssid=1
  psk="very secret passphrase"
  priority=2
}
  
# Only WPA-PSK is used. Any valid cipher combination is accepted
network={
  ssid="example"
  proto=WPA
  key_mgmt=WPA-PSK
  pairwise=CCMP TKIP
  group=CCMP TKIP WEP104 WEP40
  psk=06b4be19da289f475aa46a33cb793029d4ab3db7a23ee92382eb0106c72ac7bb
  priority=2
}
  
# Plaintext connection (no WPA, no IEEE 802.1X)
network={
  ssid="plaintext-test"
  key_mgmt=NONE
}
  
# Shared WEP key connection (no WPA, no IEEE 802.1X)
network={
  ssid="static-wep-test"
  key_mgmt=NONE
  # Keys in quotes are ASCII keys
  wep_key0="abcde"
  # Keys specified without quotes are hex keys
  wep_key1=0102030405
  wep_key2="1234567890123"
  wep_tx_keyidx=0
  priority=5
}
  
# Shared WEP key connection (no WPA, no IEEE 802.1X) using Shared Key
# IEEE 802.11 authentication
network={
  ssid="static-wep-test2"
  key_mgmt=NONE
  wep_key0="abcde"
  wep_key1=0102030405
  wep_key2="1234567890123"
  wep_tx_keyidx=0
  priority=5
  auth_alg=SHARED
}
  
# IBSS/ad-hoc network with WPA-None/TKIP
network={
  ssid="test adhoc"
  mode=1
  proto=WPA
  key_mgmt=WPA-NONE
  pairwise=NONE
  group=TKIP
  psk="secret passphrase"
}
```
The [wireless tools project](http://www.hpl.hp.com/personal/Jean_Tourrilhes/Linux/Tools.html) provides a generic way to configure basic wireless interfaces up to the WEP security level. While WEP is a weak security method it's still prevalent in the world.

Wireless tools configuration is controlled by a few main variables. The sample configuration file below should describe all that is needed. One thing to bear in mind is that no configuration means "connect to the strongest unencrypted Access Point" - wireless tools will always try and connect the system to something.

`root #``emerge --ask net-wireless/wireless-tools`
**`/etc/conf.d/net`**

**Sample iwconfig setup**

```
# Prefer iwconfig over wpa_supplicant
modules="iwconfig"
  
# Configure WEP keys for Access Points called ESSID1 and ESSID2
# You may configure up to 4 WEP keys, but only 1 can be active at
# any time so we supply a default index of [1] to set key [1] and then
# again afterwards to change the active key to [1]
# We do this incase you define other ESSID's to use WEP keys other than 1
#
# Prefixing the key with s: means it's an ASCII key, otherwise a HEX key
#
# enc open specified open security (most secure)
# enc restricted specified restricted security (least secure)
key_ESSID1="[1] s:yourkeyhere key [1] enc open"
key_ESSID2="[1] aaaa-bbbb-cccc-dd key [1] enc restricted"
  
# The below only work when we scan for available Access Points
  
# Sometimes more than one Access Point is visible so we need to
# define a preferred order to connect in
preferred_aps="'ESSID1' 'ESSID2'"
```
It is possible to add some extra options to fine-tune the AP selection, but these are not required.

One way is to configure the system so it only connects to preferred APs. By default if everything configured has failed and wireless-tools can connect to an unencrypted Access Point then it will. This can be controlled by the associate\_order variable. Here's a table of values and how they control this.

| Value | Description | 
|---|---|
| any | Default behavior. | 
| preferredonly | Only connect to visible APs in the preferred list. | 
| forcepreferred | Forceably connect to APs in the preferred order if they are not found in a scan. | 
| forcepreferredonly | Do not scan for APs - instead just try to connect to each one in order. | 
| forceany | Same as forcepreferred + connect to any other available AP. | 

There is also the blacklist\_aps and unique\_ap selection. blacklist\_aps works in a similar way to preferred\_aps. unique\_ap is a yes or no value that says if a second wireless interface can connect to the same Access Point as the first interface.

**`/etc/conf.d/net`**

**blacklist\_aps and unique\_ap example**

```
# Sometimes you never want to connect to certain access points
blacklist_aps="'ESSID3' 'ESSID4'"
  
# If you have more than one wireless card, you can say if you want
# to allow each card to associate with the same Access Point or not
# Values are "yes" and "no"
# Default is "yes"
unique_ap="yes"
```
To set the system up as an ad-hoc node when it fails to connect to any Access Point in managed mode, use this as a fallback:

**`/etc/conf.d/net`**

**Fallback to ad-hoc mode**

```
adhoc_essid_eth0="This Adhoc Node"
```
It is also possible to connect to ad-hoc networks, or to run the system in *master* mode so it becomes an access point itself.

**`/etc/conf.d/net`**

**Sample ad-hoc/master configuration**

```
# Set the mode - can be managed (default), ad-hoc or master
# Not all drivers support all modes
mode_eth0="ad-hoc"
  
# Set the ESSID of the interface
# In managed mode, this forces the interface to try and connect to the
# specified ESSID and nothing else
essid_eth0="This Adhoc Node"
  
# We use channel 3 if you don't specify one
channel_eth0="9"
```
There are some more variables that can help to get the wireless up and running due to driver or environment problems. Here's a table of other things that can be tried.

| Variable name | Default value | Description | 
|---|---|---|
| `iwconfig_eth0` |  | See the iwconfig man page for details on what to send iwconfig. | 
| `iwpriv_eth0` |  | See the iwpriv man page for details on what to send iwpriv. | 
| `sleep_scan_eth0` | 0 | The number of seconds to sleep before attempting to scan. This is needed when the driver/firmware needs more time to active before it can be used. | 
| `sleep_associate_eth0` | 5 | The number of seconds to wait for the interface to associate with the Access Point before moving onto the next one. | 
| `associate_test_eth0` | MAC | Some drivers do not reset the MAC address associated with an invalid one when they lose or attempt association. Some drivers do not reset the quality level when they lose or attempt association. Valid settings are MAC, quality and all. | 
| `scan_mode_eth0` |  | Some drivers have to scan in ad-hoc mode, so if scanning fails try setting ad-hoc here. | 
| `iwpriv_scan_pre_eth0` |  | Sends some iwpriv commands to the interface before scanning. See the iwpriv man page for more details. | 
| `iwpriv_scan_post_eth0` |  | Sends some iwpriv commands to the interface after scanning. See the iwpriv man page for more details. | 

In this section, we show how to configure network settings based on the ESSID. For instance, with the wireless network with ESSID *ESSID1* configure a static IP address while ESSID *ESSID2* uses DHCP.

**`/etc/conf.d/net`**

**override network settings per ESSID**

```
config_ESSID1="192.168.0.3/24 brd 192.168.0.255"
routes_ESSID1="default via 192.168.0.1"
  
config_ESSID2="dhcp"
fallback_ESSID2="192.168.3.4/24"
fallback_route_ESSID2="default via 192.168.3.1"
  
# We can define nameservers and other things too
# NOTE: DHCP will override these unless it's told not to
dns_servers_ESSID1="192.168.0.1 192.168.0.2"
dns_domain_ESSID1="some.domain"
dns_search_domains_ESSID1="search.this.domain search.that.domain"
  
# You override by the MAC address of the Access Point
# This handy if you goto different locations that have the same ESSID
config_001122334455="dhcp"
dhcpcd_001122334455="-t 10"
dns_servers_001122334455="192.168.0.1 192.168.0.2"
```










Four functions can be defined in /etc/conf.d/net:

- `preup()`, called before an interface is brought up;
- `predown()`, called before an interface is brought down;
- `postup()`, called after an interface is brought up; and
- `postdown()`, called after an interface is brought down.

Each of these these functions is called with the interface name, available within each function via the `IFACE` variable, so that one function can control multiple interfaces.

The return values for the `preup()` and `predown()` functions should be:

- 0 to indicate success, and that configuration or de-configuration of the interface can continue.
- A non-zero value otherwise.

If `preup()` returns a non-zero value, interface configuration will be aborted. If `predown()` returns a non-zero value, the interface will not be allowed to continue de-configuration.

Return values for the `postup()` and `postdown()` functions are ignored since there's nothing to do if they indicate failure.

`${IFACE}` is set to the interface being brought up/down. `${IFVAR}` is `${IFACE}` converted to variable name bash allows.

**`/etc/conf.d/net`**

**pre/post up/down function examples**

```
() {
  # Test for link on the interface prior to bringing it up.  This
  # only works on some network adapters and requires the ethtool
  # package to be installed.
  if ethtool ${IFACE} | grep -q 'Link detected: no'; then
    ewarn "No link on ${IFACE}, aborting configuration"
    return 1
  fi
  
  # Remember to return 0 on success
  return 0
}
  
predown() {
  # The default in the script is to test for NFS root and disallow
  # downing interfaces in that case.  Note that if you specify a
  # predown() function you will override that logic.  Here it is, in
  # case you still want it...
  if is_net_fs /; then
    eerror "root filesystem is network mounted -- can't stop ${IFACE}"
    return 1
  fi
  
  # Remember to return 0 on success
  return 0
}
  
postup() {
  # This function could be used, for example, to register with a
  # dynamic DNS service.  Another possibility would be to
  # send/receive mail once the interface is brought up.
       return 0
}
  
postdown() {
  # This function is mostly here for completeness... I haven't
  # thought of anything nifty to do with it yet ;-)
  return 0
}
```
Two functions can be defined in /etc/conf.d/net:

- `preassociate()`, called before association.
- `postassociate()`, called after association.

Each of these these functions is called with the interface name, available within each function via the IFACE variable, so that one function can control multiple interfaces.

The return values for the preassociate() function should be:

- 0 to indicate success, and to continue configuration.
- A non-zero value otherwise.

If `preassociate()` returns a non-zero value, interface configuration will be aborted.

The return value for the `postassociate()` function is ignored since there's nothing to do if it indicates failure.

Within each function, the exact ESSID of the AP the system is connecting to is available via the `ESSID` variable. `${ESSIDVAR}` is `${ESSID}` converted to a variable name bash allows.

**`/etc/conf.d/net`**

**pre/post association functions**

```
() {
  # The below adds two configuration variables leap_user_ESSID
  # and leap_pass_ESSID. When they are both configured for the ESSID
  # being connected to then we run the CISCO LEAP script
  
  local user pass
  eval user=\"\$\{leap_user_${ESSIDVAR}\}\"
  eval pass=\"\$\{leap_pass_${ESSIDVAR}\}\"
  
  if [[ -n ${user} && -n ${pass} ]]; then
    if [[ ! -x /opt/cisco/bin/leapscript ]]; then
      eend "For LEAP support, please emerge net-misc/cisco-aironet-client-utils"
      return 1
    fi
    einfo "Waiting for LEAP Authentication on \"${ESSID//\\\\//}\""
    if /opt/cisco/bin/leapscript ${user} ${pass} | grep -q 'Login incorrect'; then
      ewarn "Login Failed for ${user}"
      return 1
    fi
  fi
  
  return 0
}
  
postassociate() {
  # This function is mostly here for completeness... I haven't
  # thought of anything nifty to do with it yet ;-)
  
  return 0
}
```








With laptops, systems can be always on the move. As a result, the system may not always have an Ethernet cable or plugged in or an access point available. Also, the user may want networking to automatically work when an Ethernet cable is plugged in or an access point is found.

In this chapter, we cover how this can be done.

[ifplugd](http://0pointer.de/lennart/projects/ifplugd/) is a daemon that starts and stops interfaces when an Ethernet cable is inserted or removed. It can also manage detecting association to Access Points or when new ones come in range.

`root #``emerge --ask sys-apps/ifplugd`
Configuration for ifplugd is fairly straightforward too. The configuration is held in /etc/conf.d/net. Run man ifplugd for details on the available variables. Also, see /usr/share/doc/netifrc-\*/net.example.bz2 for more examples.

**`/etc/conf.d/net`**

**Sample ifplug configuration**

```
# Replace eth0 with the interface to be monitored
ifplugd_eth0="..."
  
# To monitor a wireless interface
ifplugd_eth0="--api-mode=wlan"
```
In addition to managing multiple network connections, users may want to add a tool that makes it easy to work with multiple DNS servers and configurations. This is very handy when the system receives its IP address via DHCP.

`root #``emerge --ask net-dns/openresolv`
See man resolvconf to learn more about its features.
