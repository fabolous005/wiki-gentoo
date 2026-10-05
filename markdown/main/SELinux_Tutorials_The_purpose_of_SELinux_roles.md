<!-- source: https://wiki.gentoo.org/wiki/SELinux/Tutorials/The_purpose_of_SELinux_roles | group: Gentoo Wiki (Main) | wiki-title: SELinux/Tutorials/The purpose of SELinux roles -->
---
title: SELinux/Tutorials/The purpose of SELinux roles
url: https://wiki.gentoo.org/wiki/SELinux/Tutorials/The_purpose_of_SELinux_roles
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-06-23"
fingerprint: a69e0d53044453e4
license: CC BY-SA 4.0
---

# SELinux/Tutorials/The purpose of SELinux roles

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## The purpose of SELinux roles

We have seen that a process' context defines what the process is allowed to do, and that a context can change as part of a domain transition. This access control is called *type enforcement*, but SELinux is able to do more than that. Enter *SELinux roles*...

### SELinux user domains

Let's take a look at the context of a regular *Linux user*, returned by **id** (or **id -Z** but we'll show the full output first).

`user $``id`
uid=1000(swift) gid=100(users) groups=100(users),16(cron),...,995(gorg) context=user\_u:user\_r:user\_t

The context shows that the user is inside the *user\_t* domain.

Let's take a look at the context of the root user, when directly logged on (i.e. on the console).

`root #``id -Z`
root:sysadm\_r:sysadm\_t

In this case, it shows that the root user is inside the *sysadm\_t* domain.

SELinux policy dictates that the regular user domain (*user\_t*) is an unprivileged domain: it should never be allowed to do any administrative tasks. Even if you grant this user root access (through **sudo** or **su**) the user will not be able to do much damage to the system. This isn't only because of the *user\_t* domain however - it is also because of the *SELinux role* that that user is in.

### The role part in the context

In the context we've seen, the role is the second field:

user\_u:user\_r:user\_t

As you can see, the naming convention states that it should end with *\_r*. However - again - this is a policy convention and not dictated by SELinux itself.

The *SELinux role* dictates what domains (contexts) are possible to be in. It doesn't mean that the user can freely choose which domains he launches in next (there are still the domain transitions that govern this); it means that, even if he was allowed to transition, if the domain is not attached to his role, the transition will fail.

The result of such a failed transition will be logged as an SELinux error, like so:

invalid context: user\_u:user\_r:portage\_t

This is very important to understand. Consider a developer trying to start the mysql daemon from the command line (so not in daemon mode). Even if he was allowed to execute the *mysqld\_exec\_t* labeled file, and transition, then *still* it would not be allowed as *mysqld\_t* is not a type (domain) allowed for any of the user roles (*user\_r*, *staff\_r*, *sysadm\_r*).

#### Listing the approved domains

With the **seinfo** tool, you can list what domains are allowed for a particular role.

`user $``seinfo -ruser_r -x````
   user_r
      Dominated Roles:
         user_r
      Types:
         chromium_renderer_t
         user_gkeyringd_t
         ...
```
If you are trying to start up an application (let's not consider services yet) and it fails, it might be a good idea to look if the domain of that application (like *mozilla\_t* or *chromium\_t*) is a supported domain for the role you are in.

#### Switching roles

Users can switch roles if they want. However, they can only do so if their SELinux user is allowed to "be" in the other role. With **semanage user -l** you can see if that is the case.

`root #``semanage user -l`
SELinux User    SELinux Roles
root            staff\_r sysadm\_r
staff\_u         staff\_r sysadm\_r
sysadm\_u        sysadm\_r
system\_u        system\_r
unconfined\_u    unconfined\_r
user\_u          user\_r

We will talk about SELinux users later. For now, it suffices to say that the SELinux user (first column) has to match the first entry in the context of the user (as returned by **id -Z**):

user\_u:user\_r:user\_t

So in the case of the above context, we see that this user is *only* allowed to be in the *user\_r* role. In other words, this user is not able to switch roles.

root:sysadm\_r:sysadm\_t

In this case, the user could switch from the *sysadm\_r* role to the *staff\_r* role (and vice versa).

Switching roles is done using **newrole -r \<targetrole>**. It is most commonly used to switch from the *staff\_r* role to the *sysadm\_r* role:

`user $``newrole -r sysadm_r`
Password:

#### Standard set of roles

The following table gives an impression of the standard roles available on an SELinux system. Your means may vary, as more roles might exist on your system.

| Role | Description | 
|---|---|
| user\_r | The regular user role, which is meant to only allow user applications and other non-privileged domains | 
| staff\_r | Similar to the *user\_r* role, but might be allowed to receive more system information than a regular user. This role is mostly given to users that should be allowed to switch towards other roles. | 
| sysadm\_r | System administrative role; this is a very powerful role as it is allowed most target domains, including privileged domains. Use with care. | 
| system\_r | System role, not meant to be switched to directly (and newrole will even disallow it as it doesn't have a default user domain associated with it - something we'll talk about later) | 

### What you need to remember

What you should remember from this tutorial is that

- roles dictate which domains that the user (or session) is allowed to go in
- changing roles is done using newrole
- the SELinux user definitions declare what the supported roles are that a user can "go" to
