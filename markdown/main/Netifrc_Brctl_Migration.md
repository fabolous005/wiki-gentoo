<!-- source: https://wiki.gentoo.org/wiki/Netifrc/Brctl_Migration | group: Gentoo Wiki (Main) | wiki-title: Netifrc/Brctl Migration -->
---
title: Netifrc/Brctl Migration
url: https://wiki.gentoo.org/wiki/Netifrc/Brctl_Migration
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-02-25"
fingerprint: "4b153f3933ee9ff2"
license: CC BY-SA 4.0
---

# Netifrc/Brctl Migration

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article outlines details necessary to migrate a netifrc-based bridge setup from brctl to iproute.

The utilities from the [sys-apps/iproute2](https://packages.gentoo.org/packages/sys-apps/iproute2) package can manage bridges. It is superior than using the old specific utilities like the brctl command from [net-misc/bridge-utils](https://packages.gentoo.org/packages/net-misc/bridge-utils).

Modern Linux kernels expose bridge setting via sysfs, as result there is no need for iproute2 to support complex configuration as brctl utility, same sysfs configuration can be used for brctl based configurations as well.

## brctl to iproute2 migration

The migration of brctl to iproute can be done in two phases:

1. Migrate bridge configuration to sysfs, this can be done in stable [net-misc/netifrc](https://packages.gentoo.org/packages/net-misc/netifrc).
2. Migrate bridge management into iproute2 and drop brctl usage, this requires [net-misc/netifrc](https://packages.gentoo.org/packages/net-misc/netifrc) >= 0.4.0.

Once sysfs migration is completed, migration to iproute2 will be done as soon as netifrc supports iproute2, at this time [net-misc/bridge-utils](https://packages.gentoo.org/packages/net-misc/bridge-utils) can be safely removed from system.

#### brctl syntax

#### sysfs syntax

## Migration

### Old sysfs keys

In the past, bridge and brport settings were specified as variables without a prefix, now one should specify bridge\_ or brport\_ prefix, for example:

Should be specified as:

### brctl settings

#### Bridge master interface

These are setting of the bridge master device, the name of interface is the bridge name.

| brctl option | sysfs option | notes | 
|---|---|---|
| setageing | bridge\_ageing\_time | brctl is in seconds, sysfs is in 1/100 second, multiple by 100 | 
| setgcint | N/A | unsupported | 
| stp | bridge\_stp\_state | 'on', 'yes', '1' are translated to 1 otherwise 0 | 
| setbridgeprio | bridge\_priority |  | 
| setfd | bridge\_forward\_delay | brctl is in seconds, sysfs is in 1/100 second, multiple by 100 | 
| sethello | bridge\_hello\_time | brctl is in seconds, sysfs is in 1/100 second, multiple by 100 | 
| setmaxage | bridge\_max\_age | brctl is in seconds, sysfs is in 1/100 second, multiple by 100 | 

#### Bridge slave (port)

These are setting of the bridge slave device (port), the name of interface is the slave name.

| brctl option | sysfs option | notes | 
|---|---|---|
| setpathcost | brport\_path\_cost |  | 
| setportprio | brport\_priority |  | 
| hairpin | brport\_hairpin\_mode | '1' or '0' | 

### Examples

Various bridge attributes can be verified after porting the configuration by reading data from the /sys/class/net/br0/bridge/\* location:

`root #``ls /sys/class/net/br0/bridge`
ageing\_time      max\_age                            multicast\_snooping                stp\_state
bridge\_id        multicast\_igmp\_version             multicast\_startup\_query\_count     tcn\_timer
default\_pvid     multicast\_last\_member\_count        multicast\_startup\_query\_interval  topology\_change
flush            multicast\_last\_member\_interval     multicast\_stats\_enabled           topology\_change\_detected
forward\_delay    multicast\_membership\_interval      nf\_call\_arptables                 topology\_change\_timer
gc\_timer         multicast\_mld\_version              nf\_call\_ip6tables                 vlan\_filtering
group\_addr       multicast\_querier                  nf\_call\_iptables                  vlan\_protocol
group\_fwd\_mask   multicast\_querier\_interval         no\_linklocal\_learn                vlan\_stats\_enabled
hash\_elasticity  multicast\_query\_interval           priority                          vlan\_stats\_per\_port
hash\_max         multicast\_query\_response\_interval  root\_id
hello\_time       multicast\_query\_use\_ifaddr         root\_path\_cost
hello\_timer      multicast\_router                   root\_port



#### stp

**`/etc/conf.d/net`**

```
brctl_br0="setfd 15
sethello 2
stp on"
```
Becomes:

**`/etc/conf.d/net`**

```
bridge_forward_delay_br0=1500
bridge_hello_time_br0=200
bridge_stp_state_br0=1
```
#### port

**`/etc/conf.d/net`**

```
brctl_br0="setbridgeprio 50
setportprio eth0 60"
```
Becomes:

**`/etc/conf.d/net`**

```
bridge_priority_br0=50
brport_priority_eth0=60
```
