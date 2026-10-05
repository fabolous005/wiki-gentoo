<!-- source: https://wiki.gentoo.org/wiki/WireGuard | group: Gentoo Wiki (Main) | wiki-title: WireGuard -->
---
title: WireGuard
url: https://wiki.gentoo.org/wiki/WireGuard
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-21"
fingerprint: fd1ac5a5c6109c3
license: CC BY-SA 4.0
---

# WireGuard

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



**WireGuard** is a modern, simple, and secure VPN that utilizes state-of-the-art cryptography. Considered an alternative to [OpenVPN](https://wiki.gentoo.org/wiki/OpenVPN), it can be used to create secure connections. Its goals are to be fast, simple, lean, and easy to configure. Wireguard consists of two components: userspace tools and a kernel module.

Wireguard is written and maintained by [Jason A. Donenfeld (zx2c4)](https://wiki.gentoo.org/wiki/User:Zx2c4)

Official and potentially more up-to-date installation instructions can be [found upstream](https://www.wireguard.com/install/).

## Installation

### Kernel

Starting with Linux kernel 5.6, Wireguard is included in the upstream kernel sources. It is enabled via the following menuconfig option:

**Enable`CONFIG_WIREGUARD`**

### USE flags


### Emerge

Install the wireguard-tools package to generate encryption keys and manage Wireguard interfaces:

`root #``emerge --ask net-vpn/wireguard-tools`
## Configuration

### Kernel module loading

If WireGuard support has been added as a module, it may be necessary to instruct the selected init system to load the WireGuard kernel modules when the system boots.

Create a new file in the /etc/modules-load.d/ directory in order to instruct the module loading service to get the module loaded on boot:

**`/etc/modules-load.d/wireguard.conf`**

Or if VPN access is not needed very often, load kernel module manually:

`root #``modprobe wireguard`
Loaded modules can be reviewed with lsmod.

### Files

#### wg.conf

A WireGuard configuration file (the first of which is normally named wg0.conf) can be written as outlined in the man page. Review the CONFIGURATION FILE FORMAT and CONFIGURATION FILE FORMAT EXAMPLE sections in man 8 wg:

`user $``man 8 wg`
As mentioned in the man page, WireGuard configuration files are defined in the INI format. A typical configuration file looks something like the following:

**`wg0.conf`**

**Wireguard configuration file example**

```
[Interface]
PrivateKey = yAnz5TF+lXXJte14tji3zlMNq+hd2rYUIgJBgB3fBmk=
ListenPort = 51820
[Peer]
PublicKey = xTIBA5rboUvnH4htodjb6e697QjLERt1NAB4mZqp8Dg=
Endpoint = 192.95.5.67:1234
AllowedIPs = 10.192.122.3/32, 10.192.124.1/24
[Peer]
PublicKey = TrMvSoP4jYQlY6RIzBgbssQqY3vxI2Pi+y71lOWWXX0=
Endpoint = [2607:5300:60:6b0::c05f:543]:2468
AllowedIPs = 10.192.122.4/32, 192.168.0.0/16
[Peer]
PublicKey = gN65BkIKy1eCE9pP1wdc8ROUtkHLF2PfAqYdyYBz6EA=
Endpoint = test.wireguard.com:18981
AllowedIPs = 10.10.10.230/32
```
## Usage

### Generate a keypair

Before using WireGuard a keypair has to be generated. This can be accomplished using wg(8):

`user $``(umask 077; wg genkey | tee privatekey | wg pubkey > publickey)`
### Network management methods

Various [network management](https://wiki.gentoo.org/wiki/Network_management) methods are available to supervise Wireguard tunnels.

#### wg-quick

Configuration can be automated using the wg-quick utility, which will create tunnels using configuration files in the /etc/wireguard file:

`root #``wg-quick up wg0`
For more information on wg-quick consult man 8 wg-quick.

The [net-misc/netifrc](https://packages.gentoo.org/packages/net-misc/netifrc) scripts (typically used with OpenRC) can be used to quickly bring Wireguard interfaces. Presuming a correctly defined /etc/wireguard/wg0.conf file has been created:

`root #````
ln -s /etc/init.d/wg-quick /etc/init.d/wg-quick.wg0
```
`root #````
rc-update add wg-quick.wg0 default
```
To bring up the interface now:

`root #``/etc/init.d/wg-quick.wg0 start`


#### netifrc

This implementation of WireGuard uses the same logic as wg-quick, but putting the logic directly in /etc/conf.d/net for netifrc.


Using the `wg-quick` USE flag will add [virtual/resolvconf](https://packages.gentoo.org/packages/virtual/resolvconf) as a dependency. This might be a deal-breaker for systems that want to keep the resolv.conf file from being modified by external utilities. Luckily, this implementation does not use such dependencies; it's okay to add the `-wg-quick` USE flag to [net-vpn/wireguard-tools](https://packages.gentoo.org/packages/net-vpn/wireguard-tools).

**`/etc/portage/package.use/wireguard`**


Copy the following netifrc configuration file to /etc/conf.d/net and edit the interfaces as needed to work with the machine:

`root #``cat /etc/conf.d/net````
# Make sure to change all values for our setup! These are just an example.
#
# Make sure that net-dns/openresolv, virtual/resolvconf, their dependencies, and other resolv.conf managers
# aren't installed to keep the setup simple and secure! This might require us to add -wg-quick to
# net-vpn/wireguard-tools and rebuild the package.
# Bring up hardware network interface; change enp5s0, to our interface, shown when running: ip addr
config_enp5s0="dhcp"
# Setup a DNS server here if we're going to change to another DNS server when bringing up the Wireguard interface.
# Some providers give us an address; it's better to change the address for privacy reasons.
# Change enp5s0 to our interface, shown when running: ip addr
dns_servers_enp5s0="1.2.3.4 123.12.21.1"
# Bring up Wireguard virtual network interface; the IP here is from our VPN provider or custom Wireguard setup.
config_wg0="5.6.7.8/32"
# Connect to Wireguard endpoint using config; we need to create this config if it doesn't exist.
wireguard_wg0="/etc/wireguard/wg0.conf"
# Prevent DHCP from setting a DNS server gotten from our router.
dhcp="nodns"
postup()
{
	# This WireGuard implementation uses the same logic as what's inside the 'wg-quick' script.
	if [[ "$IFACE" =~ ^wg[[:digit:]]+$ ]]; then
		local current_wg_address=''
		# Get the name of the variable that contains the addresses assigned to the WG interface.
		local -r  WG_ADDRESS_VAR="config_$IFACE"
		# This is a list of the IP addresses that will be routed to the WG interface; to route all IPv4 and IPv6 packets, add
		# "AllowedIPs = 0.0.0.0/0, ::/0" to the WG config file.
		local -ar WG_ALLOWED_IP=($(wg show "$IFACE" allowed-ips | gawk '{$1 = ""; print}'))
		# Get the firewall mark used by the WG interface.
		local -r  WG_FWMARK="$(wg show "$IFACE" fwmark)"
		# The WG routing table *CAN* be different than the WG firewall mark. It's the same here so that each WG interface has it's own unique table.
		local -r  WG_ROUTING_TABLE="$WG_FWMARK"
		sysctl -q net.ipv4.conf.all.src_valid_mark=1
		for current_wg_address in "${WG_ALLOWED_IP[@]}"; do
			ip route add "$current_wg_address" dev "$IFACE" table "$WG_ROUTING_TABLE"
		done
		# If there are multiple WG interfaces, the following commands might break them:
		ip -4 rule add not fwmark "$WG_FWMARK" table "$WG_ROUTING_TABLE"
		ip -6 rule add not fwmark "$WG_FWMARK" table "$WG_ROUTING_TABLE"
		ip -4 rule add table main suppress_prefixlength 0
		ip -6 rule add table main suppress_prefixlength 0
		# Instead of running multiple 'nft' commands, we add all the modifications into one 'nft' command to keep nftables atomic.
		local nftcmd=''
		printf -v nftcmd '%sadd table %s %s\n'                                                                "$nftcmd" inet "${IFACE}_inet_table"
		printf -v nftcmd '%sadd chain %s %s preraw { type filter hook prerouting priority -300; }\n'          "$nftcmd" inet "${IFACE}_inet_table"
		printf -v nftcmd '%sadd chain %s %s premangle { type filter hook prerouting priority -150; }\n'       "$nftcmd" inet "${IFACE}_inet_table"
		printf -v nftcmd '%sadd chain %s %s postmangle { type filter hook postrouting priority -150; }\n'     "$nftcmd" inet "${IFACE}_inet_table"
		local ip_version=''
		for current_wg_address in ${!WG_ADDRESS_VAR}; do
			[[ "$current_wg_address" =~ \. ]] && ip_version=ip || ip_version=ip6
			printf -v nftcmd '%sadd rule %s %s preraw iifname != "%s" %s daddr %s fib saddr type != local drop\n' "$nftcmd" inet "${IFACE}_inet_table" "$IFACE" "$ip_version" "$current_wg_address"
		done
	
		printf -v nftcmd '%sadd rule %s %s postmangle meta l4proto udp mark %s ct mark set mark \n'           "$nftcmd" inet "${IFACE}_inet_table" "$WG_FWMARK"
		printf -v nftcmd '%sadd rule %s %s premangle meta l4proto udp meta mark set ct mark \n'               "$nftcmd" inet "${IFACE}_inet_table"
		nft -f <(echo -n "$nftcmd")
	fi
	# This function should return 0 on success.
	return 0
}
predown()
{
	# Prevent bringing down interface in case there's a NFS root.
	# taken from: https://github.com/gentoo/netifrc/blob/4bd8be5f43d07a9e92b73174c7fbef8b989aaa55/doc/net.example.Linux.in
	if is_net_fs /; then
		eerror "root filesystem is network mounted -- can't stop ${IFACE}"
		return 1
	fi
	# This WireGuard implementation uses the same logic as what's inside the 'wg-quick' script.
	if [[ "$IFACE" =~ ^wg[[:digit:]]+$ ]]; then
		local -r WG_FWMARK="$(wg show "$IFACE" fwmark)"
		local -r WG_ROUTING_TABLE="$WG_FWMARK"
		nft delete table inet "${IFACE}_inet_table"
		ip -4 rule delete table "$WG_ROUTING_TABLE"
		ip -6 rule delete table "$WG_ROUTING_TABLE"
		# If there are multiple WG interfaces, the following commands might break them:
		ip -4 rule delete table main suppress_prefixlength 0
		ip -6 rule delete table main suppress_prefixlength 0
		# This step is already done by netifrc.
		#ip link delete dev "$IFACE"
	fi
	# This function should return 0 on success.
	return 0
}
```
Remove any wg-quick extensions in /etc/wireguard/wg0.conf as they will not be recognized by wg (the tool that netifrc uses). Read the man page for wg to know what is syntactically correct. In short, remove the following lines in the configuration file if they exist:

The contents of Address = should go into config\_wg0=. The contents of DNS = should go into dns\_servers\_wg0=.




Once the WireGuard and netifrc configuration files are done, we are ready to use them. To bring up the WireGuard interface, make the symlink for wg0 and start it just like any other physical interface:

`root #````
ln -s /etc/init.d/net.lo /etc/init.d/net.wg0
```
`root #````
rc-service net.wg0 start
```
To have WireGuard start at system boot:

`root #``rc-update add net.wg0 default`


#### Network Namespaces

This implementation of WireGuard uses network namespaces, which are isolated networks that can be assigned interfaces. The benefit of doing this instead of changing firewall rules is that it's more clear that all or specified packets will go through a WireGuard interface even if the WireGuard interface goes down or gets deleted.

TL;DR:

To bring up a WireGuard interface (simplified):

- Make a new network namespace, "physical"
- Make a new WireGuard interface, "wg0", in network namespace "physical"
- Move "wg0" from network namespace "physical" to "init"/"1"
- Move all interfaces that can connect to the internet in network namespace "init"/"1" to "physical"


To bring down a WireGuard interface (simplified):

- Move all interfaces that can connect to the internet in network namespace "physical" to "init"/"1"
- Delete "wg0"
- Delete "physical"


To bring up a WireGuard interface, we make a new network namespace with some name like, "physical"; then, we make a new WireGuard interface with the name "wg0" in the "physical" namespace then move it to the "init"/"1" namespace (the namespace that all interfaces start in). Then, move all interfaces that can connect to the internet to the "physical" interface. To bring down a WireGuard interface, we move all the interfaces that we previously moved to "physical" back to "init"/"1". Then, we delete the "wg0" WireGuard interface and the "physical" network namespace since we're not using them anymore. This is a simplified explanation, there are other steps that happen in between, such as routing packets to the WireGuard interface, but this is the biggest difference compared to other implementations.

This implementation also has the benefit of giving the user the ability to allow specified applications to temporarily bypass the WireGuard interface. For example, let's say we're using public WiFi at a hotel, shop, airport, etc. and we want to route all our packets through WireGuard to our home network. Everything works fine except when the hotel requires users to authenticate themselves through a web portal before they actually give us a connection to the internet.

Before, we would have to bring down the WireGuard interface, authenticate ourselves through the portal via our browser of choice (such as [LibreWolf](https://wiki.gentoo.org/wiki/LibreWolf)), then bring up the WireGuard interface again. In the time when the WireGuard interface was down, other running applications with network capabilities might connect to the internet. This can be a serious security, privacy, and anonymity concern if we **really** don't want anyone else to know what our machine is trying to connect to.

With network namespaces, we can handle the situation without ever bringing down the WireGuard interface. When WireGuard is set up using network namespaces, the only interface running applications can see/use is the WireGuard interface. All the interfaces that actually connect to the internet are in a different namespace and cannot be directly seen/used unless we give an application the ability to do so. We can do this by prefixing the command to run the application with another that temporarily bypasses the WireGuard interface until we close the application.

##### Setup

Normally we move interfaces with the ip command provided by [sys-apps/iproute2](https://packages.gentoo.org/packages/sys-apps/iproute2), but for wireless ones, we need to use the iw command provided by [net-wireless/iw](https://packages.gentoo.org/packages/net-wireless/iw). If our machine has wireless interfaces, such as a laptop, we need to install iw.

`root #``emerge --ask net-wireless/iw`
Using the `wg-quick` USE flag will add [virtual/resolvconf](https://packages.gentoo.org/packages/virtual/resolvconf) as a dependency. This might be a deal-breaker for systems that want to keep the resolv.conf file from being modified by external utilities. Luckily, this implementation does not use such dependencies; it's okay to add the `-wg-quick` USE flag to [net-vpn/wireguard-tools](https://packages.gentoo.org/packages/net-vpn/wireguard-tools).

**`/etc/portage/package.use/wireguard`**


Now, this implementation involves copy-pasting a WireGuard configuration file, some scripts, and optionally setting up a cron job for automated starting of the WireGuard interface.

Make the directory for WireGuard configuration files:

`root #``mkdir -p /etc/wireguard`
Set the correct permissions because this directory can contain keys:

`root #``chmod 700 /etc/wireguard`
Copy the following WireGuard configuration file to /etc/wireguard/wg0.conf:

`root #``cat /etc/wireguard/wg0.conf`
\# /etc/wireguard/wg0.conf
\[Interface\]
PrivateKey = INTERFACE\_PRIVATE\_KEY\_PLACEHOLDER
# Space-separated list of addresses for the WG interface.
Address =
# (Optional) Maximum Transmission Unit (MTU) of the WG interface. If there is no specified MTU, then 'ip' will decide the optimal value.
MTU =
# (Optional) Space-separated list of IPv4, IPv6, or hostname Domain Name System (DNS) for the WG interface.
DNS =
# The name of the namespace where all the non-WG interfaces will be.
Namespace = physical
# (Optional) Space-separated list of interfaces that the WG interface will use. The specified interfaces will be in the same namespace. If there are no
# specified interfaces, then all interfaces will be used except for the loopback (lo) and other WG interfaces. If the only specified interface is "NONE",
# no interfaces will be modified.
PhysicalInterface =
# (Optional) Space-separated list of services that should use the WG interface; the only services that \*NEED\* to be listed here are the ones that are stopped
# because of an interface stopping (a service with a 'net' dependency). If we're not sure, run 'rc-status' after this script runs to see which services get
# stopped, then add them to this variable.
# 
# All other services that are still running will use the WG interface. :)
ExtraServiceYes =
# (Optional) Space-separated list of services that should \*NOT\* use the WG interface. These services will bypass the WG interface and run in the
# "wg\_outgoing\_namespace" namespace.
ExtraServiceNo =
\[Peer\]
PublicKey = PEER\_PUBLIC\_KEY\_PLACEHOLDER
PresharedKey = PRESHARED\_KEY\_PLACEHOLDER
AllowedIPs = 0.0.0.0/0, ::/0
Endpoint = IP\_ADDRESS\_OR\_DOMAIN\_PLACEHOLDER:51820
# This can help keep the connection alive when this machine is behind a NAT.
PersistentKeepAlive = 25

(Optional) Set the correct permissions for the file; this is not required because we already set the permissions for the directory this file sits in:

`root #``chmod 600 /etc/wireguard/wg0.conf`
Copy the following scripts into /usr/local/bin so that they can be found by the PATH environment variable:

`root #``cat /usr/local/bin/wg-netns````
#!/bin/bash
# /usr/local/bin/wg-netns
# NOTE:
# * This script was written for OpenRC, but don't let that scare one from adapting it to other init systems or turning it into a generic script. All of the
#   OpenRC-specific commands in this script can be found by searching for 'rc-'; there's only like... 4 :)
#   Understand that it might not be as simple as replacing the 'rc-*' text, one needs to understand the syntax of the line of code and the surrounding code.
#
# * This implementation of WireGuard (WG) uses "The New Namespace Solution" provided by the official WG docs.
#   https://www.wireguard.com/netns/#the-new-namespace-solution
#
# * The "init" namespace is the one that all interfaces start in (process id 1).
#
# * At any point, we can check what interfaces are in what namespace by running:
#   - 'ip link'                (shows interfaces in the "init" / "1" namespace)
#   - 'ip -n <namespace> link' (shows interfaces in the "namespace"  namespace)
#
# * To run a command inside a specific namespace, prefix the command with:
#   'ip netns exec <namespace>'
#
# * We only need one WG interface because we can have multiple peers per interface.
#
# * If the system uses Pluggable Authentication Modules (PAM) for OpenRC, add the following line to /etc/pam.d/run_init:
#   "auth sufficient pam_rootok.so"
#   This makes it so that running OpenRC commands as root (or running this script as root) will not prompt for further authentication.
# -----------------------------------   ----------------------------------------
# |The "init"/"1" namespace         |   |"mynetns0" namespace                  |
# |Seen by running 'ip link'        |   |Seen by running 'ip -n mynetns1 link' |
# |                                 |   |                                      |
# |Applications will normally       |   |                                      |
# |use this namespace and      wg0---------+                             eth0-----> Internet
# |only see the WG                  |   |                                      |
# |interfaces.                      |   |                                      |
# |                                 |   |                                eth1-----> Internet
# |If we want an app to        wg1---------+                                   |
# |bypass the WG interfaces,        |   |                                      |
# |we can prefix the command to run |   |                               wlan0-----> Internet
# |the app with:                    |   |                                      |
# |'ip netns exec <namespace>'      |   |                                      |
# |to run the app in the specified  |   ----------------------------------------
# |namespace.                       |
# |                                 |   ----------------------------------------
# |                                 |   |"mynetns1" namespace                  |
# |                                 |   |                                      |
# |                                 |   |                                      |
# |                                 |   |                                      |
# |                            wg2---------+                             eth2-----> Internet
# |                                 |   |                                      |
# |                                 |   |                                      |
# |                                 |   ----------------------------------------
# |                                 |
# |                                 |                  ...
# |                                 |
# |                                 |
# |                                 |   ----------------------------------------
# |                                 |   |"mynetns999" namespace                |
# |                                 |   |                                      |
# |                                 |   |                                      |
# |                                 |   |                                      |
# |                          wg999---------+                           eth999-----> Internet
# |                                 |   |                                      |
# |                                 |   |                                      |
# -----------------------------------   ----------------------------------------
####################
# Internal variables
####################
# NOTE:
# The following variables should **NOT** be modified; these are made by this script.
# This is an internal variable not intended for the user.
declare -a Interface_list=()
# This is an internal variable not intended for the user.
declare    wg_config_file_data_built_by_this_script=''
###########################
# User-modifiable variables
###########################
# NOTE:
# The following variables can be modified here. Array variables are incremental to the WG config file; non-array variables get overridden by the WG config file.
# Space-separated list of addresses for the WG interface.
declare -a Wg_address=()
# (Optional) Maximum Transmission Unit (MTU) of the WG interface. If there is no specified MTU, then 'ip' will decide the optimal value.
declare    wg_mtu=''
# (Optional) Space-separated list of IPv4, IPv6, or hostname Domain Name System (DNS) for the WG interface.
declare -a Wg_dns=()
# The name of the namespace where all the non-WG interfaces will be.
declare    wg_outgoing_namespace=''
# (Optional) Space-separated list of interfaces that the WG interface will use. The specified interfaces will be in the same namespace. If there are no
# specified interfaces, then all interfaces will be used except for the loopback (lo) and other WG interfaces. If the only specified interface is "NONE",
# no interfaces will be modified.
declare -a Wg_physical_interface=()
# (Optional) Space-separated list of services that should use the WG interface; the only services that **NEED** to be listed here are the ones that are stopped
# because of an interface stopping (a service with a 'net' dependency). If we're not sure, run 'rc-status' after this script runs to see which services get
# stopped, then add them to this variable.
# 
# All other services that are still running will use the WG interface. :)
declare -a Extra_service_through_wg=()
# (Optional) Space-separated list of services that should **NOT** use the WG interface. These services will bypass the WG interface and run in the
# "wg_outgoing_namespace" namespace.
declare -a Extra_service_through_no_wg=()
# What this script will do.
declare -r SELF_ACTION="$1"
# The name of the WG interface.
declare -r WG_INTERFACE="$2"
# What the WG interface name should match; must start with '^' and end with '$'.
declare -r WG_INTERFACE_REGEX='^wg[[:digit:]]+$'
# This is just a way to tell the difference between the initial namespace and a WireGuard one.
declare -r INITIAL_NAMESPACE="initial_namespace"
###########
# Functions
###########
# This function prints the command about to be executed to stderr before it actually executes. This is helpful for debugging.
print_cmd()
{
	# The '>&2' sends stdout to stderr so that it doesn't affect code that reads stdout.
	echo "CMD: $*" >&2
	# This is where the command actually executes.
	"$@"
}
# This function prints a message and a spinner to stderr to let the user know that the script is still running. This is so that we don't fill multiple lines
# waiting for something.
message_spin()
{
	local spinner=''
	# The '>&2' sends stdout to stderr so that it doesn't affect code that reads stdout.
	echo -n "$1...  " >&2
	while true
	do
		for spinner in / - \\\\ \|
		do
			echo -en "\b${spinner}" >&2
			sleep .1
		done
	done
}
# This is a wrapper for the message_spin function to easily add a message and spinner to any command.
message_spin_wrapper()
{
	local -i process_id=0
	message_spin "$1" &
	process_id=$!
	shift
	"$@"
	kill "$process_id"
	echo -e "\b\b done!" >&2
}
# Configure the WG interface.
configure_wg_interface()
{
	local current_wg_address=''
	# Add a WG interface directly to the "wg_outgoing_namespace" namespace.
	print_cmd ip -n "$wg_outgoing_namespace" link add "$WG_INTERFACE" type wireguard
	# The birthplace namespace of "WG_INTERFACE" is now the "wg_outgoing_namespace" namespace, which means the ciphertext UDP sockets will be assigned to
	# devices that can connect to the internet. We can now move "WG_INTERFACE" into the "init" namespace; it will still remember its birthplace for the
	# sockets, however.
	# 
	# NOTE:
	# We specify "1" as the "init" namespace, because that's the PID of the first process on the system.
	print_cmd ip -n "$wg_outgoing_namespace" link set "$WG_INTERFACE" netns 1 ${wg_mtu:+mtu "$wg_mtu"}
	# Configure the "WG_INTERFACE" interface like usual, and set it as the default route.
	wg setconf "$WG_INTERFACE" <(echo "$wg_config_file_data_built_by_this_script")
	for current_wg_address in "${Wg_address[@]}"
	do
		print_cmd ip address add "$current_wg_address" dev "$WG_INTERFACE"
	done
	print_cmd ip link set "$WG_INTERFACE" up
	# Select which IP addresses will be routed to the WG interface; to route all IPv4 and IPv6 packets, add "AllowedIPs = 0.0.0.0/0, ::/0" to the
	# WG config file.
	for current_wg_address in $(wg show "$WG_INTERFACE" allowed-ips | gawk '{$1 = ""; print}')
	do
		print_cmd ip route add "$current_wg_address" dev "$WG_INTERFACE"
	done
}
# Move an interface from one namespace to another.
#
# If the working namespace is the initial namespace, move the specified interface into the "wg_outgoing_namespace" namespace, and vice-versa.
move_interface()
{
	local -i wiphy_index=0
	# This is a test to see if the interface is a wireless one.
	if ${are_we_working_in_wg_namespace:+ip netns exec "$wg_outgoing_namespace"} iw dev "$current_interface" info &>/dev/null
	then
		# Use the 'iw' command since the interface is wireless.
		#
		# NOTE:
		# 'iw' doesn't use the name of the interface, it uses it's "physical index".
		wiphy_index=$(${are_we_working_in_wg_namespace:+ip netns exec "$wg_outgoing_namespace"} iw dev "$current_interface" info | gawk '$1 == "wiphy" {print $2}')
		print_cmd ${are_we_working_in_wg_namespace:+ip netns exec "$wg_outgoing_namespace"} iw phy phy"$wiphy_index" set netns ${are_we_working_in_wg_namespace:-name "$wg_outgoing_namespace"}
		return
	fi
	# The interface is not wireless, so we use the 'ip' command.
	print_cmd ${are_we_working_in_wg_namespace:+ip netns exec "$wg_outgoing_namespace"} ip link set "$current_interface" netns ${are_we_working_in_wg_namespace:-"$wg_outgoing_namespace"}
}
# This function might or might not be needed depending on the init system.
wait_for_interface_to_be_up()
{
	# NOTE:
	# * Any services that depend on a network interface (like netmount) might be stopped when the interface stops; if this happens, we can
	#   run a command to start it again. Prefix the command to start up the service with 'ip netns exec "$wg_outgoing_namespace"' if we want it
	#   to be started in the same namespace as our non-WG interfaces; if we want the service to use the WG interface, don't use the prefix.
	#
	# * For whatever reason, we have to wait for the interfaces that start in the wg_outgoing_namespace namespace to be "UP"
	#   (can be seen by 'ip -n "$wg_outgoing_namespace" link') before we start services that depend on them even though OpenRC can schedule the sevices that
	#   depend on the interfaces. It seems that scheduled services are ran with the same command that the dependency uses. In other words, if an interface
	#   starts in namespace "XYZ", then a scheduled service that depends on that interface will also start in namespace "XYZ".
	#   Not sure if this is an OpenRC bug.
	local current_interface=''
	# Wait until all interfaces that are supposed to be up are actually up.
	for current_interface in "${Interface_list[@]}"
	do
		# Skip interfaces that are not supposed to be up.
		[[ ! "$current_interface" =~ :UP$ ]] && continue
		# Wait until the current interface is up.
		message_spin_wrapper "Waiting for ${current_interface%:*} to be up" wait_for_current_interface_to_be_up
	done
}
wait_for_current_interface_to_be_up()
{
	while true
	do
		[[ -n "$(${are_we_working_in_wg_namespace:+ip netns exec "$wg_outgoing_namespace"} ip link | grep -E -- "${current_interface%:*}.*state UP")" ]] && break
		sleep 1
	done
}
# Perform an action on a service.
act_on_extra_service()
{
	local current_service=''
	# Start and stop are the only valid actions.
	[[ ! "$INTERFACE_ACTION" =~ ^(start|stop)$ ]] && return
	
	# If we are starting services, we need to wait for the interfaces to be up.
	[[ "$INTERFACE_ACTION" == "start" ]] && wait_for_interface_to_be_up
	# Start/stop services that should use the WG interface.
	for current_service in "${Extra_service_through_wg[@]}"
	do
		print_cmd rc-service "$current_service" "$INTERFACE_ACTION" &
	done
	
	# Start/stop services that should **NOT** use the WG interface.
	#
	# NOTE:
	# Technically, we don't have to wait for the interfaces to be up before we start services that will be ran in the same namespace as said interfaces;
	# this is due to the OpenRC behavior explained in the "wait_for_interface_to_be_up" function. The code is placed here anyway to keep this script as
	# simple as possible.
	for current_service in "${Extra_service_through_no_wg[@]}"
	do
		print_cmd ${are_we_working_in_wg_namespace:+ip netns exec "$wg_outgoing_namespace"} rc-service "$current_service" "$INTERFACE_ACTION" &
	done
	# We run service commands in parallel to speed things up, but we still have to wait for all of them to finish.
	wait -f
}
get_interface_to_act_on()
{
	local   print_interface=''
	local current_interface=''
	# If the physical interface list is "NONE", then we are not going to modify any physical interfaces.
	[[ "${Wg_physical_interface[@]}" == "NONE" ]] && return 
	# If we have any specified interfaces, we will only act on them.
	for current_interface in "${Wg_physical_interface[@]}"
	do
		# Build the 'ip link' output for the specified interfaces.
		print_interface+="$(${are_we_working_in_wg_namespace:+ip netns exec "$wg_outgoing_namespace"} ip link show "$current_interface")"$'\n'
	done
	# If this variable is empty, it means that we did not specify any interfaces. The behavior now, is to act on all interfaces.
	if [[ -z "$print_interface" ]]
	then
		print_interface="$(${are_we_working_in_wg_namespace:+ip netns exec "$wg_outgoing_namespace"} ip link)"
	fi
	
	# This function will return the name and state of the final set of interfaces in the form of '<name>:<state>'.
	echo "$print_interface" | gawk '$1 ~ /^[[:digit:]]+:$/ {print $2 gensub(/.*state ([[:upper:]]+).*/, "\\1", "g")}'
}
# Perform an action on an interface.
act_on_interface()
{
	local current_interface=''
	# This determines what we do to the interface. "start", "stop", "move", etc.
	local -r INTERFACE_ACTION="$1"
	# This is the network namespace that we are working in.
	local -r WORKING_NAMESPACE="$2"
	# This is answering a question. If the answer is yes, then the value is "1"; if the answer is no, then the value is an empty string.
	local are_we_working_in_wg_namespace=''
	# In several parts of this script, we will see the following parameter expansions:
	# ${are_we_working_in_wg_namespace:+<some string>}
	# ${are_we_working_in_wg_namespace:-<some string>}
	#
	# These parameter expansions will modify the commands that they're in. We use these because we might need to run a command in a specific network
	# namespace or make some modification to a value due to the specific network namespace. Using these parameter expansions prevents us from writing x2
	# as much code.
	[[ "$WORKING_NAMESPACE" != "$INITIAL_NAMESPACE" ]] && are_we_working_in_wg_namespace="1"
	# We only care about performing the following on a "stop" because we will lose information about our system (specifically network interfaces) when we
	# "stop".
	if [[ "$INTERFACE_ACTION" == "stop" ]]
	then
		# Before we do anything, we want to have all of our network interfaces that are supposed to be started actually started. This is different from
		# the interface being "UP" as stated by 'ip link'.
		#
		# NOTE:
		# There seems to be a bug where OpenRC cannot resume network interfaces in a different network namespace when recovering from sleep.
		# This causes a permanent "inactive" status on the interface, causing the script to loop forever. To prevent this from happening, we can skip
		# this step when bringing down WireGuard.
		[[ -z "$are_we_working_in_wg_namespace" ]] && message_spin_wrapper "Waiting for the init system to start network interfaces" wait_for_init_system_to_start_network_interface
		# Keep track of the state of each interface that we are going to modify so that we know which ones to start up (the ones that were already up).
		Interface_list=($(get_interface_to_act_on))
	fi
	for current_interface in "${Interface_list[@]}"
	do
		# Only start interfaces that were up before we stopped them.
		[[ ! "$current_interface" =~ :UP$ && "$INTERFACE_ACTION" == "start" ]] && continue
		current_interface="${current_interface%:*}"
		# If the interface is the loopback or a WG one, skip.
		[[ "$current_interface" == "lo" || "$current_interface" =~ $WG_INTERFACE_REGEX ]] && continue
		case "$INTERFACE_ACTION" in
		start|stop)
			start_stop_interface
			;;
		move)
			move_interface
			;;
		esac
	done
	# We run service commands in parallel to speed things up, but we still have to wait for all of them to finish.
	wait -f
	act_on_extra_service
}
start_stop_interface()
{
	# We use the '-D' option because it prevents other services from stopping.
	#
	# NOTE:
	# * If this script is executed from an OpenRC Service Script, the "depend" function in **THAT** script should contain "need net"
	#   because WG might need an internet connection to resolve a dynamic DNS (DDNS).
	#   
	#   Due to the above statement, the '-D' option would now be required (if using an OpenRC Service Script) because this script would be
	#   trying to stop a service it depends on (OpenRC would get stuck). When adding the OpenRC Service Script (that executes this script)
	#   to the same runlevel as the "net.*" services, testing has shown that booting the system works, but shutting down does not. :(
	#
	#   The conclusion to all of this is that this script is best used manually or automatically via a cron job.
	print_cmd ${are_we_working_in_wg_namespace:+ip netns exec "$wg_outgoing_namespace"} rc-service -D net."$current_interface" "$INTERFACE_ACTION" &
}
act_on_dns()
{
	local -r    TEMP_DNS=/tmp/resolv_"$WG_INTERFACE".old
	local -a  Nameserver=()
	local -a      Search=()
	local    current_dns=''
	case "$1" in
	use_new)
		# If we don't have any new DNS entries to use, don't do anything to the current DNS configuration.
		[[ "${#Wg_dns[@]}" -eq 0 ]] && return
		print_cmd cp /etc/resolv.conf "$TEMP_DNS"
		echo "This file has been modified by wg-netns for the \"$WG_INTERFACE\" WireGuard interface." >/etc/resolv.conf
		for current_dns in "${Wg_dns[@]}"
		do
			[[ "$current_dns" =~ ^[[:digit:].]+$|: ]] && Nameserver+=("$current_dns") || Search+=("$current_dns")
		done
		# /etc/resolv.conf has specific syntax:
		# - only one 'nameserver' per keyword
		# - if there are multiple 'search' directives, only the search list from the last instance is used
		# See 'man resolv.conf' for more info.
		{
			[[ "${#Nameserver[@]}" -gt 0 ]] && printf 'nameserver %s\n' "${Nameserver[@]}"
			[[ "${#Search[@]}"     -gt 0 ]] && printf 'search     %s\n' "${Search[*]}"
		} \
		>>/etc/resolv.conf
		;;
	use_old)
		# Restore the system DNS configuration.
		[[ ! -f "$TEMP_DNS" ]] && return
		print_cmd cp "$TEMP_DNS" /etc/resolv.conf
		print_cmd rm "$TEMP_DNS"
		;;
	esac
}
# This function might or might not be needed depending on the init system.
wait_for_init_system_to_start_network_interface()
{
	# Wait until all network interfaces that OpenRC is trying to start actually start. "inactive"/"starting" means that OpenRC has began the process of
	# bringing up an interface, but it's not usable ('ip link' does not show "status UP" for that interface) yet.
	while true
	do
		[[ -z "$(rc-status | gawk '$1 ~ /^net\./ && $3 ~ /^(inactive|starting)$/')" ]] && break
		sleep 1
	done
}
# This function returns a status of "0" (yes/true) or "1" (no/false), answering the question:
is_there_another_wg_interface_using_this_namespace()
{
	# Get all of the WG interfaces that exist in the "init" namespace.
	local -ar EXISTING_WG_INTERFACE=($(ip link | gawk -v reg="@/${WG_INTERFACE_REGEX%$}:$/" '$2 ~ reg {print gensub(/:/, "", "g", $2)}'))
	local                      line=''
	local                       key=''
	local -a                  value=()
	local      current_wg_interface=''
	# For each existing WG interface...
	for current_wg_interface in "${EXISTING_WG_INTERFACE[@]}"
	do
		# Skip the interface that we are wanting to modify.
		[[ "$current_wg_interface" == "$WG_INTERFACE" ]] && continue
		# ...check if its config file is using the "wg_outgoing_namespace" namespace.
		while read -r line
		do
			# Remove comments.
			line="$(echo "$line" | grep -oE '^[^#]+')"
			key="$(echo "$line" | gawk '{print $1}')"
			value=($(echo "$line" | gawk '{$1 = ""; $2 = ""; print}'))
			# If another existing WG interface is using the "wg_outgoing_namespace" namespace, return true.
			[[ "$key" == "Namespace" && "${value[0]}" == "$wg_outgoing_namespace" ]] && return 0
	
		done \
		</etc/wireguard/"$current_wg_interface".conf
	done
	# If we did not find an existing WG interface that is using the "wg_outgoing_namespace" namespace, return false.
	return 1
}
bring_wg_interface_up()
{
	# Make a network namespace where all of the interfaces that can connect to the internet will live.
	print_cmd ip netns add "$wg_outgoing_namespace"
	# We need to setup WG before cutting our internet in case our WG endpoint is a dynamic DNS (DDNS).
	configure_wg_interface
	# Stop nearly all interfaces (specifically, the ones that can connect to the internet).
	act_on_interface stop "$INITIAL_NAMESPACE"
	# Move interfaces that can connect to the internet into the "wg_outgoing_namespace" namespace.
	act_on_interface move "$INITIAL_NAMESPACE"
	# Use the new DNS entries for the system DNS resolver.
	act_on_dns use_new
	# We can now configure the wg_outgoing_namespace devices using the ordinary tools, but we launch them inside the "wg_outgoing_namespace" network
	# namespace.
	act_on_interface start "$wg_outgoing_namespace"
}
bring_wg_interface_down()
{
	act_on_interface stop "$wg_outgoing_namespace"
	# Move nearly all interfaces specified by the current WG conf file back to the init namespace.
	act_on_interface move "$wg_outgoing_namespace"
	# Delete the WG interface because we are no longer using it.
	print_cmd ip link del "$WG_INTERFACE"
	# We don't want to delete this namespace if another WG interface is using it.
	is_there_another_wg_interface_using_this_namespace || print_cmd ip netns del "$wg_outgoing_namespace"
	# Undo the changes to the system DNS resolver.
	act_on_dns use_old
	act_on_interface start "$INITIAL_NAMESPACE"
}
add_data_from_conf_file()
{
	local -r WG_CONF_FILE=/etc/wireguard/"$WG_INTERFACE".conf
	local            line=''
	local             key=''
	local -a        value=()
	# The WG config file must be a file and readable.
	if [[ ! ( -f "$WG_CONF_FILE" && -r "$WG_CONF_FILE" ) ]]
	then
		echo "$WG_CONF_FILE does not exist, is not a file, or cannot be read!"
		exit 1
	fi
	while read -r line
	do
		# Remove comments.
		line="$(echo "$line" | grep -oE '^[^#]+')"
		key="$(echo "$line" | gawk '{print $1}')"
		value=($(echo "$line" | gawk '{$1 = ""; $2 = ""; print}'))
		case "$key" in
		Address)
			Wg_address+=(${value[@]//,/ })
			;;
		MTU)
			wg_mtu="${value[0]}"
			;;
		DNS)
			Wg_dns+=(${value[@]//,/ })
			;;
		Namespace)
			wg_outgoing_namespace="${value[0]}"
			;;
		PhysicalInterface)
			Wg_physical_interface+=(${value[@]//,/ })
			;;
		ExtraServiceYes)
			Extra_service_through_wg+=(${value[@]//,/ })
			;;
		ExtraServiceNo)
			Extra_service_through_no_wg+=(${value[@]//,/ })
			;;
		*)
			# This is building up a config file that the 'wg' command can use.
			wg_config_file_data_built_by_this_script+="$line"$'\n'
			;;
		esac
	done \
	<"$WG_CONF_FILE"
}
check_data_before_running_this_script()
{
	local -i error_count=0
	if [[ "$(id -un)" != "root" ]]
	then
		echo "This script must run as root!"
		error_count=1
	fi
	if [[ "${#Wg_address[@]}" -eq 0 ]]
	then
		echo "There are no addresses assigned to the WireGuard interface!"
		error_count=1
	fi
	if [[ -z "$wg_outgoing_namespace" ]]
	then
		echo "There is no name assigned to the network namespace!"
		error_count=1
	fi
	if [[ "$wg_outgoing_namespace" == "$INITIAL_NAMESPACE" ]]
	then
		echo "The name of the network namespace cannot be the same as the defined initial namespace: $INITIAL_NAMESPACE"
		error_count=1
	fi
	if [[ ! "$WG_INTERFACE_REGEX" =~ ^\^.*\$$ ]]
	then
		echo "The WireGuard regular expression must start with '^' and end with '$'!"
		error_count=1
	fi
	# We want to be able to identify which interfaces are WG ones.
	if [[ ! "$WG_INTERFACE" =~ $WG_INTERFACE_REGEX ]]
	then
		echo "The WireGuard interface name must match the regular expression: $WG_INTERFACE_REGEX"
		error_count=1
	fi
	# Check if we're using PAM; if so, check for the correct authentication.
	if [[ -f /etc/pam.d/run_init && -z "$(gawk '$1 == "auth" && $2 == "sufficient" && $3 == "pam_rootok.so" && NF == 3' /etc/pam.d/run_init)" ]]
	then
		echo "The following line needs to be added to /etc/pam.d/run_init:"
		echo "\"auth sufficient pam_rootok.so\""
		error_count=1
	fi
	# If we are starting a WG interface that is already up, do nothing.
	if [[ "$SELF_ACTION" == "up" && -n "$(ip link show "$WG_INTERFACE" 2>/dev/null)" ]]
	then
		echo "$WG_INTERFACE is already up!"
		error_count=1
	fi
	
	# If we are stopping a WG interface that is already down, do nothing.
	if [[ "$SELF_ACTION" == "down" && -z "$(ip link show "$WG_INTERFACE" 2>/dev/null)" ]]
	then
		echo "$WG_INTERFACE is already down!"
		error_count=1
	fi
	# If we encounter any errors, don't run the script.
	[[ $error_count -ne 0 ]] && exit 1
}
################
# Script startup
################
add_data_from_conf_file
check_data_before_running_this_script
case "$SELF_ACTION" in
up)
	bring_wg_interface_up
	;;
down)
	bring_wg_interface_down
	;;
esac
```
`root #``cat /usr/local/bin/wg-bypass````
#!/bin/bash
# /usr/local/bin/wg-bypass
# Run a command in a specific network namespace.
wg-bypass()
{
	local -r NAMESPACE='physical'
	# If the user is root, then we don't need to use 'sudo'.
	if [[ "$(id -un)" == "root" ]]; then
		ip netns exec "$NAMESPACE" "$@"
		return
	fi
	
	# The first 'sudo' is needed because 'ip netns' needs root permissions; the second 'sudo' is needed to run the command we want to run as our original
	# user. If we didn't have the second 'sudo', all of our commands would run as root.
	sudo ip netns exec "$NAMESPACE" sudo -u "$(id -un)" -- "$@"
}
wg-bypass "$@"
```
Set the correct permissions so that we can execute the files:

`root #``chmod 755 /usr/local/bin/{wg-netns,wg-bypass}` ##### Configuration

Now that we have all the files we need, we can begin using our custom values. We are mainly going to be modifying the wg0.conf file, but the wg-netns file can be edited too if needed. Regardless, each file contains comments that should explain why it does the things it does.


In the wg0.conf file, the only variables that need to be modified are:

- PrivateKey
- Address
- PublicKey
- PresharedKey (if used)
- AllowedIPs (if we don't want to route all packets)
- Endpoint

Everything else is either optional or already has a default value.

**`wg0.conf`**

**Example configuration**

```
[Interface]
PrivateKey = ecoetnhuastn0620757ostnhoausho
Address = 123.123.123.123/32, a::cee:25/128, 222::666/128
MTU = 1420
DNS =
Namespace = physical
PhysicalInterface = eth0, wlan0, wlan1
ExtraServiceYes = uwsgi.searxng, nginx.myotherwebsite
ExtraServiceNo = nginx.mycoolwebsite
[Peer]
PublicKey = 0624204euoeu24424042thaoestuhuoeauh
PresharedKey = oesutuh75757557tnuhoeasuthoeuseohu
AllowedIPs = 10.0.20.0/24, 5555::/64
Endpoint = my.dynamicdns.org:55333
PersistentKeepAlive = 25
[Peer]
PublicKey = hhhhhhuhetuheteu062000206020620
PresharedKey = 75357375thhthuteohuonuheoutoehuo
AllowedIPs = 11.55.222.0/24, 1234::/64
Endpoint = 173.10.3.5:61199
```
The wg0.conf file is basically a normal WireGuard configuration file with extra features.


Users that have [PAM](https://wiki.gentoo.org/wiki/PAM) enabled on their system might need to modify their PAM configuration file for OpenRC. By default, PAM will require authentication for each OpenRC command (even if the user is root). The wg-netns has multiple rc- commands in it; if PAM fails, the script will fail. To fix this, edit the /etc/pam.d/run\_init file and add the line:

There should already be a message in this file with this exact line commented-out; simply uncomment the line, save, and exit. This change will make it so that we don't need to authenticate ourselves to PAM when we run an OpenRC command as root (being root is already enough).


Once we are done editing our wg0.conf file, we can bring up the WireGuard interface by running:

`root #``wg-netns up wg0`
And bring it down by running:

`root #``wg-netns down wg0`
We can check the status of the "init"/"1" namespace by running:

`user $``ip address`
And the status of a specific namespace with:

`root #``ip -n physical address`
To have an application bypass the WireGuard interface (such as LibreWolf):

`user $``wg-bypass librewolf`
In this case, LibreWolf will not use the WireGuard interface until it closes and starts without wg-bypass.

##### (Optional) Cron job

To automatically start the WireGuard interface at system boot, we can set up a cron job to execute wg-netns. We will be using Fcron ([sys-process/fcron](https://packages.gentoo.org/packages/sys-process/fcron)) as our cron daemon.

Open the system crontab using fcrontab:

`root #``fcrontab -u systab -e`
Add the following line:

Once we save and exit, the output should look similar to the following:

`root #``fcrontab -u systab -e`
0000-00-00 00:00:00 INFO fcrontab: editing systab's fcrontab
Modifications will be taken into account right now.

#### NetworkManager

Wireguard is officially supported by NetworkManager as of version 1.16<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>. Managing WireGuard is possible through the nmcli and nmtui commands. Review the [latest documentation upstream](https://networkmanager.dev/docs/api/latest/settings-wireguard.html) for the extensive list of key-value properties.

After creating a WireGuard configuration file (such as wg0.conf), the file can be imported into NetworkManager as a connection profile:

`user $``nmcli connection import type wireguard file /path/to/wg0.conf`
After the configuration has been imported, the connection can be activated via:

`user $``nmcli connection up wg0`
After adding a new connection, restart the interface by bringing it down, then back up up in a single command. Using the single command avoids disruption when working over the VPN connection itself:

`user $``nmcli connection down wg0 && nmcli connection up wg0`
See the [NetworkManager](https://wiki.gentoo.org/wiki/NetworkManager) article for more details on managing connection profiles.

## Removal

### Unmerge

To find Wireguard-related packages:

`root #``emerge --ask --search wireguard`
To remove the userspace tools:

`root #``emerge --ask --depclean --verbose net-vpn/wireguard-tools`
## Troubleshooting

### Rebuilding modules on kernel upgrades for kernels less than 5.6

When upgrading to a newer kernel that is less than version 5.6 (version 4.9.x LTS is a fitting example), it is important to re-emerge the 
Wireguard kernel modules. This is handled by default when using [genkernel](https://wiki.gentoo.org/wiki/Genkernel), but can be quickly performed using the following auto-generated Portage set:

`root #``emerge --ask @module-rebuild`
## See also

- [OpenVPN](https://wiki.gentoo.org/wiki/OpenVPN) — software that enables the creation of secure point-to-point or site-to-site connections.
- [vpnc](https://wiki.gentoo.org/wiki/Vpnc) — IPsec (Cisco/Juniper) VPN concentrator client

## External resources

- [https://github.com/trailofbits/algo](https://github.com/trailofbits/algo) - A project with scripts to help setup a personal VPN server.
- [http://lkml.iu.edu/hypermail/linux/kernel/1606.3/02833.html](http://lkml.iu.edu/hypermail/linux/kernel/1606.3/02833.html) - The initial Request for Comments post to the Kernel Mailing List.
- [https://www.latacora.com/blog/2018/05/16/there-will-be/](https://www.latacora.com/blog/2018/05/16/there-will-be/) - A blog post complementing WireGuard.
- [https://www.maketecheasier.com/set-up-wireguard-vpn-on-linux/](https://www.maketecheasier.com/set-up-wireguard-vpn-on-linux/) - How to Set Up Wireguard VPN for a Client and a Sever (e.g. for hosting a private game server for friends)
