<!-- source: https://wiki.gentoo.org/wiki/SELinux/Gentoo_profiles | group: Gentoo Wiki (Main) | wiki-title: SELinux/Gentoo profiles -->
---
title: SELinux/Gentoo profiles
url: https://wiki.gentoo.org/wiki/SELinux/Gentoo_profiles
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-27"
fingerprint: c49b213e46d2b380
license: CC BY-SA 4.0
---

# SELinux/Gentoo profiles

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Gentoo profiles enable and tune SELinux-specific aspects for a Gentoo system. By default, Gentoo provides a couple of SELinux-enabled profiles, but it is very well possible to update other profiles to enable SELinux.

## Profile structure

In order to simplify the management of SELinux settings in profiles, the features/selinux profile part is created to be as independent of other profiles as possible. In other words, it does not contain a parent file to inherit settings from other profiles. As a result, the SELinux specific settings as offered through the profile can be "injected" in other profiles easily.

### Usage of the selinux part

The features/selinux profile part is enabled currently in the following profiles:

This is done by referencing the features/selinux profile part in the profiles' parent file, like so:

`root #``cat hardened/linux/amd64/selinux/parent`
..
../../../../features/selinux

This means that the profile is the same as hardened/linux/amd64 but with the features/selinux part overriding the settings (if any).

## Default make settings

The SELinux settings in Gentoo are done through the following set of changes:

### Default USE settings

The following USE flags are enabled by default when a SELinux profile is set.

| USE flag | Description | 
|---|---|
| `selinux` | Enable SELinux support in applications or pull in the proper SELinux policy | 
| `unconfined` | Enable support for [unconfined domains](https://wiki.gentoo.org/wiki/SELinux/Unconfined_domains) | 

The `unconfined` USE flag is not mandatory if the policy store that is going to be used is `strict` or, depending on the need for unconfined domains, `mcs` and `mls`.

### Default FEATURES

The following `FEATURES` are enabled by default when a SELinux profile is set.

| FEATURE | Description | 
|---|---|
| selinux | Enable SELinux support in Portage | 
| sesandbox | Enable SELinux sandbox domain in Portage (not related to *SELinux sandbox* application as part of older [sys-apps/policycoreutils](https://packages.gentoo.org/packages/sys-apps/policycoreutils) package!) | 
| sfperms | Enable smart file system permissions (update setuid/setgid files to remove read rights so only execute is left) | 

### Enabling POLICY\_TYPES

The `POLICY_TYPES` variable is declared as follows:

This variable defines, in Gentoo, for which [policy stores](https://wiki.gentoo.org/wiki/SELinux/Policy_store) policies need to be built and managed.

### Enabling PORTAGE\_T

The `PORTAGE_T` variable is declared as follows:

This variable defines the domain in which regular Portage operations are performed, and is used by Portage for dynamic domain transitions and domain validation.

### Enabling PORTAGE\_FETCH\_T

The `PORTAGE_FETCH_T` variable is declared as follows:

This variable defines the domain in which portage tree manipulation operations are performed.

### Enabling PORTAGE\_SANDBOX\_T

The `PORTAGE_SANDBOX_T` variable is declared as follows:

This variable defines the domain in which application builds are done by Portage.

## Masked packages

No packages are marked as being specifically masked in SELinux enabled profiles.

## Base packages

The following packages are made part of the `@system` set when a SELinux profile is used:

## Package-level forced USE flags

The following forced USE flags are set:

- [sys-libs/libselinux](https://packages.gentoo.org/packages/sys-libs/libselinux), [sys-libs/libsemanage](https://packages.gentoo.org/packages/sys-libs/libsemanage) and [app-admin/setools](https://packages.gentoo.org/packages/app-admin/setools) now have `USE="python"` forced, as the management utilities on SELinux systems are based on Python. The build of Python in the libraries is only optional if it is used for embedded systems.
- [dev-lang/python](https://packages.gentoo.org/packages/dev-lang/python) has `USE="xml"` set, as [sys-apps/policycoreutils](https://packages.gentoo.org/packages/sys-apps/policycoreutils) requires it and, as it is part of the base, needs to be forced for the immediate installation of SELinux (including to build stages)

## System-wide forced USE flags

Unsurprisingly, `USE="selinux"` is forced enabled system-wide.

The [audit](https://packages.gentoo.org/useflags/audit) [USE flag is also forced-on because the Linux audit subsystem is commonly used for debugging SELinux issues, as well as logging violations.](https://wiki.gentoo.org/wiki/USE_flag)

## Environment overrides

The following settings are enabled:

### SANDBOX\_WRITE

The definition of `SANDBOX_WRITE` is extended to allow writes to /selinux and /sys/fs/selinux as SELinux-aware applications need to be able to write to this file system (in order to perform SELinux queries).

The same `SANDBOX_WRITE` is also extended to allow writes to /proc/self/ to support the `setfscreatecon` call.
