<!-- source: https://wiki.gentoo.org/wiki/SELinux/Tutorials/Linux_services_and_the_system_u_SELinux_user | group: Gentoo Wiki (Main) | wiki-title: SELinux/Tutorials/Linux services and the system u SELinux user -->
---
title: SELinux/Tutorials/Linux services and the system u SELinux user
url: https://wiki.gentoo.org/wiki/SELinux/Tutorials/Linux_services_and_the_system_u_SELinux_user
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-06-23"
fingerprint: "67ba8d718518cbc4"
license: CC BY-SA 4.0
---

# SELinux/Tutorials/Linux services and the system u SELinux user

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Linux services and the system\_u SELinux user

So now we know that the security context of a process contains a part called the *SELinux user*, one *SELinux role* and the *SELinux domain* that the process runs in. We have also talked a bit about SELinux users and that they are somewhat immutable (the user cannot change the SELinux user). Well, in this tutorial we'll talk about an exception to this rule, mainly when managing Linux services.

### The system\_u SELinux user

The *system\_u* SELinux user is meant to be used for running system services. It should not be assigned to end users. On most servers thus, most daemons will be running as the *system\_u* SELinux user, most likely (or even solely?) in the *system\_r* role.

This is because SELinux is primarily a *type enforcement* mandatory access control system, thus most access controls are situated around the domain part (i.e. the third field in the context, the one ending in *\_t*, like *sshd\_t*). For these daemons, no additional access controls or constraints should be declared, so they run as the *system\_u* SELinux user and *system\_r* SELinux role.

It is for user domains (local users) that *SELinux user* and *SELinux role* constraints are positioned more, as we want to govern the activities that local users can do, as local user access is often considered a risk and thus warrants proper control.

### Linux service scripts

Most Linux service scripts are, as you know, located in /etc/init.d (I will not talk about **systemd** yet as I have no experience with it). These service scripts are launched by the **init** system upon boot (or runlevel change), but can also be called directly by administrators.

For the standard SELinux policies, these scripts are either labeled *initrc\_exec\_t* or *\<domain>\_initrc\_exec\_t*:

`user $``ls -lZ /etc/init.d/sshd`
-rwxr-xr-x. 1 root root system\_u:object\_r:initrc\_exec\_t 2057 Nov 21 18:20 /etc/init.d/sshd

The idea is that, when **init** or the administrator runs those scripts, the script itself runs in the *initrc\_t* domain. However, if you double-check, you will notice that *only* the *system\_r* role is allowed the *initrc\_t* domain - so a regular user domain should not be able to transition towards the *initrc\_t* domain.

`user $````
seinfo -rsysadm_r -x | grep initrc_t
```
`user $``seinfo -rsystem_r -x | grep initrc_t`
initrc\_t

So what is happening when you do start the script? After all, everything seems to run, right?

`root #``/etc/init.d/sshd start`
Authenticating root.
Password: 
\* Starting SSH daemon...          \[ started \]

#### The process to transition to initrc\_t

What happens is that, upon executing the init script, a transition occurs to the *run\_init\_t* domain (Gentoo). Then, OpenRC (Gentoo's init system) calls the **run\_init** command. This command will invoke the script (again) but with a transition, not only to the target *initrc\_t* domain, but also with the *system\_u:system\_r* related context (so it switches SELinux user and role).

On non-Gentoo systems, it is very likely that administrators are already asked to use the **run\_init** command upon managing services.

`root #``run_init /etc/init.d/sshd status`
\* sshd: started

The *run\_init\_t* domain is one of the few domains that are allowed (by policy) to do an SELinux user- and role change.

#### Domain-specific init scripts

Some services have a domain-specific init script. For instance, the /etc/init.d/nscd script has the *system\_u:object\_r:nscd\_initrc\_exec\_t* context.

In this case, if the system administrator executes this script, *no* such transition occurs. This is because the *nscd\_initrc\_exec\_t* script is not defined as an entrypoint for the *run\_init\_t* domain. Instead, it is only known as an entrypoint for the *initrc\_t* domain itself. When trying to execute it directly, SELinux attempts to transition to the *initrc\_t* domain but, as there is no switch in user/role, it fails. In the audit logs (or messages file) you then find an error like the following:

type=SELINUX\_ERR msg=audit(1363638369.361:144): security\_compute\_sid:  invalid context staff\_u:system\_r:initrc\_t for 
scontext=staff\_u:sysadm\_r:sysadm\_t tcontext=system\_u:object\_r:nscd\_initrc\_exec\_t tclass=process

To force the change in user/role, you *need* to call the **run\_init** command for this (if you are allowed this command), like so:

`root #``run_init /etc/init.d/nscd status`
Authenticating root.
Password:
 \* nscd: started

#### Purpose of domain-specific init scripts

The purpose of this distinction is to allow specific users (or roles) to execute the domain-specific init scripts, while not granting privileges to execute the other scripts. In this case, users that are not allowed to execute *run\_init\_t* can be allowed to execute the mentioned domain-specific init script. In this case, the policy will *automatically* (i.e. without using **run\_init\_t**) switch the role towards *system\_r* (but mind you, this requires that the user is allowed the *system\_r* role).

When this occurs however, no change in SELinux user happens. As a result, these services will then run under the *SELinux user* of the user that (re)started the service. As the process runs in the *system\_r* and proper domain however, this has no additional impact on the system.

Later in this series, when we know how to create our own policies, we will be creating our own role and type which *will* be allowed to execute such scripts.

### What you need to remember

What you should remember from this tutorial is that

- calling linux service scripts (init scripts) causes a change in SELinux user, role and domain, partially due to policy and partially due to the **run\_init** command
- some service scripts are not labeled the regular *initrc\_exec\_t* and require a different approach for calling them, and might result in the process to remain running under the users' SELinux user (but that's okay)
- it is the SELinux policy that governs changes in roles and users

This article is meant to review a bit how SELinux functions (so we use the init scripts here as a common example).
