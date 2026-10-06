<!-- source: https://wiki.gentoo.org/wiki/Gitea | group: Gentoo Wiki (Main) | wiki-title: Gitea -->
---
title: gitea
url: https://wiki.gentoo.org/wiki/Gitea
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-25"
fingerprint: be2858c379858e81
license: CC BY-SA 4.0
---

# gitea

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Resources**

This article has some todo items:


[Services: runit](https://wiki.gentoo.org#Service)

**Gitea** is painless self-hosted [git](https://wiki.gentoo.org/wiki/Git) service, a fork of gogs.

## Installation

Gitea requires the use of a database backend, the following are supported:

- [MariaDB](https://wiki.gentoo.org/wiki/MariaDB)/MySQL
- [PostgreSQL](https://wiki.gentoo.org/wiki/PostgreSQL)
- SQLite *\<- recommended for small, private installations*

### USE flags


### USE flags for
            [www-apps/gitea](https://packages.gentoo.org/packages/www-apps/gitea)
            
            A painless self-hosted Git service

| [+acct](https://packages.gentoo.org/useflags/+acct) | User and group management via acct-\*/git packages | 
| [+filecaps](https://packages.gentoo.org/useflags/+filecaps) | Use Linux file capabilities to control privilege rather than set\*id (this is orthogonal to USE=caps which uses capabilities at runtime e.g. libcap) | 
| [gogit](https://packages.gentoo.org/useflags/gogit) | (EXPERIMENTAL) Use go-git variants of Git commands. | 
| [pam](https://packages.gentoo.org/useflags/pam) | Add support for PAM (Pluggable Authentication Modules) - DANGEROUS to arbitrarily flip | 
| [pie](https://packages.gentoo.org/useflags/pie) | Build programs as Position Independent Executables (a security hardening technique) | 
| [sqlite](https://packages.gentoo.org/useflags/sqlite) | Add support for sqlite - embedded sql database | 

### Emerge

`root #``emerge --ask www-apps/gitea`
## Configuration

### Files

- /etc/gitea/app.ini - User configuration file, see [bug #714844](https://bugs.gentoo.org/show_bug.cgi?id=714844)

- /etc/gitea/custom/conf/app.ini - Recommanded file to edit as said by the documentation inside app.ini (see above).


The path does not exist, create it:



`root #``mkdir -p /etc/gitea/custom/conf`


### Service

#### OpenRC

Starting *gitea* in the background:

`root #``rc-service gitea start`
Current status of *gitea* service:

`root #``rc-service gitea status`
Starting automatically at system boot:

`root #``rc-update add gitea default`


#### Systemd

Starting *gitea* in the background:

`root #``systemctl start gitea`
Current status of *gitea* service:

`root #``systemctl status gitea`
Starting automatically at system boot:

`root #``systemctl enable gitea`
## Usage

Start and/or enable **gitea** service.

The web interface should be available at [http://localhost:3000/](http://localhost:3000/), when running at first time, it should be redirected to [http://localhost:3000/install](http://localhost:3000/install).

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose www-apps/gitea`
## See also

## External resources

- [ArchWiki Gitea](https://wiki.archlinux.org/index.php/Gitea#Usage) - can be helpful while configuration sections is incomplete on Gentoo wiki
- [Official documentation](https://docs.gitea.io/)
