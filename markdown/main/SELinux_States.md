<!-- source: https://wiki.gentoo.org/wiki/SELinux/States | group: Gentoo Wiki (Main) | wiki-title: SELinux/States -->
---
title: SELinux/States
url: https://wiki.gentoo.org/wiki/SELinux/States
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2015-06-10"
fingerprint: e7fedd2fe3424b80
license: CC BY-SA 4.0
---

# SELinux/States

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

SELinux can be easily disabled or put in a more permissive mode. For many administrators, disabling SELinux is used as a fast way of working around permission failures on a system. However, disabling SELinux is similar to removing a firewall between systems because it was preventing a network connection. Sure, the access is now allowed, but administrators ought to look at the reason why the failure occurred and resolve that, rather than removing the security measure itself.

## Introduction

SELinux can be disabled or enabled. When enabled, it can be in a permissive mode or enforcing mode. When running in enforcing mode, it is possible to mark specific domains as permissive.

### Enabled or not

When the Linux kernel has SELinux support enabled (which is of course a prerequisite for having an SELinux-enabled system) the SELinux subsystem starts in a permissive mode (unless the `selinux=0` kernel boot parameter is used). The Linux init system then locates the SELinux policy and SELinux configuration file (/etc/selinux/config). If the configuration has `SELINUX=disabled` then SELinux will not do anything further and has the system boot without SELinux support.

Assuming `SELINUX=permissive` or `SELINUX=enforcing`, then the SELinux policy is loaded. If no policy can be found, but `SELINUX=enforcing` is set, then the system will freeze; otherwise the system will continue to boot without SELinux (as no policy could be found). In most cases though, the init system will successfully load the SELinux policy and the system will boot further with SELinux enabled.

### Permissive or enforcing

When SELinux is configured to start in permissive mode (or through the `enforcing=0` kernel parameter) then SELinux is enabled and will log every action that it *would* deny. However, as the name implies, SELinux will not actually enforce any policy decisions.

That doesn't mean that the system will behave as if there is no SELinux though. Some applications are *SELinux-aware*, meaning that their code checks SELinux information (policies, but also SELinux state) and alters its own behavior based on this information. If such a SELinux-aware application does not properly check the mode (permissive or not) then it might deny any action that the policy says shouldn't occur - even if SELinux is running in permissive mode.

In enforcing mode of course, the SELinux security subsystem will enforce policy decisions.

### Permissive domains

The SELinux policy supports marking domains as permissive. In this case, the mentioned domain runs in a permissive mode (SELinux will not enforce policy decisions for any action taken by that domain) while the rest of the system is in enforcing mode.

## Managing SELinux state

### Disabling SELinux

To disable SELinux, it is sufficient to edit /etc/selinux/config and set `SELINUX=disabled`.

**`/etc/selinux/config`**

**Disabling SELinux**

Then reboot the system.

To disable SELinux during a single boot, it is also possible to use the `selinux=0` kernel boot parameter.

### Switching permissive and enforcing mode

To mark SELinux in permissive or enforcing mode, edit /etc/selinux/config and set `SELINUX` to *permissive* or *enforcing* depending on the requirement.

**`/etc/selinux/config`**

**Booting in enforcing mode**

Then reboot the system.

It is also possible to switch between permissive and enforcing mode directly. For this, the `setenforce` command can be used.

For instance, to have SELinux run in permissive mode:

`root #``setenforce 0`
### Querying current SELinux state

The current state of SELinux can be obtained through the `sestatus` command:

`root #``sestatus | grep mode`
Current mode:               enforcing

### Listing permissive domains

With `semanage`, the current list of permissive domains can be queried:

`root #``semanage permissive -l`
Builtin Permissive Types 
  
  
Customized Permissive Types
  
portage\_t

### Marking a domain as permissive

To mark a domain as permissive domain, use `semanage`:

`root #``semanage permissive -a portage_t`
### Removing the permissive state of a domain

To remove a permissive state from a domain, use `semanage`:

`root #``semanage permissive -d portage_t`
