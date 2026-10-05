<!-- source: https://wiki.gentoo.org/wiki/SELinux/Tutorials/SELinux_Multi-Category_Security | group: Gentoo Wiki (Main) | wiki-title: SELinux/Tutorials/SELinux Multi-Category Security -->
---
title: SELinux/Tutorials/SELinux Multi-Category Security
url: https://wiki.gentoo.org/wiki/SELinux/Tutorials/SELinux_Multi-Category_Security
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-06-23"
fingerprint: "435b8e2596b446c5"
license: CC BY-SA 4.0
---

# SELinux/Tutorials/SELinux Multi-Category Security

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## SELinux Multi-Category Security

With the theory behind SELinux' MLS behind us, we want to tell you a bit about a specific MLS implementation, called MCS.

### Multi-Category Security

The SELinux *Multi-Category Security*, abbreviated to mcs, is a special case in the MLS support. It is a MLS implementation, but where the number of sensitivity levels is one (and no more). As a result, the MLS security features (dominance checks and rules) are fully based on the categories that are identified. Hence the name, multi-category security.

By limiting the sensitivity levels, dominance rules are reduced as well:

- a process *dominates* a resource (or other process) if the process as a superset (the same set or more) of the categories assigned to the resource.
- a process *is dominated by* a resource (or other process) if the process has a subset (the same set or less) of the categories assigned to the resource.
- if the process and the resource both have at least one category that the other doesn't have, then the two are *incomparable*

#### Use of MCS in SELinux systems

MCS is used in a couple of applications, where sVirt is the most documented one. sVirt, or Secure Virtualization, uses categories to restrict the access of guest images and inter-process communication between differently labeled guests.

Guests are started in the *svirt\_t* domain, but with a special, unique category. Let's say *c1,c2*. The image for this guest, labeled with *svirt\_image\_t* has category *c1,c2* as well. As a result, this guest can (luckily) read and write to its image. Another guest has another category set, like *c3,c4*. Because of that, this guest - even while running in the *svirt\_t* domain, cannot access the image of the other guest, because the categories are incomparable.

This is handled by the sVirt code (and the administrator). SELinux itself only enforces the security constraints that are put in place for MLS (and thus also MCS).

Categories are set by the applications or by the users. SELinux does not calculate the sensitivity and category of a new domain upon transitioning, or when writing files. It either allows or denies the activity, but it is the user (or application) responsibility to switch this level (within the limits allowed by the policy).

### What you need to remember

What you should remember from this tutorial is that

- MCS is a 'smaller' version of MLS
- users (or applications) are responsible for handling the sensitivity (within the limits of what is allowed by policy), SELinux does not automatically 'deduce' the right context
