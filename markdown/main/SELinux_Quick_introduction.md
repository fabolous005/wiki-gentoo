<!-- source: https://wiki.gentoo.org/wiki/SELinux/Quick_introduction | group: Gentoo Wiki (Main) | wiki-title: SELinux/Quick introduction -->
---
title: SELinux/Quick introduction
url: https://wiki.gentoo.org/wiki/SELinux/Quick_introduction
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-24"
fingerprint: "263f1d1f021e43c4"
license: CC BY-SA 4.0
---

# SELinux/Quick introduction

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

SELinux is an access control mechanism, supported through the Linux kernel, that works alongside the Linux regular access controls. But unlike the regular access controls, it is a *mandatory* access control system, ensuring that users cannot work around the rules set by the administrator. But before we get into the gritty details of SELinux, let's first see how the regular access controls on a Linux system work. Then, we continue with the SELinux features one by one, explaining most of the models supported by SELinux.

## Regular Linux access control

Assume a user wants to execute a script, then the Linux discretionary access control system will check the information of the process that the user is running (which is most likely a shell process) and that of the script itself to decide if the execution can go through.

![Schematic overview of a script execution Schematic overview of a script execution](https://wiki.gentoo.org/images/e/e9/Linux_DAC_permissions.png)


In the example above, Linux will allow the execution, because the permissions on the file (which are `-rwxr-xr-x`) allow other users to execute it.

The discretionary part comes from the fact that the owner of the file can decide on the access rights to the file. Even if the file wasn't executable at first, the user could easily assign the proper permission set to the file:

`user $``chmod o+x /path/to/script`
On a multi-user system, a user will be able to share whatever resources he has access to with the other users, as well as enable any action (such as writing to or executing the files). All he needs to do is to open up the rights to these resources for other users (or if they all share the same group, to the group owner). And although the example of "sharing" files is a simple one, it can go further than that. In some enterprises, access to particular software on a system is governed through the group membership. Developers might be assigned a *developer* group, allowing them access to the system compiler. But once a developer can access the compiler, he can easily also copy all compiler files into his own directory and share that directory with other, non-developer users who can then call the compiler as well.

The discretionary part of the Linux permission system goes further than just users. Services too, such as the Apache web server, can decide how they open up their resources. And although it might not be by design, a malfunctioning service might expose sensitive data to users through this aspect.

## Mandatory access control

With a mandatory access control, the permissions are not governed by the owner of the resource, nor can they be worked around by users. Instead, they are set in stone by the security administrator. In SELinux, this is done through the SELinux policy that is loaded at the start of the system.

![Schematic overview of a script execution under SELinux Schematic overview of a script execution under SELinux](https://wiki.gentoo.org/images/8/8d/SELinux_MAC_permissions.png)


The interaction shown is still the same, but an *additional* set of components is added: the SELinux subsystem (running in the Linux kernel) and the policy.

### SELinux security subsystem

SELinux is a security subsystem that works inside the Linux kernel. It uses Linux Security Modules (LSM) hooks provided by the Linux kernel developers to intercept any call and check if the call is allowed or not. If it is, then the activity can go through. Otherwise, a *Permission Denied* error is returned to the system and a denial message is sent to the audit subsystem. This way, administrators can investigate [denials](https://wiki.gentoo.org/wiki/SELinux/Logging) (as they usually reflect unwanted behavior being exerted on the system) and rest assured that security rules cannot be worked around.

It is important to understand that the SELinux permission check happens *after* the Linux DAC check. In other words, SELinux cannot be used to override access controls on the system. If the regular permission system disallows an activity, then SELinux is not even consulted.

### Context based approach

SELinux uses *contexts* to identify resources. In the example, two contexts take part in the activity:

1. the context of the user process (such as a shell), which is `user_u:user_r:user_t`
2. the context of the target file, which is `system_u:object_r:lib_t`

A context in SELinux consists out of 3, sometimes 4 parts:

1. it starts with a [SELinux user](https://wiki.gentoo.org/wiki/SELinux/Users_and_logins) (which is *not* the same as a Linux user),
2. followed by the SELinux role,
3. followed by the SELinux type,
4. and then *optionally* followed by a sensitivity

Or, in a regular schematic representation:

![Schematic overview of a SELinux context Schematic overview of a SELinux context](https://wiki.gentoo.org/images/6/69/SELinux_context.png)


Each field is used by SELinux for deciding access controls. Most of the rules however are made for the *SELinux type* of a context, which is why contexts are often reduced to just the type.

### SELinux policy

The SELinux security subsystem checks any access attempt through the policy. SELinux uses a deny-by-default setup, so it looks for explicit allow statements on the *SELinux type*. In our case, it looks for an allow rule that allows `user_t` `execute` rights on a `file` resource with the `lib_t` type assigned to it:

In our example, it doesn't find such a rule, so it denies the execution attempt.

The SELinux policy is what defines the behavior of the SELinux subsystem. Widely different policies will result in different behavior on the system. And the term *behavior* here is well chosen: the policy is a write-down of what acceptable, normalized behavior is. It is a full description of how processes on the system are allowed to behave.

The policy is separate from the SELinux subsystem; an update to the policy does not require kernel rebuilds or application updates. Instead, policies are built into binary *policy packages* that can be dynamically loaded, similar to how kernel modules can be loaded.

## Access controls

The SELinux subsystem has various access controls in place. One of them we roughly saw, which is called the *type enforcement*. Next to this, SELinux also supports *role-based access control* and *user-based access control*. The last access control system we'll briefly cover is the *multi-level security* model.

### Type enforcement

The *[type enforcement](https://wiki.gentoo.org/wiki/SELinux/Type_enforcement)* access control system focuses on the *SELinux type* within a SELinux context. It is also what most of the SELinux rules are written for and covers the vast majority of SELinux rules in a SELinux policy.

The access check that SELinux performs as part of the type enforcement is based on the **access vector** built up from the attempt. Such an access vector contains
- the source context (such as `user_t`)
- the target context (such as `lib_t`)
- the class of the target (such as `file`)
- the activity that is invoked (such as `execute`)

If an allow statement exists for the entire access vector, then the activity is allowed. If not, it is denied. The allow statement for our example would look like so:

The source context type and target context type are defined by the policy developer (or security administrator). On an average SELinux system several hundreds, if not thousands of types exist. These types are declared every time differentiation is needed on the access. For instance, policy developers noticed that there were cases where a shell script only needed to call the shell interpreter itself (such as `bash`) but no other binaries. If the shell interpreter was labeled as `bin_t` then these shell scripts had access to all binaries labeled `bin_t`. So the policy developers created a second type, `shell_exec_t`, so that shell scripts only needed access to that type and were prevented from executing other, generic binaries.

Next to the types, SELinux also supports a large set of classes. This allows SELinux to differentiate accesses based on the class of a resource. Privileges on a `lib_t` file versus those of a `lib_t` socket file or directory are completely separate for SELinux. This is unlike the discretionary access controls on a Linux system.

The set of classes can be seen on a SELinux enabled system like so:

`root #``ls /sys/fs/selinux/class`
appletalk\_socket  db\_procedure  file            netlink\_audit\_socket           node           socket              x\_cursor     x\_screen
association       db\_schema     filesystem      netlink\_dnrt\_socket            nscd           sock\_file           x\_device     x\_selection
blk\_file          db\_sequence   ipc             netlink\_firewall\_socket        packet         system              x\_drawable   x\_server
capability        db\_table      kernel\_service  netlink\_ip6fw\_socket           packet\_socket  tcp\_socket          x\_event      x\_synthetic\_event
capability2       db\_tuple      key             netlink\_kobject\_uevent\_socket  passwd         tun\_socket          x\_extension
chr\_file          dbus          key\_socket      netlink\_nflog\_socket           peer           udp\_socket          x\_font
context           db\_view       lnk\_file        netlink\_route\_socket           process        unix\_dgram\_socket   x\_gc
db\_blob           dccp\_socket   memprotect      netlink\_selinux\_socket         rawip\_socket   unix\_stream\_socket  x\_keyboard
db\_column         dir           msg             netlink\_socket                 security       x\_application\_data  x\_pointer
db\_database       fd            msgq            netlink\_tcpdiag\_socket         sem            x\_client            x\_property
db\_language       fifo\_file     netif           netlink\_xfrm\_socket            shm            x\_colormap          x\_resource

Each class has its own set of supported privileges. The supported permissions of a regular file for instance:

`root #``ls /sys/fs/selinux/class/file/perms/`
append      execmod           getattr  lock     quotaon      relabelto  swapon
create      execute           ioctl    mounton  read         rename     unlink
entrypoint  execute\_no\_trans  link     open     relabelfrom  setattr    write

The supported permissions for a TCP socket:

`root #``ls /sys/fs/selinux/class/tcp_socket/perms/`
accept      bind       create   ioctl   name\_bind     node\_bind  recv\_msg     send\_msg  setopt
acceptfrom  connect    getattr  listen  name\_connect  read       relabelfrom  sendto    shutdown
append      connectto  getopt   lock    newconn       recvfrom   relabelto    setattr   write

With such a vast set of possible access vectors, policy development is a large undertaking. Luckily, distributions such as Gentoo Linux come with a complete set of default policies to use, with thanks to the [reference policy](https://github.com/SELinuxProject/refpolicy) project which manages a base policy for distributions to work with.

For more information on type enforcement, please read our [type enforcement](https://wiki.gentoo.org/wiki/SELinux/Type_enforcement) article.

### Role-based access control

From the *SELinux type*, we now get into the realm of the *SELinux role*. Remember the context in our example? It had `user_r` as the SELinux role in the context of the process that was trying to execute a file.

Roles are like caps that a user can put on. A user is always assigned to a role, but can decide to switch roles (of course if allowed to switch to a role). An unprivileged user might only have access to one role (`user_r`) whereas an administrator might have access to several roles (`staff_r` for regular operations, and `sysadm_r` for system administrative tasks). This is also how roles in organizations are used. There are developer roles, system engineer roles, system administrator roles, IT architect roles, database administrator roles, etc.

In SELinux, roles decide which types a process context can be in. The `user_r` role has the right to have processes run in the `user_t` type. Types for processes are also called **domains**. So the role-based access control decides which *domains* a role is allowed to have.

Consider a regular user role (`user_r`) versus database administrator role (`dbadm_r`). The regular user will not have access to the backup infrastructure, so the `backup_t` domain is not allowed for the `user_r` role. Even if the user would somehow be able to run backup software (say from a USB stick) this software will be running in the user domain (`user_t`) and not the backup domain (`backup_t`). And as the user domain is not allowed to access database files directly, the user will not be able to backup or restore data.

Unlike the database administrator, whose `dbadm_r` role is allowed to access `backup_t`. The database administrator can therefore execute backup software, which (assuming the policies are all correct) run in the `backup_t` domain, which is allowed to manage database files. As a result, the database administrator can backup and restore databases.

The set of domains that a role has access to can be requested on a SELinux system through the `seinfo` command:

`user $``seinfo -ruser_r -x````
  user_r
    Types:
      git_session_t
      httpd_user_script_t
      ...
```
Security administrators will usually design *least privilege access* through the role-based access controls, and then assign the roles to the users on a need-to-have basis. So privileges are not assigned to users individually (as that doesn't scale well) but to the domains, which are assigned to the roles, and users are assigned one or more roles to perform their duties.

### User-based access control

The first part of a context is the [SELinux user](https://wiki.gentoo.org/wiki/SELinux/Users_and_logins). The idea behind the SELinux user is that it has to remain immutable, unlike roles which can be switched on user request.

The SELinux user decides which roles someone is allowed to go to. For instance, the `user_u` SELinux user specifies that only the `user_r` role is allowed, whereas the `staff_u` user specifies that the allowed roles are `staff_r` and `sysadm_r`. This can be seen by either `seinfo` or `semanage`:

`root #``seinfo -ustaff_u -x````
  staff_u
    roles:
      staff_r
      sysadm_r
```
`root #``semanage user -l`
SELinux User       SELinux Roles
root               staff\_r sysadm\_r
staff\_u            staff\_r sysadm\_r
sysadm\_u           sysadm\_r
system\_u           system\_r
user\_u             user\_r

On SELinux systems, Linux users are *mapped* to an SELinux user. This mapping is what decides what a user is allowed to do on a system.

Mappings can be seen (and managed) using `semanage login`:

`root #``semanage login -l`
Login Name         SELinux User
\_\_default\_\_        user\_u
swift              staff\_u
root               root

This is still part of the role-based access control, but important to know as we can now go to the used-based access control.

On Gentoo, if `USE="ubac"` is set, User Based Access Control (UBAC) is enabled. This ensures that unprivileged users with a different SELinux user cannot access each others' resources even if the types would allow this.

Remember the example of one user sharing access to his files to other users? Well, if all users run with the `user_t` type (which is normal for unprivileged users) and have the files labeled as `user_home_t` (which is normal for the contents in the user /home/\* locations) then type enforcement would still allow one user to access another users' resources if that user opened up his home directory. After all, SELinux has the following rule to allow users to manage their own files:

However, we can create SELinux users for each user individually, and map each Linux user to a specific SELinux user. With that in place, the contexts of userA's and userB's files differ:

`root #``ls -ldZ /home/userA/somefile /home/userB/somefile`
-rwx------. userA userA  userA\_u:object\_r:user\_home\_t /home/userA/somefile
-rwxrw-rw-. userB userB  userB\_u:object\_r:user\_home\_t /home/userB/somefile

In the example, *userB* has opened up access to his /home/userB/somefile file (making it world writable). But userA will not be able to access it, as userA will run with the `userA_u` SELinux user, which is not allowed to access `userB_u` labeled resources of type `user_home_t` (UBAC works on a per type basis, not all types are what is called *ubac-constrained*).

### Multi-level security

The last access control to discuss is the *multi-level* security.

In the security world, the sensitivity can be seen as the classification of data. For instance, you can have *strictly confidential* data, *confidential* data, *internal* data and *public* data. There usually is a strict order in this classification. Someone who has clearance to read confidential data can read internal and public data as well, but not strictly confidential data. Similarly, someone working on confidential data should not at the same time be allowed to write to internal or public data (as that would lead to information leakage).

This gives an information flow model that is often mentioned as "no read up, no write down".

In SELinux, this model is supported through the sensitivity of a context. In that case, a user context could look like `user_u:user_r:user_t:s0` or `sysadm_u:sysadm_r:sysadm_t:s0-s0:c0.c1023`. Note however that the added part is just a single set, so `s0-s0:c0.c1023` is *one* added field to the context.

This added field is the sensitivity, and contains one, sometimes two parts.

1. The first part is the sensitivity level, which is an integer representation
2. The second part is the category set, which are integers as well

The sensitivity level represents the classification. For instance, `s0` is public data, `s1` internal, `s2` confidential and `s3` strictly confidential. It is up to the security administrator to decide how many layers he wants.

The category represents a set of integers which can be assigned to resources. Each integer is some sort of "tag" on a resource, and can be used to differentiate classes of data. One could use a category for every department in an organization, or a category for every company on a multi-tenant system. A few examples of category sets are:
- `c0` meaning category 0
- `c0,c4` meaning categories 0 and 4
- `c0.c4` meaning categories 0 up to 4 (so 0, 1, 2, 3 and 4)

The clearance shows what a domain is allowed to access. A clearance of `s0-s1:c0,c4.c8` means that the process runs in the sensitivity level `s0` and is allowed to access resources with sensitivity up to `s1` and categories 0 and 4 to 8. A resource with a subset of categories assigned is also accessible, but a resource that has another category that is not part of the clearance will not be accessible by that domain.

SELinux users can be given a default clearance set, and the login mappings can reduce the clearance of users even further.

Multi-level security is supported in Gentoo through the *mls* policy store. The *mcs* policy store also supports MLS, but only with a single sensitivity.

## Integration

So far for all the features, but how is SELinux integrated in Gentoo then?

### Enabling SELinux support in packages

First of all, the SELinux support in the applications is enabled through the *selinux* USE flag. This flag is not configurable by end users - for that a SELinux profile needs to be enabled.

`root #``eselect profile list | grep selinux`
\[2\]   default/linux/amd64/13.0/selinux
 \[12\]  hardened/linux/amd64/selinux \*
 \[14\]  hardened/linux/amd64/no-multilib/selinux

But just enabling this profile isn't sufficient as a set of actions need to be taken to convert a system to become a SELinux system. These instructions are available in the [SELinux handbook](http://www.gentoo.org/proj/en/hardened/selinux/selinux-handbook.xml) for now, and will be in the [Installation](https://wiki.gentoo.org/wiki/SELinux/Installation) page on the wiki later.

### Policy

The SELinux policy is provided through the various `sec-policy/selinux-*` packages, with a [sec-policy/selinux-base](https://packages.gentoo.org/packages/sec-policy/selinux-base) package that contains the base policy (a minimal set of policies that cannot be segregated from each other) and a [sec-policy/selinux-base-policy](https://packages.gentoo.org/packages/sec-policy/selinux-base-policy) package that contains the minimal set of SELinux policy modules for a Gentoo system.

The SELinux policy itself is compiled in a binary format, whose resulting files are available at /usr/share/selinux/\* and all have an `.pp` suffix. These binary *policy packages* are loaded by the `semodule` command, similar to how Linux kernel modules are loaded by the `insmod` command.

Thanks to the USE flag integration, depending policies are automatically loaded when a user installs software that has a policy module for. Applications that have specific SELinux support (such as the core utilities to display SELinux labels) will also build in this support when `USE="selinux"` is set.

## Administration

Once a system is SELinux-enabled, some additional administration is needed. It is not our intention to cover all administrative commands, but instead give the necessary pointers to the right resources.

### SELinux state and mode

The `sestatus` command tells us what the current SELinux state is.

`root #``sestatus`
SELinux status:                 enabled
SELinuxfs mount:                /sys/fs/selinux
SELinux root directory:         /etc/selinux
Loaded policy name:             mcs
Current mode:                   enforcing
Mode from config file:          enforcing
Policy MLS status:              disabled
Policy deny\_unknown status:     denied
Max kernel policy version:      28

Possible [states](https://wiki.gentoo.org/wiki/SELinux/States) are *disabled* or *enabled*. The *disabled* state means SELinux is completely turned off and does not effect system behavior. If SELinux is enabled, then its *mode* can be *permissive* or *enforcing*.

In *permissive* mode, SELinux will be consulted but will not enforce its decision. Instead, it will only [log denials](https://wiki.gentoo.org/wiki/SELinux/Logging) and still allow the action to continue. This is great for troubleshooting issues, as we can temporarily disable SELinux to see if it is really to blame, but also to convert a system to an SELinux system: we can verify that everything would work (no specific denials) after which we can enable SELinux.

In *enforcing* mode, SELinux will enforce its policy on the system. Denials are of course still logged, but now the applications will actually be prevented from doing any action that isn't allowed by the policy.

It is also possible to flag a specific domain as *permissive* so that applications running in this domain will be treated as if the system were in *permissive* mode: any actions taken outside of the policy will be logged, but they will be allowed to happen. This is another troubleshooting feature, so that a newly-added domain can be analyzed via logged denials while the remainder of the system runs in the full-security *enforcing* mode.

As a separate concept from the above modes, the policy can include a highly privileged domain that has almost full permissions. Such a domain with (almost) no restrictions imposed by SELinux is referred to as an *unconfined* domain. SELinux will allow (almost) all actions taken by an *unconfined* domain because the policy says that it is ok, regardless of whether SELinux is in *enforcing* or *permissive* mode. There will not be any access violations to be logged in this case, as opposed to a more restrictive domain being handled permissively (either due to domain-specific permissive flag or system-wide permissive mode) and logs denials that would otherwise have occurred.

### Listing contexts

Many Linux utilities support showing the SELinux context. As everything SELinux handles must have a context, viewing the context associated with every resource is very important. Contexts can be easily viewed for many resource types.

Inside a shell, the `id` command can display the context of the current user/session:

`user $``id -Z`
staff\_u:sysadm\_r:sysadm\_t

File, directory and other file-represented resource contexts can be seen through `ls -Z` or `stat`:

`user $``ls -lZ metadata.xml`
-rw-r--r--. 1 swift users staff\_u:object\_r:user\_home\_t 1020 Dec 10  2011 metadata.xml

Process contexts can be seen through `ps -Z`:

`user $``ps -eZ | grep init`
system\_u:system\_r:init\_t            1 ?        00:00:00 init
staff\_u:staff\_r:staff\_t          6045 tty1     00:00:00 xinit

Port contexts can be queried with `seinfo`:

`user $``seinfo --portcon=80````
        portcon tcp 80 system_u:object_r:http_port_t
        portcon tcp 1-511 system_u:object_r:reserved_port_t
        portcon udp 1-511 system_u:object_r:reserved_port_t
```
For more information about contexts and labels, please refer to our [SELinux labels](https://wiki.gentoo.org/wiki/SELinux/Labels) article.

### Label management

The most common administrative set of commands are related to labeling of resources. Remember that for SELinux, everything is managed through the contexts. When problems arise, administrators will need to check and, if necessary, update the context of the resources in order to have everything working again.

Contexts can be set explicitly using `chcon`:

`root #``chcon -t net_conf_t /etc/resolv.conf`
However, SELinux utilities have a specific database in which the "correct" labels for all files are stored. This database uses path expressions to match a file path against and to decide what the right context should be. If this database is correctly defined, then a *relabeling* operation can help to *reset* the context of a file. This can be done using `restorecon`:

`root #``restorecon /etc/resolv.conf`
If all files of a Gentoo package need to be relabeled, `rlpkg` can be used. For instance, to relabel all files provided by the [app-misc/screen](https://packages.gentoo.org/packages/app-misc/screen) package:

`root #``rlpkg screen`
Administrators can query the current file label list using `semanage`:

`root #``semanage fcontext -l | grep resolv`
/etc/resolv\.conf.\*                       regular file     system\_u:object\_r:net\_conf\_t
/usr/libexec/polkit-resolve-exe-helper.\*  regular file     system\_u:object\_r:policykit\_resolve\_exec\_t

The file label list can be updated as well. For instance, to have /etc/puppet-resolv.conf also marked as `net_conf_t`:

`root #``semanage fcontext -a -t net_conf_t /etc/puppet-resolv\.conf`
After updating the database, the file needs to be relabeled to reset its context to the newly defined value:

`root #``restorecon /etc/puppet-resolv.conf`
For more information about label management, please refer to our [SELinux labels](https://wiki.gentoo.org/wiki/SELinux/Labels) article.

### User management

SELinux users are managed through `semanage user` whereas Linux user account mappings are managed with `semanage login`.

For instance, to map the Linux account 'john' to the `staff_u` SELinux user:

`root #``semanage login -a -s staff_u john`
Additional SELinux users can be created using `semanage user`, like so:

`root #``semanage user -a -R "staff_r sysadm_r" myuser_u`
For more information, see our [SELinux users and logins](https://wiki.gentoo.org/wiki/SELinux/Users_and_logins) article.

### Sensitivities and categories

Security clearance for users is set during the definition of the user mapping (with `semanage login`) or on the SELinux user itself (with `semanage user`). Once a user has a clearance set, he can reduce his own clearance through the `runcon` command.

`root #``id -Z`
root:sysadm\_r:sysadm\_t:s0-s0:c0.c1023

`root #````
runcon -l s0-s0:c0.c10,c12 sh
```
`root #``id -Z`
root:sysadm\_r:sysadm\_t:s0-s0:c0.c10,c12

Categories on files and other resources can be set using `chcat`, like so:

`user $``chcat +c12 metadata.xml`
To make category and sensitivity management simpler, it is possible to create translations for this. By editing /etc/selinux/mcs/mcstrans.conf and launching the `mcstrans` daemon, instead of showing something like `s0:c12` this sensitivity could be named `ITDepartment`.

### Troubleshooting denials

When SELinux prevents an access, it will log this in the audit subsystem (or to the system logger if auditing isn't enabled).

To query the audit logs, `ausearch` can be used:

`root #``ausearch -m avc -ts recent````
----
time->Sat Apr 26 22:06:22 2014
type=MMAP msg=audit(1398542782.012:19315): fd=24 flags=0x11
type=SYSCALL msg=audit(1398542782.012:19315): arch=c000003e syscall=9 success=no exit=-13 a0=1000 a1=a0000 a2=7 a3=11 items=0 ppid=15803 pid=15856 auid=4294967295 uid=0 gid=0 euid=0 suid=0 fsuid=0 egid=0 sgid=0 fsgid=0 tty=(none) ses=4294967295 comm="edidvbe" exe="/usr/nsh/NSH/nativetool/libexec/platform/linux-2.6-x86/edidvbe" subj=system_u:system_r:unconfined_t:s0 key=(null)
type=AVC msg=audit(1398542782.012:19315): avc:  denied  { mmap_zero } for  pid=15856 comm="edidvbe" scontext=system_u:system_r:nsh_t:s0 tcontext=system_u:system_r:nsh_t:s0 tclass=memprotect
```
In this example, the `nsh_t` domain wanted to call *mmap\_zero* for the *memprotect* class but was prevented to do so by SELinux.

For more information about SELinux' logging, please refer to our [logging and denials](https://wiki.gentoo.org/wiki/SELinux/Logging) article.

### Adding SELinux policy rules

It is not possible to easily *remove* rules from an active policy, but it is possible to add them.

For simple policy enhancements, Gentoo offers a `selocal` script to do just that. For instance, to allow the above mentioned access:

`root #``selocal -a "allow nsh_t nsh_t:memprotect mmap_zero;" -Lb`
When the enhancements are frequent, it is better to create a SELinux policy module ourselves that contains the rule(s) and load it:

`root #``vim mynsh.te`
policy\_module(mynsh, 1.0)
gen\_require(\`
  type nsh\_t;
')
allow nsh\_t nsh\_t:memprotect mmap\_zero;

`root #````
make -f /usr/share/selinux/mcs/include/Makefile mynsh.pp
```
`root #``semodule -i mynsh.pp`
## Summary

SELinux is a very flexible, mandatory access control system. In this article we just scratched the surface of the SELinux subsystem and gave a quick introduction to the various areas. For a complete overview of SELinux and how to manage SELinux on Gentoo, please refer to the [SELinux](https://wiki.gentoo.org/wiki/SELinux) wiki page.
