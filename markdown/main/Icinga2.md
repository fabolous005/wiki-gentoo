<!-- source: https://wiki.gentoo.org/wiki/Icinga2 | group: Gentoo Wiki (Main) | wiki-title: Icinga2 -->
---
title: Icinga2
url: https://wiki.gentoo.org/wiki/Icinga2
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-03-14"
fingerprint: "30ace5e2ed09a950"
license: CC BY-SA 4.0
---

# Icinga2

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Introduction

Icinga2 is similar to [Icinga](https://wiki.gentoo.org/wiki/Icinga) and [Nagios](https://wiki.gentoo.org/wiki/Nagios) and was officially released in 2014. It enables monitoring of hosts and services. This includes:

- A local host running Gentoo Linux, e.g. a server at home
- A remote host offering Icinga2 services
- Any remote host offering services like SSH or HTTP, not running any part of Icinga2

## Installation

### Gentoo host

The following steps setup Icinga2-monitoring with web interface on a host running Gentoo. The Icinga2-service can then be used to monitor remote hosts, too. But the focus is on monitoring of the Icinga2-enabled host. [PostgreSQL](https://wiki.gentoo.org/wiki/PostgreSQL) is the authentication backend and will hold monitoring data, too.

Packages:

Optional:

- [sys-apps/lm-sensors](https://packages.gentoo.org/packages/sys-apps/lm-sensors) for hardware monitoring

What will be monitored out of the box:

- /proc statistics, load
- mounted volumes/ disks
- HTTP-endpoints
- validity of TLS-certificates

Add apache user to group icingaweb2 so resources can be accessed:

`root #``gpasswd -a apache icingaweb2`
PostgreSQL[\[1\]](https://icinga.com/docs/icinga-2/latest/doc/02-installation/#setting-up-the-postgresql-database):

`user $````
psql -c "CREATE ROLE icinga WITH ENCRYPTED LOGIN PASSWORD 'yourSecret'"
```
`user $````
createdb -O icinga -E UTF8 icinga
```
Configure DB-access:

**`/etc/icinga2/features-enabled/ido-pgsql.conf`**

#### IcingaWeb2

PostgreSQL:

create user icingaweb2 with encrypted password 'icingaweb2';
create database icingaweb2;
GRANT ALL PRIVILEGES ON DATABASE icingaweb2 TO icingaweb2;

1. use /usr/share/icingaweb2/bin/icingacli
2. create token
3. configure rest through web interface

#### Hardware monitoring through lm-sensors

- broken configuration, see [bug #759595](https://bugs.gentoo.org/show_bug.cgi?id=759595)
- create local shell script
- setup CheckCommand manually

#### Graphs through pnp4nagios

- Package: [net-analyzer/pnp4nagios](https://packages.gentoo.org/packages/net-analyzer/pnp4nagios), USE=-nagios icinga
- Package: [www-apps/icingaweb2-module-pnp4nagios](https://packages.gentoo.org/packages/www-apps/icingaweb2-module-pnp4nagios)
- module pnp must be enabled in icinga2web
- /etc/icinga2/features-enabled +perfdata.conf (issues with icingacli, must be done as root/ manually)
- filling perfdata.conf is a bit fragile, default paths after installation don't match between icinga2 and npcd ingester

**`/etc/icingaweb2/navigation/host-actions.ini`**

**`/etc/icinga2/features-enabled/perdata.conf`**

Not running out of the box:



#### Trees and maps with NagVis

[NagVis](https://www.nagvis.org/) works with current Icinga and IcingaWeb2 through [icingaweb2-module-nagvis](https://github.com/Icinga/icingaweb2-module-nagvis).



- install NagVis to /usr/local/nagvis to keep it out of portage controlled paths
- copy into icingaweb2-module-nagvis /usr/share/icingaweb2/modules/ so IcingaWeb2 can pick it up
