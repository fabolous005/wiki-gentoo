<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:Inserting_base_module_in_module_store_fails_with_duplicate_declaration | group: Gentoo Knowledge | wiki-title: Knowledge_Base:Inserting_base_module_in_module_store_fails_with_duplicate_declaration -->
---
title: Knowledge Base:Inserting base module in module store fails with duplicate declaration
url: https://wiki.gentoo.org/wiki/Knowledge_Base:Inserting_base_module_in_module_store_fails_with_duplicate_declaration
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-30"
fingerprint: fa269a784cb585dc
license: CC BY-SA 4.0
---

# Knowledge Base:Inserting base module in module store fails with duplicate declaration

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Synopsis

When installing the [sec-policy/selinux-base-policy](https://packages.gentoo.org/packages/sec-policy/selinux-base-policy) package, the postinstall phase fails with the following error:

`root #``emerge sec-policy/selinux-base-policy`
...
>>> Original instance of package unmerged safely.
 \* Inserting base module into mcs module store.
libsepol.scope\_copy\_callback: screen: Duplicate declaration in module: type/attribute sysadm\_screen\_t (No such file or directory).
libsemanage.semanage\_link\_sandbox: Link packages failed (No such file or directory).
semodule:  Failed!
 \* ERROR: sec-policy/selinux-base-policy-2.20110726-r9 failed (postinst phase):
 \*   Could not load in new base policy

The package however is installed correctly.

## Environment

This article applies to Gentoo Linux installations with a SELinux profile:

`root #``eselect profile show`
Current /etc/make.profile symlink:
  hardened/linux/amd64/selinux

## Analysis

The error itself means that the new policy cannot be inserted because an already running, but different, module provides the same type or attribute. For instance, suppose that `sysadm_screen_t` is declared by the `screen.pp` SELinux module, then a definition of `sysadm_screen_t` in the base policy will hold back the installation of this module as long as the `screen` module is still loaded.

Sadly, there is no method available (yet) to look into modules and see what they offer, which means it is sometimes a trial and error. Luckily, we can extract some information from the modules that helps this trail and error method to be sufficiently fast.

## Resolution

First find the culprit module that is also defining the offending type (in the example, this is `sysadm_screen_t`). One method is to find any occurrence of the string "`sysadm_screen_t`" in the available modules:

`root #````
cd /etc/selinux/mcs/modules/active/modules
```
`root #``for MOD in *.pp; do grep -H sysadm_screen_t ${MOD}; done`
Binary file screen.pp matches
Binary file fixscreen.pp matches

In the above example, there are two modules that have some reference to `sysadm_screen_t`; one is the "official" screen module (`screen.pp`), the second one is a module created by the user to update the policy (here called `fixscreen.pp` but can be anything). Assuming the latter is the offending module, unload it:

`root #``semodule -r fixscreen`
After this, try to reload the base policy:

`root #``semodule -b /usr/share/selinux/mcs/base.pp`
If inserting the base module still fails, unload the other module as well (`screen.pp` in our example) and retry.

Afterwards, rebuild the offending modules (through [sec-policy/selinux-screen](https://packages.gentoo.org/packages/sec-policy/selinux-screen) so that the updated policy is back in place.
