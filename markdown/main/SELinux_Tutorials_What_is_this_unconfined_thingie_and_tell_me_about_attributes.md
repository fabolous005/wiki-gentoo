<!-- source: https://wiki.gentoo.org/wiki/SELinux/Tutorials/What_is_this_unconfined_thingie_and_tell_me_about_attributes | group: Gentoo Wiki (Main) | wiki-title: SELinux/Tutorials/What is this unconfined thingie and tell me about attributes -->
---
title: SELinux/Tutorials/What is this unconfined thingie and tell me about attributes
url: https://wiki.gentoo.org/wiki/SELinux/Tutorials/What_is_this_unconfined_thingie_and_tell_me_about_attributes
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-06-23"
fingerprint: "20890dd60682ff81"
license: CC BY-SA 4.0
---

# SELinux/Tutorials/What is this unconfined thingie and tell me about attributes

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## What is this unconfined thingie (and tell me about attributes)?

If you've been following along with previous tutorial on an SELinux-based system, you've probably already seen a type called *unconfined\_t* popping up in the output of *ps -Z* and *ls -Z*. This type (or domain, when part of a process' security context) is important enough to be subject if this next tutorial. We'll also cover *type attributes*, an SELinux feature which simplifies policy writing.

### What is so special about the unconfined domain

Let's start with an example of a process that runs in the *unconfined\_t* domain.

`user $``ps -eZ | grep gnome`
unconfined\_u:unconfined\_r:unconfined\_t    test  3125  ?  0:00 /usr/bin/gnome-keyring-daemon

Nothing special about this, right? Just another domain like all the others we've seen... only, in SELinux, unconfined domains are allowed to do almost anything (hence the name).

Applications which usually run in confined domains (meaning anything *other* than *unconfined\_t*) may sometimes run in the *unconfined\_t* domain if, for example, they're launched by a user process which is already in the *unconfined\_t* domain. However, domain transitions *away* from *unconfined\_t* are quite unusual.

#### Why using unconfined domains

The idea behind unconfined domains is to support SELinux-enabled systems in which the network-facing daemons (the services) are running in confined domains (like *auditd\_t*, *sshd\_t*, etc.), while regular users processes (like shells and GUI applications) are allowed to run more or less unrestricted by SELinux.

In such configurations, SELinux is meant to primarily protect against remote attacks. Local attacks (those that require local access) are then not mitigated, since local users processes run in the unconfined domain.

Although not as secure, such configuration offer a significant advantage in making policy development much easier, while minimizing the interference of SELinux with user's activities. On the flip side, systems that do not support unconfined domains require users to have sufficient knowledge of SELinux to work effectively.

### Not only unconfined\_t is unconfined

It's easy to sport unconfined processes by noting that they run in the *unconfined\_t* domain. However, other domains exist which can also be unconfined, depending on whether the system supports unconfined domains or not.

To put it differently, if unconfined domains are available on a system, then it's highly likely that some domains other than *unconfined\_t* also allow processes to run without no intervention by SELinux. To make additional domains unconfined, the policy writers make use of type *attributes*. Let's take a closer look at those.

#### SELinux attributes

We've already encountered type 'attributes *in the tutorial on [controlling file contexts](https://wiki.gentoo.org/wiki/SELinux/Tutorials/Controlling_file_contexts_yourself).*

In order to write concise policies, the ability to group multiple contexts together is very useful. That way, you can add a single rule like *All end user domains should be allowed to read the /etc/passwd file*. Without the ability to refer to contexts as a group, the policy writer would be forced to add one rule for every single domain on the system, as in:

allow user\_t etc\_t:file read;
allow staff\_t etc\_t:file read;
allow sysadm\_t etc\_t:file read;
allow unconfined\_t etc\_t:file read;

*SELinux type attributes* provide this grouping functionality. It allows policy writers to group contexts, and then to write rules which apply to every context which is a member of the group. To continue the example above, if each of the types mentioned above has the attribute *userdomain*, the policy writer replace all the individual with a single rule naming the attribute instead of a domain:

allow userdomain etc\_t:file read

attributes have seen increasing adoption by policy writers, and are used increasingly to make policies smaller, more readable and more flexible, without sacrificing the fine-grained which is SELinux' great strength.

You will probably have already noticed that domains or types that do not end in *\_t* regularly appear in the output produced by the **sesearch** utility,
When this is the case, the shown domain or type is actually a type attribute (the convention is that these attributes do not get a suffix). To query the type attributes currently in the policy, you may use the **seinfo** tool. For instance, to get an overview of all types that have the *userdomain* attribute set:

`user $``seinfo -auserdomain -x````
   userdomain
      sysadm_t
      staff_t
      user_t
```
The syntax here is **-a** (for *attribute*) immediately followed (without space in between) by the attribute name.

Vice versa, you can ask **seinfo** to show all attributes assigned to a particular type or domain. So for the *user\_t* domain:

`user $``seinfo -tuser_t -x````
   user_t
      privfd
      process_user_target
      xserver_unconfined_type
      xdrawable_type
      userdomain
      xcolormap_type
      dbusd_system_bus_client
      ubac_constrained_type
      unpriv_userdomain
      nsswitch_domain
      x_domain
      domain
```
#### So what's this unconfined business, anyhow?

As we mentioned earlier, on systems which allow *unconfined\_t*, it is actually not the sole unconfined domain.

Policy writers can mark a domain as unconfined by assigning it the *unconfined\_domain\_type* attribute. Although attribute is always defined in the policy, it only takes effect when *unconfined domains* are allowed. Then, any domain bearing this attribute will be granted the necessary privileges to become unconfined.

You can list all the domains which bear the *unconfined\_domain\_type' attribute with **seinfo**:*

`user $``seinfo -aunconfined_domain_type -x````
  unconfined_domain_type
    kernel_t
    initrc_t
    ...
```
Any process which runs in one of these domain will be unconfined, as long as unconfined domains are enabled.

#### When are unconfined domains allowed?

Unconfined domains are enabled when *unconfined* SELinux module is loaded (SELinux modules are covered in a later tutorial). Since this module provides the *unconfined\_t* type, unconfined domains are allowed if and only if this type is available. To check if *unconfined\_t* is available, we again use *seinfo*:

`user $``seinfo -tunconfined_t`
ERROR: could not find datum for type unconfined\_t

In this case, the output shows *unconfined\_t* is not available on the system, which means unconfined domains are not allowed.

`user $``seinfo -tunconfined_t`
unconfined\_t

In this case, the *unconfined\_t* domain is available, which means unconfined domains are allowed.

### What you need to remember

What you should remember from this tutorial is that

- unconfined domains generally run without any interference from SELinux
- unconfined domains are used to allow *users* to use the system with little interference from SELinux, while network-facing daemons run in confined, SELinux-protected domains
- SELinux has support for *type attributes*, which group contexts together and enable policy writers to write rules which apply to all contexts in the group.
