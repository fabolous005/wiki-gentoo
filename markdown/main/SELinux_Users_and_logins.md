<!-- source: https://wiki.gentoo.org/wiki/SELinux/Users_and_logins | group: Gentoo Wiki (Main) | wiki-title: SELinux/Users and logins -->
---
title: SELinux/Users and logins
url: https://wiki.gentoo.org/wiki/SELinux/Users_and_logins
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2014-05-04"
fingerprint: "26bb2dbb007a41cc"
license: CC BY-SA 4.0
---

# SELinux/Users and logins

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

In an SELinux context, the first part is called the *SELinux user*. The purpose of a SELinux user is to have an *immutable* part in a context (i.e. one that the user cannot change himself) both to assist in auditing (who did what) as well as access controls (users cannot work around SELinux user based restrictions). But a SELinux user is not the same as a Linux account. For that, *user mappings* are put in place that map a Linux user to a SELinux user.

## Introduction

The relation between SELinux users, Linux accounts, SELinux roles and the supported domains is shown in the following drawing.

![SELinux users.png](https://wiki.gentoo.org/images/a/a0/SELinux_users.png)


### SELinux users

A SELinux user is an identifier that administrators can use to limit which SELinux roles can be used. A Linux account is mapped to one (and only one) SELinux user, whereas a SELinux user can be linked to multiple roles. As shown in the drawing above, SELinux users define which roles can be used. As roles define the privileges of a user, this is effectively used to limit the ability of users to run or execute particular applications.

For instance:

- The `user_u` SELinux user linked with the `user_r` role. This role is allowed to run non-administrative applications (the set of supported SELinux domains).
- The `staff_u` SELinux user on the other hand is linked with both the `staff_r` role and `sysadm_r` role. That means that users mapped to the `staff_u` SELinux user can switch between those two roles.

One of the supported domains, `newrole_t`, is meant for the `newrole` application which allows users to switch roles. As the `user_u` SELinux user does not need to switch roles, the `user_r` role is not allowed to use the `newrole_t` domain.

The following table gives an non-exhaustive list of SELinux users which can be found on most SELinux systems.

| SELinux user | Description | Used for | 
|---|---|---|
| unconfined\_u | SELinux user meant for unrestricted users. Unconfined users have hardly any restrictions in a SELinux context and are meant for systems where only Internet-facing services should run confined (i.e. the *targeted* SELinux policy store). | All users on a targeted system | 
| root | The SELinux user meant for the root account | The Linux root account | 
| sysadm\_u | SELinux user with direct system administrative role assigned | Linux accounts that only perform administrative tasks | 
| staff\_u | SELinux user for operators that need to run both non-administrative commands (through the `staff_r` role) and administrative commands (through the `sysadm_r` role). | Linux accounts used for both end user usage as well as administrative tasks | 
| user\_u | SELinux user for non-privileged accounts | Unprivileged Linux accounts | 
| system\_u | Special SELinux user meant for system services | Not used directly | 

The list is definitely not complete though. Some distributions offer additional SELinux users by default, such as `git_shell_u`, `guest_u` and `xguest_u`. As the purpose of SELinux users is to restrict what a user can do, the purpose of these specific SELinux users can usually be retrieved from their name - and if not, by querying the roles that they are linked with.

Next to the role mapping, SELinux users can also define the highest clearance of all users mapped to the SELinux user. Linux users cannot gain a higher security clearance than is set for the SELinux user (but they can be assigned a lower clearance).

### SELinux login mappings

SELinux login mappings allow for the Linux administrator to map a Linux account to a SELinux user. When a Linux user logs in, a user table is consulted which contains the mappings of users to SELinux users. Based on that information, the initial context is set up for the user (which also contains the SELinux role and user domain). Unlike SELinux users, which can be linked with multiple SELinux roles, user mappings map a user to one (and only one) SELinux user. Multiple Linux users can be mapped to the same SELinux user though.

On a targeted system, all users are mapped to the `unconfined_u` SELinux user. Otherwise, administrative users are mapped to either `staff_u` or `sysadm_u` whereas unprivileged users are mapped to `user_u`.

On MLS-enabled systems, login mappings can also provide a lower security clearance for individual users, even though the SELinux user has a higher clearance set. Once a clearance is set for a particular user, this user cannot gain a higher clearance - so the clearance of the SELinux user is not a reachable set, but should be seen as a "default" value for login mappings if no clearance is given to the mapping itself.

## Managing login mappings

Most administrators will not need to manage SELinux users, but the Linux mappings will be an important table to maintain.

### Listing current mappings

With `semanage login`, the current mappings can be shown.

`root #``semanage login -l`
Login Name                SELinux User             
  
\_\_default\_\_               user\_u                   
root                      root                     
swift                     staff\_u                  
system\_u                  system\_u

If the system supports MLS, then the sensitivity / security clearance of the users is also shown.

`root #``semanage login -l`
Login Name           SELinux User         MLS/MCS Range        Service
  
\_\_default\_\_          user\_u               s0                   \*
oper                 staff\_u              s0-s0:c0.c1023       \*
root                 root                 s0-s0:c0.c1023       \*
system\_u             system\_u             s0-s0:c0.c1023       \*
test                 user\_u               s0                   \*
user                 user\_u               s0                   \*

The `__default__` mapping is a special, fall-back mapping. When no particular mapping matches, then the `__default__` mapping is used.

Also, if a login name starts with the `%` sign, then it will match group membership. For instance, a mapping `%users` mapping will match all Linux users whose primary group is the `users` group.

### Creating a new mapping

To create a new user mapping, use `semanage login`.

For instance, to map the Linux user *darcia* to the `staff_u` SELinux user:

`root #``semanage login -a -s staff_u darcia`
On MLS-enabled systems, a security clearance can be passed on as well:

`root #``semanage login -a -s staff_u -r s0-s0:c0.c100 darcia`
### Modifying an existing mapping

Modification of existing mappings is also done with `semanage login`.

For instance, to modify the mapping for user *darcia* to now be mapped with the `sysadm_u` user:

`root #``semanage login -m -s sysadm_u darcia`
Or to modify the security clearance on an MLS system:

`root #``semanage login -m -r s0-s0:c0.c20 darcia`
If the SELinux user for a user has been modified, it is important to relabel the files of that user using the `--force` (`-F`) argument:

`root #``restorecon -RF /home/darcia`
Also have the user log out before modifying and resetting the contexts.

### Deleting a mapping

It is possible to delete a mapping so that the user falls back to the `__default__` mapping:

`root #``semanage login -d darcia`
If this results in a change of SELinux user, do not forget to relabel the files of the user:

`root #``restorecon -RF /home/darcia`
## Managing SELinux users

Management of SELinux users is done using `semanage user`.

### Listing SELinux users

List the current set of SELinux users with `semanage user -l`:

`root #``semanage user -l`
SELinux User    SELinux Roles
  
root            staff\_r sysadm\_r
staff\_u         staff\_r sysadm\_r
sysadm\_u        sysadm\_r
system\_u        system\_r
unconfined\_u    unconfined\_r
user\_u          user\_r

On MLS-enabled systems, the sensitivity and the security clearance is listed as well:

`root #``semanage user -l`
Labeling   MLS/       MLS/                          
SELinux User    Prefix     MCS Level  MCS Range                      SELinux Roles
  
root            sysadm     s0         s0-s0:c0.c1023                 staff\_r sysadm\_r
staff\_u         staff      s0         s0-s0:c0.c1023                 staff\_r sysadm\_r
sysadm\_u        sysadm     s0         s0-s0:c0.c1023                 sysadm\_r
system\_u        user       s0         s0-s0:c0.c1023                 system\_r
unconfined\_u    unconfined s0         s0-s0:c0.c1023                 unconfined\_r
user\_u          user       s0         s0                             user\_r

### Creating custom SELinux users

It is possible to create additional SELinux users with `semanage user`.

For instance, to create a SELinux user `swift_u` with access to the `staff_r` and `dbadm_r` roles:

`root #``semanage user -a -R "staff_r dbadm_r" swift_u`
On MLS-enabled systems, the default clearance for the SELinux user can also be provided:

`root #``semanage user -a -R "staff_r dbadm_r" -r s0-s0:c0.c100,c201 swift_u`
### Updating custom SELinux users

SELinux user settings can be modified using `semanage user`.

For instance, to update the role set of the `swift_u` SELinux user to become `staff_r` `dbadm_r` and `webadm_r`:

`root #``semanage user -m -R "staff_r dbadm_r webadm_r" swift_u`
The MLS clearance can be modified as well.

`root #``semanage user -m -r s0-s0:c0.c50 swift_u`
### Removing custom SELinux users

Custom SELinux users can be removed using `semanage user`.

For instance, to remove the `swift_u` user:

`root #``semanage user -d swift_u`
Make sure that all mappings towards this SELinux user are updated before removing the SELinux user itself.

Also relabel all resources that might currently be "owned" by this SELinux user so that their SELinux user is not invalidated.

## Summary

By supporting SELinux users and login mappings, additional constraints on users' permissions can be set.
