<!-- source: https://wiki.gentoo.org/wiki/SELinux/Policy | group: Gentoo Wiki (Main) | wiki-title: SELinux/Policy -->
---
title: SELinux/Policy
url: https://wiki.gentoo.org/wiki/SELinux/Policy
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-30"
fingerprint: deae9b6fccd64d88
license: CC BY-SA 4.0
---

# SELinux/Policy

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The SELinux policy is what contains all the rules that SELinux will enforce. It segregates control from definition, making it possible to distribute the same access control rules towards a multitude of systems.

## Introduction

### Control vs definition

A main advantage of having a policy is that it segregates control from definition. The access rules are described / defined in the policy. This policy is then interpreted by the SELinux subsystem to verify and manage access control.

![SELinux MAC permissions.png](https://wiki.gentoo.org/images/8/8d/SELinux_MAC_permissions.png)


### Fully enclosed

A SELinux policy is *fully enclosed*. All information that SELinux needs to govern access controls is available inside the policy. No additional information is needed.

### Modularity

In order to make the SELinux policy more manageable, SELinux supports multiple policy modules. Similar to a kernel configuration, where the entire Linux kernel can be build in a monolithic, single image, but also in a base kernel image with additional, loadable Linux kernel modules.

The base SELinux policy contains rules that cannot be loaded/unloaded, as well as the definitions for the core Linux system controls. Unlike a Linux kernel, even the base policy module can be replaced - but a base policy always needs to be loaded.

Next to the base policies, individual policy modules are defined. These contain all the rules related to an application (or functional coherent set of applications).

### Binary representation

The SELinux policy rules are compiled in a binary format - the human readable rules are not loaded verbatim into the SELinux subsystem. The binary representation allows for much smaller memory requirements, as well as optimized policy decisions.

This binary format might change as features are added. When this occurs, the binary version is increased. The table below gives the current list of binary versions and their added feature.

| Version | Added feature | Since Linux kernel | 
|---|---|---|
| 12 | (Old API for SELinux, now deprecated) |  | 
| 15 | (New API for SELinux) | 2.6.0 | 
| 16 | Conditional policy extensions added | 2.6.5 | 
| 17 | IPV6 support added | 2.6.6 | 
| 18 | Fine-grained netlink socket support added | 2.6.8 | 
| 19 | Enhanced multi-level security | 2.6.12 | 
| 20 | Access vector table size optimizations | 2.6.14 | 
| 21 | Object classes in range transitions | 2.6.19 | 
| 22 | Policy capabilities (features) | 2.6.25 | 
| 23 | Per-domain permissive mode | 2.6.26 | 
| 24 | Explicit hierarchy (type bounds) | 2.6.28 | 
| 25 | Filename based transition support | 2.6.39 | 
| 26 | Role transition support for non-process classes | 3.0 | 
| 27 | Support flexible inheritance of user and role for newly created objects | 3.5 | 
| 28 | Support flexible inheritance of type for newly created objects | 3.5 | 
| 29 | Support constraint naming | 3.13 | 
| 30 | Support for xen target platform and expanded existing I/O memory range to 64 bits |  | 
| 31 | Added InfiniBand (IB) partition key (Pkey) and IB end port object labeling | 4.13 | 
| 32 | Specify glblub as default\_range default and the computed transition for intersection of MLS range of the two contexts |  | 
| 33 | Implements a more space-efficient form of storing filename transition rules | 5.8 | 

### Policy differentiation

It is possible to maintain multiple policies on the system. This differentiation is supported through the concept of *policy stores*. A policy store contains a single policy build, together with various SELinux configuration files used by the SELinux user space (such as default contexts, user mappings and so forth). The active policy store is defined through the /etc/selinux/config file's `SELINUXTYPE` variable.

Gentoo supports four policy types within SELinux: *strict*, *targeted*, *mcs* and *mls*.

The differentiation between *strict* and *targeted* is based upon the *unconfined* domain. When loaded, the processes on the system that are not specifically confined within a particular policy module will be part of the unconfined domains whose purpose is to allow most activities by default (rather than deny by default). As a result, processes that run inside unconfined domains have no restrictions apart from those already enforced by standard Linux security. Although running without the unconfined domains is considered more secure, it will also be more challenging for the administrator to make sure the system still functions properly as there are no policy modules for each and every application "out there".

Next to *targeted* and *strict*, administrators can opt for *mcs* to allow categorization of resources. This is useful on multi-tenant systems such as web servers, virtualization hosts, ... where multiple processes will be running, most of them in the same security domain, but in different categories. Note though that to take advantage of the additional category support, either the applications themselves (such as the web server or hypervisor tools) need to configure the SELinux categories (so they need to support SELinux) or the administrator will need to script around to start the individual instances with separate categories. Otherwise, *mcs* is just the same as *targeted* or *strict*.

Finally, Gentoo also provides *mls* to support a true multi-level security system. However, MLS is currently still considered experimental in Gentoo and as such not recommended.

More information can be found in our [SELinux policy store](https://wiki.gentoo.org/wiki/SELinux/Policy_store) page.

## Policy operations

### Rebuilding existing policy

To rebuild the existing policy (i.e. build the final from the binary base and policy modules), it is sufficient to call `semodule`:

`root #``semodule -B`
If the rules themselves need to be recompiled into binary format, the Gentoo packages that provide the policies need to be rebuild.

`root #``emerge -1 $(qlist -IC sec-policy)`
This will also reload the policies.

### Loading and unloading specific modules

Specific modules can be unloaded and loaded using `semodule`.

For instance, to unload the *screen* policy module:

`root #``semodule -r screen`
In order to load it again:

`root #``semodule -i /usr/share/selinux/mcs/screen.pp`
### Disabling dontaudit rules

It is possible to disable `dontaudit` rules so that SELinux policy *deny* decisions are always logged, even if the policy developer configured it not to have them displayed.

`root #``semodule -DB`
To re-enable `dontaudit` rules:

`root #``semodule -B`
## Integration in Gentoo

### Tweaking policies

The existing policy can be enhanced using the `selocal` script. This script allows for additional SELinux rules to be defined.

For instance, to assign the `zabbix_admin` interface for the staff user:

`root #``selocal -a "zabbix_admin(staff_t, staff_r)" -c "Enabling Zabbix administration for the staff user" -Lb`
The overview of rules managed by `selocal` can be displayed as follows:

`root #``selocal -l`
### Policy packages

Gentoo provides the SELinux policy modules through the `sec-policy/selinux-*` packages. For instance, [sec-policy/selinux-screen](https://packages.gentoo.org/packages/sec-policy/selinux-screen) offers the SELinux policy module for the screen application.

Alongside the standard policy packages, two specific ones are provided:

- [sec-policy/selinux-base](https://packages.gentoo.org/packages/sec-policy/selinux-base) contains the base SELinux policy
- [sec-policy/selinux-base-policy](https://packages.gentoo.org/packages/sec-policy/selinux-base-policy) loads the additional policies that are needed for a functioning system

The latter builds an entire set of additional policy modules which cannot be loaded individually (due to cross-dependencies).

By installing a policy package, the policy is compiled into a binary policy module (like screen.pp) and then loaded into the actively running policy.
