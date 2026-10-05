<!-- source: https://wiki.gentoo.org/wiki/Monit | group: Gentoo Wiki (Main) | wiki-title: Monit -->
---
title: Monit
url: https://wiki.gentoo.org/wiki/Monit
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-07-04"
fingerprint: "8a13dd2a79d339bc"
license: CC BY-SA 4.0
---

# Monit

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**monit** is a utility for managing and monitoring processes, programs, files, directories and filesystems on a UNIX system.

## Configuration

### Installing monit

The [app-admin/monit](https://packages.gentoo.org/packages/app-admin/monit) application has the following USE flags:


| [pam](https://packages.gentoo.org/useflags/pam) | Add support for PAM (Pluggable Authentication Modules) - DANGEROUS to arbitrarily flip | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [ssl](https://packages.gentoo.org/useflags/ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 

Once the USE flags are properly determined, install [app-admin/monit](https://packages.gentoo.org/packages/app-admin/monit) through emerge:

`root #``emerge --ask app-admin/monit`
### Monit configuration files

The Monit application uses /etc/monitrc as its configuration file.

To make adding and removing monitoring definitions easy, monit supports including files inside a specified directory (usually /etc/monit.d. To enable this, edit /etc/monitrc like so:

**`/etc/monitrc`**

**Allowing flexible configuration entries**

When a Monit related configuration file is altered, tell monit to reread its configuration settings:

`root #``monit reload`
### Automatically starting monit at boot

It is recommended to start monit through the /etc/inittab so that init itself launches the monit application, and will automatically relaunch it when monit would suddenly die. Starting monit through an init script would not provide this functionality.

**`/etc/inittab`**

**Auto restart monit in case of failure**

After updating /etc/inittab, monit can be immediately started through telinit q.

### User management

Users added to the monit or users group will be able to manipulate monit through its web interface.

To add users to one of these groups, use gpasswd (note, replace `${LOGNAME}` by the user's actual login name):

`root #````
gpasswd -a ${LOGNAME} monit
```
`root #``gpasswd -a ${LOGNAME} users`
Inside the /etc/monitrc file, the `allow` statement should refer to these groups, like so:

**`/etc/monitrc`**

**Granting groups access to the web interface**

It is also possible to hard-code usernames and passwords in the monitrc file, but this is not recommended. Check the monitrc file for default passwords and remove those, or alter them to use a strong, unique password. The syntax used then is `allow <username>:<password>`.

### Monit web interface

The default location of the web interface is at [localhost:2812](http://localhost:2812), with `admin` as admin username and `monit` as default password. Make sure to change this!

## Monitoring applications through monit

The Monit application uses PID file checks to see if an application is still running or not. That implies that a PID file *must* be available for an application, otherwise monit cannot guard it. If a daemon does not create a PID file, use a [wrapper](https://mmonit.com/wiki/Monit/FAQ#pidfile) to create one.

Through using the /etc/monit.d/ location, it is easy to add in additional monitoring rules.

For instance, to automatically restart [MySQL](https://wiki.gentoo.org/wiki/MySQL) when it would die:

**`/etc/monit.d/mysql`**

**Auto restart mysql**

Another example is to manage the memory usage of a process and create an alert when it grows beyond a certain threshold:

**`/etc/monit.d/squid`**

**Check squid and alert on memory consumption bigger than 512 MByte**

## Debugging monit

### Running monit in the foreground

To run monit in the foreground and provide feedback on everything it is detecting, use the `-Ivv` option:

`root #``monit -Ivv`
...
'squid' total mem amount of 525748kB matches resource limit \[total mem amount>524288kB\]

## External resources

For more information about Monit, the following resources can help out.
