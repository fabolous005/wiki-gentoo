<!-- source: https://wiki.gentoo.org/wiki/User/Maffblaster/Drafts/Zabbix | group: Gentoo Wiki (Main) | wiki-title: User/Maffblaster/Drafts/Zabbix -->
---
title: User/Maffblaster/Drafts/Zabbix
url: https://wiki.gentoo.org/wiki/User/Maffblaster/Drafts/Zabbix
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-09-22"
fingerprint: e75dcc5cc98c286f
license: CC BY-SA 4.0
---

# User/Maffblaster/Drafts/Zabbix

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Zabbix is software for monitoring applications, networks, and servers, and cloud services.

Zabbix is written in C and PHP.

## Installation

### USE flags


| [+agent2](https://packages.gentoo.org/useflags/+agent2) | Enable go-based zabbix agent 2 (for to-be-monitored machines) | 
| [+openssl](https://packages.gentoo.org/useflags/+openssl) | Use dev-libs/openssl as TLS backend | 
| [+postgres](https://packages.gentoo.org/useflags/+postgres) | Add support for the postgresql database | 
| [agent](https://packages.gentoo.org/useflags/agent) | Enable zabbix agent (for to-be-monitored machines) | 
| [curl](https://packages.gentoo.org/useflags/curl) | Add support for client-side URL transfer library | 
| [frontend](https://packages.gentoo.org/useflags/frontend) | Enable zabbix web frontend | 
| [gnutls](https://packages.gentoo.org/useflags/gnutls) | Prefer net-libs/gnutls as SSL/TLS provider (ineffective with USE=-ssl) | 
| [ipv6](https://packages.gentoo.org/useflags/ipv6) | Turn on support of IPv6 | 
| [java](https://packages.gentoo.org/useflags/java) | Enable Zabbix Java JMX Management Gateway | 
| [ldap](https://packages.gentoo.org/useflags/ldap) | Add LDAP support (Lightweight Directory Access Protocol) | 
| [libxml2](https://packages.gentoo.org/useflags/libxml2) | Use libxml2 client library | 
| [mysql](https://packages.gentoo.org/useflags/mysql) | Add mySQL Database support | 
| [odbc](https://packages.gentoo.org/useflags/odbc) | Enable Database Monitor and use UnixODBC Library by default | 
| [openipmi](https://packages.gentoo.org/useflags/openipmi) | Enable openipmi things | 
| [oracle](https://packages.gentoo.org/useflags/oracle) | Enable Oracle Database support | 
| [proxy](https://packages.gentoo.org/useflags/proxy) | Enable proxy support | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [server](https://packages.gentoo.org/useflags/server) | Enable zabbix server | 
| [snmp](https://packages.gentoo.org/useflags/snmp) | Add support for the Simple Network Management Protocol if available | 
| [sqlite](https://packages.gentoo.org/useflags/sqlite) | Add support for sqlite - embedded sql database | 
| [ssh](https://packages.gentoo.org/useflags/ssh) | SSH v2 based checks | 
| [static](https://packages.gentoo.org/useflags/static) | Build statically linked binaries | 

### Zabbix server

On hosts acting as a Zabbix server, ensure the `server` flag is enabled. Typically the same host will serve the web front end as well as an agent for self-monitoring as well.

The ebuild's `server` USE flag requires exactly-one-of either `mysql` or `postgres`, so disable whichever will not be utilized.

**`/etc/portage/package.use`**

**Enable flags for server use**

The [fping](https://gitweb.gentoo.org/repo/gentoo.git/plain/licenses/fping) software license will be required to be accepted when running Zabbix in a server configuration:

`root #``echo "net-analyzer/fping fping" >> /etc/portage/package.license`
**`/etc/portage/package.license`**

**Accepting the fping software license**

### Zabbix clients

Systems to be monitored will need to run the agent only.

**`/etc/portage/package.use`**

**Enable client flag**

### Emerge

`root #``emerge --ask net-analyzer/zabbix`
### Web interface (frontend)

Copy the appropriate webapp files from the /usr/share/webapps/zabbix directory into the area used by the web server.

`root #``cp -r /usr/share/webapps/zabbix/<version>/htdocs/* /var/www/<place for>/zabbix`
### PHP requirements

**`/etc/portage/package.use`**

**PHP USE flag's**

## Configuration

### Services file

Adding info about zabbix services into the /etc/service file. This file serves as a reference database for network services. It contains service names and their associated port number and protocols.

**`/etc/services`**

**Adding info about zabbix services**

### MySQL backend database

Zabbix requires a database to hold state information.

`root #````
mysql -u root -p <password> 
```
`root #````
create database zabbix character set utf8 collate utf8_bin;
```
`root #````
grant all privileges on zabbix.* to zabbix@localhost identified by '<zabbix-user-password>';
```
`root #````
quit;
```
`root #````
mysql -u zabbix -p zabbix < /usr/share/zabbix/database/mysql/schema.sql
```
`root #````
mysql -u zabbix -p zabbix < /usr/share/zabbix/database/mysql/images.sql
```
`root #````
mysql -u zabbix -p zabbix < /usr/share/zabbix/database/mysql/data.sql
```
## See also

- [Nagios](https://wiki.gentoo.org/wiki/Nagios) — a complete monitoring and alerting for servers, switches, applications, and services.
