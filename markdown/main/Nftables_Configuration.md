<!-- source: https://wiki.gentoo.org/wiki/Nftables/Configuration | group: Gentoo Wiki (Main) | wiki-title: Nftables/Configuration -->
---
title: Nftables/Configuration
url: https://wiki.gentoo.org/wiki/Nftables/Configuration
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-28"
fingerprint: fdb8f159e3434eab
license: CC BY-SA 4.0
---

# Nftables/Configuration

[Nftables](https://wiki.gentoo.org/wiki/Nftables)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


[Nftables configuration] describes how
nftables rulesets are maintained and loaded on Gentoo Linux.



## nftables command file

The starting point of nftables is the main nftables command file.

Main nftables command file defaults to:

- /etc/nftables/rules/main.nft


A nftables command file contains the ruleset.



## Administrative models

During bootup, init subsystem loads the nftables kernel with ruleset from its main command file.


The init subsystem of **nftables** provides two administrative models of ruleset loading:

- **Direct File**
- **Saved State**


The **Direct File** model uses an administrator-maintained command file as the source of the ruleset.  Bootup reads only from the main command file.

The **Saved State** model stores the active ruleset in a file for restoration at startup.  Bootup reads from a state file containing nftables rulset saved from last reboot.

Both OpenRC and systemd init subsystems can use either model, but not both.

The main.nft file is used on this page as the administrator-maintained nft command file containing the ruleset.

## OpenRC

OpenRC supports both the **Direct File** and **Saved State** administrative models. Gentoo's [net-firewall/nftables](https://packages.gentoo.org/packages/net-firewall/nftables) package provides an OpenRC service.

### Files

| File | Description | 
|---|---|
| /etc/nftables/rules/main.nft | Ruleset maintained by the administrator. Test and load the ruleset, then do /etc/init.d/nftables save to persist it. Any filename and location may be used. May or may not be loaded directly at bootup. /etc/nftables/rules/main.nft is used on this page as the administrator's ruleset. | 
| /var/lib/nftables/rules-save | Persistent nftables ruleset. Created or updated by /etc/init.d/nftables save. Loaded at boot by /etc/init.d/nftables start or manually by /etc/init.d/nftables reload. Path specified by NFTABLES\_SAVE= in /etc/conf.d/nftables | 
| /etc/init.d/nftables | OpenRC service script for loading and saving the persistent **nftables** ruleset. | 
| /etc/conf.d/nftables | OpenRC configuration for nftables. NFTABLES\_SAVE= specifies the persistent ruleset file. Used by /etc/init.d/nftables | 

### Services

#### Direct File

The **Direct File** model loads the administrator-maintained command file into the active kernel ruleset.

Update the quoted filepath in NFTABLES\_SAVE key in nftables init configuration /etc/conf.d/nftables file.

`root #``$EDITOR /etc/conf.d/nftables`
**`/etc/conf.d/nftables`**

**nftables OpenRC init service configuration file**

```
# /etc/conf.d/nftables
...
# Location in which nftables initscript will save set rules on 
# service shutdown
# NFTABLES_SAVE="/var/lib/nftables/rules-save"
NFTABLES_SAVE="/etc/nftables/nftables.nft"
...
```

Test the command file:

`root #``nft -c -f /etc/nftables/nftables.nft`

Load the command file:

`root #``nft -f /etc/nftables/nftables.nft`

Enable the OpenRC service:

`root #``rc-update add nftables default`

Start the service:

`root #``rc-service nftables start`

The service configuration must be arranged so that the administrator-maintained command file, rather than a saved state, is loaded at boot.

#### Saved State

The **Saved State** model stores the active kernel ruleset in the saved-state file.

Default filepath of administrator maintained nftables command file is /etc/nftables/rules/main.nft.

First load and test the administrator-maintained command file:

`root #``nft -c -f /etc/nftables/rules/main.nft``root #``nft -f /etc/nftables/rules/main.nft`

Save the active kernel ruleset:

`root #``rc-service nftables save`

The saved state is written to the file specified by NFTABLES\_SAVE= in /etc/conf.d/nftables.

Start or reload the OpenRC service to restore the configured saved state:

`root #``rc-service nftables start``root #``rc-service nftables reload`


## systemd

Gentoo's systemd integration supports both the **Direct File** and **Saved State** administrative models.

### Direct File

#### Files

| File | Description | 
|---|---|
| /etc/nftables/rules/main.nft | Default Netfilter ruleset maintained by administrator. User-definable filename and filepath. Loaded directly at bootup. Path defined in /usr/lib/systemd/system/nftables.service | 
| /usr/lib/systemd/system/nftables.service | systemd service file for loading /etc/nftables/rules/main.nft. | 


The filename and location used by the service are determined by the systemd service configuration (/usr/lib/systemd/system/nftables\*.service; default is /etc/nftables/rules/main.nft.  To change filepath of nftables command file, see [custom filepath](https://wiki.gentoo.org#CustomFilepath).

Use temporary command file during testing before replacing the canonical command file.

`root #``nft -c -f /path/to/test.nft && nft -f /path/to/test.nft`


#### Services

Disable **Saved State** services:

`root #``systemctl disable nftables-load``root #``systemctl disable nftables-store`

Enable the **Direct File** service:

`root #``systemctl enable nftables`

Start the service:

`root #``systemctl start nftables`


### Saved State

The **Saved State** model restores the persistent saved state.
Instead of **Saved State** loading the administrator-maintained command file

#### Files

| File | Description | 
|---|---|
| /etc/nftables/rules/main.nft | Netfilter ruleset maintained by administrator. User-definable filename and filepath. Path defined in /usr/lib/systemd/system/nftables.service | 
| /usr/lib/systemd/system/nftables-load.service | systemd service file for loading /var/lib/nftables/rules-save into the kernel | 



#### Services

Disable the **Direct File** service:

`root #``systemctl disable nftables`

Disable shutdown persistence unless it is specifically required:

`root #``systemctl disable nftables-store`

Enable the **Saved State** loader:

`root #``systemctl enable nftables-load`

Start the loader:

`root #``systemctl start nftables-load`


### Saved State with shutdown persistence

The systemd Saved State workflow can optionally save the active kernel ruleset at shutdown.

nftables-store.service creates or updates:

/var/lib/nftables/rules-save

from the active kernel ruleset.

On the following boot, nftables-load.service can restore that saved state.



#### Files

| File | Description | 
|---|---|
| /etc/nftables/rules/main.nft | Netfilter ruleset maintained by administrator. User-definable filename and filepath. Path defined in /usr/lib/systemd/system/nftables.service | 
| /usr/lib/systemd/system/nftables-load.service | systemd service file for loading /var/lib/nftables/rules-save into the kernel | 
| /usr/lib/systemd/system/nftables-store.service | systemd service for saving the kernel ruleset at shutdown. | 



#### Services

Disable the **Direct File** service:

`root #``systemctl disable nftables`

Enable the **Saved State** services:

`root #``systemctl enable nftables-load``root #``systemctl enable nftables-store`

Start the loader:

`root #``systemctl start nftables-load`


## SysVinit

Where provided, Gentoo's SysVinit integration loads the configured ruleset during system startup.

Applicable command file and service configuration depend on the installed Gentoo integration.



## Customization

Customization are organized by init subsystem selected



### OpenRC customization

OpenRC init subsystem for nftables can be customized:

- Save active (live) kernel when stopping a service
- Save active (live) kernel for reuse at next bootup
- panic level - hard; defaults to all-traffic-stopped
- panic level - soft; only blocks new or invalid traffic, leaving existing traffic alone
- logger runs before nftables



#### **Save State** upon stopping nftable service

To save the current state of live nftables into its saved state file at /etc/init.d/nftables stop command, change SAVE\_ON\_STOP= value to "yes":

**`/etc/conf.d/nftables`**

**Nftables init configuration for OpenRC**

```
# Save state on stopping nftables
SAVE_ON_STOP="yes"
```
This will save its current ruleset to re-use at next boot.



#### Panic Block on Demand

To block all traffic upon To change filepath location of the nftables command file, execute:

For a complete block, no new nor old connections allowed:

`root #``/etc/init.d/nftables panic`

or shutting down all traffic except established connections shall continue:

`root #``/etc/init.d/nftables soft_panic`


#### Panic Block upon Stop

For a shutdown or an admin-stop to block all traffic upon using /etc/init.d/nftables stop.

Add or change the PANIC\_ON\_STOP= value to "hard" or "soft"; see previous section for details.

Then execute as needed:

`root #``/etc/init.d/nftables stop`
#### Start local logger before nftables

To start a local logger before the nftables starts reporting traffic log, edit /etc/conf.d/nftables:

`root #``$EDITOR /etc/conf.d/nftables`

Add or change rc\_use= value to "logger"

**`/etc/conf.d/nftables`**

**nftables OpenRC configuration file**

```
rc_use="logger"
```

Next, update OpenRC:

`root #``rc-update -u`
### Systemd customization

The systemd init subsystem for nftables can be customized:

- Save active (live) kernel during shutdown, for next (re)boot
- Flush always when stopped
- Custom filepath in systemd **Direct File** model



#### Flush always when stopped

Flushing an entire ruleset before loading a new one:

**`/etc/systemd/system/nftables.service.d/override.conf`**

**nftables OpenRC init service configuration file**

```
### Editing /etc/systemd/system/nftables.service.d/override.conf
### Anything between here and the comment below will become the contents of the drop-in file
[Service]
ExecStart=/sbin/nft 'flush ruleset; include "/etc/nftables/nftables.nft"'
ExecReload=/sbin/nft 'flush ruleset; include "/etc/nftables/rules/main.nft"'
### Edits below this comment will be discarded
```

Reload the new service file to accept the change:

`root #``systemctl daemon-reload`

Reload the ruleset

`root #``systemctl reload nftables`

To activate and load at startup, follow [here](https://wiki.gentoo.org#ServiceDirectFileSystemd).

#### Selective flush

Other network engineers want to selective flush their own rules.

**`/etc/systemd/system/nftables.service.d/override.conf`**

**nftables OpenRC init service configuration file**

```
### Editing /etc/systemd/system/nftables.service.d/override.conf
### Anything between here and the comment below will become the contents of the drop-in file
[Service]
ExecStart=/sbin/nft -f /etc/nftables/nftables.nft
ExecReload=/sbin/nft -f /etc/nftables/nftables.nft
### Edits below this comment will be discarded
```

Reload the new service file to accept the change:

`root #``systemctl daemon-reload`

Reload the ruleset

`root #``systemctl reload nftables`

To activate and load at startup, follow [here](https://wiki.gentoo.org#ServiceDirectFileSystemd).



#### Custom filepath in systemd **Direct File** model

To change filepath location of the nftables command file,

Create the nftables systemd override file (which is constructed as /etc/systemd/system/nftables.service.d/override.conf:

`root #``systemctl edit nftables`
Insert or clone in \[Service\] and both ExecStart= and ExecReload= with your new filepath value:



**`/etc/systemd/system/nftables.service.d/override.conf`**

**nftables systemd init service configuration file**

```
### Editing /etc/systemd/system/nftables.service.d/override.conf
### Anything between here and the comment below will become the contents of the drop-in file
[Service]
ExecStart=/sbin/nft -f /etc/nftables/nftables.nft
ExecReload=/sbin/nft -f /etc/nftables/nftables.nft
ExecStop=/sbin/nft -f /etc/nftables/panic-block-hard.nft
### Edits below this comment will be discarded
```

Reload the new service file:

`root #``systemctl daemon-reload`
Reload the ruleset

`root #``systemctl reload nftables`

To activate and load at startup, follow [here](https://wiki.gentoo.org#ServiceDirectFileSystemd).

### Logger

The log statement can send packet-log messages to the kernel logging facility or to an NFLOG group.

group selects an NFLOG group. NFLOG delivers packet-log messages through the Netfilter nfnetlink\_log subsystem using the Netlink API to a userspace application.

ulogd can receive NFLOG messages and process or record them.

The log rule statement without group uses the kernel logging path. The group parameter selects NFLOG instead.

For more detail on `log` statement, see [Nftables/Rules/Log](https://wiki.gentoo.org/wiki/Nftables/Rules/Log).

### Multiple nftables command files

The nftables command file can be split up into multiple files. Primary motivation for smaller files is ease of administration.

The main command file includes other command files. Included command files may also include additional command files.

The include command keyword includes that other command file.

**`/etc/nftables/rules/main.nft`**

**Main nftables command file**

```
#!/sbin/nft -f
include "/etc/nftables/rules/file1.nft"
include "/etc/nftables/rules/file2.nft"
include "/etc/nftables/rules/file3.nft"
```


#### Organize by single-flow

**`/etc/nftables/rules/main.nft`**

**Main nftables command file - Single flow**

```
#!/sbin/nft -f
include "/etc/nftables/rules/inet-ingress.nft"
include "/etc/nftables/rules/inet-input.nft"
include "/etc/nftables/rules/inet-forward.nft"
include "/etc/nftables/rules/inet-output.nft"
include "/etc/nftables/rules/inet-egress.nft"
```
#### Organize by functional flow

**`/etc/nftables/rules/main.nft`**

**Main nftables command file - by functional flow**

```
#!/sbin/nft -f
include "/etc/nftables/rules/inbound-ssh.nft"
include "/etc/nftables/rules/forward-proxy.nft"
include "/etc/nftables/rules/outbound-dns-query.nft"
```
#### Directory pickup

Use a glob pattern such as "\*.nft" to include multiple nftables command files from a directory.

**`/etc/nftables/rules/main.nft`**

**Main nftables command file - Directory Read All**

```
#!/sbin/nft -f
# Read all command files in a single directory
include "/etc/nftables/rules.d/*.nft"
```


Matching files are loaded in C-locale collation lexicographic order.

## Modular Ruleset

nft supports atomic rule replacement by using nft -f. Thus it is possible to conveniently manage the rules using files.

Nftables allows including additional files and directories. A directory, such as /etc/nftables.conf.d/ can be created which contains additional rulesets to be loaded.

The following file creates a basic skeleton of a ruleset, which can be used with modules, loaded from /etc/nftables.conf.d/:

**`/etc/nftables.rules`**

**Basic router ruleset**

```
#! /sbin/nft -f
 
flush ruleset
 
table netdev filter {
  # Basic filter chain, devices can be configued to jump here
  chain ingress_filter {
    # Drop all fragments.
    ip frag-off & 0x1fff != 0 counter drop
 
    # Drop XMAS packets.
    tcp flags & (fin|syn|rst|psh|ack|urg) == fin|syn|rst|psh|ack|urg counter drop
 
    # Drop NULL packets.
    tcp flags & (fin|syn|rst|psh|ack|urg) == 0x0 counter drop
 
    # Drop uncommon MSS values.
    tcp flags syn tcp option maxseg size 1-535 counter drop
 
  }
}
 
table inet filter {
  chain input {
    type filter hook input priority 200; policy drop;
    counter jump input_hook
    counter jump base_filter
    log prefix "Dropped input traffic: " counter drop
  }
  chain input_hook {
  }
 
  chain base_filter {
    counter jump drop_filter
    ct state vmap {
      established: accept,
      related: accept,
      new: continue,
      invalid: drop
    }
    # Allow loopback traffic
    iifname lo counter accept
    oifname lo counter accept
  }
 
  chain drop_filter {
  }
 
  chain forward {
    type filter hook forward priority 200; policy drop;
    counter jump forward_hook
    counter jump base_filter
    log prefix "Dropped forwarded traffic: " counter drop
  }
 
  chain forward_hook {
  }
 
  chain output {
    type filter hook output priority 200; policy drop;
    counter jump output_hook
    counter jump base_filter
    log prefix "Dropped output traffic: " counter drop
  }
  chain output_hook {
  }
}
 
table inet nat {
  chain prerouting {
    type nat hook prerouting priority 0;
  }
  chain postrouting {
    type nat hook postrouting priority 500;
  }
}
include "/etc/nftables.rules.d/*.rules"
```
#### Variable definitions

**`/etc/nftables.rules.d/00-definitions.rules`**

**Define variables which can be used in other modules.**

```
#! /sbin/nft -f
 
define lan_interface = fib.lan
define wifi_interface = ax1800
define management_interface = fib.management
define dmz_interface = ethernet2
 
define wan_interface = fib.wan
 
define wifi_network = 192.168.1.0/24
define lan_network = 192.168.10.0/24
define management_network = 192.168.255.0/24
define dmz_network = 192.168.2.0/24
 
table inet filter {
  set trusted_nets {
    type ipv4_addr
    flags interval
    elements = { $lan_network, $wifi_network }
  }
  set dmz_nets {
    type ipv4_addr
    flags interval
    elements = { $dmz_network }
  }
  set untrusted_nets {
    type ipv4_addr
    flags interval
    elements = { $dmz_network }
  }
}
```
#### Jumping to the ingress filter

**`/etc/nftables.rules.d/00-ingress.rules`**

**Hook the device ingress for*fib1* and *fib2*, filtering with the defined *ingress\_filter* chain.**

```
#! /sbin/nft -f
 
table netdev filter {
  chain ingress {
    type filter hook ingress device fib1 priority -500;
    jump ingress_filter
  }
  chain ingress {
    type filter hook ingress device fib2 priority -500;
    jump ingress_filter
  }
}
```
#### Basic drop filter

**`/etc/nftables.rules.d/01-drop-policy.rules`**

**Populate the*drop\_filter* chain to drop some spammy traffic found on the DMZ.**

```
#! /sbin/nft -f
 
define dmz_spam_udp = { 1234 }
define dmz_spam_tcp = { 2350 }
 
table inet filter {
  set spam_udp {
    type inet_service
    elements = { $dmz_spam_udp }
  }
  set spam_tcp {
    type inet_service
    elements = { $dmz_spam_tcp }
  }
 
  chain drop_filter {
    tcp dport @spam_tcp counter drop
    udp dport @spam_udp counter drop
  }
}
```
#### Basic ICMP filter

**`/etc/nftables.rules.d/01-icmp.rules`**

**Allow basic ICMP traffic**

```
#! /sbin/nft -f
 
define allowed_icmp_types = { echo-reply, echo-request }
define trusted_icmp_types = { destination-unreachable, time-exceeded }
define allowed_icmpv6_types = { nd-router-solicit, nd-router-advert, nd-neighbor-solicit, nd-neighbor-advert, echo-request, echo-reply }
 
table inet filter {
  chain base_filter {
    ip protocol icmp jump icmp_filter
    ip6 nexthdr icmpv6 jump icmpv6_filter
  }
  chain icmp_filter {
    icmp type $allowed_icmp_types counter accept
    ip saddr @trusted_nets icmp type $trusted_icmp_types counter accept
    ip daddr @trusted_nets icmp type $trusted_icmp_types counter accept
  }
  chain icmpv6_filter {
    icmpv6 type $allowed_icmpv6_types counter accept
  }
}
```
#### Allow DHCP traffic

**`/etc/nftables.rules.d/05-dhcp.rules`**

**Allow DHCP client and server traffic on appropriate interfaces.**

```
#! /sbin/nft -f
 
define dhcp_server_interfaces = { $lan_interface, $wifi_interface, $dmz_interface }
define dhcp_client_interfaces = { $management_interface, $wan_interface }
 
table inet filter {
  set dhcp_server {
   type inet_service;
   elements = { 67 }
  }
  set dhcp_client {
   type inet_service;
   elements = { 68 }
  }
  set dhcp6_server {
   type inet_service;
   elements = { 547 }
  }
  set dhcp6_client {
   type inet_service;
   elements = { 546 }
  }
  chain input_hook {
    iifname $dhcp_client_interfaces udp sport @dhcp_server udp dport @dhcp_client counter accept comment "Allow DHCP client input traffic"
    iifname $dhcp_client_interfaces udp sport @dhcp6_server udp dport @dhcp6_client counter accept comment "Allow DHCPv6 client input traffic"
    iifname $dhcp_server_interfaces udp dport @dhcp_server udp sport @dhcp_client counter accept comment "Allow DHCP server input traffic"
    iifname $dhcp_server_interfaces udp dport @dhcp6_server udp sport @dhcp6_client counter accept comment "Allow DHCPv6 server input traffic"
  }
  chain output_hook {
    oifname $dhcp_client_interfaces udp dport @dhcp_server udp sport @dhcp_client counter accept comment "Allow DHCP client output traffic"
    oifname $dhcp_client_interfaces udp dport @dhcp6_server udp sport @dhcp6_client counter accept comment "Allow DHCPv6 client output traffic"
    oifname $dhcp_server_interfaces udp sport @dhcp_server udp dport @dhcp_client counter accept comment "Allow DHCP server output traffic"
    oifname $dhcp_server_interfaces udp sport @dhcp6_server udp dport @dhcp6_client counter accept comment "Allow DHCPv6 server output traffic"
  }
}
```
#### Allow inbound and forwarded SSH traffic

**`/etc/nftables.rules.d/22-ssh.rules`**

**Allow inbound and forwarded SSH from the*$management\_network*; allow SSH to be forwarded to GitHub servers, from certain networks, and users on the router.**

```
#! /sbin/nft -f
define github_ssh_servers = { 140.82.112.3, 140.82.112.4, 140.82.114.3, 140.82.113.4, 140.82.114.4, 140.82.113.3 }
 
table inet filter {
  set external_ssh_servers {
    type ipv4_addr
    elements = { $github_ssh_servers }
  }
  set external_ssh_clients {
    type ipv4_addr
    flags interval
    elements = { $lan_network }
  }
  set ssh_clients {
    type ipv4_addr
    flags interval
    elements = { $management_network }
  }
  set ssh_ports {
    type inet_service;
    elements = { 22 }
  }
  chain ssh_filter {
    ip saddr @external_ssh_clients ip daddr @external_ssh_servers counter accept comment "Allow these users to SSH to specified external servers"
    ip saddr @ssh_clients counter accept comment "Allow this set to SSH anywhere, including the router itself"
    skuid 1000 ip daddr @external_ssh_servers counter accept comment "Allow SSH traffic from UID 1000 to allowed external servers"
  }
  chain base_filter {
    tcp dport @ssh_ports counter jump ssh_filter
  }
}
```
#### Allow outbound and forwarded NTP traffic

**`/etc/nftables.rules.d/21-ntp.rules`**

**Allow forwarded NTP traffic, outbound from the NTP user.**

```
#! /sbin/nft -f
table inet filter {
  set ntp_ports {
    type inet_service
    elements = { 123 }
  }
  set ntp4_ports {
    type inet_service
    elements = { 4460 }
  }
  chain base_filter {
    tcp dport @ntp4_ports counter jump ntp_filter
    udp dport @ntp_ports counter jump ntp_filter
  }
  chain ntp_filter {
    ip saddr @trusted_nets counter accept
    skuid 123 counter accept
  }
}
```
#### NAT LANs

**`/etc/nftables.rules.d/05-lan-nat.rules`**

**NAT some local networks.**

```
#! /sbin/nft -f
 
table inet nat {
  set nat_nets {
    type ipv4_addr
    flags interval
    elements = { $lan_network, $wifi_network, $dmz_network }
  }
  chain	postrouting {
    oifname $wan_interface ip saddr @nat_nets counter masquerade
  }
}
```
#### Masquerade Docker Traffic

**`/etc/nftables.rules.d/10-docker.rules`**

**Masquerade docker traffic**

```
#! /sbin/nft -f
 
define docker_default_net = 172.17.0.0/16
define docker_server_net = 10.100.100.0/24
define docker_nets = { $docker_default_net, $docker_server_net }
table inet filter {
  set docker_nets {
    type ipv4_addr
    flags interval
    elements = { $docker_nets }
  }
  set dns_ports {
    type inet_service
    elements = { 53 }
  }
  set web_ports {
    type inet_service
    elements = { 443 }
  }
 
  chain docker_filter {
    udp dport @dns_ports counter accept
    oifname $wan_interface tcp dport @web_ports counter accept
  }
  chain forward_hook {
    ip saddr @docker_nets counter jump docker_filter
  }
}
 
table inet nat {
  set nat_nets {
    type ipv4_addr
    flags interval
    elements = { $docker_nets }
  }
}
```
## Troubleshooting

### Old settings keep coming back

Check which service is responsible for loading or saving the nftables ruleset.

With systemd, inspect the nftables' systemd service units to determine which ruleset file is being loaded or saved.

`root #``systemctl cat nftables`

or if using systemd saved-unit method,

`root #``systemctl cat nftables-load``root #``systemctl cat nftables-store`

With OpenRC, check /etc/conf.d/nftables for the saved-state configuration.

## See also

- [nft](https://wiki.gentoo.org/wiki/Nft) — configures and inspects the Linux kernel's nftables packet handling framework
- [Nftables/Ruleset](https://wiki.gentoo.org/wiki/Nftables/Ruleset) — collection of tables, chains, rules, and other objects configured in the [Netfilter](https://wiki.gentoo.org/wiki/Netfilter) **nf\_tables** subsystem.
- [Nftables/Ruleset/Chain](https://wiki.gentoo.org/wiki/Nftables/Ruleset/Chain) — contains a group of rules used to process network traffic.
- [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules) — match packet properties and execute statements.
- [Nftables/Configuration]
- [nftables examples](https://wiki.gentoo.org/wiki/Nftables/Examples)
- [Nftables](https://wiki.gentoo.org/wiki/Nftables) — the Linux packet-handling framework
- [Netfilter](https://wiki.gentoo.org/wiki/Netfilter) — Linux kernel’s packet-filtering framework
- [Security Handbook](https://wiki.gentoo.org/wiki/Security_Handbook) — valuable guidance on Gentoo Linux security and cybersecurity in general.
