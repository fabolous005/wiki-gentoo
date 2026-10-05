<!-- source: https://wiki.gentoo.org/wiki/SELinux/cron | group: Gentoo Wiki (Main) | wiki-title: SELinux/cron -->
---
title: SELinux/cron
url: https://wiki.gentoo.org/wiki/SELinux/cron
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-03-29"
fingerprint: "2588bd5f4cd313c4"
license: CC BY-SA 4.0
---

# SELinux/cron

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Description

The **cron** SELinux module supports various Unix cron daemons, including (but not limited to) vixie-cron, cronie, fcron and anacron.

The SELinux cron support is somewhat more complex than most other SELinux domains, because the cron daemon is responsible for executing workload in the context of end users as well as the overall system. Most Cron implementations are also SELinux-aware, so having some understanding of how they operate is important.

Most of these cron implementations use the SELinux ownership of the crontab file (the file which contains the execution task definitions) to determine in which context a task is to be executed. For instance, if a crontab file installed in `/var/spool/cron/crontabs` has a SELinux context whose SELinux owner is *staff\_u*, then the tasks defined in it will be run through either the general cronjob domain (*cronjob\_t*) or the end user domain (*staff\_t*) depending on the value of the *cron\_userdomain\_transition* boolean.

This boolean, if set to 1 (true), will have the tasks run in the user domain (such as *staff\_t*, *sysadm\_t*, *unconfined\_t*, etc.) whereas, if it is set to 0 (false), will have the tasks run in the general cronjob domain (*cronjob\_t*) for end user tasks, or the system cronjob domain (*system\_cronjob\_t*) for system tasks.

The latter is also an important detail - if for some reason packages deploy their tasks as end user cronjobs, then the resulting commands might not be running in the proper domain. As a general rule, system cronjobs are defined in either `/etc/crontab` or in files in the `/etc/cron.d` directory. End user cronjobs are defined in files in the `/var/spool/cron/crontabs` directory.

### System administration

To perform system administration tasks (non-end user tasks) through cron jobs, take the following considerations into account:

- To ensure that the jobs run in the right context (*system\_cronjob\_t* for starts), make sure that the cronjob definitions (the crontab files) are inside `/etc/crontab` or in the `/etc/cron.d` directories.
- Have the scripts to be executed labeled properly, and consider using a domain transition for these scripts (through `cron_system_entry()`).
- Make sure the `HOME` directory is set to `/` so that the target domains do not need any privileges inside end user locations (including `/root`).

### User cronjobs

When working with end user crontabs (those triggered / managed through the **crontab** command), take care that this is done as the SELinux user which is associated with the file. This is for two reasons:

1. If `USE="ubac"` is set, then the SELinux User Based Access Control is enabled. This could prevent one SELinux user from editing (or even viewing) the crontab files of another user.
2. The owner of the crontab file is also used by most cron implementations to find out which context the user cronjob should run in. If this ownership is incorrect, then the cronjob might not even launch properly, or run in the wrong context.

If this was not done correctly, you will get the following error:

cron\[20642\]: (root) ENTRYPOINT FAILED (crontabs/root)

If the above error still comes up even though the ownership of the `crontab` file is correct, then check the state of the *cron\_userdomain\_transition* boolean and the `default_contexts` file. If the boolean is set to true, then the `default_contexts` file (or the user-specific files in the `users/` directory) should target the user domains instead of the cronjob domains:

`root #``getsebool cron_userdomain_transition`
cron\_userdomain\_transition --> on

`root #``grep crond_t /etc/selinux/*/contexts/{default_contexts,users/*}`
system\_r:crond\_t:s0   user\_r:user\_t staff\_r:staff\_t sysadm\_r:sysadm\_t

Remember that the default context definitions in the `users/` directory take priority over the ones defined in the `default_contexts` files.

## Booleans

The following booleans are defined through the **cron** SELinux policy module. They can be toggled using `setsebool`, like so:

`root #``setsebool -P cron_userdomain_transition on`
- cron\_can\_relabel
- Allow system cron jobs to relabel files on the file system (and restore the context of files). This privilege is assigned to the *system\_cronjob\_t* domain.
- cron\_userdomain\_transition
- If enabled, end user cron jobs run in their default associated user domain (such as *user\_t* or *unconfined\_t*) instead of the general end user cronjob domain (*cronjob\_t*).
This also requires that the `default_contexts` file (inside `/etc/selinux/*/contexts`) is updated accordingly, mentioning that the target contexts are now the user domains rather than the cronjob domains.
- fcron\_crond
- Enable additional SELinux policy rules needed for the fcron cron implementation.

## Domains

### crond\_t

The main cron domain is *crond\_t*, used by the cron daemon. It is generally responsible for initiating the cronjob tasks, detecting changes on the crontab files and reloading the configuration if that happens.

Almost all cron implementations are launched through their respective init script.

Some cron implementations which are not SELinux-aware might have the cronjobs themselves also run through the *crond\_t* domain.

### cronjob\_t

The *cronjob\_t* domain is used for end user generic cronjobs.

### system\_cronjob\_t

The *system\_cronjob\_t* domain is used for system cronjobs.

### crontab\_t

The *crontab\_t* domain is used by end users’ **crontab** execution (the command used to manipulate end user crontab files).

### admin\_crontab\_t

The *admin\_crontab\_t* domain is used by administrators4 **crontab** execution (the command used to manipulate crontab files).

## Locations

The following list of locations identify file resources that are used by the cron domains. They are by default allocated towards the default locations for cron, so if you use a different location, you will need to properly address this. You can do so through `semanage`, like so:

`root #``semanage fcontext -a -t system_cron_spool_t "/usr/local/etc/cron\.d(/.*)?"`
The above example marks the */usr/local/etc/cron.d* location as the location where system cronjob definitions are stored.

### Functional

- cron\_spool\_t
- is used for the end user cronjob definition files
- sysadm\_cron\_spool\_t
- is used for the administrator cronjob definition files
- system\_cron\_spool\_t
- is used for the system cronjob definition files

### Executables

- anacron\_exec\_t
- is used for the **anacron** binary
- crond\_exec\_t
- is used for the cron daemon binary
- crond\_initrc\_exec\_t
- is used for the cron init script (such as `/etc/init.d/crond`)
- crontab\_exec\_t
- is used for the **crontab** binary

### Daemon files

- cron\_log\_t
- is used for the cron log files
- cron\_var\_lib\_t
- is used for the variable state information of the cron daemon
- crond\_tmp\_t
- is used for the temporary files created/managed by the cron daemon
- crond\_var\_run\_t
- is used for the variable runtime information of the cron daemon

## Policy

The following interfaces can be used to enhance the default policy with cron-related provileges. More details on these interfaces can be found in the interface HTML documentation, we will not list all available interfaces here.

### Domain interaction

The most interesting definition in the policy is the `cron_system_entry` interface. It allows for the system cronjob domain (*system\_cronjob\_t*) to execute a particular type (second argument) and transition to a given domain (first argument).

For instance, to allow a system cronjob to execute any portage commands:

cron\_system\_entry(portage\_t, portage\_exec\_t)

It is generally preferred to transition a system cron job as fast as possible to a specific domain rather than enhancing the *system\_cronjob\_t* with additional privileges.

### Role interfaces

The following role interfaces allow users and roles access to the specified domains. Only to be used for user domains and roles.

- cron\_role
- is used to allow users and roles access to the cron related domains. This one should be used for end users, not administrators. For instance: cron\_role(myuser\_r, myuser\_t)
- cron\_admin\_role
- is used to allow users and roles administrative access to the cron related domains. For instance: cron\_admin\_role(myuser\_r, myuser\_t)

## Bugs

### Munin

The [net-analyzer/munin](https://packages.gentoo.org/packages/net-analyzer/munin) package deploys the munin cronjobs as end user cronjobs inside `/var/spool/cron/crontabs`. The munin cronjobs are meant to be executed as the munin Linux account, but the jobs themselves are best seen as system cronjobs (as they are not related to a true interactive end user).

The default deployed files might not get the *system\_u* SELinux ownership assigned. To fix this, execute the following command:

`root #``chcon -u system_u /var/spool/cron/crontabs/munin`
For more information, see [bug #526532](https://bugs.gentoo.org/show_bug.cgi?id=526532).

## See also

- [SELinux](https://wiki.gentoo.org/wiki/SELinux) — a mandatory access control system which enables a more fine-grained mechanism permitting the security administrator to define user privileges.
- [Project:Hardened](https://wiki.gentoo.org/wiki/Project:Hardened)
