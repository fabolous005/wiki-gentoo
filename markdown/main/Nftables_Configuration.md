<!-- source: https://wiki.gentoo.org/wiki/Nftables/Configuration | group: Gentoo Wiki (Main) | wiki-title: Nftables/Configuration -->
---
title: Nftables/Configuration
url: https://wiki.gentoo.org/wiki/Nftables/Configuration
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-30"
fingerprint: d53af3d18b03a1ab
license: CC BY-SA 4.0
---

# Nftables/Configuration

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

Gentoo's [net-firewall/nftables](https://packages.gentoo.org/packages/net-firewall/nftables) package provides an OpenRC service.

OpenRC supports both the **Direct File** and **Saved State** administrative models.



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


The filename and location used by the service are determined by the systemd service configuration (/usr/lib/systemd/system/nftables\*.service; default is /etc/nftables/rules/main.nft.  To change filepath of nftables command file, see [custom filepath](https://wiki.gentoo.org/wiki/Nftables/Configuration#CustomFilepath).

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

Customization are organized by init subsystem selected.



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


Reload the new service file to accept the change:

`root #``systemctl daemon-reload`

Reload the ruleset

`root #``systemctl reload nftables`

To activate and load at startup, follow [here](https://wiki.gentoo.org/wiki/Nftables/Configuration#ServiceDirectFileSystemd).

#### Selective flush

Other network engineers want to selective flush their own rules.

**`/etc/systemd/system/nftables.service.d/override.conf`**

**nftables OpenRC init service configuration file**


Reload the new service file to accept the change:

`root #``systemctl daemon-reload`

Reload the ruleset

`root #``systemctl reload nftables`

To activate and load at startup, follow [here](https://wiki.gentoo.org/wiki/Nftables/Configuration#ServiceDirectFileSystemd).



#### Custom filepath in systemd **Direct File** model

To change filepath location of the nftables command file,

Create the nftables systemd override file (which is constructed as /etc/systemd/system/nftables.service.d/override.conf:

`root #``systemctl edit nftables`
Insert or clone in \[Service\] and both ExecStart= and ExecReload= with your new filepath value:



**`/etc/systemd/system/nftables.service.d/override.conf`**

**nftables systemd init service configuration file**


Reload the new service file:

`root #``systemctl daemon-reload`
Reload the ruleset

`root #``systemctl reload nftables`

To activate and load at startup, follow [here](https://wiki.gentoo.org/wiki/Nftables/Configuration#ServiceDirectFileSystemd).

### Logger

The log statement can send packet-log messages to the kernel logging facility or to an NFLOG group.

group selects an NFLOG group. NFLOG delivers packet-log messages through the Netfilter nfnetlink\_log subsystem using the Netlink API to a userspace application.

ulogd can receive NFLOG messages and process or record them.

The log rule statement without group uses the kernel logging path. The group parameter selects NFLOG instead.



### Multiple nftables command files

The nftables command file can be split up into multiple files. Primary motivation for smaller files is ease of administration.

The main command file includes other command files. Included command files may also include additional command files.

The include command keyword includes that other command file.

**`/etc/nftables/rules/main.nft`**

**Main nftables command file**



#### Organize by single-flow

**`/etc/nftables/rules/main.nft`**

**Main nftables command file - Single flow**

#### Organize by functional flow

**`/etc/nftables/rules/main.nft`**

**Main nftables command file - by functional flow**

#### Directory pickup

Use a glob pattern such as "\*.nft" to include multiple nftables command files from a directory.

**`/etc/nftables/rules/main.nft`**

**Main nftables command file - Directory Read All**



Matching files are loaded in C-locale collation lexicographic order.

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
- [Nftables/Ruleset](https://wiki.gentoo.org/wiki/Nftables/Ruleset)
- [Nftables/Ruleset/Chain](https://wiki.gentoo.org/wiki/Nftables/Ruleset/Chain) — contains a group of rules used to process network traffic.
- [Nftables/Rules](https://wiki.gentoo.org/wiki/Nftables/Rules)
- [Nftables/Configuration]
- [nftables examples](https://wiki.gentoo.org/wiki/Nftables/Examples)
- [Nftables](https://wiki.gentoo.org/wiki/User:Egberts/Drafts/Nftables) — the Linux packet-handling framework
- [Netfilter](https://wiki.gentoo.org/wiki/Netfilter) — Linux kernel’s packet-filtering framework
- [Security Handbook](https://wiki.gentoo.org/wiki/Security_Handbook) — valuable guidance on Gentoo Linux security and cybersecurity in general.
