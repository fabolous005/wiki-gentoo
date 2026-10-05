<!-- source: https://wiki.gentoo.org/wiki/SELinux/Logging | group: Gentoo Wiki (Main) | wiki-title: SELinux/Logging -->
---
title: SELinux/Logging
url: https://wiki.gentoo.org/wiki/SELinux/Logging
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "6c7edda8295e4145"
license: CC BY-SA 4.0
---

# SELinux/Logging

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

When SELinux denies a particular activity, it will usually log this through the audit subsystem or, if auditing is disabled, through the kernel logging. *Usually*, because SELinux policy developers can tell the SELinux subsystem not to log a particular denial. But even those settings can be overruled by the system administrator to have SELinux log *all* denials.

## Introduction

SELinux bases its decisions (allow or deny) on policy rules. Whenever a particular activity is performed, its *access vectors* is taken by SELinux and checked with the *access vector cache* (AVC). This cache contains the access vectors together with the allow/deny state. If the access vector is allowed, then the operation can continue. If not, then the operation is denied. If there is no specific `dontaudit` statement active, then an AVC denial will be logged in the audit subsystem.

This additional logging is extremely important to debug permission issues as "just" *Permission Denied* does not help us much. The denial logging contains a vast majority of information that administrators can use to troubleshoot permission issues further.

### Format of an AVC denial

The following is an example AVC denial:

The structure of a denial depends on the denial type itself. The above denial is for file access, the following one is for a capability:

The most important part of the denial is the permission (between `{ ... }`), class (as referenced by the `tclass=` parameter) and contexts (`scontext` for the source context, and `tcontext` for the target context).

### Reasons for denials

A denial is due to the SELinux policy. However, it is not always due to a missing access vector rule. For instance, the following denial might occur, even when a proper access vector rule is available for it:

`root #``sesearch -s user_t -t user_home_dir_t -c dir -p getattr -Ad````
Found 1 semantic av rules:
   allow user_t user_home_dir_t : dir { ioctl read write create getattr setattr lock relabelfrom relabelto unlink link rename add_name remove_name reparent search rmdir open } ;
```
In the above case, the denial came from a feature called *User-Based Access Control*, where resources of one [SELinux user](https://wiki.gentoo.org/wiki/SELinux/Users_and_logins) (`staff_u`) are not accessible for processes of another SELinux user (`oper_u`).

Such a feature is implemented through SELinux constraints, which not only focuses on the types in a context (`user_t` and `user_home_dir_t`) but also the relation of SELinux users and SELinux roles.

In the vast majority of cases though, denials are due to missing access vector rules.

## Audit logs

### auditd

To save audit logs to /var/log/audit/audit.log and be able to use user-space utilities (such as `ausearch`), it is necessary to install the [sys-process/audit](https://packages.gentoo.org/packages/sys-process/audit) package and enable `auditd`.

### Listing AVC denials

To view the AVC denials through the audit subsystem, use `ausearch`:

`root #``ausearch -m avc -ts recent`
The `-ts` argument takes a time stamp or word indicating how far into the past to search for AVC denials. In the above example, the word `recent` is used which means 10 minutes ago. Another helpful word is `boot` which means since the system was last booted.

The audit logs are usually also readable at /var/log/audit but the time stamp displayed in the logs will need to be manually converted in that case (as it is not localized).

### Clearing the audit logs

It is not recommended to clear the audit logs as they might contain information needed in the future for troubleshooting or security investigations. However, if that is not the case, just empty the audit log:

`root #``> /var/log/audit/audit.log`
## Managing SELinux denial logging

### Disabling dontaudit statements

It is possible to rebuild the SELinux policy and disable the `dontaudit` statements. These statements are put in the policy by the SELinux policy developers to hide particular denials from the regular audit reporting, as the policy developer believes that the denial is cosmetic and can be ignored.

To rebuild and disable:

`root #``semodule -DB`
To re-enable the `dontaudit` statements, just rebuild the policy again:

`root #``semodule -B`
### Allowing an access vector

It is possible to allow a particular access vector by enhancing the currently loaded SELinux policy.

A rule can be added using the (Gentoo-only) `selocal` script:

`root #``selocal -a "allow mutt_t mutt_etc_t:lnk_file read_file_perms;" -Lb`
Another possibility is to use `audit2allow` to build a SELinux policy module based on the audit events:

`root #````
ausearch -m avc -ts recent | audit2allow -M myupdates
```
`root #``semodule -i myupdates.pp`
