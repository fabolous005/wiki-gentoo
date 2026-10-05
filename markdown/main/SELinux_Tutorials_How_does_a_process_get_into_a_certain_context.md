<!-- source: https://wiki.gentoo.org/wiki/SELinux/Tutorials/How_does_a_process_get_into_a_certain_context | group: Gentoo Wiki (Main) | wiki-title: SELinux/Tutorials/How does a process get into a certain context -->
---
title: SELinux/Tutorials/How does a process get into a certain context
url: https://wiki.gentoo.org/wiki/SELinux/Tutorials/How_does_a_process_get_into_a_certain_context
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-28"
fingerprint: e72c9d1314d042c4
license: CC BY-SA 4.0
---

# SELinux/Tutorials/How does a process get into a certain context

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## How does a process get into a certain context

In the previous tutorials, we already got across the context in which a process resides, and which can be checked using utilities like **ps -Z**.

`root #``ps -eZ | grep auditd`
system\_u:system\_r:kernel\_t:s0     448 ?        00:00:04 kauditd
system\_u:system\_r:auditd\_t:s0    6731 ?        00:00:22 auditd

But how do processes get into this context?

### Context inheritance

First of all, know that SELinux supports inheritance of contexts. Furthermore, inheritance of contexts is the default behavior: if there is no policy in SELinux that specifies otherwise, then anything created will inherit the context of its parent.

- A process running with context *foo\_t* spawning another process (forking) will cause creation of a new process with *foo\_t* context (as long as SELinux has no rules specifying otherwise)
- A file or directory created in a directory with context *bar\_t* will get the *bar\_t* context as well (as long as SELinux has no rules specifying otherwise)

To reiterate, existing file context mappings do not determine the file context at creation time. Even if there is a context mapping definition saying that /var/log/audit should be *auditd\_log\_t*, if you create this audit directory within /var/log then this directory will inherit the *var\_log\_t* context from its parent directory. Only later, when you run **restorecon**, will the context be changed.

Since we are talking about inheritance, there probably is a root context for everything. Well, there is - of course this is defined by the policy and SELinux support, but it is safe to assume that the root context (domain) for processes is the *kernel\_t* context (as this is the context that the kernel runs as, and it is the kernel that spawns init, which itself spawns various other processes) and the root context for the file system is *root\_t*.

`user $``ls -ldZ /`
dr-xr-xr-x. root root system\_u:object\_r:root\_t:s0      /

### Transitioning

Of course, SELinux would be quite dull if it didn't support switching to other contexts. And in this tutorial we want to talk about process context (domain) transitions: the idea that a process, once created, is within a different context than its parent process.

SELinux supports domain transitions when a process forks (spawns another process). And when you execute something, we mean that a process referred to some file (most likely a binary or a script) saying "execute this", so that the referred file is loaded and 'becomes' a process.

What an SELinux policy writer can do in these cases is to define a domain transition, like so:

type\_transition init\_t initrc\_exec\_t : process initrc\_t;

Which reads:

When an init\_t process executes a file with context initrc\_exec\_t, 
then the resulting process should run in the initrc\_t context.

Or, illustrated as an ASCII-art flowchart:

\[init\_t\] --(execute initrc\_exec\_t)--> \[initrc\_t\]

Basically what the SELinux policy writer wants to achieve is that, when the init process (running on the Linux system) executes an init script (to be found in /etc/rc.d/init.d or /etc/init.d) then this script will run in the *initrc\_t* domain.

Such a statement defines a default transition (that overrides the 'inherit parent context' rule - it can be used for file/directory contexts as well), but one can also use tools like **runcon** or SELinux API directly to achieve a non-default domain transition as long as it is allowed (read on).

#### The need for the file context

In the previous examples, you notice that the file context of the target file (to be executed) is used. Remember the previous tutorials on file contexts? SELinux does not use paths internally - it always uses contexts. So a policy writer cannot use /etc/init.d/sysstat, but has to refer to the context that this file would have - *initrc\_exec\_t*.

The syntax used is again a naming convention, but is not mandatory. SELinux also doesn't parse the name; the context could very well be called *garble\_mumble\_joy*. Naming conventions though are interesting since they allow us mere humans to understand what the purpose is of the context. So in case of *initrc\_exec\_t*, we could read this as "the context for an executable file that, when executed, might have the resulting process run in *initrc\_t*".

#### Real-life example: startup of SSH

Let's look at a real-life example: the start-up of the SSH daemon.

Basically, what happens when the SSH daemon is started through its service, is

1. the Linux kernel executes /sbin/init, resulting in the init process
2. the init process executes /etc/init.d/sshd, resulting in this init service script running for a (short) while
3. this sshd init service script executes /usr/sbin/sshd, resulting in the sshd daemon

For SELinux, with the common policy loaded, this could be illustrated as:

\[kernel\_t\] --(execute init\_exec\_t)--> \[init\_t\]
\[init\_t\] --(execute initrc\_exec\_t)--> \[initrc\_t\]
\[initrc\_t\] --(execute sshd\_exec\_t)--> \[sshd\_t\]

What we are defining here are *domain transitions* from one domain to another, through the execution of a file.

### When is a transition allowed

Now, such a transition can only occur when the following three requirements are satisfied.

1. The origin domain has execute permission on the file
2. The file context itself is identified as an *entry point* for the target domain
3. The origin domain is allowed to transition to the target domain

If, and only if, all three requirements are satisfied, will such a domain transition be allowed to occur. Let's look at these three rules in more detail.

#### Execute permission on the file

As we have seen before, SELinux has a very fine-grained set of permissions that it can control. The *execute* permission on files is one of them. If a domain does not have *execute* permission on the file, then it will not be able to execute that file. Simple as that.

This too is a common source of denials or permission issues: a process tries to execute some file but is not allowed to. Again, as we learned in the past, this is very probably because of a wrong file context. On a Gentoo system, daemons that fail to execute files are most likely such problems because they are trying to execute a file with the *lib\_t* context (often the case for files in /usr/lib somewhere) instead of the *bin\_t* context.

So in case of the init script (*initrc\_t*) calling the sshd binary (*sshd\_exec\_t*):

`root #``sesearch -As initrc_t -t sshd_exec_t -c file -p execute````
allow initrc_t exec_type:file { execute execute_no_trans ioctl lock map open read };
```
Great, so the init service script is allowed to execute the sshd binary.

#### Entrypoint permission for a domain on the file

By itself, executing a file with a specific label isn't sufficient to allow a domain transition. Remember that SELinux does not parse contexts itself - it has no knowledge that *sshd\_exec\_t* has anything to do with SSH daemons, let alone *sshd\_t*. So we need to tell it that *sshd\_exec\_t* and *sshd\_t* are related, by saying that the *sshd\_exec\_t* file is an **entrypoint** for the *sshd\_t* domain.

`root #``sesearch -As sshd_t -t sshd_exec_t -c file -p entrypoint -dt````
allow sshd_t sshd_exec_t:file { entrypoint execute execute_no_trans getattr ioctl lock map open read };
```
As a counter-example, checking if *ssh\_exec\_t* (which is used for the SSH client) is known as an entrypoint for the SSH daemon (*sshd\_t*) shows no hits:

`root #``sesearch -As sshd_exec_t -t sshd_t -c file -p entrypoint -dt`
What we can do is ask **sesearch** for what domains the *ssh\_exec\_t* type *is* an entrypoint for.

`root #``sesearch -At ssh_exec_t -c file -p entrypoint -dt````
allow nx_server_ssh_t ssh_exec_t:file { entrypoint execute getattr ioctl lock map open read };
allow ssh_t ssh_exec_t:file { entrypoint execute execute_no_trans getattr ioctl lock map open read };
allow virsh_ssh_t ssh_exec_t:file { entrypoint execute getattr ioctl lock map open read };
allow xm_ssh_t ssh_exec_t:file { entrypoint execute getattr ioctl lock map open read };
```
#### Process domain transition permission

The third requirement is that there must be an SELinux policy saying that that a transition from the source domain to the target domain is allowed. Let's look at the one from *initrc\_t* to *sshd\_t*:

`root #``sesearch -As initrc_t -t sshd_t -c process -p transition -dt`
allow initrc\_t sshd\_t:process transition;

Consider our previous example of the SSH client: the *ssh\_exec\_t* was entrypoint for multiple domains, including *xm\_ssh\_t* and *nx\_server\_ssh\_t*. However, the domains that are allowed to execute the SSH client to transition to the *xm\_ssh\_t* and those that are allowed to execute the SSH client to transition to the *nx\_server\_ssh\_t* are distinct.

`root #``sesearch -At xm_ssh_t -c process -p transition -dt````
allow xm_ssh_t xm_ssh_t:process { dyntransition fork getattr getcap getpgid getrlimit getsched getsession noatsecure rlimitinh setcap setkeycreate setpgid setsched setsockcreate share sigchld siginh sigkill signal signull sigstop transition };
allow xm_t xm_ssh_t:process { getattr transition };
```
`root #``sesearch -At nx_server_ssh_t -c process -p transition -dt````
allow nx_server_ssh_t nx_server_ssh_t:process { dyntransition fork getattr getcap getpgid getrlimit getsched getsession noatsecure rlimitinh setcap setkeycreate setpgid setsched setsockcreate share sigchld siginh sigkill signal signull sigstop transition };
allow nx_server_t nx_server_ssh_t:process { getattr transition };
```
### What you need to remember

What you should remember from this tutorial is that

- SELinux by default inherits contexts, be it from processes (on fork) or parent directories (on entry creation)
- Context transition (change) can be triggered either by policy, by tools like **runcon** or via the SELinux API
- Additionally, Process context (domain) transitions can happen only when the following three conditions hold:
  - the target file context is executable for the source domain
  - the target file context is marked as an entrypoint for the target domain
  - the source domain is allowed to transition to the target domain
