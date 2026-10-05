<!-- source: https://wiki.gentoo.org/wiki/SELinux/munin | group: Gentoo Wiki (Main) | wiki-title: SELinux/munin -->
---
title: SELinux/munin
url: https://wiki.gentoo.org/wiki/SELinux/munin
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-03-29"
fingerprint: "2c6d1c5e48562791"
license: CC BY-SA 4.0
---

# SELinux/munin

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

# DESCRIPTION

The *munin* SELinux module supports the Munin networked resource management tool.

# DOMAINS

The following is a list of munin related domains.

- munin\_t
- is the main domain for the munin daemon
- ‘\*’\_munin\_plugin\_t
- is a set of domains related to the munin plugins

# LOCATIONS

The following list of locations identify file resources that are used by the munin domains. They are by default allocated towards the default locations for munin, so if you use a different location, you will need to properly address this. You can do so through `semanage`, like so:

`root #``semanage fcontext -a -t system_cron_spool_t "/usr/local/share/munin/plugins(/.*)?"`
The above example marks the */usr/local/share/munin/plugins* location as the location where munin plugin executables are stored.

## FUNCTIONAL

- munin\_etc\_t
- is used for the munin configuration files

## EXECUTABLES

- munin\_exec\_t
- is used for the munin binaries
- munin\_initrc\_exec\_t
- is used for the munin init script
- ‘\*’\_munin\_plugin\_exec\_t
- is used for the munin plugin executables

## DAEMON FILES

- munin\_log\_t
- is used for the munin logs
- munin\_plugin\_state\_t
- is used for the munin plugin state information
- munin\_var\_lib\_t
- is used for the variable information used by munin
- munin\_var\_run\_t
- is used for the variable runtime state information of munin

# POLICY

The following interfaces can be used to enhance the default policy with munin-related provileges. More details on these interfaces can be found in the interface HTML documentation, we will not list all available interfaces here.

## Plugin template

With the `munin_plugin_template` interface, additional munin plugin domains can be created. The interface takes a single prefix (like “disk”) and will create the proper types and privileges, including (using “disk” as the example):

- *disk\_munin\_plugin\_t* as plugin domain
- *disk\_munin\_plugin\_exec\_t* as plugin executable type
- *disk\_munin\_plugin\_tmp\_t* as plugin temporary file type

To enable it:

munin\_plugin\_template(disk)

## Administrative role

The `munin_admin` interface grants a user role and type administrative access to the munin types:

munin\_admin(myuser\_t, myuser\_r)

# BUGS

## Munin

The `net-analyzer/munin` package deploys the munin cronjobs as end user cronjobs inside `/var/spool/cron/crontabs`. The munin cronjobs are meant to be executed as the munin Linux account, but the jobs themselves are best seen as system cronjobs (as they are not related to a true interactive end user).

The default deployed files might not get the *system\_u* SELinux ownership assigned. To fix this, execute the following command:

`root #``chcon -u system_u /var/spool/cron/crontabs/munin`
For more information, see [bug #526532](https://bugs.gentoo.org/show_bug.cgi?id=526532).

# See also

- [SELinux](https://wiki.gentoo.org/wiki/SELinux) — a mandatory access control system which enables a more fine-grained mechanism permitting the security administrator to define user privileges.
- [Project:Hardened](https://wiki.gentoo.org/wiki/Project:Hardened)
