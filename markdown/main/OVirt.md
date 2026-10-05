<!-- source: https://wiki.gentoo.org/wiki/OVirt | group: Gentoo Wiki (Main) | wiki-title: OVirt -->
---
title: oVirt
url: https://wiki.gentoo.org/wiki/OVirt
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-07-04"
fingerprint: ebd6061985c6a3f2
license: CC BY-SA 4.0
---

# oVirt

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**oVirt** is a complete open sourced virtualization management platform working with [KVM](https://wiki.gentoo.org/wiki/QEMU). The project is made of:

- The **Engine core**, which is the backend server that does the management.
- The various **VDSM agents** installed on each host you 'll use as a hypervisor for VMs
- A **client side UI** (GWT based) and/or **RESTful API** to control the engine core.

## ovirt-engine

### Preparations

#### PostgreSQL

- Install PostgreSQL server:

`root #``emerge --ask dev-db/postgresql`
- Configure PostgreSQL server:

`root #``emerge --config dev-db/postgresql`
- Allow network access:

**`/etc/postgres-*/pg_hba.conf`**

- Start:

`root #``/etc/init.d/postgres* start`
#### Database

- Create user and database for engine:

`root #``su - postgres -c "psql -d template1"````
template1=# create user engine password 'engine';
template1=# create database engine owner engine template template0
    encoding 'UTF8' lc_collate 'en_US.UTF-8' lc_ctype 'en_US.UTF-8';
```
- OPTIONAL: Create user and database for dwh:

`root #``su - postgres -c "psql -d template1"````
template1=# create user ovirt_engine_history password 'ovirt_engine_history';
template1=# create database ovirt_engine_history owner ovirt_engine_history template template0
    encoding 'UTF8' lc_collate 'en_US.UTF-8' lc_ctype 'en_US.UTF-8';
```
- OPTIONAL: Create user and database for reports:

`root #``su - postgres -c "psql -d template1"````
template1=# create user ovirt_engine_reports password 'ovirt_engine_reports';
template1=# create database ovirt_engine_reports owner ovirt_engine_reports template template0
    encoding 'UTF8' lc_collate 'en_US.UTF-8' lc_ctype 'en_US.UTF-8';
```
## Installation

Portage overlay exists here: [http://github.com/alonbl/ovirt-overlay](https://github.com/alonbl/ovirt-overlay)

Add the overlay, creating /etc/portage/repos.conf/ovirt-overlay.conf:

**`/etc/portage/repos.conf/ovirt-overlay.conf`**

**oVirt overlay**

```
[ovirt-overlay]
location = /usr/local/portage/ovirt-overlay
sync-type = git
sync-uri = https://github.com/alonbl/ovirt-overlay
```
This can be done automatically with [app-eselect/eselect-repository](https://packages.gentoo.org/packages/app-eselect/eselect-repository):

Sync the repository:

`root #``emerge --sync ovirt-overlay`
Install ovirt-engine:

`root #``emerge --ask app-emulation/ovirt-engine`
You may need to add unstable keywords to various packages.

OPTIONAL: Emerge ovirt-engine-dwh and/or ovirt-engine-reports:

`root #``emerge --ask app-emulation/ovirt-engine-dwh app-emulation/ovirt-engine-reports`
Configure ovirt-engine:

`root #``emerge --config app-emulation/ovirt-engine`
Follow instructions.

## Post configuration

#### Apache

- Enable mod\_proxy, add after APACHE2\_OPTS="XXX":

**`/etc/conf.d/apache2`**

- Restart:

`root #``/etc/init.d/apache2 restart`
### Testing

Login into [http://localhost/ovirt-engine](http://localhost/ovirt-engine)

Logs are at /var/log/ovirt-engine

## Install vdsm

- Install dev-python/pyflakes as it is needed by vdsm:

`root #``emerge --ask dev-python/pyflakes`
- Obtain VDSM source RPM:

- Convert rpm package to tgz:

`user $``rpm2tgz vdsm-4.9.0-0.200.g2fc4e63.fc16.src.rpm`
- Unpack the archive:

`user $``tar zxvf vdsm-4.9.0-0.200.g2fc4e63.fc16.src.tgz`
- Enter the directory and do the configure-make-make install magic (Recommended to use `–prefix` when compiling from source so you can have all files under one directory per package):

`root #````
cd vdsm-4.9.0-0.200.g2fc4e63.fc16
```
`root #````
./configure --prefix=/path/to/install/directory && make && make install
```
## How to contribute

- oVirt project is working with Gerrit code review for code contribution.
- In order to register and login to oVirt's Gerrit, you'll need an OpenID account.
  - You can use a Google OpenID, or register to some other provider and use it,
- All other details can be found here: [http://www.ovirt.org/wiki/Working\_with\_oVirt\_Gerrit](https://www.ovirt.org/wiki/Working_with_oVirt_Gerrit)

## Advanced features

### oVirt Node integration

- By default development setup works with hosts based on base distro's installations.
- In order to be able to work with oVirt Node (which is a sub-set of the base OS), you'll need to setup a Public Key environment.
- More details on Engine and oVirt Node integration can be found here: [http://www.ovirt.org/wiki/Engine\_Node\_Integration](https://www.ovirt.org/wiki/Engine_Node_Integration).
  - Note that by default Gentoo does not have /etc/pki folder, and you'll need to create it (or write an ebuild which will do that).

### Troubleshooting JBoss (obsolete)

If you're being attacked by exceptions, follow this list:

- Verify jboss folder owner and permissions.
- If your machine has an SELinux policy installed, make sure it will not block JBoss. Here is a very dirty and insecure jboss.te just to temporarily pass these denials (you'll also need selinux-java)

- Used TCP ports: 8702/8083/1090/4457
  - Since JBoss binds to the hostname, your hostname should be resolvable, or you may add it to /etc/hosts for local resolution.

127.0.0.1 localhost engine-dev

## See also

- [Virtualization](https://wiki.gentoo.org/wiki/Virtualization) — the concept and technique that permits running software in an environment separate from a computer operating system.

## External resources

- [Additional information (including presentations)](https://www.ovirt.org/wiki/Workshop_November_2011)
- Users list: users@ovirt.org
