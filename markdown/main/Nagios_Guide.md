<!-- source: https://wiki.gentoo.org/wiki/Nagios/Guide | group: Gentoo Wiki (Main) | wiki-title: Nagios/Guide -->
---
title: Nagios/Guide
url: https://wiki.gentoo.org/wiki/Nagios/Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2020-01-03"
fingerprint: b4fcdc5afc8be9f6
license: CC BY-SA 4.0
---

# Nagios/Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

For system and network monitoring, the Nagios tool is a popular choice amongst various free software users. This guide helps you discover Nagios and how you can integrate it with your existing Gentoo infrastructure.

## Introduction

### System Monitoring

Single system users usually don't need a tool to help them identify the state of their system. However, when you have a couple of systems to administer, you will require an overview of your systems' health: do the partitions still have sufficient free space, is your CPU not overloaded, how many people are logged on, are your systems up to date with the latest security fixes, etc.

System monitoring tools, such as the Nagios software we discuss here, offer an easy way of dealing with the majority of metrics you want to know about your system. In larger environments, often called "enterprise environments", the tools aggregate the metrics of the various systems onto a single location, allowing for centralized monitoring management.

### About Nagios

The [Nagios](http://www.nagios.org) software is a popular software tool for host, service and network monitoring for Unix (although it can also capture metrics from the Microsoft Windows operating system family). It supports:

- obtaining metrics for local system resources, such as diskspace, CPU usage, memory consumption, ...
- discovering service availability (such as SSH, SMTP and other protocols),
- assuming network outages (when a group of systems that are known to be available on a network are all unreachable),

and more.

Basically, the Nagios software consists of a core tool (which manages the metrics), a web server module (which manages displaying the metrics) and a set of plugins (which obtain and send the metrics to the core tool).

### About this Document

The primary purpose of this document is to introduce you, Gentoo users, to the Nagios software and how you can integrate it within your Gentoo environment. The guide is not meant to describe Nagios in great detail - I leave this up to the documentation editors of Nagios itself.

## Setting Up Nagios

### Installing Nagios

Before you start installing Nagios, draw out and decide which system will become your master Nagios system (i.e. where the Nagios software is fully installed upon and where all metrics are stored) and what kind of metrics you want to obtain. You will not install Nagios on every system you want to monitor, but rather install Nagios on the master system and the Nagios plugins on the systems you want to receive metrics from.

Install the Nagios software on your central server:

`root #``emerge --ask nagios`
Follow the instructions the ebuild displays at the end of the installation (i.e. adding `nagios` to your active runlevel, configuring web server read access and more). 
Really. Read it.

Install plugins on the server as well:

`root #``emerge --ask nagios-plugins``root #``emerge --ask nagios-plugins-snmp`
### Restricting Access to the Nagios Web Interface

The Nagios web interface allows for executing commands on the various systems monitored by the Nagios plugins. For this purpose (and also because the metrics can have sensitive information) it is best to restrict access to the interface.

For this purpose, we introduce two access restrictions: one on IP level (from what systems can a user connect to the interface) and a basic authentication one (using the username / password scheme).

First, edit /etc/apache2/modules.d/99\_nagios3.conf and edit the `allow from` definitions:

For Apache >=2.4, "Order" and "Allow" as been deprecated and need to be changed for:

Next, create an Apache authorization table where you define the users who have access to the interface as well as their authorizations. The authentication definition file is called .htaccess and contains where the authentication information itself is stored.

Place this file inside the /usr/share/nagios/htdocs and /usr/lib/nagios/cgi-bin directories.

Create the /etc/nagios/auth.users file with the necessary user credentials. By default, the Gentoo nagios ebuild defines a single user called `nagiosadmin` . Let's create that user first:

`root #````
htpasswd2 -c /etc/nagios/auth.users nagiosadmin
```
`root #``chown nagios:nagios /etc/nagios/auth.users`
### Accessing Nagios

Once Nagios and its dependencies are installed, fire up Apache and Nagios:

`root #````
/etc/init.d/nagios start
```
`root #``/etc/init.d/apache2 start`
Next, fire up your browser and connect to [http://localhost/nagios](http://localhost/nagios) . Log on as the `nagiosadmin` user and navigate to the *Host Detail* page. You should be able to see the monitoring states for the local system.

## Monitoring nodes

There are various methods available to monitor nodes.

1. Collect data using check\_nrpe plugin where *NRPE* daemon is available on ssh hosts
2. Use a password-less SSH connection to execute the command remotely
3. Poll hosts with [SNMP](https://wiki.gentoo.org/wiki/SNMP)
4. Receive SNMP traps send by nodes and create alerts from it.

Monitoring via NRPE and SNMP are 2 different approaches, a significant difference between NRPE and SNMP monitoring is:

- configuration of NRPE is more *node* focused, where you actually have privileged access to a monitored node, and define there which checks are interesting for your monitoring system, and allow them.
- configuration of SNMP is more *server* focused, f.e. where there is no possibility to configure each node, you get only the SNMP community/SNMP user to a certain device, and configure checks on the server side.

### Using NRPE method

With NRPE, each remote host runs a daemon (the NRPE deamon) which allows the main Nagios system to query for certain metrics. One can run the NRPE daemon by itself or use an inetd program. I'll leave the inetd method as a nice exercise to the reader and give an example for running NRPE by itself.

First install the NRPE plugin:

`root #``emerge --ask nrpe`
Next, edit /etc/nagios/nrpe.cfg to allow your main Nagios system to access the NRPE daemon and customize the installation to your liking. Another important change to the nrpe.cfg file is the list of commands that NRPE supports. For instance, to use `nrpe` version 2.12 with Nagios 3, you'll need to change the paths from /usr/nagios/libexec to /usr/lib/nagios/plugins . Finally, launch the NRPE daemon:

`root #``/etc/init.d/nrpe start`
Finally, we need to configure the main Nagios system to connect to this particular NRPE instance and request the necessary metrics. To introduce you to Nagios' object syntax, our next section will cover this a bit more throroughly.

#### Configuring a Remote Host

First, edit /etc/nagios/nagios.cfg and place a `cfg_dir` directive. This will tell Nagios to read in all object configuration files in the said directory - in our example, the directory will contain the definitions for remote systems.

**`/etc/nagios/nagios.cfg`**

Create the directory and start with the first file, nrpe-command.cfg . In this file, we configure a Nagios command called `check_nrpe` which will be used to trigger a plugin (identified by the placeholder `$ARG1$` ) on the remote system (identified by the placeholder `$HOSTADDRESS$` ). The `$USER1$` variable is a default pointer to the Nagios installation directory (for instance, /usr/nagios/libexec ).

**`/etc/nagios/objects/remote/nrpe-command.cfg`**

**Configuration example 1**

Next, create a file nrpe-hosts.cfg where we define the remote host(s) to monitor. In this example, we define two remote systems:

**`/etc/nagios/objects/remote/nrpe-hosts.cfg`**

**Configuration example 2**

Finally, define the service(s) you want to check on these hosts. As a prime example, we run the system load test and disk usage plugins:

**`/etc/nagios/objects/remote/nrpe-services.cfg`**

**Configuration example 3**

That's it. If you now check the service details on the Nagions monitoring site you'll see that the remote hosts are connected and are transmitting their monitoring metrics to the Nagios server.

#### Using Passwordless SSH Connection

Just as we did by creating the `check_nrpe` command, we can create a command that executes a command remotely through a passwordless SSH connection. We leave this up as an interesting exercise to the reader.

A few pointers and tips:

- Make sure the passwordless SSH connection is set up for a dedicated user (definitely not root) - most checks you want to execute do not need root privileges anyway
- Creating a passwordless SSH key can be accomplished with `ssh-keygen` , you install a key on the destination system by adding the public key to the .ssh/authorized\_keys file

### Using SNMP method

With SNMP each node runs a SNMP daemon, not every node will offer SSH and NRPE monitoring, as a example network infrastructure often offers only to monitor via SNMP.

#### Configuring nodes

Configure SNMP access on remote nodes as explained in [SNMP](https://wiki.gentoo.org/wiki/SNMP) wiki article.
Start SNMP daemon on remote node

`root #``/etc/init.d/snmpd start`
#### Configuring polling server

On server configure /etc/nagios/resources.cfg, add the **SNMP community** used on remote nodes:

**`/etc/nagios/resources.cfg`**

emerge [net-analyzer/nagios-plugins](https://packages.gentoo.org/packages/net-analyzer/nagios-plugins)

`root #``emerge --ask net-analyzer/nagios-plugins`
Define a SNMP check for remote hosts, getting the usage of remote hard disk. notice the usage of **$USER161$** in that check:

**`/etc/nagios/objects/commands/chec_snmp_disk.cfg`**

Test the previously defined check, as nagios user

`root #``su - nagios`
These commands are runs as the **nagios** system user:

`user $``cd /usr/lib/nagios/plugins`
Possible output from example host **gentoo**, showing a WARNING state, the target rootfilesystem **/** exceeds the defined warning level **-w 80**%,

`user $``./check_snmp_disk_monitor.pl -H gentoo -C my-own-SNMP-community -m / -w 80 -c 90`
Disk WARNING - 85% used on /|/,124223229952,105289957376

**-s** shows staticstical or sometimes called *performance data* which can be used to create MRTG graphs with [net-analyzer/pnp4nagios](https://packages.gentoo.org/packages/net-analyzer/pnp4nagios) visualizing usage of service parameters over a period of time.

`root #``emerge --ask net-analyzer/pnp4nagios`
## More Resources

### Adding Gentoo Checks

It is quite easy to extend Nagios to include Gentoo-specific checks, such as security checks (GLSAs). Gentoo developer Wolfram Schlich has a `check_glsa.sh` script [available](http://dev.gentoo.org/~wschlich/misc/nagios/nagios-plugins-extra/nagios-plugins-extra-4/plugins/) amongst others.

## External resources

- [the official Nagios website](http://www.nagios.org)
- [Nagios Exchange](http://www.nagiosexchange.org), where you can find Nagios plugins and addons
- [Nagios Forge](http://www.nagiosforge.org), where developers host Nagios addon projects
- [PNP4Nagios official website](http://docs.pnp4nagios.org/pnp-0.6/start), to create graphs with collected Nagios data
