<!-- source: https://wiki.gentoo.org/wiki/SELinux/Policy_store | group: Gentoo Wiki (Main) | wiki-title: SELinux/Policy store -->
---
title: SELinux/Policy store
url: https://wiki.gentoo.org/wiki/SELinux/Policy_store
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-30"
fingerprint: "5e2c93076496dd18"
license: CC BY-SA 4.0
---

# SELinux/Policy store

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

A policy store contains the SELinux policy package and system administrator modifications combined in a single, logical entity. Multiple stores can be used on a system, allowing administrators to have separate SELinux policies which can be switched (either directly or after reboot).

## Introduction

### Policy store location

The policy store is located in /etc/selinux in a subdirectory called after the policy store.

Pre-defined policy stores are *strict*, *targeted*, *mcs* and *mls*, but this can be fully configured by the administrator.

### Policy differentiation

By allowing multiple policy stores, administrators can support different policies on a single system.

For instance, an administrator might have both *strict* and *mcs* available. The *strict* policy does not support MLS, whereas *mcs* does (but with a single security level).

### Active policy store

The active policy store is configured in /etc/selinux/config through the `SELINUXTYPE` variable:

**`/etc/selinux/config`**

**Active store declaration**

## Gentoo integration

### POLICY\_TYPES in make.conf

The policy stores that need to be maintained on a Gentoo system are covered by the `POLICY_TYPES` variable.

By default, this variable is defined in the Gentoo profile and set as follows:

The variable can be overridden through the /etc/portage/make.conf file.

## Managing policy stores

### Switching active policy store

In order to switch the active policy store (i.e. the `SELINUXTYPE` in /etc/selinux) it is necessary to ensure that the base policy and other policy modules are built. In Gentoo, this is handled through the `POLICY_TYPES` variable which can be defined in /etc/portage/make.conf.

Assuming the current active policy store is *mcs* and the target policy store is *strict*, then verify that both are set in the variable.

If this was not the case, update the variable and then rebuild all SELinux policy packages to make sure both policy stores are available and up to date.

`root #``emerge -1 $(qlist -IC sec-policy)`
Now switch to *permissive* mode. This is needed because in the next steps the new policy is loaded and a full file system relabel operation will be launched. This cannot be done using the existing policy in enforcing mode.

`root #``setenforce 0`
Edit /etc/selinux/config and modify `SELINUXTYPE` to the new value (*strict* in the example).

`root #``nano -w /etc/selinux/config`
Edit /etc/selinux/sepolgen.conf and modify `SELINUX_DEVEL_PATH` to the new paths in the new policy store (*strict* in the example).

`root #``nano -w /etc/selinux/sepolgen.conf`
Load the policy modules for the new policy store.

`root #````
cd /usr/share/selinux/strict
```
`root #``semodule -b base.pp -i $(ls *.pp | grep -v base.pp)`
The *strict* policy store is now active. The next step is to relabel all files. This is done in two steps:

1. relabel all files accessible
2. relabel the files that are hidden beneath existing mount points

`root #````
rlpkg -a -r
```
`root #````
mount -o bind / /mnt/gentoo
```
`root #````
setfiles -r /mnt/gentoo /etc/selinux/strict/contexts/files/file_contexts /mnt/gentoo/dev
```
`root #````
setfiles -r /mnt/gentoo /etc/selinux/strict/contexts/files/file_contexts /mnt/gentoo/lib64
```
`root #``umount /mnt/gentoo`
Edit the /etc/fstab file so that the `rootcontext=` and other SELinux mount parameters are adjusted accordingly. The main change needed here is when the previous policy store and the new policy store are different with respect to their MLS support. So for a *mcs* to *strict* switch, a trailing `:s0` would need to be removed from all contexts.

This is all to it. Now reboot the system and the new policy store should be running, in enforcing mode.
