<!-- source: https://wiki.gentoo.org/wiki/Audit | group: Gentoo Wiki (Main) | wiki-title: Audit -->
---
title: Audit
url: https://wiki.gentoo.org/wiki/Audit
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-01-07"
fingerprint: e8deb71c17d3b9a6
license: CC BY-SA 4.0
---

# Audit

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The Linux Audit System is designed to make Linux compliant with the requirements from Common Criteria, PCI-DSS, and other security standards by intercepting system calls and serializing audit log entries from privileged user space applications. The framework allows the configured events to be recorded to disk and distributed to plugins in realtime. Each audit event contains the date and time of event, type of event, subject identity, object acted upon, and result (success/fail) of the action if applicable.

## Installation

### USE flags


### USE flags for
            [sys-process/audit](https://packages.gentoo.org/packages/sys-process/audit)
            
            Userspace utilities for storing and processing auditing records

| [build](https://packages.gentoo.org/useflags/build) | !!internal use only!! DO NOT SET THIS FLAG YOURSELF!, used for creating build images and the first half of bootstrapping \[make stage1\] | 
| [gssapi](https://packages.gentoo.org/useflags/gssapi) | Enable GSSAPI support | 
| [io-uring](https://packages.gentoo.org/useflags/io-uring) | Enable the use of io\_uring for efficient asynchronous IO and system requests | 
| [ldap](https://packages.gentoo.org/useflags/ldap) | Add LDAP support (Lightweight Directory Access Protocol) | 
| [python](https://packages.gentoo.org/useflags/python) | Add optional support/bindings for the Python language | 
| [split-usr](https://packages.gentoo.org/useflags/split-usr) | Enable behavior to support maintaining /bin, /lib\*, /sbin and /usr/sbin separately from /usr/bin and /usr/lib\* | 
| [ssl](https://packages.gentoo.org/useflags/ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 
| [static-libs](https://packages.gentoo.org/useflags/static-libs) | Build static versions of dynamic libraries as well | 

### Emerge

`root #``emerge --ask sys-process/audit`
## Usage

### Daemon

To start the daemon for OpenRC systems, run:

`root #``rc-service auditd start``root #``rc-update add auditd default`
For systemd systems:

`root #``systemctl enable --now auditd`
