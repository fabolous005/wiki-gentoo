<!-- source: https://wiki.gentoo.org/wiki/Ejabberd | group: Gentoo Wiki (Main) | wiki-title: Ejabberd -->
---
title: ejabberd
url: https://wiki.gentoo.org/wiki/Ejabberd
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-08-10"
fingerprint: ed10b15cd98e79e6
license: CC BY-SA 4.0
---

# ejabberd

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**ejabberd** is an open source, multi-platform, XMPP application server and MQTT broker, written mainly in Erlang.

## Installation

### USE flags


| [+stun](https://packages.gentoo.org/useflags/+stun) | Enable STUN/TURN support | 
| [captcha](https://packages.gentoo.org/useflags/captcha) | Support for CAPTCHA Forms (XEP-158) on registration | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [full-xml](https://packages.gentoo.org/useflags/full-xml) | Use XML features in XMPP stream (ex: CDATA), requires XML compliant clients | 
| [ldap](https://packages.gentoo.org/useflags/ldap) | Add LDAP support (Lightweight Directory Access Protocol) | 
| [mssql](https://packages.gentoo.org/useflags/mssql) | Enable Microsoft SQL Server support (via ODBC) for data storage | 
| [mysql](https://packages.gentoo.org/useflags/mysql) | Enable MySQL support for data storage | 
| [odbc](https://packages.gentoo.org/useflags/odbc) | Enable ODBC support to access data storage | 
| [pam](https://packages.gentoo.org/useflags/pam) | Add support for PAM (Pluggable Authentication Modules) - DANGEROUS to arbitrarily flip | 
| [postgres](https://packages.gentoo.org/useflags/postgres) | Enable PostgreSQL support for data storage | 
| [redis](https://packages.gentoo.org/useflags/redis) | Enable Redis support for transient data | 
| [roster-gw](https://packages.gentoo.org/useflags/roster-gw) | Turn on workaround for processing gateway subscriptions | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [sip](https://packages.gentoo.org/useflags/sip) | Enable SIP support | 
| [sqlite](https://packages.gentoo.org/useflags/sqlite) | Enable SQLite database support | 
| [zlib](https://packages.gentoo.org/useflags/zlib) | Enable Stream Compression (XEP-0138) using zlib | 

### Emerge

Install ejabberd:

`root #``emerge --ask net-im/ejabberd`
Adding various modules through USE flags will trigger things like: [dev-lang/erlang](https://packages.gentoo.org/packages/dev-lang/erlang), [net-im/ejabberd](https://packages.gentoo.org/packages/net-im/ejabberd), etc. to be installed with ejabberd.

## Configuration

### Files

In /etc/jabber/ejabberd.cfg put:

**`/etc/jabber/ejabberd.cfg`**

And:

**`/etc/jabber/ejabberd.cfg`**

Where foo.bar is what is required for the accounts, like bob@foo.bar (so the server should be available at foo.bar. If not, clientside configuration needs extra server parameter).

In /etc/jabber/ejabberctl.cfg put:

**`/etc/jabber/ejabberctl.cfg`**

So the node will be called ejabberd@süpercomputer while süpercomputer is the one configured in /etc/conf.d/hostname If this is changed, remember to issue:

`root #``rc-service hostname restart`
### Service

#### OpenRC

Then start:

`root #``rc-service ejabberd start`
Then create users:

`user $``ejabberdctl register {name} {domain} {password}`
For example:

`user $``ejabberdctl register bob foo.bar süpersecret`
### Set up a jabber server using ejabberd

This often fails at first try, because the whole ejabberd-erlang-mnesia thing can be really picky sometimes. So, one hint may be to not initialize/start/test anything until the final hostname selections are in every config file. Changing hostname afterwards can cause problems, at least before becoming familiar with the above mentioned tools.

Second hint: If errors are encountered when restarting here, Erlang nodes might have to be stopped, which unfortunately are not called 'erlang' or something, but 'beam', so this might be found useful:

`user $``killall beam -9`
