<!-- source: https://wiki.gentoo.org/wiki/SELinux/Tutorials/Creating_a_daemon_domain | group: Gentoo Wiki (Main) | wiki-title: SELinux/Tutorials/Creating a daemon domain -->
---
title: SELinux/Tutorials/Creating a daemon domain
url: https://wiki.gentoo.org/wiki/SELinux/Tutorials/Creating_a_daemon_domain
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-06-23"
fingerprint: c08ca4664c94139d
license: CC BY-SA 4.0
---

# SELinux/Tutorials/Creating a daemon domain

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## A domain skeleton

In order to create a SELinux domain (a set of types that helps confine a daemon or service) we start off with a simple skeleton policy. In the example, we're going to create a domain for an application called *test*, so the domain will be called `test_t` and the policy file test.te.

**`test.te`**

**A sample skeleton**

That's it. If we would now label the daemon binary with `test_exec_t` and have it started through an init script, you'll notice that the daemon would start running (and presumably immediately fail because the `test_t` domain has very few privileges) as the `test_t` domain.

## Enhancing the policy with additional types

The next step is to enhance the policy with types used for the various resources that the daemon would handle.

For instance, it probably is going to write to log files, so:

**`test.te`**

**Adding log types**

It probably also creates a PID file, so:

**`test.te`**

**Adding PID file support**

And you can go on with this using lock files (`test_lock_t`), configuration files (`test_etc_t`), variable files (`test_var_lib_t`) and so on. Make sure to look for example domains in publicly accessible repositories (such as gentoo's hardened-refpolicy repository) for good examples on how to do this.

## Test and enhance

Next, try to test out the daemon by marking its binary as `test_exec_t` and launching it through an init script:

`root #````
chcon -t test_exec_t /path/to/testDaemonBinary
```
`root #``run_init rc-service test start`
You will probably start getting denials in the audit logs. Take a good look at those and start enhancing the policy. With every addition, retake the test.

Some pointers:

1. Try testing with enforcing on. Many resources use permissive to develop a policy, but this might yield unnecessary privileges to be assigned to the domain
2. Try only "allowing" the first denial you get (and if possible, one that is related with an error message displayed by the daemon) and not all denials that follow. The denials that follow the first one might be caused by not being able to do what it is supposed to do, and therefore shouldn't be in the policy.

## Asking for peer review

If you properly document the policy (with comments) then it might be a good idea to request for peer review.

You can try asking on the `#selinux` IRC channel, or on `#gentoo-hardened`, but there are also mailinglists that you can consult such as the reference policy mailinglist.

## Next steps

Right now we have a (hopefully) working policy, but none of the user domains (unless you have unconfined users) will be able to handle the daemon. Our next tutorial will therefore be about creating proper interfaces that you can assign to user domains in order to interact with the daemon domain.
