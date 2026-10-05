<!-- source: https://wiki.gentoo.org/wiki/SELinux/Tutorials/Defining_SELinux_users | group: Gentoo Wiki (Main) | wiki-title: SELinux/Tutorials/Defining SELinux users -->
---
title: SELinux/Tutorials/Defining SELinux users
url: https://wiki.gentoo.org/wiki/SELinux/Tutorials/Defining_SELinux_users
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-06-23"
fingerprint: e6b90daf00da4140
license: CC BY-SA 4.0
---

# SELinux/Tutorials/Defining SELinux users

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Defining SELinux users

In the previous tutorial, we briefly touched SELinux users as to explain how SELinux roles work. In this tutorial, we're going to focus on the SELinux user aspect itself.

### The SELinux user part in the context

If we go back to the current user context, then we already know that the second field is the role and the third field the domain.

`user $``id -Z`
user\_u:user\_r:user\_t

You have probably already guessed that the first field is the SELinux user.

Just like with the SELinux role (which defines what the possible domains are), it is the SELinux user that defines what the possible roles are.

#### Roles assigned to an SELinux user

The roles that are assigned to a particular SELinux user can be seen from **semanage user -l**.

`root #``semanage user -l`
SELinux User    SELinux Roles
root            staff\_r sysadm\_r
staff\_u         staff\_r sysadm\_r
sysadm\_u        sysadm\_r
system\_u        system\_r
unconfined\_u    unconfined\_r
user\_u          user\_r

But unlike with the roles and their influence on the domains, we *can* change which roles are assigned to what SELinux user. We'll get to that later in this tutorial, let's first look at the difference between a user account and an SELinux user.

#### Linux accounts versus SELinux users

It is important to know that an SELinux user is not a Linux account or vice versa. Linux accounts are mapped to SELinux users, most of the time in a many-to-few relationship. In other words, many Linux accounts will, if they are logged on to a system, be running as the same SELinux user. This is not mandatory though - you can create separate SELinux accounts for each Linux user, but if all these users have similar rights, it would only incur overhead.

You can see the mapping of Linux accounts to SELinux users using **semanage login -l**.

`root #``semanage login -l`
Login Name                SELinux User             
\_\_default\_\_               user\_u
root                      root
swift                     staff\_u
system\_u                  system\_u

In the above example, there is

- the *root* user, which is mapped to the *root* SELinux user
- the *swift* account, which is mapped to the *staff\_u* SELinux user
- the special *system\_u* account (which doesn't exist - it is a special definition for system daemons)
- the special *\_\_default\_\_* account (which doesn't exist) which plays the role of the catch-all account

The catch-all account *\_\_default\_\_* here ensures that all newly created Linux account users are mapped to the (unprivileged) *user\_u* SELinux user.

If you notice any *Login Name* that starts with the percentage sign (%), then it means that it is not about the Linux account, but the primary group. For instance:

`root #``semanage login -l | grep oper`
%operators                staff\_u

In the above case, all users whose primary group is the *operators* group will be mapped to the *staff\_u* user.

### Manipulating SELinux user information

As mentioned before, we *can* update this information - both the mapping towards SELinux user as the associated roles with an SELinux user.

#### Managing login mappings

First of all, to manage login mappings, we use the **semanage login** set of commands.

For instance, to create the *operators* group mapping:

`root #``semanage login -a -s staff_u %operators`
Removing mappings is done with the **-d** instead of **-a** option. You can also modify a mapping using **-m**.

However, if you change a mapping for a user that already owns files on the file system, it is *very important* to reset the context of these files completely (i.e. using **-F** with **restorecon**) or set the new SELinux owner and role using **chcon**.

`root #``chcon -R -u staff_u -r staff_r /home/oper`
If not, then the files are owned by the 'wrong' SELinux user which might cause permission troubles later on.

#### Managing user to role mappings

Similar as with the login mappings, we now use **semanage user** to modify the user/role mappings.

For instance, to create a new SELinux user called *infra\_u* and grant it the *staff\_r* and *sysadm\_r* roles:

`root #``semanage user -a -R "staff_r sysadm_r" infra_u`
### Properties of an SELinux user

The SELinux user feature in SELinux has a few interesting properties.

Generally speaking, SELinux user information is never changed in a session. This isn't SELinux itself per se that dictates it, but is implemented as policy rules. There are a few deviations to this (such as when starting services), but are closely guarded policy-wise. Unlike the effective user id of a session, which can be changed using tools like **su** or **sudo**, the SELinux user information thus remains the same. This makes it possible for improved auditing, but it is mostly a security measure (to make sure that someone can never change their role to a role that they shouldn't be in).

SELinux users also allow for specific, additional constraints to be put. One of these is *user based access control* which, roughly speaking, ensures that files owned by one SELinux user can never be accessed by another SELinux user (but *sysadm\_u*, *root* and *system\_u* are exempt from this) if that file has a context with the *ubac\_constrained\_type* attribute assigned to it.

### What you need to remember

What you should remember from this tutorial is that

- SELinux users define what roles a user is allowed to go to
- Linux accounts are mapped to SELinux users (but they are not the same) in a many-to-few relationship (usually)
- the **semanage login** and **semanage user** set of commands are used to manipulate these settings
- SELinux user information rarely changes in a session (mostly only when services are launched where they become *system\_u*)
