<!-- source: https://wiki.gentoo.org/wiki/SELinux/Constraints | group: Gentoo Wiki (Main) | wiki-title: SELinux/Constraints -->
---
title: SELinux/Constraints
url: https://wiki.gentoo.org/wiki/SELinux/Constraints
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-27"
fingerprint: "6fbd2f33965acf84"
license: CC BY-SA 4.0
---

# SELinux/Constraints

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Constraints

### Introduction

Constraints are a set of rules that further define the allowed actions within an SELinux system. Even if a regular allow rule says that something is, well, allowed, a constraint might impose further restrictions on it.

The most well-known constraint we have in place is the *User Based Access Control* system, enabled if the [ubac](https://packages.gentoo.org/useflags/ubac) [USE flag is set.](https://wiki.gentoo.org/wiki/USE_flag)

The following is the list of constraints enabled on a system.

| Description | Enabled if... | 
|---|---|
| UBAC - User Based Access Control | USE=ubac | 
| Identity change constraints | Always | 
| Domain transitions | Always | 

When describing constraints, we talk about *actors* (processes, users, ... that initiate an action) and *objects* (files, directories, processes, ...). The actor invokes an action against an object, and the constraint validates if this action is to be allowed or not.

Please be aware that constraints are only checked if the regular SELinux rules allow the action. It is not because a constraint says that an action is allowed that it is always allowed - first the regular SELinux enforcement rules need to apply.

### UBAC - User Based Access Control

The user based access control generally tries to protect access to files, directories and other resources owned by a particular SELinux user (not regular Linux user, but the assigned SELinux user, like *user\_u*) from access by other SELinux users.

The basic conditions to allow access are as follows:

- The actor SELinux user and target object SELinux user are the same, or
- The actor SELinux user or target object SELinux user is *system\_u* , or
- The actor SELinux type or target object SELinux type do not have *ubac\_constrained\_type* attribute set, or
- The actor SELinux type has one of *ubacfile*, *ubacproc*, *ubackey*, ... attributes set

When at least one of the above rules is true, then the action is allowed. Note that the use of the *ubac\_constrained\_type* attribute is important here - whenever either the source domain or target type does not have this attribute set, then the action is not governed by this constraint. As an example, take the *user\_home\_t* type (regular user files):

`user $``seinfo -tuser_home_t -x````
   user_home_t
      file_type
      non_security_file_type
      polyparent
      mountpoint
      polymember
      ubac_constrained_type
      non_auth_file_type
```
Next to the *ubac\_constrained\_type*, other attributes play an important role within this constraint too. These attributes are the *ubacfile*, *ubacproc*, *ubackey*, ... attributes. When this attribute is set for the actor type, then the action isn't governed by the constraint. This allows a type to take part of the constraint when it is the target object (because it has the *ubac\_constrained\_type* set) but not when it is the actor object.

### Identity change constraints

Any activity that would create an object identity (be it the *create* permission, or *relabelto / relabelfrom*) is only allowed if the actor SELinux user and target object SELinux user are the same, or when the actor type has the *can\_change\_object\_identity* attribute set.

This constraint is for the various file classes (directory, file, sockets, ...), socket classes (tcp, udp, netlink, ...).

### Domain transitions

Domain transitions are allowed if at least one of the following rules is true:

- The actor SELinux user and target object SELinux user are the same
- The actor SELinux type has the *can\_change\_process\_identity* attribute set, and the target object SELinux type has the *process\_user\_target* attribute set
- The actor SELinux type has the *cron\_source\_domain* attribute set, and either the target object SELinux user is *system\_u* or its type has the *cron\_job\_domain* attribute set
- The actor SELinux type has the *can\_system\_change* attribute set and the target object SELinux user is *system\_u*
- The actor SELinux type has the *process\_uncond\_exempt* attribute set

Next to these rules, at least one of the following rules need to be valid too:

- The actor SELinux role and target object SELinux role are the same, or
- The actor SELinux type has the *can\_change\_process\_role* attribute set and the target object SELinux type has the *process\_user\_target* attribute set, or
- The actor SELinux type has the *cron\_source\_domain* attribute set, and the target object SELinux type has the *cron\_job\_domain* attribute set, or
- The actor SELinux type has the *can\_system\_change* attribute set and the target object SELinux role is *system\_r*, or
- The actor SELinux type has the *process\_uncond\_exempt* attribute set
