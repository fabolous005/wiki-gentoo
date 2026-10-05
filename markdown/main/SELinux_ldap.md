<!-- source: https://wiki.gentoo.org/wiki/SELinux/ldap | group: Gentoo Wiki (Main) | wiki-title: SELinux/ldap -->
---
title: SELinux/ldap
url: https://wiki.gentoo.org/wiki/SELinux/ldap
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2013-05-08"
fingerprint: fd9bb87c5ee07a95
license: CC BY-SA 4.0
---

# SELinux/ldap

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Structure

### Domains

The slapd daemon runs within the slapd\_t domain and can only be transitioned towards through the sysadm\_t (general system administrative domain) or initrc\_t (init script launched) domains.

### File types/labels

The following table lists the file type/labels defined in the ldap module.

| Type | Function | Description | 
|---|---|---|
| slapd\_exec\_t | Entrypoint | Executable entry point for the slapd daemon binaries | 
| slapd\_etc\_t | Configuration | Label for OpenLDAP configuration files | 
| slapd\_cert\_t | Configuration | Label for certificate keystores used by OpenLDAP | 
| slapd\_db\_t | Configuration | Label for the OpenLDAP database files (backend content) | 
| slapd\_replog\_t | Configuration | Label for the slurpd replication log location | 
| slapd\_lock\_t |  | Label for the lock files (runtime) | 
| slapd\_tmp\_t |  | Label for the temporary files | 
| slapd\_var\_run\_t |  | Label for the runtime variable data | 
| slapd\_initrc\_exec\_t |  | Label for non-Gentoo init script |
