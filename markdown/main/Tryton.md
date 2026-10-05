<!-- source: https://wiki.gentoo.org/wiki/Tryton | group: Gentoo Wiki (Main) | wiki-title: Tryton -->
---
title: Tryton
url: https://wiki.gentoo.org/wiki/Tryton
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-01-18"
fingerprint: c81cd70a9d0639d6
license: CC BY-SA 4.0
---

# Tryton

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Tryton** is a three-tiers high-level general purpose application platform under the license GPL-3 written in Python and using PostgreSQL as database engine.

It is the core base of a complete business solution ([ERP](https://en.wikipedia.org/wiki/Enterprise_resource_planning)) providing modularity, scalability and security.

## Installation

First step: install PostgreSQL, following [wiki entry](https://wiki.gentoo.org/wiki/PostgreSQL).

`root #``emerge --ask postgresql-server`
Second step: install eselect-repository

`root #``emerge --ask app-eselect/eselect-repository`
Third step: activate the overlay

`root #``eselect repository enable tryton`
Fourth step: install Tryton server and clients

`root #``emerge --ask --autounmask=y --autounmask-write trytond tryton sao``root #``dispatch-conf``root #``emerge --ask trytond tryton sao`
For more information: [Knowledge Base:Accepting a keyword for a single package](https://wiki.gentoo.org/wiki/Knowledge_Base:Accepting_a_keyword_for_a_single_package)

## Server configuration

The server options are setup in /etc/conf.d/trytond The options are:

- `CONFIG`: the path to the [configuration](https://docs.tryton.org/projects/server/en/latest/topics/configuration.html) (ex: `/etc/trytond/trytond.conf`)
- `LOGCONF`: the path to the [logging configuration](https://docs.tryton.org/projects/server/en/latest/topics/logs.html) (ex: `/etc/trytond/log.conf`)

To connect to postgresql, add this section to the configuration file:

**`/etc/trytond/trytond.conf`**

```
[database]
uri = postgresql://
```
To use the web client, add this section to the configuration file:

**`/etc/trytond/trytond.conf`**

```
[web]
root=/usr/share/sao
```
Don't forget start the database server.

For PostgreSQL:

`root #``/etc/init.d/postgresql start`
Setup the database following: [Upstream database documentation](https://docs.tryton.org/projects/server/en/latest/topics/setup_database.html#topics-setup-database)

You can start the server now:

`root #``rc-service trytond start`
If you want to start the server when de system boot up:

`root #``rc-update add trytond default`
## Client configuration

Configure the tryton client just running the application:

`user $``tryton`
The client ask about the server connection. Create a new profile with the next data:

Host: localhost:8000
Database: database name
Username: admin

## Modules installation

Initially, there are only have 2 modules: **ir** and **res**. To install other modules, watch the app-tryton category: /var/lib/layman/tryton/app-tryton. You can find the Tryton modules. Install what you need:

`root #``emerge --ask --autounmask=y --autounmask-write app-tryton/account``root #``dispatch-conf``root #``emerge --ask app-tryton/account`
After that, tell the database that there is new modules installed, for that:

`root #``/usr/bin/trytond-admin -c <config_file> -d <db_name> --update-modules-list`
