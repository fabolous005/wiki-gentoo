<!-- source: https://wiki.gentoo.org/wiki/SELinux/bind | group: Gentoo Wiki (Main) | wiki-title: SELinux/bind -->
---
title: SELinux/bind
url: https://wiki.gentoo.org/wiki/SELinux/bind
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2013-05-08"
fingerprint: "91eaba4c6ef37a95"
license: CC BY-SA 4.0
---

# SELinux/bind

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Structure

### Domains

The named\_t domain can only be transitioned towards through the initrc\_t domain (i.e. through init scripts). The ndc\_t domain (for the named domain controller) can be transitioned towards through the initrc\_t and sysadm\_t (general system administration) domains.

### File types/labels

The following table lists the file type/labels defined in the bind module.

| Type | Function | Description | 
|---|---|---|
| named\_exec\_t | Entrypoint | Entrypoint domain for the named binaries | 
| named\_initrc\_exec\_t | Entrypoint | Entrypoint domain for non-Gentoo init scripts | 
| named\_checkconf\_exec\_t | Entrypoint | Entrypoint for the checkconf binary | 
| ndc\_exec\_t | Entrypoint | Entrypoint for the ndc binaries | 
| dnssec\_t | Configuration | Label for the key files used by the named daemon | 
| named\_zone\_t | Configuration | Label for the primary zone files | 
| named\_cache\_t | Configuration | Label for the cached zone files | 
| named\_conf\_t | Configuration | Label for the named configuration files | 
| named\_log\_t | Configuration | Label for the named log files | 
| named\_tmp\_t |  | Label for the named temporary files | 
| named\_var\_run\_t |  | Label for the named runtime variable data | 

## Using the bind SELinux module

### SELinux boolean: named\_write\_master\_zones

The named policy offers one boolean called named\_write\_master\_zones which, when enabled, allows the named daemon to write to its master zone files (i.e. named\_zone\_t). This is used in master/slave setups.
