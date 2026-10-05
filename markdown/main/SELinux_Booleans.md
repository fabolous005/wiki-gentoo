<!-- source: https://wiki.gentoo.org/wiki/SELinux/Booleans | group: Gentoo Wiki (Main) | wiki-title: SELinux/Booleans -->
---
title: SELinux/Booleans
url: https://wiki.gentoo.org/wiki/SELinux/Booleans
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-07"
fingerprint: a6ccc9af67070b4b
license: CC BY-SA 4.0
---

# SELinux/Booleans

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

SELinux booleans are configurable settings, like sysctls, which enable or disable additional policy controls. Unlike sysctls though, booleans can only be enabled (on) or disabled (off).

## Introduction

As a SELinux policy defines the acceptable behavior of the processes on a system, having a single, static policy would make it almost impossible to have a single policy set that is usable for the majority of systems. Some systems might want to have the web server be able to access a database, while others don't. Considering all possible use cases this would result in a few hundreds of thousands of policies.

### Toggling code on or off

To support a more flexible policy management approach, *SELinux booleans* came to live. Administrators can set particular booleans through commands such as `setsebool` or `semanage boolean` to enable or disable SELinux policies.

In the SELinux policy code, a boolean is called a *tunable* and is managing additional SELinux policy rules like so:

**`ssh.te`**

**Using the use\_nfs\_home\_dirs boolean**

In this example, two policy macros (which are interfaces that group one or more SELinux policy rules) are enabled when the `use_nfs_home_dirs` boolean is set.

An administrator who enables NFS home directories on his systems will need to set `use_nfs_home_dirs` to *on*.

### Permanently storing boolean values

When an administrator enables or disables a SELinux boolean, this can be done just during the session or permanently.

If the policy setting is not persisted, then the policy changes are not persisted and upon the next system boot the previous configuration is used again. This allows administrators to either temporarily enable policy settings (and also disable it later on) or try out policy changes that have the potential of rendering the system unusable - at which a reboot fixes things again.

## Managing booleans

Booleans are provided by the SELinux policy. Once a policy is loaded, policies can be queried and toggled.

### Listing booleans

The list of currently defined booleans can be obtained through `getsebool`.

`root #``getsebool -a`
allow\_execheap --> off
allow\_execmem --> off
allow\_execmod --> off
...

The same can be done using `semanage boolean`, which also displays some additional information about each boolean.

`root #``semanage boolean -l`
SELinux boolean                State  Default Description
  
xdm\_sysadm\_login               (off  ,  off)  Allow xdm logins as sysadm
chromium\_read\_generic\_user\_content (on   ,   on)  Allow chromium to read generic user content (i.e. content that is not specific to an application).
...

### Getting individual boolean information

To get information about an individual boolean, use `getsebool`.

`root #``getsebool allow_execheap`
allow\_execheap --> off

Of course, one can also `grep` the results from `semanage boolean`:

`root #``semanage boolean -l | grep allow_execheap`
allow\_execheap                 (off  ,  off)  Allow unconfined executables to make their heap memory executable.  Doing this is a really bad idea. Probably indicates a badly coded executable, but could indicate an attack. This executable should be reported in bugzilla

### Toggling booleans

To set a boolean, the `setsebool` command can be used.

`root #``setsebool user_dmesg on`
This will however not persist the setting. To persist it, add the `-P` argument.

`root #``setsebool -P user_dmesg on`
Similarly, `semanage boolean` can be used (which also persist the change):

`root #``semanage boolean -m --on user_dmesg`
### Viewing affected policy rules

To view the policy rules that are enabled (or disabled) when a boolean is set (or unset), use `sesearch`:

`root #``sesearch -b user_dmesg -A`
Found 4 semantic av rules:
ET allow user\_t kernel\_t : system syslog\_read ; \[ user\_dmesg \]
ET allow user\_t user\_t : capability2 syslog ; \[ user\_dmesg \]
ET allow staff\_t kernel\_t : system syslog\_read ; \[ user\_dmesg \]
ET allow staff\_t staff\_t : capability2 syslog ; \[ user\_dmesg \]

In the above example, `user_dmesg` is currently enabled. As a result, all four rules are shown as *enabled* as well (E). The second character (T) tells us that the rule becomes active if the boolean is enabled (T stands for True).

A different example:

`root #``sesearch -b cron_can_relabel -A````
...
EF allow system_cronjob_t etc_t : dir { getattr search open } ; [ cron_can_relabel ]
...
```
In this example, the rule is enabled currently, which is due to `cron_can_relabel` being unset.
