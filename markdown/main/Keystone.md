<!-- source: https://wiki.gentoo.org/wiki/Keystone | group: Gentoo Wiki (Main) | wiki-title: Keystone -->
---
title: Keystone
url: https://wiki.gentoo.org/wiki/Keystone
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-08-13"
fingerprint: "91dd65588dcc1a9f"
license: CC BY-SA 4.0
---

# Keystone

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Keystone** is a basic service on the openstack architecture providing Identity services.

## Installation

### USE flags

*sys-auth/keystone*correct?

### Emerge

`root #``emerge --ask sys-auth/keystone`
## Configuration

### Postgresql

First we generate a complex enough password...

`root #``uuidgen`
15877e78-0026-4120-9b51-257e198232ee

Connect to postgresql:

`root #``psql -U postgres`
Create keystone user, database etc... in postgresql:

`postgres=#````
CREATE USER keystone;
```
`postgres=#````
ALTER USER keystone WITH PASSWORD '15877e78-0026-4120-9b51-257e198232ee';
```
`postgres=#````
CREATE DATABASE keystone;
```
`postgres=#````
GRANT ALL PRIVILEGES ON DATABASE keystone TO keystone;
```
`postgres=#``\q`
Now Modify the configuration file:

**`/etc/keystone/keystone.conf`**

**keystone.conf**

```
[database]
...
connection = postgresql://keystone:15877e78-0026-4120-9b51-257e198232ee@127.0.0.1/keystone
...
[token]
...
provider = fernet
...
```
Emerge the postgresql python driver:

`root #``emerge --ask dev-python/psycopg`
Sync with the database:

`root #``keystone-manage db_sync`
Setup the fermet key repos:

`root #````
 keystone-manage fernet_setup --keystone-user keystone --keystone-group keystone
```
`root #````
 keystone-manage credential_setup --keystone-user keystone --keystone-group keystone
```
Boostrap the identity service:

`root #`
keystone-manage bootstrap --bootstrap-password <<root_pass>> \
  --bootstrap-admin-url http://<<hostname>>:35357/v3/ \
  --bootstrap-internal-url http://<<hostname>>:35357/v3/ \
  --bootstrap-public-url http://<<hostname>>:5000/v3/ \
  --bootstrap-region-id RegionOne

Now lets configure the webserver, if you do not have it emerge apache:

`root #``emerge --ask www-servers/apache`
