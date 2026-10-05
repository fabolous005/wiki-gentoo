<!-- source: https://wiki.gentoo.org/wiki/SELinux/Tutorials/Creating_your_own_policy_module_file | group: Gentoo Wiki (Main) | wiki-title: SELinux/Tutorials/Creating your own policy module file -->
---
title: SELinux/Tutorials/Creating your own policy module file
url: https://wiki.gentoo.org/wiki/SELinux/Tutorials/Creating_your_own_policy_module_file
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-06-23"
fingerprint: dfaeca6a6cd64c11
license: CC BY-SA 4.0
---

# SELinux/Tutorials/Creating your own policy module file

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Creating your own policy module file

Eventually, you will need to write your own policy module files and maintain them over the lifecycle of your system. Maintaining SELinux policies isn't that difficult, but it is not a fire-and-forget method: changes you make on the policy will be propagated through the updates. In some cases, you might need to update your policy to be compatible with upstream SELinux policy changes. When that occurs, it is important to keep track of the policies you made.

But let's first look at how a policy file looks like...

### The structure of a SELinux policy module

When developing or updating SELinux policies, it is recommended to use the modular approach. In this situation, you can create your own policy module (say localpolicy.pp and deal with it as if it was part of the global set of policies. Such a policy is written using a *.te* file (type enforcement) and an optional *.fc* file (file contexts) and *.if* file (interfaces).

Let's create a local policy that contains an allow rule.

**`localpolicy.te`**

**Simple policy with an allow rule**

Note the use of the backtick (\`) right after the *gen\_require* call. This is important, as it denotes the start of the block (and the other regular tick character (') which denotes the end of the block).

Given this file, you can now easily create the SELinux policy module localpolicy.pp:

`root #``make -f /usr/share/selinux/strict/include/Makefile localpolicy.pp`
Important to remember here is that the name of the file (localpolicy.te) and the name of the module inside the file (*policy\_module(localpolicy, 1.0)*) and the name of the resulting binary file (localpolicy.pp) are related and need to use the same name (just a different suffix).

You can then load the resulting binary file:

`root #``semodule -i /path/to/localpolicy.pp`
Once loaded, it will remain loaded even after reboots. You *could* remove the localpolicy.te and other files if you want, but we strongly recommend that you keep the source files so that, if you need to make enhancements, you can just edit the files, rebuild the *.pp* file and load it again.

### The reference policy framework

The mentioned *gen\_require* block is new - it is part of the reference policy build framework, and will be translated during the **make -f ...** command earlier on towards SELinux rules. Gentoo uses the reference policy framework, just like most other SELinux-enabled distributions, for the policy development.

One of the principles within the reference policy framework is that you do not include types or attributes from other policies explicitly. Instead, you need to use the interfaces that are made available by the policy modules. In the above example, the role of the policy we had is to enhance the rights of the *user\_t* domain. Hence, we can assume the *user\_t* domain is indeed a good type to directly use (otherwise you'd need to update the unprivuser.te policy, which is governed by Gentoo's policy). The other type (*var\_log\_t*) is an external one (it is a target type), so we need to use an interface (if it exists).

If you have USE="doc" set when installing [sec-policy/selinux-base](https://packages.gentoo.org/packages/sec-policy/selinux-base) then you will have an overview of the available interfaces at /usr/share/doc/selinux-base-\<version> as an easy-to-browse HTML site. In the case of the permissions mentioned in the example, we need to use *logging\_list\_logs*:

**`localpolicy.te`**

**Using refpolicy interfaces**

This makes the intention of the rule more clear. You can also add in comments in the file as you can see, making it easy to document your policy as you add to it more and more.

### Multiple modules

We used a single module for a small update; you can use separate modules or put it all in one. This is a matter of preference, although we recommend that you combine all statements related to the same actor(s) (like the *user\_t* in the above case) and use different modules for different actors. Also name the modules to something that you know are your own policy changes and not provided by Gentoo or upstream projects (for instance by prepending all policy names with *local* or *my*).

Keep the sources of your policy close to you; also, it doesn't hurt to install the binary files (the *\*.pp* ones) at the same location as the other policy modules. This way, if conflicts occur, the full reload that Gentoo's packages will trigger (meaning that they will attempt to load all installed policies at once instead of gradually) will take these modules into account as well.

### Gentoo support for additional modules

Finally, if you have additional modules, you can easily integrate them in Gentoo's way of handling policy files. Simply create a package using the following ebuild (filled in for the *localpolicy* module):

**`selinux-localpolicy-1.0.ebuild`**

**Ebuild for a local policy**

```
# Copyright 1999-2013 Gentoo Foundation
# Distributed under the terms of the GNU General Public License v2
# $Header: /var/cvsroot/gentoo/xml/htdocs/proj/en/hardened/selinux-development.xml,v 1.6 2012/10/28 15:21:07 swift Exp $
EAPI="4"
IUSE=""
MODS="localpolicy"
BASEPOL="2.20120725-r12"
POLICY_FILES="localpolicy.te localpolicy.fc"
inherit selinux-policy-2
DESCRIPTION="SELinux policy for local stuff"
KEYWORDS="~amd64 ~x86"
```
Add your files into the files/ directory of the package, and you're all set.

### What you need to remember

What you should remember from this tutorial is that

- policies are written through the .te files, with optional .fc and .if files
- policy modules are build based on the above files, resulting in a loadable .pp file
- policy development prefers the use of the interfaces provided by other modules
